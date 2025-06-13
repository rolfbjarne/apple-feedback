# Basic Information

## Please provide a descriptive title for your feedback:

[AVAudioApplication sharedInstance].recordPermission crashes

## Which area are you seeing an issue with?

Audio

## What type of feedback are you reporting?

Application Crash

## Details

## What was the specific time when this occurred?  We need this to pinpoint your issue in the logs.

N/A

## Description

Please describe the issue and what steps we can take to reproduce it:
Repro:

    AVAudioApplicationRecordPermission rp = [AVAudioApplication sharedInstance].recordPermission;

Crashes like this:

    (lldb) bt
    * thread #1, queue = 'com.apple.main-thread', stop reason = EXC_BAD_ACCESS (code=1, address=0x0)
        frame #0: 0x0000000104164bd8 libsystem_platform.dylib`os_unfair_lock_lock + 12
        frame #1: 0x00000001de8bae80 AudioSession`-[AVAudioApplication recordPermission] + 52
      * frame #2: 0x0000000104058c28 ios26betatest`-[AppDelegate application:didFinishLaunchingWithOptions:](self=0x0000600000004b40, _cmd="application:didFinishLaunchingWithOptions:", application=0x0000000106005780, launchOptions=0x0000000000000000) at AppDelegate.m:22:81
        frame #3: 0x00000001861f4fa0 UIKitCore`-[UIApplication _handleDelegateCallbacksWithOptions:isSuspended:restoreState:] + 332
        frame #4: 0x00000001861f64dc UIKitCore`-[UIApplication _callInitializationDelegatesWithActions:forScene:payload:fromOriginatingProcess:] + 3036
        frame #5: 0x00000001861fb908 UIKitCore`-[UIApplication _runWithMainScene:transitionContext:completion:] + 788
        frame #6: 0x00000001857bb824 UIKitCore`-[_UISceneLifecycleMultiplexer completeApplicationLaunchWithFBSScene:transitionContext:] + 88
        frame #7: 0x00000001861f8650 UIKitCore`-[UIApplication _compellApplicationLaunchToCompleteUnconditionally] + 44
        frame #8: 0x00000001861f8940 UIKitCore`-[UIApplication _run] + 736
        frame #9: 0x00000001861fcb1c UIKitCore`UIApplicationMain + 124
        frame #10: 0x0000000104058e6c ios26betatest`main(argc=1, argv=0x000000016bda5b18) at main.m:17:12
        frame #11: 0x000000010406d3d4 dyld_sim`start_sim + 20
        frame #12: 0x000000010418eb98 dyld`start + 6076

Note: I have only tried in the simulator, not on an actual device.
