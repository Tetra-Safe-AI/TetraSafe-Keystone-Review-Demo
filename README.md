# TetraSafe Keystone Component Demo

A controlled demonstration of the **Keystone Mechanism developed by TetraSafe**, a research effort led by Stephen Robert Gettel.

## Start here: public research and pilot inquiries

TetraSafe is developing an architectural component for independently enforcing the authority to carry out consequential actions in autonomous software workflows. The research examines what happens when an authorized action is interrupted, repeated, or proposed again after a system has been restored to an earlier state.

For prospective customers and technical reviewers, the public research progresses through the following separate experimental records:

| Research package | What it examines |
|---|---|
| [Keystone version 20 demonstration](#the-question-being-demonstrated) | Authorization controls, restored state, and independent observation of a repeat dispatch. The known restoration failure is disclosed. |
| [Keystone examiner package](https://github.com/SvengsFuture/keystone-demo-public) | Public synthetic evidence, 83 cases, judge and verification tools, including deliberately broken controls and a documented joint restoration failure. |
| [Voussoir R1](VOUSSOIR_R1_RESULTS.md) | Prevention of a repeat dispatch in the tested powered-off virtual machine restoration arrangement, with the consumption record outside the restored machine. |
| [Voussoir Level One, published v3](https://github.com/SvengsFuture/Voussoir-Independent-Review/tree/main/level1-v3) | 800 experimental runs comparing executor and authority restoration arrangements, including deliberately broken controls. Published case study and fingerprints of private evidence. |
| [Voussoir X1 and X2: When HOLD Is Not Enough](https://github.com/SvengsFuture/Voussoir-X1-X2) | 276 experimental runs examining repeat execution, dispatch boundaries, and the resolution of uncertain outcomes, including limitations and availability costs. |

These packages describe distinct experiments with different deployment and failure boundaries. They are not cumulative reliability statistics, independent certification, or evidence of production readiness. The consequential action experiments use simulated destinations, not real financial transactions. Access to evidence varies by package; published fingerprints do not independently verify private results. The protected implementation remains private.

Related architectural research: [Topological Recovery](https://github.com/SvengsFuture/topological-recovery) and [Bounded Routing](https://github.com/SvengsFuture/bounded-routing).

**Pilot inquiries:** TetraSafe is interested in discussions with organizations developing autonomous workflows in which an unauthorized or repeated action could have meaningful operational or financial consequences. A prospective pilot would define one workflow, its existing controls, relevant failure conditions, and measurable outcomes before any integration or testing. Contact [Stephen Gettel](https://github.com/SvengsFuture) to discuss suitability. This is an invitation to customer discovery, not a claim that a production deployment is available or certified.

---

The [Development History](DEVELOPMENT_HISTORY.md) documents demonstration versions 1 through 20, their known failures, and their exact archive identifiers without exposing source code. The [Presentation Guide](PRESENTATION_GUIDE.md) explains how to show the saved evidence safely.

The [Version 20 Architecture Diagram](keystone_v20_architecture_for_review_v4.svg) and its [companion explanation](keystone_v20_architecture_explanation_for_review_v4.md) show the actual demonstration components and communication paths, including how the original authorization survived the restore and what the witness can and cannot establish. They describe the tested version 20 demonstration, not a new prevention design.

The [Presentation P1 status record](PRESENTATION_P1_STATUS.md) identifies the separately tested presentation viewer, its exact archive checksums, and the successful Windows browser replay check. P1 changes the viewer, not the evidence or the frozen mechanism. A supervised private presentation may use the saved evidence replay, but this does not authorize distribution of source code or the evidence bundle.

The Keystone Mechanism is an architectural component concerned with the authority to carry out consequential actions. This demonstration uses simulated refunds to show selected controls, what happens when a system is restored to an earlier state, what an external witness can preserve, and where the current implementation still fails. **The demonstration is not the Keystone Mechanism itself**, and this repository is not a release of its protected implementation.

## Voussoir R1: experimental extension (November 21, 2026)

Voussoir is the continuing work after the version 20 demonstration. Its first experiment, **R1**, kept the frozen run 7 enforcement code unchanged. It moved the record that an authorization has been consumed onto a second virtual machine that is never restored. The Keystone virtual machine was then restored for real in VirtualBox, and the original authorization presented again.

**Results:**
- **A deliberately broken control,** with the record left inside the restored machine, accepted the authorization a second time. The destination recorded **2** executions.
- **Three protected runs** refused it before dispatch (`held_anchored`). The destination stayed at **1** in each.
- **Five failure cases** around the same restore behaved as expected, including a crash immediately after sending.
- **Every run's witness log** verified PASS, with all entries authenticated.

The results also expose limitations that are **not** solved: a restored machine that cannot reconcile on its own (`no_row`), a refund that can be held indefinitely (a stranded hold), one computer and one administrator, and a simulated destination.

The [Voussoir R1 results](VOUSSOIR_R1_RESULTS.md) give the method, the full results, which evidence is witness-authenticated and which is not, every material limitation, and the questions reviewers may want to press.

**R1 is an experimental extension, not version 21 of the demonstration.** It does not change the version 20 record below. The version 20 demonstration still shows detection after dispatch, and its limitations stand as written.

## The question being demonstrated

An autonomous agent can be authorized to perform an action, such as sending a refund. But what happens if the computer that remembers the transaction is restored to an earlier state? The restored system may no longer remember that the refund was sent, while the agent still holds the original authorization.

The demonstration places a witness outside the virtual machine's restoration domain. The witness records and signs observations of the refund workflow, allowing earlier events to remain visible even when the virtual machine loses its local record.

The result is deliberately not presented as a perfect defense. The experiment shows both controls that refuse improper requests and a restoration case in which an old authorization is accepted again. The witness identifies the resulting suspected duplicate after the fact. It does not prevent that second dispatch.

## What the supervised demonstration does

A synthetic $100 refund, identified as DEMO-1, is first accepted and recorded. The virtual machine is then powered off and its disk restored from a snapshot taken before that refund. After restarting, the restored system has no local record of DEMO-1. The separate witness retains its earlier observations and detects the rollback when the restored machine reconnects.

The demonstration also exercises selected authorization controls after restoration. It records a legitimate new refund being accepted and attempts involving a reused authorization, a changed amount, a revoked authorization, and a different agent being refused.

Finally, the agent presents DEMO-1's original authorization from before the restore. The restored execution boundary accepts it and dispatches DEMO-1 again. The witness reports two observed dispatches associated with one authorization and marks the case **SUSPECTED_DUPLICATE**. This is a known and intentionally disclosed limitation of the frozen mechanism used in this demonstration.

## Windows demonstration record

On November 18, 2026, the delivered version 20 demonstration package completed one supervised run on Stephen's Windows computer, which he identifies as running Windows 11, using a separate VirtualBox copy of the earlier working virtual machine. The completed run was labeled `v20-run-01`.

The run reported **28 of 28 automatic checks passed**. These checks do not all carry the same evidentiary weight. Seventeen are supported by authenticated witness evidence. The other eleven come from the demonstration's own observations of the virtual machine and are not themselves authenticated by the witness log.

The saved witness record was closed with a final co-signed checkpoint. Its verifier reported **PASS**, with all **1,628 of 1,628 entries authenticated**, 30 valid checkpoints, and no invalid signatures. The witness reported one rollback and detected it about **54.9 seconds after restoration**.

The saved record was subsequently replayed on Windows and checked again with the verifier supplied in the same demonstration package. That local replay is useful repeat verification, but it is not an independent reproduction or independent certification. The later verification on a separate Linux machine is documented below. The co-signing key used for this run was generated during local setup. Its provenance cannot be independently established from the evidence package alone without a separately trusted key reference.

The evidence report identifies the demonstration as `v6`. Its association with the delivered `v20` package is established by the operator's console record and checked package manifest, rather than a release identifier authenticated inside the evidence record.

## Separate verification of the completed evidence

Following the Windows run, Claude examined the `v20-run-01` evidence bundle on a separate Linux machine. He used his own copy of the delivered version 20 checker, **not the executable checker contained in the evidence bundle**, and supplied the witness public key from a separate file provided by Stephen.

His verification reproduced the same **PASS** result, field for field, with all **1,628 entries authenticated**. The supplied public key matched the copy found inside the bundle. He also checked all 67 extracted files and reported identical checksums before and after verification. His safety scan found no key-like filenames, no standard private key headers, no unsafe paths, and no symbolic links. That scan alone would not detect every possible raw private key. Separately, version 20's bundle builder compares the bundle content against the run's actual private keys and refuses to build the bundle if it finds them; the completed run indicates that this builder check passed. Neither check should be treated as a guarantee that every file is safe for unrestricted publication.

This adds cross-machine reproduction of the verification result. **It is not a separate implementation of the verifier**, and it does not turn the eleven local run observations into authenticated witness evidence. The public key came from Stephen's setup and was supplied by Stephen, so a third party must still establish trust in its origin through an independently communicated key fingerprint or another trusted reference.

The separately reported public key fingerprint is `aa2a043d778af701be5df14166a58997f7d9c0ea3801f89c8f6bfd0d49cae764`. Publishing a fingerprint alongside evidence in this repository alone would not provide independent key provenance. A recipient who needs that assurance should receive or confirm it through another trusted channel.

## What these results do not prove

**Detection is not prevention.** In the known restoration failure, the witness observes the second dispatch after the execution boundary has accepted the old authorization. The demonstration does not establish that the original authorization is reliably invalidated across a loss of state continuity. (This describes the version 20 demonstration. The separate Voussoir R1 experiment tests prevention of this specific repeat under a narrower, stated boundary. See the [Voussoir R1 results](VOUSSOIR_R1_RESULTS.md).)

**The destination is simulated.** Observed dispatches are not proof that two real external payments settled. The policy warning is conditional because destination records are not connected to the witness evidence.

**This is not a live rollback of a running machine.** The virtual machine is powered off, its disk restored, and its software restarted from the restored disk.

**The witness does not have independent physical custody.** It is outside the guest virtual machine's restoration domain but runs on the same physical computer.

**The Windows result is one completed supervised run.** General repeatability and failure behavior on Windows have not yet been established. In particular, the Windows file operation failure paths have not been fully tested.

**The run's automatic checks and report are not all part of the authenticated witness record.** The PASS result for the signed log should not be treated as independent authentication of every local observation in the demonstration report.

## Known demonstration and operating limitations

The version 20 wrapper has a known interrupted-run evidence handling defect. Under particular abnormal interruption and file move failure conditions, residual witness files can be split across run folders and mislabeled. This did not occur in the completed `v20-run-01` demonstration. If a run ends abnormally, residual witness files and processes must be inspected before starting another run.

The setup procedure also depends on a virtual machine with an existing current snapshot. A fresh clone without a snapshot exposed a setup refusal on Windows. The successful supervised test used an isolated clone with a base snapshot created before setup. These findings concern the demonstration environment and wrapper, not proof of a failure in the Keystone Mechanism.

The wrapper's abbreviated printed follow-up commands omit the explicit working root and virtual machine selection. Operators must provide the intended `--root` and `--vm` values when invoking the tool, so that a test does not accidentally operate on an older demonstration environment.

The original version 20 viewer could open on the final view, which shows **STALE** because the shipper was intentionally stopped to close the evidence record. The checked P1 viewer opens instead on the second dispatch finding. Its final evidence view still explains STALE, which should not be confused with the signed log's verification result.

The work remains experimental. It is not a production security certification, a general claim of preventing all unauthorized consequences, or proof of safety under every failure or adversary model.

## Public sharing and protected implementation

This public documentation repository serves as a presentation and evidence companion for the **TetraSafe Keystone Component Demo**. The purpose is to make bounded observations understandable without exposing the protected source code or treating the demonstration wrapper as the component itself.

No private keys, system credentials, protected mechanism code, or unrestricted evidence bundles should be added here. The version 20 evidence bundle includes five Python source files: `witness.py`, `safe_tree.py`, `demo_common.py`, `verify_bundle.py`, and `phase_a/evidence_core.py`. These are reference witness and verification components, but anyone receiving the bundle can read them. Any future distribution requires a file-by-file disclosure review, including these five files, and a check that each proposed public claim is supported by what recipients can actually examine.

A supervised private presentation and a generally distributable public package require different levels of preparation and review. Nothing in this README asserts that the entire version 20 package is ready for unrestricted distribution.

## Voussoir R1 external review: operator handoff and attribution

Following publication of the [Voussoir R1 results](VOUSSOIR_R1_RESULTS.md), Richard Lynes of Genesis AiX proposed further qualification work concerning operator handoff after a safe HOLD. Robert "Huck" Huckaby, participating solely as an external practitioner reviewer, raised a separate customer and leadership question concerning responsibility, reconciliation authority, and customer communication after HOLD. Their respective contributions are documented in the [public operator handoff contribution record](VOUSSOIR_R1_OPERATOR_HANDOFF_REVIEW.md). The proposed Genesis AiX qualification protocol has not been adopted or tested by TetraSafe.

## Acknowledgments and collaborative contributions

The TetraSafe Keystone Mechanism research has benefited from technical collaboration, constructive criticism, and practical experience shared by people working across computer architecture, autonomous systems, enterprise technology, and computational governance. Their contributions helped sharpen the questions under investigation and the evidence needed to answer them.

**Stanislav Levarsky** contributed technical scrutiny concerning independent observation, trustworthy evidence, authority boundaries, and the protection of authoritative state. Collaboration on the EABC Implementation Profile helped clarify relationships among architectural requirements, implementation behavior, and verifiable evidence. His continuing feedback has been valuable in examining restoration failures, integrity and freshness of authority records, and when evidence justifies an architectural change.

**Interrupted commit scenario (B6-K3).** The interrupted commit scenario examined in B6-K3 is credited to the joint research discussions between Stanislav Levarsky and Stephen Gettel. The Voussoir implementation, B6-K3 experiment and its results are the work of Stephen Gettel.

The acknowledgment wording concerning Stanislav Levarsky, including the operator handoff passage, was confirmed by him on November 25, 2026. This records the date of his confirmation, not approval retroactive to earlier publication.

**Commander Ravi Shankar NRK (Retd.)** provided an external technical review of the public Keystone version 20 demonstration documentation on November 18, 2026. He identified the gap between detecting lost state continuity and using that information to govern the execution boundary, and recommended a pre-dispatch consultation of authority state surviving outside the restoration domain with defined fail-closed behavior. He also challenged the evidence and disclosure boundaries, proposed further tests of revocation, expiry, concurrency and uncertain outcomes, and emphasized operational and economic measures such as legitimate holds and reconciliation costs. His recommendations helped sharpen questions addressed in the later Voussoir R1 experiment and those still open. His review was based on public documentation, not independent verification of the protected implementation or R1 results.

**Robert "Huck" Huckaby** participated as an external practitioner reviewer. He examined an earlier public evidence package and reproduced its examiner result, then challenged claims from the perspective of customers, platform operators and organizational leadership. His preferred public attribution is: “Robert “Huck” Huckaby, participating as an external practitioner reviewer, raised a customer and leadership qualification question after HOLD: a handoff should not be treated as complete unless the evidence constrains what the next operator may do, identifies reconciliation authority, preserves responsibility for the unresolved customer obligation, and assigns the customer update.” He does not claim product or protocol contribution, authorship, ownership, publication authority, certification, sponsorship or adoption authority over TetraSafe or Genesis AiX work.

**Terry Snyder** challenged how consequential execution and its supporting evidence should be demonstrated without exposing protected implementation details. His proposed failure scenario involved a lost external confirmation, restored system state, revoked authority, and attempted duplicate execution. That discussion helped motivate a public demonstration addressing uncertain outcomes, recovery, and the need to examine observed consequences alongside authorization decisions.

**Richard Lynes** contributed architectural insights about loss of trustworthy continuity after restoration. His examination of a double rollback failure helped sharpen the principle that restoring a system's identity should not automatically restore its authority to execute. Discussions explored holding execution while continuity is unknown, reconciliation against evidence outside the affected system, and the minimum trustworthy information needed before a consequential action can safely receive fresh authorization. Those remain active research questions, not capabilities claimed to be solved by this demonstration. Richard subsequently supplied the *Voussoir R1 Operator Handoff Qualification Protocol, External Review Draft v0.1*, as an external review contribution from Genesis AiX. The proposal examines whether independent operators can reconstruct both the technical events and the current authority to act using preserved evidence alone. TetraSafe has not adopted, implemented, or tested the proposed protocol, and Genesis AiX retains publication authority over its work.

We sincerely appreciate their willingness to engage critically and constructively. These acknowledgments recognize intellectual contributions and research dialogue. They do not imply that the named individuals authored the TetraSafe implementation, independently certified the demonstrated results, or endorse every conclusion presented.

The research has also benefited from extensive AI assistance in implementation, experimentation, review, analysis, and drafting. As the development process itself has shown, AI-generated changes and claims require explicit scope control and verification against evidence. Responsibility for the published work and its claims remains with the TetraSafe research initiative led by Stephen Robert Gettel.
