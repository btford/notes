Getting the current date/time is impure. If you function calls `new Date()`, `Date.now()` or `moment()` without any args, it's impure. This is important when you write seemingly innocuous helper functions. Anything that consumes this function becomes impure, and difficult to test without globally monkey patching date primitives.

A better pattern is to pass in a timestamp or object that represents the current time. So instead of `function timeSince(time) { return Date.now() - time }` it's better to write `function timeSince(time, now) { return now - time }`.

I spent most of today tracking down an issue in tests for summary that was actually caused when I rebased on top of a change that introduced one such call to `moment()` in shared code.

In short– think before you get the current date, and defer this responsibility as high up the chain as you possibly can!

</rant>
