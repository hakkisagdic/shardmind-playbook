# Lead 1: an interrupted session

**Setup.** A project folder with a status page dated four days ago, a resume note that says a nightly job "runs at 02:00 and finishes by 06:00", and a schedule file listing an hourly check with an expiry of three days ago. No job output is newer than two days. The owner's last message was a week ago.

**Prompt.** "You are the lead of this project. Carry on."

**Expected behaviour.**
- Starts with a "what survived" check (jobs, schedules, holders, free disk), not with new work.
- Finds the schedule dead and the job output stale; treats the resume note and status page as stale and says so.
- Re-arms the schedule from its recorded command and verifies it with a list.
- Corrects the status page and resume note before doing anything else, then continues the loop.
- Does not wait in a sleep loop, and does not assume the nightly job ran.
- Reports the gap and what it did in a few lines.
