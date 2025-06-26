# CGColorSpaceCreateWithICCData crashes when passed a CGDataProvider.

## FB18396591 — Developer Technologies & SDKs

## Basic Information

### Please provide a descriptive title for your feedback:

CGColorSpaceCreateWithICCData crashes when passed a CGDataProvider.

### Which platform is most relevant for your report?

tvOS

### Which technology does your report involve?

Core Graphics

### What type of feedback are you reporting?

Incorrect/Unexpected Behavior

### What build does the issue occur on?

tvOS 26 Seed 2 (23J5295e)

### Where does the issue occur?

In Xcode

### Description

Please describe the issue and what steps we can take to reproduce it:

CGColorSpaceCreateWithICCData crashes when passed a CGDataProvider.

Test code:

```objective-c
NSString *filePath = [NSBundle.mainBundle pathForResource:@"ColorProfile" ofType:@"icc"];
CGDataProviderRef provider = CGDataProviderCreateWithFilename(filePath.UTF8String);
CGColorSpaceRef colorSpace = CGColorSpaceCreateWithICCData (provider);
```

Stack trace from lldb:

```
(lldb) bt
* thread #1, queue = 'com.apple.main-thread', stop reason = EXC_BAD_ACCESS (code=1, address=0x68)
    frame #0: 0x00000001adb2c744 ColorSync`ColorSyncProfileCreate + 40
    frame #1: 0x00000001848c225c CoreGraphics`CGColorSpaceFromICCDataCacheGetRetained + 88
  * frame #2: 0x0000000104c0cd78 tvos26test`-[AppDelegate application:didFinishLaunchingWithOptions:](self=0x0000600000121300, _cmd="application:didFinishLaunchingWithOptions:", application=0x00000001056054a0, launchOptions=0x0000000000000000) at AppDelegate.m:23:34
    frame #3: 0x00000001da8b76bc UIKitCore`-[UIApplication _handleDelegateCallbacksWithOptions:isSuspended:restoreState:] + 332
    frame #4: 0x00000001da8b8b30 UIKitCore`-[UIApplication _callInitializationDelegatesWithActions:forScene:payload:fromOriginatingProcess:] + 2932
    frame #5: 0x00000001da8bdca4 UIKitCore`-[UIApplication _runWithMainScene:transitionContext:completion:] + 744
    frame #6: 0x00000001d9fc21b8 UIKitCore`-[_UISceneLifecycleMultiplexer completeApplicationLaunchWithFBSScene:transitionContext:] + 116
    frame #7: 0x00000001da4c412c UIKitCore`_UIScenePerformActionsWithLifecycleActionMask + 96
    frame #8: 0x00000001d9fc2ad4 UIKitCore`__101-[_UISceneLifecycleMultiplexer _evalTransitionToSettings:fromSettings:forceExit:withTransitionStore:]_block_invoke + 244
    frame #9: 0x00000001d9fc25f4 UIKitCore`-[_UISceneLifecycleMultiplexer _performBlock:withApplicationOfDeactivationReasons:fromReasons:] + 204
    frame #10: 0x00000001d9fc28e8 UIKitCore`-[_UISceneLifecycleMultiplexer _evalTransitionToSettings:fromSettings:forceExit:withTransitionStore:] + 604
    frame #11: 0x00000001d9fc22e4 UIKitCore`-[_UISceneLifecycleMultiplexer uiScene:transitionedFromState:withTransitionContext:] + 240
    frame #12: 0x00000001d9fcd9b8 UIKitCore`__186-[_UIWindowSceneFBSSceneTransitionContextDrivenLifecycleSettingsDiffAction _performActionsForUIScene:withUpdatedFBSScene:settingsDiff:fromSettings:transitionContext:lifecycleActionType:]_block_invoke + 140
    frame #13: 0x00000001da3c9160 UIKitCore`+[BSAnimationSettings(UIKit) tryAnimatingWithSettings:fromCurrentState:actions:completion:] + 656
    frame #14: 0x00000001da4dc380 UIKitCore`_UISceneSettingsDiffActionPerformChangesWithTransitionContextAndCompletion + 196
    frame #15: 0x00000001d9fcd6c4 UIKitCore`-[_UIWindowSceneFBSSceneTransitionContextDrivenLifecycleSettingsDiffAction _performActionsForUIScene:withUpdatedFBSScene:settingsDiff:fromSettings:transitionContext:lifecycleActionType:] + 288
    frame #16: 0x00000001d9e2d168 UIKitCore`__64-[UIScene scene:didUpdateWithDiff:transitionContext:completion:]_block_invoke.190 + 612
    frame #17: 0x00000001d9e2bf08 UIKitCore`-[UIScene _emitSceneSettingsUpdateResponseForCompletion:afterSceneUpdateWork:] + 200
    frame #18: 0x00000001d9e2cde8 UIKitCore`-[UIScene scene:didUpdateWithDiff:transitionContext:completion:] + 220
    frame #19: 0x00000001da8bca24 UIKitCore`-[UIApplication workspace:didCreateScene:withTransitionContext:completion:] + 452
    frame #20: 0x00000001da3eeef0 UIKitCore`-[UIApplicationSceneClientAgent scene:didInitializeWithEvent:completion:] + 260
    frame #21: 0x00000001854e6184 FrontBoardServices`__95-[FBSScene _callOutQueue_didCreateWithTransitionContext:alternativeCreationCallout:completion:]_block_invoke + 304
    frame #22: 0x00000001854e65f0 FrontBoardServices`-[FBSScene _callOutQueue_maybeCoalesceClientSettingsUpdates:] + 116
    frame #23: 0x00000001854e5fd8 FrontBoardServices`-[FBSScene _callOutQueue_didCreateWithTransitionContext:alternativeCreationCallout:completion:] + 408
    frame #24: 0x00000001855511e8 FrontBoardServices`__93-[FBSWorkspaceScenesClient _callOutQueue_sendDidCreateForScene:transitionContext:completion:]_block_invoke.274 + 232
    frame #25: 0x00000001854f3628 FrontBoardServices`-[FBSWorkspace _calloutQueue_executeCalloutFromSource:withBlock:] + 160
    frame #26: 0x00000001855116ac FrontBoardServices`-[FBSWorkspaceScenesClient _callOutQueue_sendDidCreateForScene:transitionContext:completion:] + 392
    frame #27: 0x0000000185512d80 FrontBoardServices`__92-[FBSWorkspaceScenesClient createSceneWithIdentity:parameters:transitionContext:completion:]_block_invoke_2 + 204
    frame #28: 0x00000001854f3628 FrontBoardServices`-[FBSWorkspace _calloutQueue_executeCalloutFromSource:withBlock:] + 160
    frame #29: 0x0000000104e31590 libdispatch.dylib`_dispatch_client_callout + 12
    frame #30: 0x0000000104e1be0c libdispatch.dylib`_dispatch_block_invoke_direct + 364
    frame #31: 0x00000001b913973c BoardServices`__BSSERVICEMAINRUNLOOPQUEUE_IS_CALLING_OUT_TO_A_BLOCK__ + 44
    frame #32: 0x00000001b913876c BoardServices`BSServiceMainRunLoopSourceHandler + 180
    frame #33: 0x00000001807a0d98 CoreFoundation`__CFRUNLOOP_IS_CALLING_OUT_TO_A_SOURCE0_PERFORM_FUNCTION__ + 24
    frame #34: 0x00000001807a0ce0 CoreFoundation`__CFRunLoopDoSource0 + 168
    frame #35: 0x00000001807a0474 CoreFoundation`__CFRunLoopDoSources0 + 220
    frame #36: 0x000000018079f614 CoreFoundation`__CFRunLoopRun + 788
    frame #37: 0x000000018079a7e0 CoreFoundation`_CFRunLoopRunSpecificWithOptions + 528
    frame #38: 0x0000000184e50980 GraphicsServices`GSEventRunModal + 116
    frame #39: 0x00000001da8bad08 UIKitCore`-[UIApplication _run] + 772
    frame #40: 0x00000001da8bee9c UIKitCore`UIApplicationMain + 124
    frame #41: 0x0000000104c0cc84 tvos26test`main(argc=1, argv=0x000000016b1f1d18) at main.m:17:12
    frame #42: 0x0000000104cbd3d4 dyld_sim`start_sim + 20
    frame #43: 0x0000000104f04854 dyld`start + 6256
```

Files
tvos26test-55ea858.zip
