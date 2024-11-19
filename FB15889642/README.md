# Basic information

## Please provide a descriptive title for your feedback:

Can't record audio in the simulator

## Which area are you seeing an issue with?

Simulator

## What type of feedback are you reporting?

Incorrect/Unexpected Behavior

## Did you see an error message?

Yes

## What was the error?

Unsupported property: kMXSessionProperty_PrefersEchoCancelledInput, Failed to set properties, error: -50

## What devices were you using when the issue occurred?

## When did the issue most recently occur?

19 Nov 2024 at 17:29

## Please describe the issue and what steps we can take to reproduce it

Run the following code in the simulator (iOS 16):

    NSError *error = NULL;
    [[AVAudioSession sharedInstance] setCategory:AVAudioSessionCategoryRecord withOptions:0 error:&error];
    NSLog (@"error: %@", error);

This is printed in Xcode:

    AVAudioSessionImpl_Simulator.mm:137   Unsupported property: kMXSessionProperty_PrefersEchoCancelledInput
    AVAudioSessionClient_Common.mm:586   Failed to set properties, error: -50
    error: Error Domain=NSOSStatusErrorDomain Code=-50 "(null)"
---- 

Update: This is with Xcode 16.2 beta 2 (16C5013f)
