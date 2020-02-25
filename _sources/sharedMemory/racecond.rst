1.2 Race Conditions
-------------------------------------------

Introduce the integration problem, and show TSGL video to explain what the expected behavior should be.

Interactive question: Ask the students if this is an example of task parallelism or data parallelism?

Show code snippet that shows an initial parallel implementation. Students running it see an incorrect output.

Point out that this is not an issue with the program be task parallelism. In fact, the same thing can occur in a data parallel context.

Return back to array addition. Show that the same error occurs. 

Return to unplugged activity to show what a race condition is, and how critical sections can help.

Show code snippet with integration to show that the integration results now gets the correct result.

As an activity, have students apply the critical section to the array addition problem to get correct output.