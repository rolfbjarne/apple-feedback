# Basic information

## Please provide a descriptive title for your feedback:

strip crashes (buffer overrun?)

## Which area are you seeing an issue with?

Something else not on this list

## What type of feedback are you reporting?

Incorrect/Unexpected Behavior

## Description

Please describe the issue and what steps we can take to reproduce it
The strip tool crashes with a specific input.

## To repro:

1. Unzip repro.zip (available in FB13327641)
2. Execute:

```
$ cd repro && ./repro.sh
./repro.sh: line 3: 96493 Segmentation fault: 11  xcrun -v strip [...]
```

Tried with:

* Xcode 14.3.1
* Xcode 15.0.0
* Xcode 15.1 beta 2

They all crash (see attached strip-2023-11-02-181209.ips for a crash report).

I also built strip myself (from https://github.com/apple-oss-distributions/cctools/commit/cbe977a1db16a2ca268124f6825535aeb670404c), and that crashes too (see locallybuiltstrip.ips).

However, this works around the crash:

```diff
diff --git a/misc/strip.c b/misc/strip.c
index 2a04cec..0c004bb 100644
--- a/misc/strip.c
+++ b/misc/strip.c
@@ -4058,6 +4058,7 @@ enum bool *nlist_outofsync_with_dyldinfo)
            new_strsize = rnd32(new_strsize, sizeof(int32_t));
        else
            new_strsize = rnd32(new_strsize, sizeof(int64_t));
+       new_strsize *= 2;
        new_strings = (char *)allocate(new_strsize);

        /*
```

So this looks like a buffer overrun of some sort.

---- 

Update: I created a pull request with a fix: https://github.com/apple-oss-distributions/cctools/pull/2
