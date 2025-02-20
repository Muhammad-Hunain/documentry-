# Get Started with MAUI and Xamarin

<div class="warning">
  <strong>Important:</strong> Visual Studio App Center is scheduled for retirement on March 31, 2025. While you can continue to use Visual Studio App Center until it is fully retired, there are several recommended alternatives that you may consider migrating to. <a href="#">Learn more about support timelines and alternatives</a>.
</div>

The App Center SDK uses a modular architecture so you can use any or all of the services. This guide will help you set up the App Center .NET SDK in your app to use App Center Analytics and App Center Crashes.

## 1. Prerequisites

Before you begin, ensure that the following prerequisites are met:

- Your project is set up in Visual Studio or Visual Studio for Mac.
- You're targeting devices running iOS 11.0 or later or Android 5.0 (API level 21) or later.
- You're not using any other SDK that provides Crash Reporting functionality.

### Supported Platforms

- MAUI iOS
- MAUI Android
- MAUI Windows
- .NET 6.0 macOS
- Xamarin.Android
- Xamarin.iOS
- Xamarin.Mac
- Xamarin.Forms (iOS, macOS Android, UWP and Windows Desktop applications)

<div class="note">
  <strong>Note:</strong> Currently there is no MAUI platform on the App Center portal. Please use Xamarin for iOS and Android and UWP for Windows.
</div>

### Platform-Specific Notes

#### Xamarin.Android
Create an app on the App Center portal with Android as the OS and Xamarin as the platform.

#### Xamarin.iOS
Create an app on the App Center portal with iOS as the OS and Xamarin as the platform.

#### Xamarin.Mac
Create an app on the App Center portal with macOS as the OS and Xamarin as the platform.

<div class="warning">
  <strong>Warning:</strong> There is a known issue that prevents an app from being uploaded to the App Store. Follow the progress on <a href="https://github.com/microsoft/appcenter-sdk-dotnet/issues/1293">GitHub</a>.
</div>

