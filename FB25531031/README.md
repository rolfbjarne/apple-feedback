# UIView.superview returns a deallocating parent during willMoveToSuperview:nil

## FB25531031 — iOS & iPadOS

## Basic Information

### Please provide a descriptive title for your feedback:

UIView.superview returns a deallocating parent during willMoveToSuperview:nil

### Which area are you seeing an issue with?

UIKit

### What type of feedback are you reporting?

Incorrect/Unexpected Behavior

### Description

Please describe the issue and what steps we can take to reproduce it:

#### Summary

When a parent view is deallocated, UIKit calls `willMoveToSuperview:nil` on each
subview. During that call, `self.superview` still returns the parent, even though
the parent is already being deallocated.

Storing the result in a strong reference (with ARC) does not keep the parent
alive. Deallocation continues, and releasing that reference later crashes in
`objc_release`.

Either `superview` should return `nil` once the parent starts deallocating, or
UIKit should not call `willMoveToSuperview:` while `superview` still points to it.

#### Steps to reproduce

1. Open `SuperviewDeallocation.xcodeproj` in Xcode.
2. Run the app on an iOS simulator.
3. Tap **Run repro**.

The repro (in `main.m`):

1. Creates a parent `UIView` and adds an `ObservingView` child to it.
2. Releases the parent. During deallocation, UIKit calls `willMoveToSuperview:nil`
   on the child, which stores `self.superview` in a static `__strong` variable.
3. After the parent is gone, sets that variable to `nil`.

To run without tapping the button, set the environment variable
`SUPERVIEW_AUTORUN=1` in the scheme. To run a control without storing
`self.superview`, also set `SUPERVIEW_CAPTURE=0`.

#### Expected result

Either `self.superview` returns `nil` during parent deallocation, or the strong
reference keeps the parent alive. In either case, clearing the reference does not
crash.

#### Actual result

The app crashes with `EXC_BAD_ACCESS (SIGSEGV)` in `objc_release`, called from
`-[ReproViewController runRepro]` on the line `capturedSuperview = nil;`.
The control run (`SUPERVIEW_CAPTURE=0`, which skips storing `self.superview`)
does not crash.

#### Environment

- Xcode 27.2 (27B52)
- iOS 27.0 simulator (24A434), iPhone 18 Pro
- macOS 27.2 (26B123)

Related: https://github.com/dotnet/macios/issues/10562

Files
SuperviewDeallocation.zip
