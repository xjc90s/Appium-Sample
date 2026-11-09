# Appium Sample

## Setup

Use Node.js 18 or newer, install Android Studio and an Android emulator, then install project dependencies:

```sh
npm install
npx appium driver install uiautomator2
```

On npm versions that block dependency install scripts, review the blocked scripts with `npm install-scripts ls`. Appium's postinstall script is optional here because the UiAutomator2 driver is installed explicitly with the command above.

The server output confirms that Appium found the UiAutomator2 driver and accepted the Android capabilities. The session then fails because Appium cannot find the Android SDK: `ANDROID_HOME` or `ANDROID_SDK_ROOT` is not set. On a typical Linux Android Studio install, set these in the same terminal before starting Appium:

```sh
export ANDROID_HOME="$HOME/Android/Sdk"
export ANDROID_SDK_ROOT="$ANDROID_HOME"
export PATH="$PATH:$ANDROID_HOME/platform-tools:$ANDROID_HOME/emulator"
adb devices
npx appium
```

If your SDK is elsewhere, use its actual path (find it in Android Studio under **Settings → Languages & Frameworks → Android SDK**). Keep the Appium server running, then run the tests from another terminal with the same SDK environment:

```sh
npm test
```

The configured capabilities install and launch `Hiver_3.2.2_APKPure.apk` from the project directory on Android. Find the connected device ID with `adb devices`. Set `ANDROID_APP_PACKAGE` if the app package is not `com.hiverhq.app`.

For Appium Inspector, connect to `http://127.0.0.1:4723` and use these capabilities (edit device ID or platform version to match your emulator):

```json
{
  "platformName": "Android",
  "appium:automationName": "UiAutomator2",
  "appium:deviceName": "emulator-5554",
  "appium:udid": "emulator-5554",
  "appium:platformVersion": "17",
  "appium:app": "/usr/local/src/Appium-Sample/Hiver_3.2.2_APKPure.apk",
  "appium:appPackage": "com.hiverhq.app",
  "appium:noReset": true
}
```

The `platformVersion` value must match the emulator's Android version. Omit that capability if you prefer Appium to detect it.

For example, in bash:

```sh
export ANDROID_DEVICE_NAME="Android Emulator"
export ANDROID_DEVICE_UDID="emulator-5554"
export ANDROID_PLATFORM_VERSION="17" # Optional; must match the emulator
export ANDROID_APP_PACKAGE="com.hiverhq.app"
npm test
```

`ANDROID_DEVICE_UDID` is useful when more than one device is connected. Start the emulator and Appium server (`npx appium`) before running the test.
