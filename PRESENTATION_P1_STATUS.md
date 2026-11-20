# TetraSafe Keystone Component Demo: Presentation P1 validation record

**Updated November 18, 2026. Status: Presentation P1 replay has now been visually reviewed in Stephen's Windows browser and meets the recorded presentation checks. This is a public documentation record, not a software or evidence release.**

## What P1 is

`TetraSafe_Keystone_Presentation_P1_v1.zip` is a presentation revision of the delivered demonstration version 20. It is not version 21 of the Keystone Mechanism and did not produce the signed `v20-run-01` evidence. The code difference from frozen v20 is confined to `dashboard_demo.html` and `replay_server.py`; a stage aware test and a presentation note were added. The enforcement code, verifier, VM experiment and original signed evidence remain unchanged.

The viewer opens on the recorded second dispatch finding, displays the verifier result and authenticated coverage legibly, offers understandable stage names, explains the final STALE state, and retains the caveats that this is detection after dispatch and not proof of settled payments. The presentation names the **TetraSafe Keystone Component Demo**, not the underlying mechanism as though the page were the mechanism itself.

## Exact archive identifiers

Frozen delivered v20 ZIP SHA256: `3BB5FB7F04E09CC618B5F37D2D219E5F043DD29D45570D1BA6D0860E06BF1E20`.

Frozen v20 package manifest SHA256: `3105D61A164BDEA760F3EE2F7CF24344E3E9CDD87450148FA5F2C2EAC154DD5E`.

Presentation P1 v1 ZIP SHA256: `EBFE9030AC1DF402AC80CD7FA17C276D5C26ABE66444820E2BBFFE1C08A1FBFA`.

Presentation P1 v1 `PACKAGE_SHA256SUMS` SHA256: `53E2B296CED541350D583BD2708B1F4E49B381D9949E128F4CB2B329EABCDB93`.

These values identify specific archives. Do not use a P1 manifest to run setup or a live demonstration against the original v9 environment. No P1 archive, protected source, private keys, or raw bundle is stored in this documentation repository.

## Evidence and tests

The underlying Windows `v20-run-01` demonstration was completed with frozen v20, not P1. Its run reported 28 of 28 operational checks passed, comprising 17 supported by authenticated witness evidence and 11 local observations. The evidence verifier reported PASS with all 1,628 of 1,628 entries authenticated. The demonstrated second dispatch was flagged as a suspected duplicate after the fact; it was not prevented, and real payment settlement was not established.

Claude reports that the finished P1 archive passed its full Linux suite, 50 of 50, including a restore demonstration with 28 of 28 checks and PASS verification. Its presentation stage test passed 16 of 16 saved suite views, and the existing rendering check passed 24 of 24, including eight views from the real Windows record. The real saved bundle replayed in Linux Chrome with the readable PASS/1,628 coverage, the correct default stage, navigation, final STALE explanation, captured wording warning, keyboard focus and narrow layout. The same code preserved live dashboard refresh. Claude reports all 67 real evidence files unchanged.

GPT independently inspected the attached P1 v1 ZIP without executing its code. The ZIP checksum and manifest checksum above matched, the archive passed its integrity test, no unsafe archive paths or Unix symbolic links were found, and a comparison against the unpacked frozen v20 source identified the two intended changed files and the new presentation test. This inspection does not independently reproduce the full suite or Windows browser behavior.

## Windows presentation check completed

On November 18, 2026, Stephen located the P1 archive on SpiceLand at `C:\November work\TetraSafe_Keystone_Presentation_P1_v1.zip`. The Windows ZIP SHA256 matched `EBFE9030AC1DF402AC80CD7FA17C276D5C26ABE66444820E2BBFFE1C08A1FBFA`, and the unpacked `PACKAGE_SHA256SUMS` SHA256 matched `53E2B296CED541350D583BD2708B1F4E49B381D9949E128F4CB2B329EABCDB93`. He stopped the previous v20 viewer to free local port 8490 and launched P1 using the existing v20b root and VM selection with `replay` only. The Windows console reported `verifier: PASS` and announced the read only viewer at `http://127.0.0.1:8490/`.

Stephen sent a 74 second Windows browser recording for review. GPT inspected frames across the video and confirmed the **TetraSafe Keystone Component Demo** heading, readable PASS text with authenticated coverage of entries 1–1,628, the named stage navigation working, the rollback finding, the second dispatch section identifying a **suspected duplicate** without claiming confirmed settlement or prevention, and the final record's explicit STALE explanation. The page clearly labels the evidence as saved, not a live experiment. This is **a passed Windows visual presentation check**, not a repeat of the VM experiment, a separate verifier implementation, or a comprehensive Windows failure path test.

The original version 20 package and `v20-run-01` signed evidence remain the reference used to produce this result. P1 is the presentation viewer for that recorded result. For any supervised replay, use the explicit `--root` and `--vm` command in the [presentation guide](PRESENTATION_GUIDE.md). No new setup or live demonstration is needed. The original v9 environment, delivered v20, and the existing v20b evidence must be preserved. This check does not itself authorize distributing P1 source code or the evidence bundle publicly.

## Standing limitations

The witness shares the host computer with the VM and does not provide independent physical custody. The destination is simulated, the restoration is a powered off disk rollback followed by restart, and the detection comes after the second send. The external Linux verification uses the same verifier implementation and a key originally supplied by Stephen. The known abnormal interruption file handling and fresh clone snapshot prerequisite of the underlying wrapper remain disclosed; no P1 presentation change cures them.
