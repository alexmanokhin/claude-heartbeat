# Handoff — claude-heartbeat (2026-09-05)

Session is moving off `TechnoMixs-Mac-mini` to another machine. Only the
conversation moves; the environment does not. This file is the part of the
state that has to survive in GitHub.

## What this repo is

A liveness beacon, nothing more. `HEARTBEAT` holds a single timestamp,
rewritten and committed roughly every 30 minutes by a job on the Mac mini.
No secrets, no config, no code. If commits stop arriving, the mini is down.

## What was done in this session

- Committed `AGENTS.md`, which had been sitting untracked. It is a pointer
  file: it tells any agent opening the repo that the live state lives in
  `.agent/HANDOFF.md`.
- Added this file.

Nothing else in the repo changed. `HEARTBEAT` is at `2026-09-04 16:03:39`,
which is simply the last beat the mini wrote before the move.

## Unfinished / carry over to the new machine

1. **The heartbeat writer does not move with the session.** It is a scheduled
   job on the Mac mini, not in this repo. If the mini goes offline, `HEARTBEAT`
   goes stale and that is expected, not a repo problem. Do not "fix" a stale
   timestamp here by hand; it would fake liveness for a host that is down.
2. **`.agent/` is excluded locally, not by `.gitignore`.** The rule lives in
   `.git/info/exclude`, which is per-clone and is NOT pushed. So:
   - `.agent/HANDOFF.md` (auto-written by `rc-sum` every ~30 min) will not
     arrive on the new machine, and its content is not in git anywhere.
   - A fresh clone has no exclude rule, so `.agent/` will show up as untracked
     the moment anything writes there. Re-add `.agent/` to
     `.git/info/exclude` on the new clone rather than committing it, since it
     is machine-local churn that would fight the heartbeat commits.
   - The stale `.agent/HANDOFF.md` on the mini is from session
     `eaa8f81d` (2026-06-22) and describes an unrelated OpenRouter audit. It
     is not current state; ignore it.

## Where to pick up

Nothing here is blocking. Confirm the heartbeat job is running on whichever
host is meant to be the beacon, and check that `HEARTBEAT` starts advancing
again. Real work for this session is in `pobeditel-os` — see its `HANDOFF.md`.
