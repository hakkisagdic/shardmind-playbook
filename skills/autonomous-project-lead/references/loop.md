# The judgement loop

## Shape of a round
1. Read the status page and the resume note (small).
2. Run the summary scripts. They print a few lines each: job verdicts, machine room, queue state, failed checks.
3. Read the questions queue for new answers.
4. Decide, using the list below.
5. Act through scripts, agents or edits to state files.
6. Make the state files true, then stop.

## What to decide each round
- Is anything finished that needs a decision (promote, drop, extend)?
- Is anything stuck? A job that has not progressed for much longer than its expected length is a question, not a wait.
- Is the machine free enough to start the next heavy item?
- Which open item is most valuable per unit of effort and risk?
- Does anything need the owner? Queue it with a default.
- Was there an incident? Write the lesson.
- Is any agent idle or any resource unused that could be used? Capacity that is free or already paid for should not sit idle: give it the next useful reversible job, within the owner's budget. Anything that needs new spending is class three.

## What a round never does
- Sleeps and re-checks in its own context.
- Types a long test command from memory; it calls the fixed entry point.
- Counts by hand; it reads a tally the script wrote.
- Reads a raw log; it reads the script's digest and opens the log only for the failing part.
- Starts work without a place in the open items.

## Making a script out of a chore
Name it for what it answers ("is the build done", "how many samples so far"). It prints one verdict line and, if relevant, a few key numbers. It exits non-zero on failure. It lives in a durable scripts folder under version control. It writes its output to a known file so the next round can read it without rerunning.

## Cadence
Short rounds often beat long rounds rarely. Tie the cadence to the clock only through a scheduler, never through your own waiting. Whatever the schedule, each round must be safe to run twice.
