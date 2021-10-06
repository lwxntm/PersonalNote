# QT-CMAKE屏蔽黑框框

```
set_target_properties(${PROJECT_NAME} PROPERTIES
    LINK_FLAGS_RELEASE "/SUBSYSTEM:WINDOWS /ENTRY:mainCRTStartup"
```

