# android-build-tools

Common build logic for android projects


# How to publish an Android Library

To publish a library, you'll need to 
* Specify that your project is an android library
* *THEN* import the android-maven plugin. This *must* come after the android library plugin !!!!
* Apply our common publish logic from our `plugins.gradle` file

In the root of your project, the top level build.gradle should have the following buildscript sections:

```groovy
buildscript {
  repositories {
    mavenCentral()
    jcenter()
    maven { url 'https://www.testfairy.com/maven' }
  }
  dependencies {
    classpath "com.monochromeroad.gradle-plugins:gradle-aws-s3-sync:0.5"
    classpath 'com.github.dcendents:android-maven-plugin:1.2'
  }
}
```

At the top of your build.gradle file in the sub-project that is the library you want to publish:

```groovy
apply plugin: 'com.android.library'
apply plugin: 'com.github.dcendents.android-maven'
apply plugin: 'cpp'

apply from: 'https://raw.githubusercontent.com/playi/android-build-tools/master/publish.gradle'

```

To publish: `./gradlew publishArtifacts`
That's it! Note that this will publish any `master` branch builds to our snapshots repository and will publish and builds on a branch named `release` to our release repository.



# How to publish an app to TestFairy

In your top level build.gradle that is in the root of the project needs the following buildscript sections:

```groovy
buildscript {
  repositories {
    mavenCentral()
    jcenter()
    maven { url 'https://www.testfairy.com/maven' }
  }

  dependencies {
    classpath 'com.testfairy.plugins.gradle:testfairy:1.+'
    classpath 'com.android.tools.build:gradle:1.0.0-rc4'
  }
}
```


In the subproject that contains the app you want to publish:
```groovy
apply plugin: 'android'
apply plugin: 'testfairy'

android {
  .
  .
  .
  .

  testfiaryConfig {
    apiKey W2_TEST_FAIRY_API_KEY_GOES_HERE
  }
}
```

To publish: `./gradlew testfairyDebug` or `./gradlew testfairyRelease` as appropriate
