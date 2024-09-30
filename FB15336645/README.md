# Basic Information

## Please provide a descriptive title for your feedback:

"[[[CSLocalizedString alloc] init] description]" crashes

## Which platform is most relevant for your report?

iOS

## Which technology does your report involve?

Something else not on this list

## What type of feedback are you reporting?

Incorrect/Unexpected Behavior

## What build does the issue occur on?

iOS 17.6.1 (21G101)

## Where does the issue occur?

On device

## Description

Please describe the issue and what steps we can take to reproduce it:

Execute this:

    [[[CSLocalizedString alloc] init] description]

and you get a crash:

(lldb) bt
* thread #1, queue = 'com.apple.main-thread', stop reason = EXC_BAD_ACCESS (code=1, address=0x0)
    frame #0: 0x000000018041bf3c CoreFoundation`CF_IS_OBJC + 20
    frame #1: 0x000000018038d1b0 CoreFoundation`CFArrayGetCount + 28
    frame #2: 0x000000019af5a678 MetadataUtilities`MDRetrieveBestAvailableLanguage + 92
    frame #3: 0x000000019694af9c CoreSpotlight`-[CSLocalizedString localizedString] + 176
    frame #4: 0x000000019694b350 CoreSpotlight`-[CSLocalizedString length] + 40
    frame #5: 0x0000000180f027d0 Foundation`_NS_os_log_callback + 284
    frame #6: 0x00000001800a28c8 libsystem_trace.dylib`_os_log_fmt_flatten_NSCF + 60
    frame #7: 0x00000001800a21fc libsystem_trace.dylib`_os_log_fmt_flatten_object_impl + 332
    frame #8: 0x00000001800b1288 libsystem_trace.dylib`_os_log_impl_flatten_and_send + 1892
    frame #9: 0x00000001800b4168 libsystem_trace.dylib`_os_log_with_args_impl + 400
    frame #10: 0x000000018048699c CoreFoundation`_CFLogvEx3 + 184
    frame #11: 0x0000000180f03dec Foundation`_NSLogv + 120
    frame #12: 0x0000000180f03e30 Foundation`NSLog + 48
  * frame #13: 0x0000000102fb5dec iosapitest`-[AppDelegate application:didFinishLaunchingWithOptions:](self=0x0000600000245440, _cmd="application:didFinishLaunchingWithOptions:", application=0x00000001046055e0, launchOptions=0x0000000000000000) at AppDelegate.m:22:5
    frame #14: 0x0000000185a9f4ec UIKitCore`-[UIApplication _handleDelegateCallbacksWithOptions:isSuspended:restoreState:] + 312
    frame #15: 0x0000000185aa09e4 UIKitCore`-[UIApplication _callInitializationDelegatesWithActions:forCanvas:payload:fromOriginatingProcess:] + 2936
    frame #16: 0x0000000185aa5ae8 UIKitCore`-[UIApplication _runWithMainScene:transitionContext:completion:] + 976
    frame #17: 0x00000001850be070 UIKitCore`-[_UISceneLifecycleMultiplexer completeApplicationLaunchWithFBSScene:transitionContext:] + 148
    frame #18: 0x0000000185aa2814 UIKitCore`-[UIApplication _compellApplicationLaunchToCompleteUnconditionally] + 44
    frame #19: 0x0000000185aa2b1c UIKitCore`-[UIApplication _run] + 760
    frame #20: 0x0000000185aa6d38 UIKitCore`UIApplicationMain + 124
    frame #21: 0x0000000102fb5ff8 iosapitest`main(argc=1, argv=0x000000016ce49b00) at main.m:17:12
    frame #22: 0x000000010300d410 dyld_sim`start_sim + 20
    frame #23: 0x00000001030f6154 dyld`start + 2476

    