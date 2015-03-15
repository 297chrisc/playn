This page contains miscellaneous Maven Android plugin information.

## Configure Android SDK Path ##

To inform the Maven Android plugin of the path to your Android SDK, you must add some configuration information to your `~/.m2/settings.xml` file. This file is located at `C:\Documents and Settings\USERNAME\.m2\settings.xml` on Windows XP and `C:\Users\USERNAME\.m2\settings.xml` on Vista and later.

If you have no `settings.xml` file, then simply use the below XML to create one, otherwise merge the information in the below file into your existing `settings.xml` file:

```
<settings xmlns="http://maven.apache.org/SETTINGS/1.0.0"
     xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
     xsi:schemaLocation="http://maven.apache.org/SETTINGS/1.0.0
         http://maven.apache.org/xsd/settings-1.0.0.xsd">

  <profiles>
    <profile>
      <id>android-sdk</id>
      <properties>
        <android.sdk.path>
          PATH / TO / THE / ANDROID / SDK
        </android.sdk.path>
      </properties>
    </profile>
  </profiles>

  <activeProfiles>
    <activeProfile>android-sdk</activeProfile>
  </activeProfiles>
</settings>
```