# Register a Consumer

## Overview

This endpoint enables consumer registration by securely associating device and application details with our system. Follow this documentation to properly integrate the registration process into your application.

## Endpoint

<div class="code-block">
  <div class="code-nav">
    <div class="code-nav-left">http</div>
    <div class="code-nav-right">
      <button onclick="copyCode(this)" class="copy-button">Copy</button>
    </div>
  </div>
  <pre class="code-content"><code>POST /api/app-consumer/register</code></pre>
</div>

## Request Details

### HTTP Method
`POST`

### Content-Type
`application/json`

### Required Headers

| Header Name  | Type   | Required | Description               |
|--------------|--------|----------|---------------------------|
| Content-Type | string | Yes      | Must be `application/json` |
| Accept       | string | Yes      | Must be `application/json` |

### Request Body Schema

The request body must contain a JSON object with a `consumer` property structured as follows:

<div class="code-block">
  <div class="code-nav">
    <div class="code-nav-left">json</div>
    <div class="code-nav-right">
      <button onclick="copyCode(this)" class="copy-button">Copy</button>
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

### Required Parameters

| Parameter    | Type    | Required | Description                                           | Format/Example |
|--------------|---------|----------|-------------------------------------------------------|---------------|
| app_key      | string  | Yes      | Unique application identifier                         | UUID format   |
| app_version  | string  | Yes      | Current version of your application                   | "1.10.25"     |
| device_id    | string  | Yes      | Unique identifier for the user's device              | "18997"       |
| device_model | string  | Yes      | Model name of the user's device                      | "iPhone 15"    |
| user_id      | string  | Yes      | Unique identifier for the user                       | "1145"        |
| is_guest     | boolean | Yes      | Indicates if the user is accessing as a guest        | true/false    |
| user_email   | string  | No*      | User's email address                                 | Valid email   |
| os           | string  | Yes      | Operating system with version                        | "ios 14"      |
| platform     | string  | Yes      | Platform identifier                                  | "ios"/"android"|
| country      | string  | Yes      | User's country code                                  | ISO 3166-1 alpha-2|
| language     | string  | Yes      | User's preferred language                            | ISO 639-1     |

**Important Note:** `user_email` becomes mandatory when `is_guest` is set to `false`.

## Integration Example

Use the following cURL command as a reference for your implementation:

<div class="code-block">
  <div class="code-nav">
    <div class="code-nav-left">curl</div>
    <div class="code-nav-right">
      <button onclick="copyCode(this)" class="copy-button">Copy</button>
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

Upon successful registration, the API will return a JSON response containing the registered consumer details. The response structure will align with your specific implementation requirements.

<style>
.code-block {
  background: #f8f9fa;
  border-radius: 6px;
  margin: 20px 0;
  box-shadow: 0 2px 4px rgba(0,0,0,0.1);
}

.code-nav {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 8px 16px;
  background: #e9ecef;
  border-radius: 6px 6px 0 0;
  border-bottom: 1px solid #dee2e6;
}

.code-nav-left {
  color: #495057;
  font-family: monospace;
  font-weight: 600;
}

.copy-button {
  background: #6c757d;
  color: white;
  border: none;
  padding: 6px 12px;
  border-radius: 4px;
  cursor: pointer;
  font-size: 14px;
  transition: background 0.2s;
}

.copy-button:hover {
  background: #5a6268;
}

.code-content {
  margin: 0;
  padding: 16px;
  overflow-x: auto;
}

code {
  font-family: 'Monaco', 'Menlo', 'Ubuntu Mono', 'Consolas', monospace;
  font-size: 14px;
}

table {
  width: 100%;
  border-collapse: collapse;
  margin: 20px 0;
}

th, td {
  padding: 12px;
  border: 1px solid #dee2e6;
  text-align: left;
}

th {
  background: #f8f9fa;
  font-weight: 600;
}

tr:nth-child(even) {
  background: #f8f9fa;
}
</style>

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