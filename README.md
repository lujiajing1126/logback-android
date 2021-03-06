# logback-android [![CircleCI branch](https://img.shields.io/circleci/project/tony19/logback-android/master.svg)](https://circleci.com/gh/tony19/logback-android) [![Codacy Badge](https://api.codacy.com/project/badge/grade/c1d818d1911440e3b6d685c20a425856)](https://www.codacy.com/app/tony19/logback-android)
<sup>v1.1.1-5</sup>

Overview
--------
[`logback-android`][2] brings the power of [`logback`][1] to Android. This library provides a highly configurable logging framework for Android apps, supporting multiple log destinations simultaneously:

 * files
 * SQLite databases
 * logcat
 * sockets
 * syslog
 * email

Runs on Android 2.1 or higher. See [Wiki][6] for documentation.

Quick Start
-----------
1. Create a new "Basic Activity" app in [Android Studio][5].
2. In `app/build.gradle`, add the following dependencies:

    ```groovy
    // only needed for SNAPSHOT builds
    repositories {
      maven { url 'https://oss.sonatype.org/content/repositories/snapshots' }
    }

    dependencies {
      compile 'org.slf4j:slf4j-api:1.7.21'
      compile 'com.github.tony19:logback-android-core:1.1.1-5'
      compile('com.github.tony19:logback-android-classic:1.1.1-5') {
        // workaround issue #73
        exclude group: 'com.google.android', module: 'android'
      }
    }
    ```

3. Create `app/src/main/assets/logback.xml` containing:

    ```xml
    <configuration>
      <appender name="logcat" class="ch.qos.logback.classic.android.LogcatAppender">
        <tagEncoder>
          <pattern>%logger{12}</pattern>
        </tagEncoder>
        <encoder>
          <pattern>[%-20thread] %msg</pattern>
        </encoder>
      </appender>

      <root level="DEBUG">
        <appender-ref ref="logcat" />
      </root>
    </configuration>
    ```

4. In `MainActivity.java`, add the following imports:

    ```java
    import org.slf4j.Logger;
    import org.slf4j.LoggerFactory;
    ```

5. ...and modify `onOptionsItemSelected()` to log "hello world":

    ```java
    @Override
    public boolean onOptionsItemSelected(MenuItem item) {
        Logger log = LoggerFactory.getLogger(MainActivity.class);
        log.info("hello world");
        // ...
    }
    ```

6. Build and start the app.
7. Open logcat for your device (via the _Android Monitor_ tab in Android Studio).
8. Click the app menu, and select the menu-option. You should see "hello world" in logcat.


Download
--------
_Gradle_

```groovy
// only needed for SNAPSHOT builds
repositories {
  maven { url 'https://oss.sonatype.org/content/repositories/snapshots' }
}

dependencies {
  compile 'org.slf4j:slf4j-api:1.7.21'
  compile 'com.github.tony19:logback-android-core:1.1.1-5'
  compile 'com.github.tony19:logback-android-classic:1.1.1-5'
}
```

_Prefer local jars?_

 * [logback-android-1.1.1-5.jar][3] (uber jar)
 * [slf4j-api-1.7.21.jar][4]


Build
-----
`logback-android` is built with Gradle 2.12. Use these commands to create the uber jar (with debug symbols).

    git clone git://github.com/tony19/logback-android.git
    cd logback-android
    scripts/makejar.sh

The jar would be in: `./build/logback-android-1.1.1-5.jar`

 [1]: http://logback.qos.ch
 [2]: http://tony19.github.com/logback-android
 [3]: https://bitbucket.org/tony19/logback-android-jar/downloads/logback-android-1.1.1-5.jar
 [4]: http://search.maven.org/remotecontent?filepath=org/slf4j/slf4j-api/1.7.21/slf4j-api-1.7.21.jar
 [5]: http://developer.android.com/sdk/index.html
 [6]: https://github.com/tony19/logback-android/wiki