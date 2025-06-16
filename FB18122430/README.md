# [CPListImageRowItem imageTitle] throws unrecognized selector

## FB18122430 — iOS & iPadOS

## Basic Information

### Please provide a descriptive title for your feedback:

[CPListImageRowItem imageTitle] throws unrecognized selector

### Which area are you seeing an issue with?

CarPlay

### What type of feedback are you reporting?

Application Crash

### Description

Please describe the issue and what steps we can take to reproduce it:
Test code:

    NSArray<CPListImageRowItemRowElement *> * elements = [[NSArray alloc] init];
    CPListImageRowItem* obj = [[CPListImageRowItem alloc] initWithText:@"text" elements: elements allowsMultipleLines: FALSE];
    NSArray *array = obj.imageTitles;
    NSLog (@"array: %@", array);

Results in:

    *** Terminating app due to uncaught exception 'NSInvalidArgumentException', reason: '-[CPListImageRowItem imageTitles]: unrecognized selector sent to instance 0x600002115040'
    *** First throw call stack:
    (
    	0   CoreFoundation                      0x00000001804eaf84 __exceptionPreprocess + 172
    	1   libobjc.A.dylib                     0x000000018009bf84 objc_exception_throw + 72
    	2   CoreFoundation                      0x0000000180500848 +[NSObject(NSObject) instanceMethodSignatureForSelector:] + 0
    	3   CoreFoundation                      0x00000001804ef254 ___forwarding___ + 1196
    	4   CoreFoundation                      0x00000001804f179c _CF_forwarding_prep_0 + 92
    	5   ios26betatest                       0x00000001004b8d08 -[AppDelegate application:didFinishLaunchingWithOptions:] + 176
    	6   UIKitCore                           0x00000001861f4fa0 -[UIApplication _handleDelegateCallbacksWithOptions:isSuspended:restoreState:] + 332
    	7   UIKitCore                           0x00000001861f64dc -[UIApplication _callInitializationDelegatesWithActions:forScene:payload:fromOriginatingProcess:] + 3036
    	8   UIKitCore                           0x00000001861fb908 -[UIApplication _runWithMainScene:transitionContext:completion:] + 788
    	9   UIKitCore                           0x00000001857bb824 -[_UISceneLifecycleMultiplexer completeApplicationLaunchWithFBSScene:transitionContext:] + 88
    	10  UIKitCore                           0x00000001861f8650 -[UIApplication _compellApplicationLaunchToCompleteUnconditionally] + 44
    	11  UIKitCore                           0x00000001861f8940 -[UIApplication _run] + 736
    	12  UIKitCore                           0x00000001861fcb1c UIApplicationMain + 124
    	13  ios26betatest                       0x00000001004b8f74 main + 128
    	14  dyld                                0x00000001007553d4 start_sim + 20
    	15  ???                                 0x00000001004eab98 0x0 + 4300123032
    )
    libc++abi: terminating due to uncaught exception of type NSException
    *** Terminating app due to uncaught exception 'NSInvalidArgumentException', reason: '-[CPListImageRowItem imageTitles]: unrecognized selector sent to instance 0x600002115040'
    terminating due to uncaught exception of type NSException

Note: I've only tried in the iOS (26) simulator, not on device.

Files
ios26betatest-cd1b616.zip
