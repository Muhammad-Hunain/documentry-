# App Center Crashes (MAUI and Xamarin)

<div class="warning">
  <strong>Important:</strong> Appmit  App Center is scheduled for retirement on March 31, 2025. While you can continue to use Appmit App Center until it is fully retired, there are several recommended alternatives that you may consider migrating to. <a href="#">Learn more about support timelines and alternatives</a>.
</div>

App Center Crashes automatically generates a crash log every time your app crashes. The log is first written to the device's storage and when the user starts the app again, the crash report will be sent to App Center. This feature works for both beta and live apps, providing valuable information to help fix crashes.

<div class="note">
  <strong>Note:</strong> On iOS and Mac, the SDK won't save any crash log if a debugger is attached. On Android, you can crash while having a debugger attached, but you need to continue execution after breaking into the unhandled exception.
</div>

## Generate a Test Crash

App Center Crashes provides an API to generate a test crash for easy testing of the SDK. This API only works in debug configurations.

<div class="code-block">
  <div class="code-nav">
    <div class="code-nav-left">csharp</div>
    <div class="code-nav-right">
      <button onclick="copyCode(this)">Copy</button>
    </div>
  </div>
  <pre class="code-content"><code>Crashes.GenerateTestCrash();</code></pre>
</div>

## Get Information About Previous Crashes

### Check for Low Memory Warning

You can check if the app received a memory warning in the previous session:

<div class="code-block">
  <div class="code-nav">
    <div class="code-nav-left">csharp</div>
    <div class="code-nav-right">
      <button onclick="copyCode(this)">Copy</button>
    </div>
  </div>
  <pre class="code-content"><code>bool hadMemoryWarning = await Crashes.HasReceivedMemoryWarningInLastSessionAsync();</code></pre>
</div>

### Check if App Crashed in Previous Session

You can check if the app crashed in the previous launch:

<div class="code-block">
  <div class="code-nav">
    <div class="code-nav-left">csharp</div>
    <div class="code-nav-right">
      <button onclick="copyCode(this)">Copy</button>
    </div>
  </div>
  <pre class="code-content"><code>bool didAppCrash = await Crashes.HasCrashedInLastSessionAsync();</code></pre>
</div>

### Get Details About the Last Crash

If your app crashed previously, you can get details about the last crash:

<div class="code-block">
  <div class="code-nav">
    <div class="code-nav-left">csharp</div>
    <div class="code-nav-right">
      <button onclick="copyCode(this)">Copy</button>
    </div>
  </div>
  <pre class="code-content"><code>ErrorReport crashReport = await Crashes.GetLastSessionCrashReportAsync();</code></pre>
</div>

## Customize Crash Reporting

### Should the Crash be Processed?

You can decide if a particular crash needs to be processed:

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

### Ask for User Consent

If you want to get user confirmation before sending a crash report:

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
    return true;
};

// Notify user confirmation result
Crashes.NotifyUserConfirmation(UserConfirmation.DontSend);
Crashes.NotifyUserConfirmation(UserConfirmation.Send);
Crashes.NotifyUserConfirmation(UserConfirmation.AlwaysSend);</code></pre>
</div>

## Enable or Disable App Center Crashes at Runtime

You can enable and disable App Center Crashes at runtime:

<div class="code-block">
  <div class="code-nav">
    <div class="code-nav-left">csharp</div>
    <div class="code-nav-right">
      <button onclick="copyCode(this)">Copy</button>
    </div>
  </div>
  <pre class="code-content"><code>// Disable
await Crashes.SetEnabledAsync(false);

// Enable
await Crashes.SetEnabledAsync(true);

// Check if enabled
bool isEnabled = await Crashes.IsEnabledAsync();</code></pre>
</div>

## Handled Errors

App Center also allows you to track errors using handled exceptions:

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
    var attachments = new ErrorAttachmentLog[]
    {
        ErrorAttachmentLog.AttachmentWithText("Hello world!", "hello.txt"),
        ErrorAttachmentLog.AttachmentWithBinary(Encoding.UTF8.GetBytes("Fake image"), "fake_image.jpeg", "image/jpeg")
    };
    Crashes.TrackError(exception, properties, attachments);
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