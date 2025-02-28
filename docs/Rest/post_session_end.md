# End Session

Ends an active session.

**Endpoint:** `POST /api/session/end`

*Requires authentication*

## Authentication

This endpoint requires a valid Bearer token in the Authorization header.

## Request Headers

| Header | Required | Description |
|--------|----------|-------------|
| Authorization | Yes | Bearer {YOUR_AUTH_KEY} |
| Content-Type | Yes | application/json |
| Accept | Yes | application/json |

## Request Body Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| timestamp | date | Yes | This should be in ISO8601 format. Example: 2024-12-09T15:30:00Z, 2024-12-09T15:30:00+05:30, 2024-12-09T15:30:00-08:00 |
| session_id | string | Yes | The ID of the session to end. Example: 1 |

## Example Request


<div class="code-block">
  <div class="code-nav">
    <div class="code-nav-left">curl</div>
    <div class="code-nav-right">
      <button onclick="copyCode(this)">Copy</button>
    </div>
  </div>
  <pre class="code-content"><code>
curl --request POST \
    "https://appambitv2-restless-morning-3748.fly.dev/api/session/end" \
    --header "Authorization: Bearer {YOUR_AUTH_KEY}" \
    --header "Content-Type: application/json" \
    --header "Accept: application/json" \
    --data "{
    \"timestamp\": \"2024-12-09T15:30:00Z\",
    \"session_id\": \"1\"
}"
</code></pre>
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