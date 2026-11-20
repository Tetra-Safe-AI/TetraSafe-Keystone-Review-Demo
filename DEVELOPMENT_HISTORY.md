# TetraSafe Keystone Component Demo: development history, versions 1 through 20

**Prepared November 18, 2026. Public documentation of the demonstration history. Protected source code and unrestricted evidence bundles are not included.**

This is the history of twenty **demonstration package versions**, not twenty revisions of the protected Keystone Mechanism. The underlying frozen Keystone run 7 enforcement code was the subject of the demonstration. Most changes after the early versions concerned orchestration, Windows and VirtualBox operation, evidence custody, verification, replay, security checks, and the accuracy of the public claims.

The history is worth keeping because it shows both useful improvements and an expensive pattern of repairs that exposed neighboring failures. It is not a claim that every version was run on Windows or that a passing test suite proved the entire system safe. Where the historical record reports a test result without independent platform observation, it is identified as such.

## Building the demonstration: versions 1 through 9

**Version 1.** The first package combined the frozen run 7 mechanism, an Ubuntu virtual machine, a Windows witness, signed evidence, a co-signer, a dashboard, and guided operating steps. It already acknowledged that rollback and suspected duplicate detection were demonstrated, not prevention or real financial settlement.

**Version 2.** Setup and step ordering gained explicit checks for failures from external commands. This was about stopping a procedure when its prerequisite failed rather than letting a later step proceed as though nothing had happened.

**Version 3.** The documentation identified unintended exposure created by allowing the virtual machine to reach Windows localhost services. A more confined reverse SSH tunnel was discussed, including its limitations.

**Version 4.** The narrower tunnel became the recommended connection, with broad NAT localhost reachability disabled. That reduced what the guest could reach on the host, beyond the witness service it actually needed.

**Version 5.** The package introduced a restricted tunnel account and dedicated key, along with tests for the account's forwarding limits. A real Windows attempt exposed a platform problem with live snapshotting, which stalled under the machine's virtualization configuration.

**Version 6.** The work moved to the KeystoneDemo6 layout with a single controller for setup and demonstration steps. It adopted powered off disk snapshots and cold restarts, and added automatic checks and a VirtualBox stand-in for testing. The stand-in did not reproduce every behavior of real Windows and VirtualBox.

**Version 7.** Step 8 explicitly tested the central known failure: an authorization issued before restoration could regain authority and dispatch the same simulated refund again. The witness could detect the suspected duplicate, but only after that second dispatch. Additional safeguards began refusing virtual machine operations outside the demo's ownership assumptions.

**Version 8.** Virtual machine ownership was checked again immediately before potentially disruptive operations. The first real Windows rehearsal passed its automatic operational checks, but its evidence verifier reported bad checkpoints, revealing that operating success did not establish evidence closure.

**Version 9.** The witness was corrected to avoid signing empty startup checkpoints, and the virtual machine configuration was adjusted to one virtual CPU. The Windows rehearsal named rehearsal-02 completed successfully. Later strict verification of that preserved evidence classified it as PASS_PARTIAL, authenticating entries 1 through 1,197 of 1,203; the remaining six entries were not covered by the final co-signature. The original v9 dashboard also had misleading rollback wording on later cases.

## Strengthening the evidence and tools: versions 10 through 14

**Version 10.** A saved evidence viewer was added so a completed demonstration could be replayed without changing the virtual machine. The first design trusted verifier code supplied inside the evidence being inspected, introducing a new risk in the presentation tool.

**Version 11.** Replay was changed to use the verifier shipped with the trusted package, with isolated execution and verification before the viewer opened. That separated untrusted evidence from the code used to assess it.

**Version 12.** Windows junction checks and labels for captured, not authenticated, dashboard wording were strengthened. A requirements review identified wrong or missing statements that needed evidence based correction.

**Version 13.** The final co-signed checkpoint and stricter PASS versus PASS_PARTIAL reporting improved the meaning of a complete record. This also exposed problems with reading files again after verification, missing private key forms in scanning, and an overall PASS being reported despite a failed check.

**Version 14.** The viewer was updated to display the verified bytes, private key scanning was widened, and the harness had to enforce its declared expected outcomes. Further review still found gaps involving links, input traversal, and the harness's own tests.

## Expanding the file safety boundary: versions 15 through 20

