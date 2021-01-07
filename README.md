## reproduce a bug in Xcode linker ![Test on CI](https://github.com/shifujun/reproduce-a-bug-in-Xcode-linker/workflows/Test%20on%20CI/badge.svg)

This is a bug happened in our real project. This repo reproduce it with stub files.

First, check Xcode version:
```
xcodebuild -version 
Xcode 12.2
Build version 12B45b
```

Then, check **expect** result. `/abc` is a **short** path.
```shell script
./build.sh $(./stub.sh /abc) 2>&1 | tail -n 3

ld: -filelist file '<mktemp -d>/abc/src/ftest_ios_test_result/Build/Intermediates.noindex/VVMainProject.build/Debug-iphonesimulator/VVMainProject.build/Objects-normal/x86_64/VVMainProject.LinkFileList' could not be opened, errno=2 (No such file or directory)

clang: error: linker command failed with exit code 1 (use -v to see invocation)
```

The error `-filelist file .. could not be opened` is expected. If this repo is our real project, it will build success.

Last, change `/abc` to a **very long** path, check **actual** bug result.
```
./build.sh $(./stub.sh /workspace/p-a40c117861104f0aa2e5831e9f5c93d1) 2>&1 | tail -n 3

ld: warning: argument missing after -force_load
ld: file not found: -filelist
clang: error: linker command failed with exit code 1 (use -v to see invocation)
```

Notice that `argument missing after -force_load` and `file not found: -filelist` are both not expected error.

### reproduce on Github Actions
https://github.com/shifujun/reproduce-a-bug-in-Xcode-linker/runs/1628934742?check_suite_focus=true

### Feedback to Apple
https://feedbackassistant.apple.com/feedback/8957741
https://developer.apple.com/forums/thread/670827
