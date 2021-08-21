

## Android开发过程的疑难杂症整理：


**Q:** Unable to determine application id:com.android.tools.idea.run.ApkProvisionException: No outputs for the main artifact of variant:

**A:** try the following steps:

```xml

First clean your project by

Build -> Clean project
Then rebuild project

Build -> Rebuild project
Then run your project. I hope this will work.

if not then go to

File -> Invalidate Caches / Restart -> Invalidate and restart
The last option is you can sync Gradle again in case nothing worked
```





