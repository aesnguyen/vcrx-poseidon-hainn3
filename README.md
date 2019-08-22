# react-native-vcrx-poseidon-hainn3
React native wrapper for Jitsi Meet SDK

## Install

`npm install react-native-vcrx-poseidon-hainn3 --save` 

## Use

In your component, 

1.) import JitsiMeet and JitsiMeetEvents: `import JitsiMeet, { JitsiMeetEvents } from 'react-native-vcrx-poseidon-hainn3';`

2.) add the following code: 

```
  const initiateVideoCall = () => {
    JitsiMeet.initialize();
    JitsiMeetEvents.addListener('CONFERENCE_LEFT', (data) => {
      console.log('CONFERENCE_LEFT');
    });
    setTimeout(() => {
      JitsiMeet.call(`<your url>`);
    }, 1000);
  };
```
### Events

You can add listeners for the following events:
- CONFERENCE_JOINED
- CONFERENCE_LEFT
- CONFERENCE_WILL_JOIN


## Android Manual Install

1.) In `android/app/src/main/AndroidManifest.xml` add these permissions

```xml
<uses-permission android:name="android.permission.CAMERA" />
<uses-feature android:name="android.hardware.camera" />
<uses-feature android:name="android.hardware.camera.autofocus"/>

<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE"/>
<uses-permission android:name="android.permission.MODIFY_AUDIO_SETTINGS" />
<uses-permission android:name="android.permission.RECORD_AUDIO" />
<uses-permission android:name="android.permission.WAKE_LOCK" />
<uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE"/>
<permission android:name="${applicationId}.permission.JITSI_BROADCAST"
    android:label="Jitsi Meet Event Broadcast"
    android:protectionLevel="normal"></permission>
<uses-permission android:name="${applicationId}.permission.JITSI_BROADCAST"/>
```

2.) In the `<application>` section of `android/app/src/main/AndroidManifest.xml`, add
 ```xml
 <activity android:name="com.reactnativejitsimeet.JitsiMeetNavigatorActivity" />
 ```
 
3.) In `android/settings.gradle`, include react-native-vcrx-poseidon-hainn3 module
```gradle
include ':react-native-vcrx-poseidon-hainn3'
project(':react-native-vcrx-poseidon-hainn3').projectDir = new File(rootProject.projectDir, '../node_modules/react-native-vcrx-poseidon-hainn3/android')
```

4.) In `android/app/build.gradle`, add react-native-vcrx-poseidon-hainn3 to dependencies
```gradle
dependencies {
  ...
    implementation(project(':react-native-vcrx-poseidon-hainn3'))
}
```
and add/replace the following lines:

```
project.ext.react = [
    entryFile: "index.js",
    bundleAssetName: "app.bundle",
]
```

5.) In `android/build.gradle`, add the following code 
```
allprojects {
    repositories {
        mavenLocal()
        jcenter()
        maven {
            // All of React Native (JS, Obj-C sources, Android binaries) is installed from npm
            url "$rootDir/../node_modules/react-native/android"
        }
        maven {
            url "https://maven.google.com"
        }
        maven { // <---- Add this block
            url "https://github.com/jitsi/jitsi-maven-repository/raw/master/releases"
        }
        maven { url "https://jitpack.io" }
    }
}
```

6.) In `android/app/src/main/java/com/xxx/MainApplication.java`

```java
import com.reactnativejitsimeet.JitsiMeetPackage;  // <--- Add this line
...
    @Override
    protected List<ReactPackage> getPackages() {
      return Arrays.<ReactPackage>asList(
        new MainReactPackage(),
        new JitsiMeetPackage()                  // <--- Add this line
      );
    }
```

7.) In `android/app/src/main/java/com/xxx/MainApplication.java` add/replace the following methods:

```
    @Override
    protected String getJSMainModuleName() {
      return "index";
    }

    @Override
    protected @Nullable String getBundleAssetName() {
      return "app.bundle";
    }
```

### Side-note

If your app already includes `react-native-locale-detector` or `react-native-vector-icons`, you must exclude them from the `react-native-vcrx-poseidon-hainn3` project implementation with the following code:

```
    implementation(project(':react-native-vcrx-poseidon-hainn3')) {
      exclude group: 'com.facebook.react',module:'react-native-locale-detector'
      exclude group: 'com.facebook.react',module:'react-native-vector-icons'
    }
```
