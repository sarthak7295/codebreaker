---
name: ios-simulator
description: Build, test, and interact with iOS simulators using xcodebuild and xcrun simctl. Use when building the app, running tests, managing simulators, or automating iOS workflows.
version: "1.0"
---

Build, run, and test iOS apps in the simulator using semantic commands. Prefer structured output over screenshots for navigation.

## Build & Test

```bash
# Build for simulator
xcodebuild -scheme Codebreaker \
  -destination 'platform=iOS Simulator,name=iPhone 16,OS=latest' \
  build 2>&1 | xcpretty || xcodebuild ... build

# Run unit tests
xcodebuild -scheme Codebreaker \
  -destination 'platform=iOS Simulator,name=iPhone 16,OS=latest' \
  test

# Build for testing (compile only, no run — good for CI)
xcodebuild -scheme Codebreaker \
  -destination 'generic/platform=iOS Simulator' \
  build-for-testing

# Clean build folder
xcodebuild -scheme Codebreaker clean
```

## Simulator Lifecycle (xcrun simctl)

```bash
# List all available simulators
xcrun simctl list devices available

# Boot a simulator
xcrun simctl boot "iPhone 16"

# Open Simulator app
open -a Simulator

# Install app on booted simulator
xcrun simctl install booted /path/to/App.app

# Launch app
xcrun simctl launch booted com.example.Codebreaker

# Terminate app
xcrun simctl terminate booted com.example.Codebreaker

# Uninstall app
xcrun simctl uninstall booted com.example.Codebreaker

# Erase (factory reset) a simulator
xcrun simctl erase "iPhone 16"

# Shutdown booted simulator
xcrun simctl shutdown booted
```

## Push Notifications (Testing)

```bash
# Send simulated push notification
xcrun simctl push booted com.example.Codebreaker notification.json
```

`notification.json` format:
```json
{
  "aps": {
    "alert": { "title": "Test", "body": "Push notification test" },
    "badge": 1,
    "sound": "default"
  }
}
```

## Permissions

```bash
# Grant a permission
xcrun simctl privacy booted grant photos com.example.Codebreaker

# Revoke a permission
xcrun simctl privacy booted revoke photos com.example.Codebreaker

# Reset all permissions
xcrun simctl privacy booted reset all com.example.Codebreaker

# Available services: photos, camera, microphone, contacts, calendars,
# reminders, location, notifications, motion, health, homekit, medialibrary
```

## Status Bar Override (Clean Screenshots)

```bash
# Set clean status bar for screenshots
xcrun simctl status_bar booted override \
  --time "9:41" \
  --batteryState charged \
  --batteryLevel 100 \
  --wifiBars 3 \
  --cellularBars 4

# Reset status bar
xcrun simctl status_bar booted clear
```

## Logs & Debugging

```bash
# Stream app logs (filter to your bundle ID)
xcrun simctl spawn booted log stream \
  --predicate 'subsystem == "com.example.Codebreaker"'

# Capture screenshot
xcrun simctl io booted screenshot screenshot.png

# Record video
xcrun simctl io booted recordVideo recording.mp4
# Press Ctrl-C to stop
```

## Tips

- Always check `xcrun simctl list devices available` before booting to confirm the device name.
- Prefer `xcodebuild test` over manual launch for regression coverage.
- Use status bar overrides before taking App Store screenshots.
- Stream logs during manual testing to catch errors without Xcode attached.