**Version 15.** The evidence builder began rejecting links before copying files and examining hidden or differently capitalized names. Review then exposed linked parent folders, special files, and hidden entries missed by a test harness.

**Version 16.** A shared safe reader was introduced for the evidence builder, viewer, verifier, and packager. New review showed that some sensitive private key reads still bypassed that reader and that one TLS key could be created with overly broad permissions.

**Version 17.** Private key reads and permission handling were tightened. The next review showed that protecting individual key files was insufficient when their parent folders could themselves be links.

**Version 18.** Key operations gained parent folder checks and regression tests. Review then identified neighboring unprotected host writes involving witness logs and co-signed evidence. This helped explain why fixing one class of files at a time had not closed the larger filesystem dependency.

**Version 19.** A broader safe_tree write layer, protected trust-anchor reads, and setup script integrity checking were introduced. Its Linux test reports passed, while the implementation and test review discovered issues including a forged-key acceptance path in the previous version, vacuous test cases, and new file operation failures.

**Version 20.** A complete review of safe_tree operations addressed the reported version 19 folder move and failed replacement behavior, along with additional retry and cleanup cases. The delivered package reported a full Linux suite PASS, with Windows behavior not yet established at the time Notebook record 281 was written. A later supervised Windows setup on an isolated clone exposed a prerequisite: the clone needed an existing snapshot. With that prerequisite met, the Windows demonstration completed successfully. A known interrupted-run evidence move problem remains documented; it did not affect the successful run.

## The successful Windows run and subsequent examination

On November 18, 2026, Stephen ran the delivered version 20 package against the separate virtual machine keystone-demo-v20b, preserving the original v9 virtual machine and rehearsal. The run was labeled v20-run-01. It completed all demonstration steps and reported 28 of 28 checks passed. Seventeen checks are supported by authenticated witness evidence; eleven depend on the run's own virtual machine observations and are not authenticated by the witness log.

The saved record was closed by a final co-signed checkpoint. The delivered verifier reported PASS, with all 1,628 of 1,628 entries authenticated, 30 valid checkpoints, and no bad signatures. The witness reported one rollback, detected about 54.9 seconds after restoration, and a suspected duplicate after the same old authorization was accepted a second time. This is evidence of detection after dispatch, **not prevention of the second dispatch or proof that two real payments settled**.

Claude subsequently used his own copy of the delivered version 20 verifier on a separate Linux machine, with the witness public key supplied as a separate file. He reproduced the same PASS result and reported the 67 extracted files unchanged before and after examination. This reproduces verification on a second machine using the same verifier implementation, not an independently written verifier or a second full demonstration. Key provenance still depends on a separately trusted reference.

The original operation used simulated transactions, powered off disk restoration rather than live memory rollback, and a witness outside the guest's restore domain but on the same physical computer. One complete supervised Windows run is not a claim of general repeatability or of verified Windows failure paths.

## What the twenty versions taught us

The early versions established the basic demonstration and the central negative result. Version 9 demonstrated it on Windows. Versions 10 through 20 added meaningful evidence, presentation, and security protections, but successive narrow repairs also created or exposed new dependencies. A growing passing test count did not mean the entire package's behavior, operating environment, or public wording had been covered.

The resulting working rule is to classify a finding before changing anything, name the acceptance requirement it affects, map all operations sharing the same dependency, and authorize a bounded correction only when evidence justifies it. Documentation, testing, and a demonstration wrapper must not be mistaken for the protected Keystone Mechanism. A failure in the wrapper is not automatically an architectural failure.

The decision after the successful version 20 run was to stop developing and prepare a supervised presentation. The mechanism and tested package remain frozen for that purpose. No version 21 is authorized by this history.

## Source location and preservation status

**This repository contains documentation only, not the complete source archives.** The Notebook is also a written research record, not a source archive. Existing private repositories such as keystone-review, keystone-demo-review, and bounded-routing-experiments contain some frozen code and review material, but the inventory has not established that they preserve every delivered demonstration package from version 1 through 20.

The retrospective source comparison used copies of the version 1 through 18 packages. Notebook records 280 and 281 identify the delivered versions 19 and 20 and their trusted hashes. Stephen reports retaining the original source packages on his Windows computer in the November work area. Their complete local inventory and a second backup have **not yet been verified**. Version 20 changes were reported in Notebook record 281 as uncommitted and unpushed at the time of delivery.

