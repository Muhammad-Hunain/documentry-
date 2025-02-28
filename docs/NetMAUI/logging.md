# App Center Crashes (MAUI and Xamarin)

<div class="warning">
  <strong>Important:</strong> Visual Studio App Center is scheduled for retirement on March 31, 2025. While you can continue to use Visual Studio App Center until it is fully retired, there are several recommended alternatives that you may consider migrating to. <a href="https://learn.microsoft.com/en-us/appcenter/sunset">Learn more about support timelines and alternatives</a>.
</div>

App Center Crashes will automatically generate a crash log every time your app crashes. The log is first written to the device's storage and when the user starts the app again, the crash report will be sent to App Center. Collecting crashes works for both beta and live apps, i.e. those submitted to the App Store. Crash logs contain valuable information for you to help fix the crash.

Follow the [Getting Started](https://docs.microsoft.com/en-us/appcenter/sdk/getting-started) section if you haven't set up the SDK in your application yet.

Also, crash logs on iOS require Symbolication, check out the [App Center Diagnostics documentation](https://docs.microsoft.com/en-us/appcenter/diagnostics/ios-symbolication) that explains how to provide symbols for your app.

<div class="note">
  <strong>Note:</strong> On iOS and Mac the SDK won't save any crash log if you attached a debugger. Make sure the debugger isn't attached when you crash the iOS and macOS app. On Android, you can crash while having debugger attached but you need to continue execution after breaking into the unhandled exception.
</div>

## Generate a test crash

App Center Crashes provides you with an API to generate a test crash for easy testing of the SDK. This API checks for debug vs release configurations. So you can only use it when debugging as it won't work for release apps.

<div class="code-block">
  <div class="code-nav">
    <div class="code-nav-left">csharp</div>
    <div class="code-nav-right">
      <button onclick="copyCode(this)">Copy</button>
    </div>
  </div>
  <pre class="code-content"><code>Crashes.GenerateTestCrash();</code></pre>
</div>

## Get more information about a previous crash

App Center Crashes has two APIs that give you more information in case your app has crashed.

### Did the app receive a low memory warning in the previous session?

At any time after starting the SDK, you can check if the app received a memory warning in the previous session:

<div class="code-block">
  <div class="code-nav">
    <div class="code-nav-left">csharp</div>
    <div class="code-nav-right">
      <button onclick="copyCode(this)">Copy</button>
    </div>
  </div>
  <pre class="code-content"><code>bool hadMemoryWarning = await Crashes.HasReceivedMemoryWarningInLastSessionAsync();</code></pre>
</div>

<div class="note">
  <strong>Note:</strong> This method must only be used after Crashes has been started, it will always return false before start.
</div>

<div class="note">
  <strong>Note:</strong> In some cases, a device with low memory can't send events.
</div>

### Did the app crash in the previous session?

At any time after starting the SDK, you can check if the app crashed in the previous launch:

<div class="code-block">
  <div class="code-nav">
    <div class="code-nav-left">csharp</div>
    <div class="code-nav-right">
      <button onclick="copyCode(this)">Copy</button>
    </div>
  </div>
  <pre class="code-content"><code>bool didAppCrash = await Crashes.HasCrashedInLastSessionAsync();</code></pre>
</div>

This comes in handy in case you want to adjust the behavior or UI of your app after a crash has occurred. Some developers choose to show additional UI to apologize to their users, or want a way to get in touch after a crash has occurred.

<div class="note">
  <strong>Note:</strong> This method must only be used after Crashes has been started, it will always return false before start.
</div>

### Details about the last crash

If your app crashed previously, you can get details about the last crash.

<div class="code-block">
  <div class="code-nav">
    <div class="code-nav-left">csharp</div>
    <div class="code-nav-right">
      <button onclick="copyCode(this)">Copy</button>
    </div>
  </div>
  <pre class="code-content"><code>ErrorReport crashReport = await Crashes.GetLastSessionCrashReportAsync();</code></pre>
</div>

<div class="note">
  <strong>Note:</strong> This method must only be used after Crashes has been started, it will always return null before start.
</div>

There are numerous use cases for this API, the most common one is people who call this API and implement their custom Crashes delegate or listener.

## Customize your usage of App Center Crashes

App Center Crashes provides callbacks for developers to do additional actions before and when sending crash logs to App Center.

<div class="note">
  <strong>Note:</strong> Set the callback before calling AppCenter.Start(), since App Center starts processing crashes immediately after the start.
</div>

### Should the crash be processed?

Set this callback if you want to decide if a particular crash needs to be processed or not. For example, there could be a system level crash that you'd want to ignore and that you don't want to send to App Center.

<div class="code-block">
  <div class="code-nav">
    <div class="code-nav-left">csharp</div>
    <div class="code-nav-right">
      <button onclick="copyCode(this)">Copy</button>
    </div>
  </div>
  <pre class="code-content"><code>Crashes.ShouldProcessErrorReport = (ErrorReport report) =>
{
    // Check the report in here and return true or false depending on the ErrorReport.
    return true;
};</code></pre>
</div>

### Ask for the user's consent to send a crash log

If user privacy is important to you, you might want to get user confirmation before sending a crash report to App Center. The SDK exposes a callback that tells App Center Crashes to await user confirmation before sending any crash reports.

If you chose to do so, you're responsible for obtaining the user's confirmation, e.g. through a dialog prompt with one of the following options: Always Send, Send, and Don't send. Based on the input, you'll tell App Center Crashes what to do and the crash will then be handled accordingly.

<div class="note">
  <strong>Note:</strong> The SDK doesn't display a dialog for this, the app must provide its own UI to ask for user consent.
</div>

<div class="note">
  <strong>Note:</strong> The app shouldn't call NotifyUserConfirmation explicitly if it doesn't implement a user confirmation dialog; the Crashes module will handle sending logs for you implicitly.
</div>

The following callback shows how to tell the SDK to wait for user confirmation before sending crashes:

<div class="code-block">
  <div class="code-nav">
    <div class="code-nav-left">csharp</div>
    <div class="code-nav-right">
      <button onclick="copyCode(this)">Copy</button>
    </div>
  </div>
  <pre class="code-content"><code>Crashes.ShouldAwaitUserConfirmation = () =>
{
    // Build your own UI to ask for user consent here. SDK doesn't provide one by default.

    // Return true if you built a UI for user consent and are waiting for user input on that custom UI, otherwise false.
    return true;
};</code></pre>
</div>

In case you return true in the callback above, your app must obtain (using your own code) user permission and message the SDK with the result using the following API.

<div class="code-block">
  <div class="code-nav">
    <div class="code-nav-left">csharp</div>
    <div class="code-nav-right">
      <button onclick="copyCode(this)">Copy</button>
    </div>
  </div>
  <pre class="code-content"><code>// Depending on the user's choice, call Crashes.NotifyUserConfirmation() with the right value.
Crashes.NotifyUserConfirmation(UserConfirmation.DontSend);
Crashes.NotifyUserConfirmation(UserConfirmation.Send);
Crashes.NotifyUserConfirmation(UserConfirmation.AlwaysSend);</code></pre>
</div>

As an example you can refer to our [custom dialog example](https://docs.microsoft.com/en-us/appcenter/sdk/crashes/xamarin#ask-for-the-users-consent-to-send-a-crash-log).

### Get information about the sending status for a crash log

At times, you want to know the status of your app crash. A common use case is that you might want to show UI that tells the users that your app is submitting a crash report, or, in case your app is crashing quickly after the launch, you want to adjust the behavior of the app to make sure the crash logs can be submitted. App Center Crashes provides three different callbacks that you can use in your app to be notified of what's going on:

#### The following callback will be invoked before the SDK sends a crash log

<div class="code-block">
  <div class="code-nav">
    <div class="code-nav-left">csharp</div>
    <div class="code-nav-right">
      <button onclick="copyCode(this)">Copy</button>
    </div>
  </div>
  <pre class="code-content"><code>Crashes.SendingErrorReport += (sender, e) =>
{
    // Your code, e.g. to present a custom UI.
};</code></pre>
</div>

In case we have network issues or an outage on the endpoint, and you restart the app, SendingErrorReport is triggered again after process restart.

#### The following callback will be invoked after the SDK sent a crash log successfully

<div class="code-block">
  <div class="code-nav">
    <div class="code-nav-left">csharp</div>
    <div class="code-nav-right">
      <button onclick="copyCode(this)">Copy</button>
    </div>
  </div>
  <pre class="code-content"><code>Crashes.SentErrorReport += (sender, e) =>
{
    // Your code, e.g. to hide the custom UI.
};</code></pre>
</div>

#### The following callback will be invoked if the SDK has failed to send a crash log

<div class="code-block">
  <div class="code-nav">
    <div class="code-nav-left">csharp</div>
    <div class="code-nav-right">
      <button onclick="copyCode(this)">Copy</button>
    </div>
  </div>
  <pre class="code-content"><code>Crashes.FailedToSendErrorReport += (sender, e) =>
{
    // Your code goes here.
};</code></pre>
</div>

Receiving FailedToSendErrorReport means a non-recoverable error such as a 4xx code occurred. For example, 401 means the appSecret is wrong.

This callback isn't triggered if it's a network issue. In this case, the SDK keeps retrying (and also pauses retries while the network connection is down).

### Add attachments to a crash report

You can add binary and text attachments to a crash report. The SDK will send them along with the crash so that you can see them in App Center portal. The following callback will be invoked right before sending the stored crash from previous application launches. It won't be invoked when the crash happens. Be sure the attachment file is not named minidump.dmp as that name is reserved for minidump files. Here is an example of how to attach text and an image to a crash:

<div class="code-block">
  <div class="code-nav">
    <div class="code-nav-left">csharp</div>
    <div class="code-nav-right">
      <button onclick="copyCode(this)">Copy</button>
    </div>
  </div>
  <pre class="code-content"><code>Crashes.GetErrorAttachments = (ErrorReport report) =>
{
    // Your code goes here.
    return new ErrorAttachmentLog[]
    {
        ErrorAttachmentLog.AttachmentWithText("Hello world!", "hello.txt"),
        ErrorAttachmentLog.AttachmentWithBinary(Encoding.UTF8.GetBytes("Fake image"), "fake_image.jpeg", "image/jpeg")
    };
};</code></pre>
</div>

<div class="note">
  <strong>Note:</strong> The size limit is currently 7 MB. Attempting to send a larger attachment will trigger an error.
</div>

## Enable or disable App Center Crashes at runtime

You can enable and disable App Center Crashes at runtime. If you disable it, the SDK won't do any crash reporting for the app.

<div class="code-block">
  <div class="code-nav">
    <div class="code-nav-left">csharp</div>
    <div class="code-nav-right">
      <button onclick="copyCode(this)">Copy</button>
    </div>
  </div>
  <pre class="code-content"><code>Crashes.SetEnabledAsync(false);</code></pre>
</div>

To enable App Center Crashes again, use the same API but pass true as a parameter.

<div class="code-block">
  <div class="code-nav">
    <div class="code-nav-left">csharp</div>
    <div class="code-nav-right">
      <button onclick="copyCode(this)">Copy</button>
    </div>
  </div>
  <pre class="code-content"><code>Crashes.SetEnabledAsync(true);</code></pre>
</div>

You don't need to await this call to make other API calls (such as IsEnabledAsync) consistent.

The state is persisted in the device's storage across application launches.

<div class="note">
  <strong>Note:</strong> This method must only be used after Crashes has been started.
</div>

### Check if App Center Crashes is enabled

You can also check if App Center Crashes is enabled or not:

<div class="code-block">
  <div class="code-nav">
    <div class="code-nav-left">csharp</div>
    <div class="code-nav-right">
      <button onclick="copyCode(this)">Copy</button>
    </div>
  </div>
  <pre class="code-content"><code>bool isEnabled = await Crashes.IsEnabledAsync();</code></pre>
</div>

<div class="note">
  <strong>Note:</strong> This method must only be used after Crashes has been started, it will always return false before start.
</div>

## Handled Errors

App Center also allows you to track errors by using handled exceptions. To do so, use the TrackError method:

<div class="code-block">
  <div class="code-nav">
    <div class="code-nav-left">csharp</div>
    <div class="code-nav-right">
      <button onclick="copyCode(this)">Copy</button>
    </div>
  </div>
  <pre class="code-content"><code>try {
    // your code goes here.
} catch (Exception exception) {
    Crashes.TrackError(exception);
}</code></pre>
</div>

An app can optionally attach properties to a handled error report to provide further context. Pass the properties as a dictionary of key/value pairs (strings only) as shown in the example below.

<div class="code-block">
  <div class="code-nav">
    <div class="code-nav-left">csharp</div>
    <div class="code-nav-right">
      <button onclick="copyCode(this)">Copy</button>
    </div>
  </div>
  <pre class="code-content"><code>try {
    // your code goes here.
} catch (Exception exception) {
    var properties = new Dictionary<string, string>
    {
        { "Category", "Music" },
        { "Wifi", "On"}
    };
    Crashes.TrackError(exception, properties); 
}</code></pre>
</div>

You can also optionally add binary and text attachments to a handled error report. Pass the attachments as an array of ErrorAttachmentLog objects as shown in the example below.

<div class="code-block">
  <div class="code-nav">
    <div class="code-nav-left">csharp</div>
    <div class="code-nav-right">
      <button onclick="copyCode(this)">Copy</button>
    </div>
  </div>
  <pre class="code-content"><code>try {
    // your code goes here.
} catch (Exception exception) {
    var attachments = new ErrorAttachmentLog[]
    {
        ErrorAttachmentLog.AttachmentWithText("Hello world!", "hello.txt"),
        ErrorAttachmentLog.AttachmentWithBinary(Encoding.UTF8.GetBytes("Fake image"), "fake_image.jpeg", "image/jpeg")
    };
    Crashes.TrackError(exception, attachments: attachments);
}</code></pre>
</div>

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
.note, .warning {
  padding: 15px;
  margin-bottom: 20px;
  border: 1px solid transparent;
  border-radius: 4px;
}
.note {
  color: #31708f;
  background-color: #d9edf7;
  border-color: #bce8f1;
}
.warning {
  color: #8a6d3b;
  background-color: #fcf8e3;
  border-color: #faebcc;
}
.code-block {
  margin-bottom: 20px;
}
.code-nav {
  display: flex;
  justify-content: space-between;
  background-color: #f5f5f5;
  padding: 5px 10px;
  border-top-left-radius: 4px;
  border-top-right-radius: 4px;
}
.code-content {
  margin-top: 0;
  border-top-left-radius: 0;
  border-top-right-radius: 0;
}
</style>

