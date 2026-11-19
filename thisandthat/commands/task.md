---
description: Turn a sentence into a DoBox task
argument-hint: <what needs doing, and by when>
---

Create a DoBox task from this: $ARGUMENTS

1. Work out a short subject, a due date if one is given ("by Friday", "next week"), a priority if one is implied, and any detail that belongs in the description.
2. Call `list_task_groups` and pick the group that fits. If more than one could fit, ask the user which one.
3. Show the user the task you're about to create (subject, group, due date, priority) and wait for them to confirm.
4. After they confirm, call `create_dobox`. `dueAt` is a Unix timestamp in milliseconds, so convert the date in the user's time zone. Priority is 0 for urgent, 1 high, 2 medium and 3 low; leave it out if nothing implies one.

Reply with the subject, group and due date of the task you created.
