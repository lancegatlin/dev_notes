

instead of `Thread.sleep` in tests, I'd really like to see us use `eventually` instead -- `eventually` on some kind of test condition that can be polled -- worst case if there is nothing available the test condition directly under the `Thread.sleep` can be used as the condition for `eventually` 

`Thread.sleep` with a fixed value is going to create flaky tests eventually because it is difficult/impossible to predict how long the worst time will be for all build agent runs --  with build agents they can often have huge lag spikes for various reasons -- each `Thread.sleep` is a small dice roll betting that the agent won't lag -- add enough dice rolls and flaky builds start

`Thread.sleep` also introduces a fixed delay where `eventually`will proceed as soon as the test condition is met -- when `eventually` fails it is clear in test output that an `eventually` failed where when `Thread.sleep` is failing because its not waiting long enough the line under the `Thread.sleep` fails -- this is confusing to anyone trying to figure out what happened -- did the line under the `Thread.sleep` fail because its test conditions failed or because the `Thread.sleep` wasn't long enough?