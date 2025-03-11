# Passing arguments to app using devicectl doesn't entirely work correctly

FB13259837: Developer Tools & Resources

## Please provide a descriptive title for your feedback:

Passing arguments to app using devicectl doesn't entirely work correctly

## Which area are you seeing an issue with?

Something else not on this list

## What type of feedback are you reporting?

Incorrect/Unexpected Behavior

## Please describe the issue and what steps we can take to reproduce it

According to “xcrun devicectl help device process launch”, it’s possible to pass arguments to the app by appending them after the bundle identifier:

> USAGE: devicectl device process launch [<options>] --device <uuid|ecid|udid|name> <bundle-identifier-or-path> [<command-line-aguments> ...]

(FWIW there’s a typo in that help line, it should read “command-line-arguments”, not “command-line-aguments”)

However, in some cases those app arguments are treated as arguments to devicectl.

Here I’m trying to pass the argument “-t” to the app, but it immediately fails:

  $ xcrun devicectl device process launch --device "Rolf's iPhone 13" com.my.bundleidentifier -t
  Error: Missing value for '-t <seconds>'
  Help:  -t <seconds>  The overall command timeout in seconds. If this limit is exceeded the command is abandoned as a failure.
  Usage: devicectl [--verbose] [--quiet] [--timeout <seconds>] [--json-output <path>] [--log-output <path>] <subcommand>
    See 'devicectl --help' for more information.

I tried using two dashes to separate the app arguments from the devicectl arguments (which isn’t documented btw):

    $ xcrun devicectl device process launch --device "Rolf's iPhone 13" com.my.bundleidentifier -- -t
    [success]

And that seems to work… except that the two dashes are passed to the app.

    $ xcrun devicectl device process launch -j launch.json --device "Rolf's iPhone 13" com.my.bundleidentifier -- -t
    [success]
    $ cat launch.json | grep launchOptions -A 5
    "launchOptions" : {
      "activatedWhenStarted" : true,
      "arguments" : [
        "--",  // <-  THIS SHOULDN’T BE HERE
        "-t"
      ],

$ xcrun devicectl --version
348.1

Xcode 15.0

iPhone 13 with iOS 17

