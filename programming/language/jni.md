<https://developer.android.com/training/articles/perf-jni.html#native_libraries>

```cpp
jint JNI_OnLoad(JavaVM* vm, void* reserved) {
    JNIEnv* env;
    if (vm->GetEnv(reinterpret_cast<void**>(&env), JNI_VERSION_1_6) != JNI_OK) {
        return -1;
    }

    // Get jclass with env->FindClass.
    // Register methods with env->RegisterNatives.

    return JNI_VERSION_1_6;
}
```

```sh
adb shell stop
adb shell setprop dalvik.vm.checkjni true
adb shell start

adb shell setprop debug.checkjni 1
```