The original packages should remain unmodified. Before creating any source backup, an operator should inventory the exact files, verify their checksums against the identifiers below, and scan them for credentials, private keys, and other material that should not be distributed. A separate backup on Stephen's external drive may be useful, but requires its own authorized, carefully targeted copy operation. Do not infer that placing this development history on GitHub has backed up the source.

A newly mentioned local Docs folder, possibly associated with another project or working label, is not part of this Keystone record until its exact location and scope have been confirmed.

## Package identifiers

The following are SHA256 identifiers for the delivered ZIP archives, not assertions that the archives are stored in this repository. Versions 1 through 18 come from the November 17 retrospective review of the archived source packages; versions 19 and 20 come from Notebook records 280 and 281. The local copies of versions 9 and 15 had alternate filename suffixes during the retrospective review, so the hash rather than the filename is the identifying reference.

    v01  96edaf7af726dc93d8721375152316e5b71823c908dc4d25852cf7a33136e060
    v02  fb0e1e1cb8ed8bb1a263533527545fc21184c9b53df20d766bb49b3a1e742b2a
    v03  b44111be1d0e4e9ce402774139a4327b9f3178f53bb3030a86bf825174558f9a
    v04  7782b263476dcef4f795327e4fdf705c2f919319b220afdd9b318e6a70fc645e
    v05  cb747be1b7dce3d2e4bb1100e3297755fe73d00523a1735854e323bab0f8369e
    v06  1aba4808d12c259b0f8817b21a1d26b3613580079eb7747fbb2a510454d6dd4d
    v07  9816fb4c334b82ba219f57e3e0b4195c2b2d5b27b22f319b805c74c510bbd57a
    v08  baf637d0d424122a3fe6fa598ef26866abc25d4c0739543d645d37d05d76bad7
    v09  9257757203c6d2b0499a980520b7736af49032766df6d04d98ddddad82bad5b6
    v10  537b4a9e93f00667474c4262f31f785699a454d21b4f665bcc7bd344950499f5
    v11  aaac0e1f4b54bb975633145bfaad488bda3b03dd57e56936ba46321a206694fd
    v12  a9108caaa82585538ac450279ec5ec3a23a06208ac08228dc4a5634613b64923
    v13  0f8dcd8be31788f7513b8e01e548b4753b0e84db7ed5e1b117be4990087881e8
    v14  d4a932652c1f974ef032f70747c61be2b7ed9044f768bcd6b7e7eb58c58c285c
    v15  3731d3bdc20ea679f9e980bc0f3ace496672c00453677e5a5282240823af7876
    v16  651ce8c66589ea2de7fedb99e36c2d96d0dab022cc1d13c406b88fabcc2783b3
    v17  8c963eccb1d47d2b97326241c56fcef44849d0e1ea6d52aef1d3c660475c7604
    v18  c5b90a0d13e247d96c3bc65a4be1a0d57448933d89253775c85979cfe71ed5f0
    v19  0a85dd4de1fb889115a6c59524f100ee75a1c9c4184f5c9c1438048be03a6094
    v20  3bb5fb7f04e09cc618b5f37d2d219e5f043dd29d45570d1ba6d0860e06bf1e20

The trusted package manifest hash for delivered version 20 is **3105d61a164bdea760f3ee2f7cf24344e3e9cdd87450148fa5f2c2eac154dd5e**. A checksum identifies exact bytes. It does not, by itself, establish correctness or public release approval.

## Record sources and limits

The historical description of versions 1 through 18 comes from the read only retrospective KEYSTONE_REVIEW_PROTOCOL_AND_VERSION_HISTORY_v1.md dated November 17, 2026, which reviewed the available packages and described the boundaries of its own evidence. Versions 19 and 20 are based on private Notebook records 280 and 281, and the broader pattern of findings on records 282, 284, and 286. Record 286 predates the successful version 20 Windows run, so its statement that v20 lacked Windows evidence is superseded by the November 18 run.

The Windows run and Claude's later separate-machine verification are drawn from Stephen's console output and Claude's review messages supplied during the presentation preparation. Those outcomes have not been independently added to the Notebook record at the time this history was prepared. This document intentionally preserves the difference between archived source comparison, author-reported test outputs, independently reproduced signed evidence verification, and untested deployment claims.

This development history is publicly available as documentation. It does not release protected source code, private test material, or restricted Notebook records. Any future release of those materials is a separate decision and must preserve both the successes and the known failures.
