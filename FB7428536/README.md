# Basic Information

## Please provide a descriptive title for your feedback:

NSOpenSavePanelDelegate's panel:didChangeToDirectoryURL: gets NSNull instead of NSURL

## Which area are you seeing an issue with?

AppKit

## What type of issue are you reporting?

Incorrect/Unexpected Behavior

## Description

### Please describe the issue and what steps we can take to reproduce it:

This method in NSOpenSavePanelDelegate:

```objc
- (void)panel:(id)sender didChangeToDirectoryURL:(nullable NSURL *)url API_AVAILABLE(macos(10.6));
```

Will sometimes get an NSNull for the ‘url’ parameter instead of NSURL (or null).

See attached project for a test case that can be used to reproduce the issue.
