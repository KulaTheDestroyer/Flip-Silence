# Flip Silence

Flip Silence is an Android application that automatically manages **Do Not Disturb (DND)** based on the phone's orientation. When the phone is placed face down, the app can enable DND; when it is turned face up, it can restore the previous DND state.

The project is written in Kotlin and uses Jetpack Compose.

## Features

- Accelerometer-based face-down and face-up detection.
- Adjustable sensitivity and activation delay.
- Optional schedule with selected weekdays and a time window, including windows crossing midnight.
- Exclusion conditions for flashlight use, media playback, connected headphones, unlocked screen, camera use, and minimum battery level.
- Separate sounds, volumes, and vibration preferences for DND activation and deactivation.
- Built-in confirmation tones, the device's current default notification sound, or a custom audio file.
- Selectable audio stream (media, notifications, or alarms).
- Configurable DND exceptions for alarms, calls, repeat callers, messages, reminders, events, media, and system sounds.
- Restoration of the previous DND filter and policy when the app turns off a DND state it enabled.
- Foreground monitoring service and boot receiver.
- Polish and English interface.

## Requirements

- Android Studio with Android SDK Platform 36.
- JDK 17.
- Gradle 8.11.1 / Android Gradle Plugin 8.10.1.

Open the `FlipSilence` folder in Android Studio, allow Gradle sync, and select **Build > Build Bundle(s) / APK(s) > Build APK(s)**.

The debug APK is generated at:

`app/build/outputs/apk/debug/app-debug.apk`

If the checkout does not contain `gradle/wrapper/gradle-wrapper.jar`, generate the wrapper on a machine with Gradle 8.11.1 installed by running `gradle wrapper --gradle-version 8.11.1` from the project root.

The GitHub Actions workflow in `.github/workflows/build-apk.yml` can also build the debug APK manually.

## First launch

1. Install and open Flip Silence.
2. Grant **Do Not Disturb access** when prompted. Android requires this special access before an app can change DND.
3. Enable Flip Silence using the main-screen switch.
4. Configure the schedule, exclusions, sounds, vibration, and allowed DND exceptions as desired.
5. Keep the foreground-service notification enabled while monitoring is active.

On some Android devices, including devices with aggressive background management, set Flip Silence's battery usage to **Unrestricted / Allow background activity** and enable auto-launch if the manufacturer provides those options.

## Audio and storage notes

- The Notification sound option reads the device's current default notification sound. Flip Silence does not modify the phone's global notification-sound setting.
- Custom audio browsing is subject to Android's storage permissions and scoped-storage rules. Android may not expose every arbitrary file or folder to an application; media-library access and user-granted folder access are governed by the operating system.
- Previewing a sound does not commit it as the selected sound. The user confirms the selection explicitly.

## Android limitations

- Android requires explicit user-granted Notification Policy Access before an app can change DND.
- The monitoring service uses the `specialUse` foreground-service type. Distribution through Google Play requires the relevant foreground-service declaration and review.
- Media detection uses `AudioManager.isMusicActive` and playback callbacks; behavior may vary by app and device.
- Torch and camera state depend on Android camera callbacks; some OEM camera stacks may delay or omit state changes.
- Accelerometer thresholds and background-service behavior should be tested on the target device.
- Android/OEM battery management can still stop background services. A persistent notification and unrestricted battery settings reduce, but cannot universally eliminate, that risk.

## Project documentation

- [`CHANGELOG.md`](CHANGELOG.md) contains the release history.
- The in-app **Changelog** dialog is intended to display the same release notes bundled with the application.
