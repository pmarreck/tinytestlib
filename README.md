# tinytestlib (TTL)

An MVP shell script testing library written in Bash that allows you to assert on stdout, stderr and return/exit codes in your shell script or terminal UI test suites.

### Design goals
1) Make it as easy as possible to use and trivial to include in an existing project- it's just one standard `bash` file (`bash 4.5+` required). The other options out there were all way too heavy/intimidating.
2) Nice simple DSL (`assert`, `assert_equal`, `assert_success`, `assert_match` etc.- see the test suite for examples)
3) Include just enough functionality, but no more (i.e. “tiny”). Happy to take criticisms or suggestions here.
4) (*Important*) Be able to assert on all 3 outputs of a single run of a command (stdout, stderr, retval/exitval). To my knowledge there's no other terminal- or shell-script-focused testing tool that does this, which I found odd/lacking.
5) Be able to run asserts on their own, independent of a suite (in which case they exit on fail and are silent on pass) or as part of a test suite block (in which case they gather stats and then report and clean up after themselves when the end of the block is reached)

In order to make the simultaneous capture of all 3 outputs of a command work, a function called `capture` is defined whose first argument is expected to be the name of an already-existing variable that is an associative array (`declare -A assoc_arr`, for example). When the capture is complete, the keys `stdout`, `stderr` and `retval` of that associative array will be populated, accessible via `${assoc_arr[stdout]}`, `${assoc_arr[stderr]}` and `${assoc_arr[retval]}`.

TTL's test suite (`./test`) uses its own test library code to test itself. It also shows example usage of many of the functions.

When run in "suite mode", a successful run outputs a report that looks like this (with green text for the dots and "Success"):

```
...........................
27 assertions
Success
```
One or more failures look like this (with red text F's):
```
...............F...........
27 assertions
Failures: 1
assert_failure was actually successful: <date>
stdout: Tue Jan 10 11:41:19 AM EST 2023
stderr:
retval: 0
```

#### #TODO

Add more test reporters and modularize that bit
