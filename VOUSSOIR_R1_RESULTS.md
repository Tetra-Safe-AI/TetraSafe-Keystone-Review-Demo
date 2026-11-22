# Voussoir R1: preventing a repeat after a real virtual machine restore

**Prepared November 21, 2026. A public summary of an experimental extension to the TetraSafe Keystone Component Demo. It is not a software release, an evidence release, or a production security certification.**

Voussoir is the name for the continuing work after the version 20 demonstration. Version 20 showed a known failure: an authorization issued before a restore could be accepted again after it, and the witness could only report the resulting duplicate after the fact. R1, the first Voussoir experiment, asks whether that particular repeat can be refused *before* dispatch.

The underlying records are private. They are Notebook records 288 to 292 (design), 320 (release) and 321 (results), and the original run evidence is kept in a private evidence repository. This summary keeps the measured results, the method and every material limitation. It leaves out the raw logs, the machine identifiers and the account paths.

## The question

When the virtual machine holding the Keystone execution boundary is restored to an earlier snapshot, it forgets that an authorization was already used. The agent still holds that authorization. **If the record that the authorization has been consumed is kept outside the machine being restored, does the boundary refuse to use it a second time?**

## What changed from version 20, and what did not

**Unchanged:**
- The enforcement code is the same frozen "run 7" code that version 20 demonstrated. Its components are the recorder, issuer, admission check, intermediary, anchor and simulated destination.
- The witness, the evidence format and the verifier are those of version 20.

**Changed: where two of those components run.**
- The **anchor**, the component that records an authorization as consumed before anything is sent, runs on a second virtual machine. That machine is never restored.
- The **simulated destination** runs there too. Executions are therefore counted outside the restored machine.
- The Keystone virtual machine reaches both through a restricted SSH relay over a VirtualBox internal network between the two virtual machines.
- The relay's restrictions were checked on both machines before any run. Each key can reach only its own socket and cannot open a shell.

This is experimental connection plumbing around unchanged enforcement code. It is not version 21 of the demonstration, and it is not a new version of the protected mechanism.

**The comparison.** The controlled comparison changes the placement of the authoritative consumption record, using the corresponding enforcement layouts. The executor restoration procedure and original token replay are the same.

## The machines

All machines ran on Stephen's Windows computer, under VirtualBox 7.2.22.

- **The Keystone virtual machine:** a copy of the version 20 demonstration machine. It is the **only** machine restored.
- **The outside virtual machine:** holds the anchor and the simulated destination's ledger, and is never restored.
  - It was made as a copy of a reference clone, with the earlier Keystone files removed and new SSH host keys and identity generated.
- **The witness and its co-signer:** run on the Windows host, as in version 20.
- **The original version 20 demonstration machines:** kept untouched. See "Protection of the original demonstration".

## How each run proceeds

Each run follows the version 20 demonstration sequence:

1. **The refund.** A synthetic $100 refund, DEMO-1, is accepted and executed once.
2. **The restore.** The Keystone machine is powered off hard and its disk is restored to a snapshot taken, with the machine powered off, before DEMO-1. It is then cold-booted from the restored disk. Like version 20, this is a disk restore followed by a restart, not a live memory rollback.
3. **Enforcement checks.** These are repeated on the restored machine.
4. **Step 8.** The agent presents DEMO-1's original authorization, the same token it kept from before the restore.

## Results

### The controlled comparison: four runs on the same machines and procedure

| Run | Where the consumption record lives | Step 8 outcome | Executions of DEMO-1 at the destination | Witness finding |
|---|---|---|---|---|
| **n2-01**, a deliberately broken control | inside the restored machine | `provider_accepted` | **2**, both under the same token | `SUSPECTED_DUPLICATE` |
| **r1-01** | on the outside machine | **`held_anchored`**: refused before dispatch | **1** | none |
| **r1-02** | on the outside machine | **`held_anchored`** | **1** | none |
| **r1-03** | on the outside machine | **`held_anchored`** | **1** | none |

**The broken control matters.** It shows that this environment, under this real restore, *can* produce and record a duplicate. A "no duplicate" result in the protected runs is therefore not explained by a test setup unable to observe one.

### Per-run detail

