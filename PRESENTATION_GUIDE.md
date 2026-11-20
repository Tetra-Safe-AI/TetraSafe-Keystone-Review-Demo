# Presentation guide for the documented demonstration

**TetraSafe Keystone Component Demo — presentation P1 viewer of version 20 evidence**

**Presentation update as of November 18, 2026:** presentation P1 passed its reported Linux checks and was visually reviewed in Stephen's Windows browser using the saved run. See the [P1 validation record](PRESENTATION_P1_STATUS.md) for archive identifiers, observations and limits. The command below now starts the **P1 viewer**, which displays evidence originally produced by frozen version 20. The signed `v20-run-01` evidence was not rewritten.

This publicly available guide accompanies the [README](README.md) and explains how to present the recorded demonstration in a supervised setting. Publication of this guide does not authorize distribution of the demonstration package, protected Keystone implementation, or complete evidence bundle.

## The purpose of the demonstration

The Keystone Mechanism is an architectural component developed by TetraSafe to control whether consequential actions can proceed under the authority currently available to an execution boundary. The demonstration is a separate piece of software designed to make a small part of that research observable without disclosing the protected mechanism.

The central question is easy to explain. An agent receives authorization for a refund and sends it. The computer is then restored to an earlier state, so it no longer remembers the refund. What happens if the agent presents the original authorization again?

In this specific version, the execution boundary accepts that old authorization after restoration. A witness outside the guest virtual machine's restoration domain still remembers the original dispatch. It detects the rollback and reports a suspected duplicate after observing a second dispatch. The mechanism in this demonstration **does not prevent the second dispatch**. The honest result includes both the working controls and this known failure.

## A suggested spoken introduction

“We've been investigating how to control consequential actions taken by autonomous software. A refund makes the problem concrete because a mistaken retry could cause another payment. The Keystone Mechanism is the architectural component we're researching. What you're seeing today is a controlled demonstration of selected behavior, not a disclosure of the mechanism's private implementation.

“First we'll see a simulated refund recorded. Then the virtual machine will be restored from a snapshot taken before that refund. The restored machine forgets the transaction. A separate witness, running outside the virtual machine, retains its signed observations and recognizes the rollback.

“Some authority controls still refuse invalid requests after restoration. But there's also a known failure: the authorization issued before restoration can be used again. We deliberately show that failure because it reveals what an execution boundary still needs to establish before a recovered system can safely act. The purpose here is to show what the evidence supports, including its limits.”

## Showing the recorded run

For a first private presentation, prefer the **saved evidence replay** of `v20-run-01`. It demonstrates the recorded sequence without changing the virtual machine, taking snapshots, reinstalling software, or generating a new run. The evidence and full run have already passed the delivered version 20 verifier on Windows, and the same verifier code reproduced the result on a separate Linux machine.

On SpiceLand, the following is the **P1 saved evidence replay** command Stephen successfully ran. It requires no change of PowerShell folder and is not a new demonstration run:

```powershell
py "C:\November work\TetraSafe_Keystone_Presentation_P1_v1\track2-run7-evidence-demo-v6\kdemo.py" --root "$HOME\KeystoneDemo6-v20b" --vm "keystone-demo-v20b" replay --label v20-run-01
```

It starts a local saved evidence viewer at `http://127.0.0.1:8490/`. The replay is read only with respect to the evidence and does not need the virtual machine. Stop the local viewer with Ctrl+C when finished. Do not substitute the shortened commands printed by the demo, because those omit explicit selection of the working root and virtual machine and may refer to an older environment.

The P1 viewer opens on **Second dispatch observed**, the central Step 8 finding. Use the clearly named stage buttons to show the first refund, the rollback detected by the witness, and the later authorization checks. The witnessed second dispatch is marked `SUSPECTED_DUPLICATE`; the page does not claim it was prevented or that real payments settled twice. The **Final evidence record** button shows **STALE** because the shipper was deliberately stopped to close the record. The P1 viewer explains that this is not failed verification. Keep the rebuilt verified view selected rather than clicking the link to the original captured wording, which the viewer identifies as unverified.

## Evidence to describe accurately

The completed Windows run on November 18, 2026 reported 28 of 28 automatic checks passed. Of those, 17 have support in the authenticated witness record and 11 come from local observations during the run. The evidence verifier itself reported **PASS**, authenticating all **1,628 of 1,628 signed entries** with a final co-signed checkpoint and no unauthenticated tail.

Claude separately checked the archived bundle on Linux using his copy of the delivered version 20 checker and a witness public key supplied outside the bundle. He reproduced the same PASS result and verified that the 67 extracted files were unchanged by examination. His separate safety scan checked for key-like filenames and standard private key headers. It was not a complete raw-key search; the completed version 20 bundle builder had additionally checked the bundle content against the run's actual private keys. That is a valuable reproduction of verification on a different machine, **not** an independently written verifier, a second demonstration of the virtual machine behavior, or a guarantee of disclosure safety.

The witness's original key was generated locally during setup. A viewer cannot independently establish the identity of the person behind that key just because the evidence and its embedded key agree. If provenance matters to a recipient, provide the recorded fingerprint through a separately trusted channel.

## Boundaries to disclose without embarrassment

The refund destination is simulated and is restored with the virtual machine. Two observed dispatches are therefore **not proof of two real payments being settled**. The warning that the policy limit might be exceeded is correctly conditional.

The experiment uses a powered off disk snapshot followed by a cold boot, not the rollback of a live machine. The witness is separated from the guest's restoration domain, but not from the host computer itself. Only one complete version 20 demonstration has been run on Windows so far; repeatability and Windows file failure paths remain untested.

The wrapper also has known operating limitations. Interrupted runs can leave evidence in a confusing state under particular file move failures, and its setup expects the virtual machine to have an existing snapshot. These are disclosed problems in the demonstration tooling, not evidence that the protected Keystone architecture must be rebuilt.

## Operating boundary

**Do not run setup or another live demonstration casually.** The working version 9 virtual machine and evidence must remain untouched. The version 20 test used a separate cloned virtual machine, `keystone-demo-v20b`, and a separate working root, `KeystoneDemo6-v20b`. Commands that act on a virtual machine require explicit root and VM arguments and a deliberate check of the intended state.

For the immediate private presentation, the saved replay is sufficient. It is appropriate to say, “This is the recorded result of a completed run, with verification reproduced on a separate Linux machine using the same delivered checker code.” It is not appropriate to say, “An independent party proved the mechanism prevents duplicate refunds,” because this particular known failure demonstrates otherwise.

The next research questions are open questions, not missing presentation slides. They concern what evidence outside a restoration domain is sufficient to re-establish continuity and authority after the local system loses trustworthy history, and what an operator needs in order to resolve a safely held transaction without guessing.

## Sharing decision

This guide and the README are publicly available documentation. No source code, evidence bundles, private keys, credentials, or packages should be added to the repository without a separate review of their contents and the claims they are intended to support. The bundle contains five reference Python source files that recipients could read, so treat any proposed distribution as a deliberate disclosure decision. Feedback on the presentation should be recorded as feedback first; discovering a limitation does not, by itself, authorize changing the mechanism or creating another version.
