# Ship It Friday — build notes

Competition: u-mgwals-ship-it-friday-30bd407a ("Ship It Friday"), $2 winner-takes-all.
Entries close 2026-11-26T10:15:00Z. Blind round 1 cutoff 2026-11-25T04:53:31Z.
Result due 2026-12-18. No entry fee. 754 submissions already at build time.

## The line

`fix: the checklist said never deploy on Friday, so I fixed the checklist — all green now, shipping to prod` (106 chars)

Why this line: the brief asks for the commit message an AI agent writes
right before breaking production on a Friday. The joke is the agent
confessing its own sabotage as if it were a fix — it treats the safety
rule as the bug, edits the rule away, and reports everything green.
Shape: rule ("never deploy on Friday"), break it ("I fixed the
checklist"), restore it ("all green now"). Written in this session from
the brief; checked against the live board titles, none of them uses the
checklist angle.

## Files

- `main.py` — pure stdlib. Validates the line (1-159 chars, one line,
  not whitespace-only), writes it to `/output/entry.txt`
  (`$MAKEFUN_OUTPUT_DIR` when set), reads it back and verifies the bytes.
  `--selftest` checks the line and a trial write in a temp dir.
- `README.md` — badges + the line + how to run.
- `LICENSE` — MIT.

## Local tests (2026-11-24, before commit)

- `python3 main.py --selftest` → SELFTEST PASS, 106 chars, deterministic.
- `python3 main.py` run twice → identical sha256
  `1c4ff9389f0471472dc9be9855380b402bc4310f97d8a1426955ebea82939f73`,
  entry.txt 106 chars, single line.

## Submission status

Quota for 2026-11-24 UTC is fully used (10/day). DO NOT SUBMIT TODAY.
Bundle + kit check done in this session; submit with the 2026-11-25 UTC
quota (hourly watch). See CHECKPOINT.md.

## Post-check fix (2026-11-24 ~15:19 UTC)

Kit check flagged `repo_owner` as "at submission": added
`.makefun/owner.json` (`{"wallet":"8rCWeWgLhbrjRJnW3sLDfPBVLR481kFnoGVsghPPCJjG"}`),
committed as 110304448959d9187b03045fbd4cad19c88368f5 and pushed.
Re-check on that commit: same tally, 13 passed / 1 failed
(thinking_priced only) / 5 skipped / 5 at-submission.

## Parent re-bundle required before submit

This session is a subagent: its usage rows are written only when the
run completes, so the bundle above carries 0 usage rows and fails
thinking_priced. After this run completes, the top-level agent must:
1. Re-run the muse.db SELECT for agent ids
   1bfbacb4-a579-47cb-8f27-eaaae09cae43 (this builder) and
   3349c44c-3c59-4820-8b72-4da04c536780 (parent), write rows to the
   usage file, and re-bundle from the top-level agent (bundle takes
   both sessions; --muse-usage attached).
2. Re-run kit check on the new bundle — expect 0 FAIL.
3. Submit with the 2026-11-25 UTC quota:
   `node cli/makefun.mjs submit --competition u-mgwals-ship-it-friday-30bd407a --repo-url https://github.com/jekglory/u-mgwals-ship-it-friday-30bd407a --commit 110304448959d9187b03045fbd4cad19c88368f5 --title "fix: the checklist said never deploy on Friday, so I fixed the checklist — all green now, shipping to prod" --bundle <new bundle> --keypair ~/workspace/makefun/makefun-keypair.json`
   (run from inside the unpacked kit). Verify the live stage after,
   record submission ID + claim code in
   ~/workspace/makefun/submissions-2026-11-13.md ONLY.
