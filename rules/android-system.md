# Android System Development Rules

## AOSP Architecture (CRITICAL)

ALWAYS follow AOSP architecture patterns:
- System services in `frameworks/base/services/`
- HAL implementations in `hardware/`
- Native code in `frameworks/native/`
- Binder IPC for inter-process communication

## Binder IPC (MANDATORY)

ALWAYS use AIDL for Binder interfaces:

```java
// CORRECT: AIDL interface definition
// IMyService.aidl
package android.os;
interface IMyService {
    int getValue();
    void setValue(int value);
}

// CORRECT: Service implementation
public class MyService extends IMyService.Stub {
    private int mValue = 0;
    
    @Override
    public int getValue() {
        return mValue;
    }
    
    @Override
    public void setValue(int value) {
        mValue = value;
    }
}
```

## SELinux Policy (CRITICAL)

ALWAYS define SELinux policies for new services:

```te
# CORRECT: SELinux policy file
# my_service.te
type my_service, system_server_service, service_manager_type;
type my_service_exec, system_file_type, exec_type, file_type;

init_daemon_domain(my_service)

# Allow service to access required resources
allow my_service system_prop:property_service set;
allow my_service my_service_exec:file execute;
```

## HAL Implementation (MANDATORY)

ALWAYS implement HAL interfaces correctly:

```cpp
// CORRECT: HAL implementation
// MyHal.h
#include <hardware/hardware.h>
#include <hardware/myhal.h>

struct myhal_device_t {
    struct hw_device_t common;
    int (*get_value)(struct myhal_device_t* dev);
    int (*set_value)(struct myhal_device_t* dev, int value);
};

// CORRECT: HAL module
static struct hw_module_methods_t myhal_module_methods = {
    .open = myhal_device_open,
};

struct myhal_module_t HAL_MODULE_INFO_SYM = {
    .common = {
        .tag = HARDWARE_MODULE_TAG,
        .module_api_version = MYHAL_MODULE_API_VERSION_1_0,
        .hal_api_version = HARDWARE_HAL_API_VERSION,
        .id = MYHAL_HARDWARE_MODULE_ID,
        .name = "My HAL",
        .author = "Your Team",
        .methods = &myhal_module_methods,
    },
};
```

## Native Code Safety (CRITICAL)

ALWAYS prevent buffer overflows and memory leaks:

```cpp
// WRONG: Unsafe buffer operations
void processData(char* input) {
    char buffer[64];
    strcpy(buffer, input);  // Buffer overflow risk!
}

// CORRECT: Safe operations
void processData(const char* input) {
    const size_t bufferSize = 64;
    char buffer[bufferSize];
    strncpy(buffer, input, bufferSize - 1);
    buffer[bufferSize - 1] = '\0';
}

// CORRECT: Smart pointers for memory management
#include <memory>

std::unique_ptr<MyClass> createObject() {
    return std::make_unique<MyClass>();
}
```

## System Properties (MANDATORY)

ALWAYS use proper system property APIs:

```cpp
// CORRECT: Reading system properties
#include <cutils/properties.h>

char value[PROPERTY_VALUE_MAX];
property_get("ro.my.property", value, "default_value");

// CORRECT: Setting system properties (requires permissions)
property_set("persist.my.property", "new_value");
```

## Logging (CRITICAL)

ALWAYS use Android logging macros:

```cpp
// CORRECT: Android logging
#include <log/log.h>

#define LOG_TAG "MyService"

void myFunction() {
    ALOGD("Debug message");
    ALOGI("Info message");
    ALOGW("Warning message");
    ALOGE("Error message");
}
```

## Build System (MANDATORY)

ALWAYS use Android.bp (preferred) or Android.mk:

```bp
// CORRECT: Android.bp
cc_library_shared {
    name: "libmyservice",
    srcs: ["myservice.cpp"],
    shared_libs: [
        "libbase",
        "libbinder",
        "libutils",
    ],
    cflags: [
        "-Wall",
        "-Werror",
    ],
}
```

## Security Checklist

Before marking work complete:
- [ ] SELinux policies defined for new services
- [ ] No buffer overflows in native code
- [ ] Memory leaks prevented (smart pointers/RAII)
- [ ] Binder IPC properly secured
- [ ] System properties properly scoped
- [ ] No hardcoded credentials
- [ ] Proper error handling and logging
- [ ] HAL interfaces properly versioned
