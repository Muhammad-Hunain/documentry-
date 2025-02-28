# Register a Consumer

## Overview

This endpoint facilitates the registration of a consumer, associating their device and application details with the system.

## Endpoint

<div class="code-block">
  <div class="code-nav">
    <div class="code-nav-left">http</div>
    <div class="code-nav-right">
      <button onclick="copyCode(this)">Copy</button>
    </div>
  </div>
  <pre class="code-content"><code>POST /api/app-consumer/register</code></pre>
</div>

## Request Details

### HTTP Method
`POST`

### Content-Type
`application/json`

### Headers

| Header Name  | Type   | Required | Description               |
|--------------|--------|----------|---------------------------|
| Content-Type | string | Yes      | Must be `application/json` |
| Accept       | string | Yes      | Must be `application/json` |

### Body Parameters

The request body should contain a JSON object with a `consumer` property. The `consumer` object has the following structure:

<div class="code-block">
  <div class="code-nav">
    <div class="code-nav-left">json</div>
    <div class="code-nav-right">
      <button onclick="copyCode(this)">Copy</button>
    </div>
  </div>
  <pre class="code-content"><code>{
  "consumer": {
    "app_key": "4217afa8-7e54-4fb2-a9af-aef7fe61d363",
    "app_version": "1.10.25",
    "device_id": "18997",
    "device_model": "iPhone 15",
    "user_id": "1145",
    "is_guest": false,
    "user_email": "admin@gmail.com",
    "os": "ios 14",
    "platform": "ios",
    "country": "us",
    "language": "en"
  }
}</code></pre>
</div>

#### Parameter Details

| Parameter    | Type    | Required | Description                                           |
|--------------|---------|----------|-------------------------------------------------------|
| app_key      | string  | Yes      | The application key (UUID)                            |
| app_version  | string  | Yes      | The application version                               |
| device_id    | string  | Yes      | The device ID                                         |
| device_model | string  | Yes      | The user's device model                               |
| user_id      | string  | Yes      | The user ID                                           |
| is_guest     | boolean | Yes      | Whether the user is a guest                           |
| user_email   | string  | No*      | The user email (required if `is_guest` is `false`)    |
| os           | string  | Yes      | The operating system                                  |
| platform     | string  | Yes      | The user's main platform                              |
| country      | string  | Yes      | The user's country (2-letter ISO code)                |
| language     | string  | Yes      | The user's language (2-letter ISO code)               |

\* `user_email` is required when `is_guest` is `false`.

## Example Request

<div class="code-block">
  <div class="code-nav">
    <div class="code-nav-left">curl</div>
    <div class="code-nav-right">
      <button onclick="copyCode(this)">Copy</button>
    </div>
  </div>
  <pre class="code-content"><code>curl -X POST \
  https://api.example.com/api/app-consumer/register \
  -H 'Content-Type: application/json' \
  -H 'Accept: application/json' \
  -d '{
  "consumer": {
    "app_key": "4217afa8-7e54-4fb2-a9af-aef7fe61d363",
    "app_version": "1.10.25",
    "device_id": "18997",
    "device_model": "iPhone 15",
    "user_id": "1145",
    "is_guest": false,
    "user_email": "admin@gmail.com",
    "os": "ios 14",
    "platform": "ios",
    "country": "us",
    "language": "en"
  }
}'</code></pre>
</div>

## Response

A successful request will return a JSON response with details of the registered consumer. The specific structure of the response will depend on your API implementation.

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