| | n2-01 | r1-01 | r1-02 | r1-03 |
|---|---|---|---|---|
| Automatic checks passed | 33 of 33 | 34 of 34 | 34 of 34 | 34 of 34 |
| Witness log entries authenticated; verifier | 265 of 265; PASS | 202 of 202; PASS | 203 of 203; PASS | 250 of 250; PASS |
| Consumption record outside after the first refund | n/a (inside) | `DEMO-1:1` | `DEMO-1:1` | `DEMO-1:1` |
| After the restore: Keystone's own records of DEMO-1 | 0 | 0 | 0 | 0 |
| After the restore: the outside record still holds `DEMO-1:1` | n/a | yes | yes | yes |
| Rollback detected by the witness | yes | yes | yes | yes |
| Other enforcement checks after the restore: legitimate new refund, reused authorization, changed amount, revoked authorization, different agent | as in frozen run 7 | same | same | same |
| Step 8b: can the restored machine reconcile DEMO-1 on its own? | n/a | **no (`no_row`)** | **no** | **no** |
| Restore to resumed operation (seconds) | 40.9 | 46.9 | 44.0 | 40.2 |

**Timing.** The restore-to-resumed time measures how long the virtual machine took to recover: the restore, a cold boot, and the software restarting. It is not a measure of how long the authorization check takes.

### Failure cases around the same restore: one run, `cases-01`

The run passed 12 of 12 checks, with 482 of 482 witness entries authenticated and the verifier PASS.

| Case | What was done | Outcome | Destination | Reading |
|---|---|---|---|---|
| **B4a** | The relay to the anchor was stopped; a new refund was attempted | not sent | 0 | If the outside record cannot be reached, nothing executes |
| **B4b** | The anchor process itself was stopped; a new refund was attempted | not sent | 0 | Same, with the anchor down |
| **B6-K2** | The executor was killed before the consumption record was written; restore; the token was presented again | accepted | 1 | Correct: nothing had been recorded or sent before the crash |
| **B6-K3** | Killed after the record was written but before sending; restore; presented again | `held_anchored` | 0 | **Safe, but stuck.** The refund is held and never completes without reconciliation. See the limitations |
| **B6-K5** | Killed immediately after sending; restore; presented again | `held_anchored` | **1** | A crash right after execution, followed by a real restore, still did not produce a second execution |

## What kinds of evidence support these numbers

The evidence comes in three kinds, and they carry different weight.

1. **The witness log is the authenticated record.**
   - It is signed and co-signed, and verified with the version 20 verifier: PASS, with every entry authenticated in all five runs.
   - It establishes what the Keystone machine reported: its sends, its rollback, and the absence or presence of a second send.
2. **The outside destination ledger and the outside consumption record.**
   - The run reads these from the outside machine and saves them in each run's evidence.
   - They are **not entries in the witness-signed log.** They are observations captured by the run, which runs on the same computer under the same administrator.
3. **The Keystone machine's own stores, before and after the restore.**
   - These are read by the run itself.
   - They are local observations, not witness-authenticated.

**Why "1 execution" is not proven by the witness alone.** The destination count comes from the second kind. The witness's own count of sends agrees with it in every run, but a reviewer should weigh these separately.

## Preparation incidents, disclosed

The following happened while the machines were being prepared. None changed the enforcement code or the evidence.

- **A preparation script stopped part-way.** Windows PowerShell 5.1 treated VirtualBox's progress messages as an error. The script was corrected and its recovery tested under the same PowerShell error-handling behaviour. The partially completed step was finished from the state actually found, after a read-only inspection.
- **An SSH connection reset during preparation.** One attempt to prepare the outside machine ended with the SSH connection being reset right after the password prompt. The cause was **not established**. A missed prompt or a login timeout was inferred, not demonstrated. Nothing had run inside the machine: it still presented its original host key, and no sudo prompt had appeared. A diagnostic login then succeeded, and the step was repeated.
- **A missed password prompt during setup.** One setup attempt stopped when the password prompt for installing the setup's login key went unanswered. Nothing had been copied into the machine. Setup was run again, as its own message directs, and completed.
- **A Windows tool fault, found earlier.** Windows `ssh-keyscan` fails to negotiate with the Ubuntu 24.04 SSH server, so an earlier connectivity check had wrongly reported "no answer". The Voussoir runs do not use that tool.

## Protection of the original demonstration

After the series, the original version 20 demonstration machines and protected folders were compared with a pinned baseline. The result was **unchanged.** Every protected virtual machine record and both SSH known-hosts records were identical. The only differences were a settings-file rotation verified earlier and newly added files in a working folder where additions are allowed.

## Material limitations

