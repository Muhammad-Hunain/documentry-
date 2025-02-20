<!-- # App Center Analytics (MAUI and Xamarin)

<div class="warning">
  <strong>Important:</strong> Visual Studio App Center is scheduled for retirement on March 31, 2025. While you can continue to use Visual Studio App Center until it is fully retired, there are several recommended alternatives that you may consider migrating to. <a href="#">Learn more about support timelines and alternatives</a>.
</div>

App Center Analytics helps you understand user behavior and customer engagement to improve your app. The SDK automatically captures session count and device properties like model, OS version, etc. You can define your own custom events to measure things that matter to you.

## Session and Device Information

Once you add App Center Analytics to your app and start the SDK, it automatically tracks sessions and device properties without additional code.

### Country Code

The SDK automatically reports a user's country code for devices with a mobile data modem and SIM card. For WiFi-only devices, you can set the country code manually:

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
  <strong>Note:</strong> Call <code>AppCenter.SetCountryCode</code> before <code>AppCenter.Start</code> for the country code to be displayed in Analytics sessions.
</div>

## Custom Events

Track custom events with up to 20 properties to understand user interactions with your app.

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

<div class="info">
  <strong>Limits:</strong>
  <ul>
    <li>Up to 200 distinct event names</li>
    <li>Maximum 256 characters per event name</li>
    <li>Maximum 125 characters per event property name and value</li>
  </ul>
</div>

## Enable or Disable Analytics at Runtime

Control Analytics collection at runtime:

<div class="code-block">
  <div class="code-nav">
    <div class="code-nav-left">csharp</div>
    <div class="code-nav-right">
      <button onclick="copyCode(this)">Copy</button>
    </div>
  </div>
  <pre class="code-content"><code>// Disable Analytics
await Analytics.SetEnabledAsync(false);

// Enable Analytics
await Analytics.SetEnabledAsync(true);

// Check if Analytics is enabled
bool isEnabled = await Analytics.IsEnabledAsync();</code></pre>
</div>

<div class="note">
  <strong>Note:</strong> These methods must only be used after Analytics has been started.
</div>

## Manage Start Session

Control the start of a new session manually:

<div class="code-block">
  <div class="code-nav">
    <div class="code-nav-left">csharp</div>
    <div class="code-nav-right">
      <button onclick="copyCode(this)">Copy</button>
    </div>
  </div>
  <pre class="code-content"><code>// Enable manual session tracking (call before SDK start)
Analytics.EnableManualSessionTracker();

// Start a new session (call after AppCenter.Start)
Analytics.StartSession();</code></pre>
</div>

<div class="warning">
  <strong>Warning:</strong> Each call to <code>Analytics.StartSession()</code> generates a new session. If not called in manual mode, all logs will have a null session value.
</div>

## Technical Details

### Local Storage
- The SDK stores up to 10 MB of logs in local storage.

### No Internet Access
- Saves up to 10 MB of logs locally when offline.
- Discards old logs when storage is full.
- Sends logs in batches when internet access is restored.

### Batching Event Logs
- Uploads logs in batches of 50.
- Sends logs every 6 seconds if fewer than 50 logs are available.
- Maximum of three batches sent in parallel.

### Retry and Back-off Logic
- 3 tries maximum per request.
- Each request has its own retry state machine.
- All transmission channels are disabled after exhausting retries.

#### Back-off Strategy
- 50% randomization
- First retry: 5-10 seconds
- Second retry: 2.5-5 minutes
- Last try: 10-20 minutes
- Retry states reset when network switches from off to on

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
.warning, .note, .info {
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
.info {
  color: #3c763d;
  background-color: #dff0d8;
  border-color: #d6e9c6;
}
</style> -->


# Submit Analytics Event

This endpoint allows you to submit analytics events for your application. Authentication is required.

## Endpoint

<div class="code-block">
  <div class="code-nav">
    <div class="code-nav-left">http</div>
    <div class="code-nav-right">
      <button onclick="copyCode(this)">Copy</button>
    </div>
  </div>
  <pre class="code-content"><code>POST https://appambitv2-restless-morning-3748.fly.dev/api/analytics</code></pre>
</div>

## Authentication

This endpoint requires authentication. Include your API key in the Authorization header.

<div class="warning">
  <strong>Important:</strong> Keep your API key confidential. Never share it in publicly accessible areas such as GitHub, client-side code, and so forth.
</div>

## Example Request

<div class="code-block">
  <div class="code-nav">
    <div class="code-nav-left">curl</div>
    <div class="code-nav-right">
      <button onclick="copyCode(this)">Copy</button>
    </div>
  </div>
  <pre class="code-content"><code>curl --request POST \
    "https://appambitv2-restless-morning-3748.fly.dev/api/analytics" \
    --header "Authorization: Bearer {YOUR_AUTH_KEY}" \
    --header "Content-Type: application/json" \
    --header "Accept: application/json" \
    --data '{
    "analytics": {
        "event_title": "\"Member Data Loaded\"",
        "session_id": "1",
        "data": "[{ \"a\": \"b\" }, {\"x\": \"y\"}]"
    }
}'</code></pre>
</div>

## Headers

| Header        | Value                     | Required | Description                       |
|---------------|---------------------------|----------|-----------------------------------|
| Authorization | Bearer {YOUR_AUTH_KEY}    | Yes      | Your API key for authentication   |
| Content-Type  | application/json          | Yes      | Indicates the request body format |
| Accept        | application/json          | Yes      | Indicates the response format     |

## Body Parameters

The request body should contain a JSON object with an `analytics` property. The `analytics` object has the following structure:

| Parameter   | Type   | Required | Description                                                   |
|-------------|--------|----------|---------------------------------------------------------------|
| event_title | string | Yes      | A brief descriptive name of the event under concern           |
| session_id  | string | Yes      | The current session id of the user                            |
| data        | json   | Yes      | A JSON array with relevant analytics data                     |

### Example Body

<div class="code-block">
  <div class="code-nav">
    <div class="code-nav-left">json</div>
    <div class="code-nav-right">
      <button onclick="copyCode(this)">Copy</button>
    </div>
  </div>
  <pre class="code-content"><code>{
  "analytics": {
    "event_title": "\"Member Data Loaded\"",
    "session_id": "1",
    "data": "[{ \"a\": \"b\" }, {\"x\": \"y\"}]"
  }
}</code></pre>
</div>

<div class="note">
  <strong>Note:</strong> The <code>data</code> field is a JSON string containing an array of objects. Ensure proper escaping when constructing the request body.
</div>

## Response

Upon successful submission, the server will respond with a confirmation message. The exact format of the response may vary based on the specific implementation.

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
</style>