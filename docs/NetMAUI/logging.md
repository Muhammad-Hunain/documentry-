# Submit App Log

This endpoint allows you to submit application logs. Authentication is required.

## Endpoint

<div class="code-block">
  <div class="code-nav">
    <div class="code-nav-left">http</div>
    <div class="code-nav-right">
      <button onclick="copyCode(this)">Copy</button>
    </div>
  </div>
  <pre class="code-content"><code>POST https://appambitv2-restless-morning-3748.fly.dev/api/app/log</code></pre>
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
    "https://appambitv2-restless-morning-3748.fly.dev/api/app/log" \
    --header "Authorization: Bearer {YOUR_AUTH_KEY}" \
    --header "Content-Type: multipart/form-data" \
    --header "Accept: application/json" \
    --form "logSummary[title]=error"\
    --form "logSummary[app_version]=neque"\
    --form "logSummary[country_iso]=fugit"\
    --form "logSummary[device_id]=magni"\
    --form "logSummary[device_model]=enim"\
    --form "logSummary[platform]=eaque"\
    --form "logSummary[groups][][title]=laudantium"\
    --form "logSummary[groups][][description]=Voluptatem sunt harum ad sit id."\
    --form "logSummary[groups][][count]=5"\
    --form "logFile=@/tmp/php4hpCIj"</code></pre>
</div>

## Headers

| Header        | Value              | Required | Description                       |
|---------------|---------------------|----------|-----------------------------------|
| Authorization | Bearer {YOUR_AUTH_KEY} | Yes      | Your API key for authentication   |
| Content-Type  | multipart/form-data | Yes      | Indicates the request body format |
| Accept        | application/json    | Yes      | Indicates the response format     |

## Body Parameters

The request body should be sent as `multipart/form-data` and include the following fields:

### logFile

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| logFile   | file | Yes      | The log file to be uploaded |

### logSummary

The `logSummary` object contains metadata about the log. It has the following structure:

| Parameter    | Type     | Required | Description                            |
|--------------|----------|----------|----------------------------------------|
| title        | string   | Yes      | Title of the log entry                 |
| app_version  | string   | Yes      | Version of the application             |
| country_iso  | string   | Yes      | ISO code of the country                |
| device_id    | string   | Yes      | Unique identifier of the device        |
| device_model | string   | Yes      | Model of the device                    |
| platform     | string   | Yes      | Platform of the device (e.g., iOS, Android) |

#### groups

The `groups` array within `logSummary` can contain multiple objects, each with the following structure:

| Parameter   | Type    | Required | Description                   |
|-------------|---------|----------|-------------------------------|
| title       | string  | Yes      | Title of the group            |
| description | string  | Yes      | Description of the group      |
| log_type    | string  | No       | Type of the log (optional)    |
| count       | integer | Yes      | Count associated with the group |

## Example Form Data

Here's an example of how the form data should be structured:



<div class="note">
  <strong>Note:</strong> The <code>logFile</code> should be a file upload. In the curl example, it's represented by <code>@/tmp/php4hpCIj</code>, which is a path to a local file.
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