#### Xamarin.Forms (iOS, macOS, Android, UWP and Windows Desktop)
Create 5 apps on the App Center – one for each OS. Select Xamarin as the platform for Android, iOS and macOS applications (UWP and Desktop applications don't have a Xamarin option).

## 2. Create Your App in the App Center Portal

1. Go to [appcenter.ms](https://appcenter.ms).
2. Sign up or log in and click "Add new" > "Add new app".
3. Enter a name and an optional description for your app.
4. Select the appropriate OS and platform for your project.
5. Click "Add new app".

Once created, you can obtain your App Secret from the Settings page.

## 3. Add the App Center SDK to Your Solution

You can integrate the App Center SDK using Visual Studio or the Package Manager Console.

<div class="code-block">
  <div class="code-nav">
    <div class="code-nav-left">shell</div>
    <div class="code-nav-right">
      <button onclick="copyCode(this)">Copy</button>
    </div>
  </div>
  <pre class="code-content"><code>Install-Package Microsoft.AppCenter.Analytics
Install-Package Microsoft.AppCenter.Crashes</code></pre>
</div>

## 4. Start the SDK

To use App Center, you must opt in to the module(s) that you want to use. By default, no modules are started and you must explicitly call each of them when starting the SDK.

### 4.1 Add the Using Statements

Add the appropriate namespaces in your files:

<div class="code-block">
  <div class="code-nav">
    <div class="code-nav-left">csharp</div>
    <div class="code-nav-right">
      <button onclick="copyCode(this)">Copy</button>
    </div>
  </div>
  <pre class="code-content"><code>using Microsoft.AppCenter;
using Microsoft.AppCenter.Analytics;
using Microsoft.AppCenter.Crashes;</code></pre>
</div>

### 4.2 Add the Start() Method

#### For MAUI and Xamarin.Forms

<div class="code-block">
  <div class="code-nav">
    <div class="code-nav-left">csharp</div>
    <div class="code-nav-right">
      <button onclick="copyCode(this)">Copy</button>
    </div>
  </div>
  <pre class="code-content"><code>AppCenter.Start("ios={Your App Secret};macos={Your App Secret};android={Your App Secret};uwp={Your App Secret};windowsdesktop={Your App Secret}", typeof(Analytics), typeof(Crashes));</code></pre>
</div>

#### For Xamarin.Android

<div class="code-block">
  <div class="code-nav">
    <div class="code-nav-left">csharp</div>
    <div class="code-nav-right">
      <button onclick="copyCode(this)">Copy</button>
    </div>
  </div>
  <pre class="code-content"><code>AppCenter.Start("{Your App Secret}", typeof(Analytics), typeof(Crashes));</code></pre>
</div>

#### For Xamarin.iOS and Xamarin.Mac

<div class="code-block">
  <div class="code-nav">
    <div class="code-nav-left">csharp</div>
    <div class="code-nav-right">
      <button onclick="copyCode(this)">Copy</button>
    </div>
  </div>
  <pre class="code-content"><code>AppCenter.Start("{Your App Secret}", typeof(Analytics), typeof(Crashes));</code></pre>
</div>

<div class="warning">
  <strong>Warning:</strong> It's not recommended to embed your App Secret in source code.
</div>

## 5. Backup Rules (Android Only)

For Android apps targeting API level 23 or higher with Auto Backup enabled, follow these steps:

1. Create `appcenter_backup_rule.xml` file in the `Resources/xml` folder.
2. Add the appropriate attribute to the `<application>` element in `AndroidManifest.xml`.
3. Add the backup rules to the `appcenter_backup_rule.xml` file.

# App Center Data Extraction Rules for Android

When using App Center in your Android application, it's important to configure proper data extraction rules. These rules ensure that sensitive App Center data is not included in cloud backups or device transfers, maintaining the integrity and security of your app's analytics and crash reporting.

## Configuration

Add the following XML to your `appcenter_backup_rule.xml` file in the `res/xml/` directory of your Android project:

<div class="code-block">
  <div class="code-nav">
    <div class="code-nav-left">xml</div>
    <div class="code-nav-right">
      <button onclick="copyCode(this)">Copy</button>
    </div>
  </div>
  <pre class="code-content"><code>&lt;data-extraction-rules xmlns:tools="http://schemas.android.com/tools"&gt;
    &lt;cloud-backup&gt;
        &lt;exclude domain="sharedpref" path="AppCenter.xml"/&gt;
        &lt;exclude domain="database" path="com.microsoft.appcenter.persistence"/&gt;
        &lt;exclude domain="database" path="com.microsoft.appcenter.persistence-journal"/&gt;
        &lt;exclude domain="file" path="error" tools:ignore="FullBackupContent"/&gt;
        &lt;exclude domain="file" path="appcenter" tools:ignore="FullBackupContent"/&gt;
    &lt;/cloud-backup&gt;
    &lt;device-transfer&gt;
        &lt;exclude domain="sharedpref" path="AppCenter.xml"/&gt;
        &lt;exclude domain="database" path="com.microsoft.appcenter.persistence"/&gt;
        &lt;exclude domain="database" path="com.microsoft.appcenter.persistence-journal"/&gt;
        &lt;exclude domain="file" path="error" tools:ignore="FullBackupContent"/&gt;
        &lt;exclude domain="file" path="appcenter" tools:ignore="FullBackupContent"/&gt;
    &lt;/device-transfer&gt;
&lt;/data-extraction-rules&gt;</code></pre>
</div>

## Explanation of Exclusion Rules

These rules exclude specific App Center-related data from both cloud backups and device transfers:

1. **SharedPreferences**: 
   - `AppCenter.xml`: Contains App Center configuration and state.

2. **Databases**:
   - `com.microsoft.appcenter.persistence`: Main database for App Center data.
   - `com.microsoft.appcenter.persistence-journal`: Journal file for the main database.

3. **Files**:
   - `error`: Directory containing error logs.
   - `appcenter`: Directory containing App Center-specific files.

<div class="note">
  <strong>Note:</strong> These exclusions are crucial for maintaining the proper functionality of App Center. Including these files in backups or transfers could lead to data inconsistencies or privacy issues when restoring on a different device.
</div>

## Implementation

To apply these rules:

1. Create the `appcenter_backup_rule.xml` file in your project's `res/xml/` directory if it doesn't already exist.
2. Copy the above XML into this file.
3. In your `AndroidManifest.xml`, add the following attribute to the `<application>` tag:

<div class="code-block">
  <div class="code-nav">
    <div class="code-nav-left">xml</div>
    <div class="code-nav-right">
      <button onclick="copyCode(this)">Copy</button>
    </div>
  </div>
  <pre class="code-content"><code>android:dataExtractionRules="@xml/appcenter_backup_rule"</code></pre>
</div>

By implementing these data extraction rules, you ensure that App Center data is handled correctly during backup and transfer operations on Android devices.

<script>
function copyCode(button) {
  const pre = button.parentElement.parentElement.nextElementSibling;
  const code = pre.querySelector('code');
  const range = document.createRange();
  range.selectNode(code);
  window.getSelection().removeAllRanges();
  window.getSelection().addRange(range);
  document.execCommand('copy');
  window.getSelection().removeAllRanges();
  button.textContent = 'Copied!';
  setTimeout(() => {
    button.textContent = 'Copy';
  }, 2000);
}
</script>

<style>
.note {
  padding: 15px;
  margin-bottom: 20px;
  border: 1px solid transparent;
  border-radius: 4px;
  color: #31708f;
  background-color: #d9edf7;
  border-color: #bce8f1;
}
</style>

<style>
.warning, .note {
  padding: 15px;
  margin-bottom: 20px;
  border: 1px solid transparent;
  border-radius: 4px;
}
.warning {
  color: #8a6d3b;
  background-color: #fcf8e3;
  border-color: #faebcc;
}
.note {
  color: #31708f;
  background-color: #d9edf7;
  border-color: #bce8f1;
}
</style>