1. **Stuck after a restore (`no_row`).** After the restore, the executor has no local record of DEMO-1. Safety held, but the organization must reconcile the case by other means. No recovery or reconciliation protocol exists yet, and none was tested.
2. **The stranded hold (B6-K3).** If the executor stops after recording consumption but before sending, the refund is held indefinitely. It is not lost or duplicated, but it does not complete. No resolution protocol exists yet.
3. **Failing closed has a cost.** When the outside record is unreachable, nothing executes. Availability now depends on the outside machine.
4. **One computer, one administrator.** The outside machine is outside the Keystone machine's *restoration domain*, but not outside anyone's *control*. An administrator who can restore or edit the outside machine can defeat the mechanism. Independent custody was not tested.
5. **Only the Keystone machine was restored.** These were not tested:
   - restoring the outside machine;
   - restoring the witness or the host;
   - restoring several machines together.
6. **The destination is simulated.** No real provider, payment or settlement was involved.
7. **Revocation or expiry across a restore** was not tested.
8. **A reduced experiment.**
   - The unchanged version 20 demonstration was not re-run in this environment. Its failure is documented separately, from November 18.
   - These were not run in this series: concurrency testing, the timing benchmark, and several further authentication cases from the full plan.
9. **What confirms the restore happened:** VirtualBox's own snapshot operations, Keystone's empty records after the restore, and the witness's rollback detection. Not separate disk forensics.
10. **The outside machine's origin.** It was derived from a copy of the demonstration image, with its earlier Keystone files removed and new keys. Its separation is logical, not an independently built machine.
11. **Three protected repetitions and one broken control,** on one computer. This shows the result repeated. It is not a statistical estimate.

## What this does and does not show

**Shown, within the boundary above:** on real VirtualBox restores of the executor machine, a consumption record kept outside that machine caused the original authorization to be refused before dispatch, three times out of three, and also in the crash-after-send case. When the record was restored along with the machine, the same authorization executed a second time and the duplicate was observed.

**Not shown:**
- independent custody;
- recovery or completion after a restore;
- behaviour when the authority machine itself is restored;
- revocation across a restore;
- behaviour with real payment providers.

## For reviewers: where to push

These are the questions we think deserve the hardest scrutiny:

- **Is the outside record really independent?** It is outside the restore, but not outside the administrator's reach. What would a credible separation of custody look like, and how would it be tested?
- **Is "held" good enough?** `no_row` and the stranded hold mean a safe outcome can still leave a customer's refund unresolved. Who owns that case, and what evidence must survive for the next operator?
- **Is the destination count trustworthy?** It comes from the outside machine, not the witness log. What would independently confirm the number of executions?
- **What wasn't restored?** What happens if the outside machine, or both machines, are restored to different points?
- **Is the comparison fair?** The broken control and the protected runs use the corresponding frozen enforcement layouts on the same machines and procedure. Is there any difference other than where the consumption record lives that could explain the result?

## Identifiers for later verification

**These values identify the exact private evidence and release.** They let a reviewer who is given access confirm that the material matches what is reported here. Publishing them here does not release that material.

**The release:**
- **R1 release manifest SHA-256:** `be368825028e83d8ac303fe18c86d468e5deaaa325ad6024ccf86778486f9742`
- **Frozen version 20 package manifest SHA-256** (unchanged): `3105d61a164bdea760f3ee2f7cf24344e3e9cdd87450148fa5f2c2eac154dd5e`

**Each run's report:**

| Run | Evidence files | `report.json` SHA-256 |
|---|---|---|
| n2-01 | 103 | `3D5652BB9F8B7BDE28844CE75A7A21CB239B74133D3BEE20D20BBA81D58D3AE3` |
| r1-01 | 101 | `320FC6C648EC58A9C13B770D4619BFC1729611444E73CD10B7817AA3D0B317F1` |
| r1-02 | 101 | `53E653CFA4D99878CCD5C7B740A217A2F7893D5608C4F5ABCDBE545043EEC214` |
| r1-03 | 105 | `88B1774310C2945A23A87BF542BD0E1B5BD8E68CED407D7A92F5A36CD624621B` |
| cases-01 | 109 | `567A727576CE7DD98D1FDA97C7DE6B23DB41F9F060E11129B8E39630AFE9C885` |

**The co-signing key.** As in version 20, the key used to verify each run's record was generated during local setup. Trusting it requires a fingerprint obtained through a separate, trusted channel.

## Not included here

This repository does not include:
- the Voussoir run evidence or raw logs;
- the experimental code;
- the protected mechanism's source;
- any private keys, credentials, machine identifiers or account paths.

Any wider release of those materials is a separate decision, and would need the same file-by-file disclosure review described in the [README](README.md).
