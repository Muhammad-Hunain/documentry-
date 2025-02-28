# App Center Analytics (MAUI and Xamarin)

<div class="warning">
  <strong>Important:</strong> Visual Studio App Center is scheduled for retirement on March 31, 2025. While you can continue to use Visual Studio App Center until it is fully retired, there are several recommended alternatives that you may consider migrating to. <a href="https://learn.microsoft.com/en-us/appcenter/sunset">Learn more about support timelines and alternatives</a>.
</div>

App Center Analytics helps you understand user behavior and customer engagement to improve your app. The SDK automatically captures session count and device properties like model, OS version, etc. You can define your own custom events to measure things that matter to you. All the information captured is available in the App Center portal for you to analyze the data.

Follow the [Getting Started](https://docs.microsoft.com/en-us/appcenter/sdk/getting-started) section if you haven't set up the SDK in your application yet.

## Session and device information

Once you add App Center Analytics to your app and start the SDK, it will automatically track sessions and device properties like OS Version, model, etc. without writing any additional code.

### Country Code

The SDK automatically reports a user's country code if the device has a mobile data modem and a SIM card installed. WiFi-only devices won't report a country code by default. To set the country code of those users, you must retrieve your user's location yourself and use the SetCountryCode method in the SDK:

<div class="code-block">
  <div class="code-nav">
    <div class="code-nav-left">csharp</div>
    <div class="code-nav-right">
      <button onclick="copyCode(this)">Copy</button>
    </div>
  </div>
  <pre class="code-content"><code>AppCenter.SetCountryCode("en");</code></pre>
</div>

<div class="note">
  <strong>Note:</strong> For country code to be displayed on Analytics sessions, AppCenter.SetCountryCode must be called prior to calling AppCenter.Start.
</div>

## Custom events

You can track your own custom events with up to 20 properties to understand the interaction between your users and the app.

Once you've started the SDK, use the TrackEvent() method to track your events with properties. You can send up to 200 distinct event names. Also, there's a maximum limit of 256 characters per event name and 125 characters per event property name and event property value.

<div class="code-block">
  <div class="code-nav">
    <div class="code-nav-left">csharp</div>
    <div class="code-nav-right">
      <button onclick="copyCode(this)">Copy</button>
    </div>
  </div>
  <pre class="code-content"><code>Analytics.TrackEvent("Video clicked", new Dictionary<string, string> {
    { "Category", "Music" },
    { "FileName", "favorite.avi"}
});</code></pre>
</div>

Properties for events are entirely optional – if you just want to track an event, use this sample instead:

<div class="code-block">
  <div class="code-nav">
    <div class="code-nav-left">csharp</div>
    <div class="code-nav-right">
      <button onclick="copyCode(this)">Copy</button>
    </div>
  </div>
  <pre class="code-content"><code>Analytics.TrackEvent("Video clicked");</code></pre>
</div>

## Enable or disable App Center Analytics at runtime

You can enable and disable App Center Analytics at runtime. If you disable it, the SDK won't collect any more analytics information for the app.

<div class="code-block">
  <div class="code-nav">
    <div class="code-nav-left">csharp</div>
    <div class="code-nav-right">
      <button onclick="copyCode(this)">Copy</button>
    </div>
  </div>
  <pre class="code-content"><code>Analytics.SetEnabledAsync(false);</code></pre>
</div>

To enable App Center Analytics again, use the same API but pass true as a parameter.

<div class="code-block">
  <div class="code-nav">
    <div class="code-nav-left">csharp</div>
    <div class="code-nav-right">
      <button onclick="copyCode(this)">Copy</button>
    </div>
  </div>
  <pre class="code-content"><code>Analytics.SetEnabledAsync(true);</code></pre>
</div>

You don't need to await this call to make other API calls (such as IsEnabledAsync) consistent.

The state is persisted in the device's storage across application launches.

<div class="note">
  <strong>Note:</strong> This method must only be used after Analytics has been started.
</div>

## Check if App Center Analytics is enabled

You can also check if App Center Analytics is enabled or not.

<div class="code-block">
  <div class="code-nav">
    <div class="code-nav-left">csharp</div>
    <div class="code-nav-right">
      <button onclick="copyCode(this)">Copy</button>
    </div>
  </div>
  <pre class="code-content"><code>bool isEnabled = await Analytics.IsEnabledAsync();</code></pre>
</div>

<div class="note">
  <strong>Note:</strong> This method must only be used after Analytics has been started, it will always return false before start.
</div>

## Manage start session

By default, the session ID depends on the lifecycle of the application. If you want to control the start of a new session manually, follow the next steps:

<div class="note">
  <strong>Note:</strong> Pay attention that each call of Analytics.StartSession() API will generate a new session. If in manual session tracker mode this API will not be called then all sending logs will have a null session value.
</div>

<div class="note">
  <strong>Note:</strong> Pay attention that after a new application launch the session id will be regenerated.
</div>

Call the following method before the SDK start:

<div class="code-block">
  <div class="code-nav">
    <div class="code-nav-left">csharp</div>
    <div class="code-nav-right">
      <button onclick="copyCode(this)">Copy</button>
    </div>
  </div>
  <pre class="code-content"><code>Analytics.EnableManualSessionTracker();</code></pre>
</div>

Then you can use the StartSession API after the AppCenter.Start:

<div class="code-block">
  <div class="code-nav">
    <div class="code-nav-left">csharp</div>
    <div class="code-nav-right">
      <button onclick="copyCode(this)">Copy</button>
    </div>
  </div>
  <pre class="code-content"><code>Analytics.StartSession();</code></pre>
</div>

## Local storage size

By default, the SDK stores up to 10 MB of logs in the storage.

## No internet access

When there isn't any network connectivity, the SDK saves up to 10 MB of logs in the local storage. Once the storage is full, the SDK will start discarding old logs to make room for the new logs. Once the device gets internet access back, the SDK will send logs in the batch of 50 or after every 6 seconds.

## Batching event logs

The App Center SDK uploads logs in a batch of 50 and if the SDK doesn't have 50 logs to send, it will still send logs after 6 seconds. There can be a maximum of three batches sent in parallel.

## Retry and back-off logic

App Center SDK supports back-off retries on recoverable network errors. Below is the retry logic:

- 3 tries maximum per request.
- Each request has its own retry state machine.
- All the transmission channels are disabled (until next app process) after one request exhausts all its retries.

### Back-off logic

- 50% randomization, first retry between 5s and 10s, second retry between 2.5 and 5 minutes, last try between 10 and 20 minutes.
- If network switches from off to on (or from wi-fi to mobile), retry states are reset and requests are retried immediately.

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
