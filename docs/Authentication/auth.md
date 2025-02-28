<style>
    
/* Custom badge for authentication */
.badge {
  display: inline-flex;
  align-items: center;
  font-size: 0.75rem;
  font-weight: 500;
  padding: 0.125rem 0.5rem;
  border-radius: 0.375rem;
  vertical-align: middle;
}

.badge-auth {
  background-color: rgba(59, 130, 246, 0.1);
  color: #3b82f6;
  border: 1px solid rgba(59, 130, 246, 0.2);
}

.badge-auth::before {
  content: "";
  display: inline-block;
  width: 0.75rem;
  height: 0.75rem;
  margin-right: 0.25rem;
  background-image: url('data:image/svg+xml;utf8,<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="%233b82f6" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><rect x="3" y="11" width="18" height="11" rx="2" ry="2"></rect><path d="M7 11V7a5 5 0 0 1 10 0v4"></path></svg>');
  background-size: contain;
  background-repeat: no-repeat;
}

/* Code block styling */
.md-typeset pre > code {
  border-radius: 0.375rem;
}

/* Admonition styling */
.md-typeset .admonition {
  font-size: 0.8rem;
}


</style>
# Authentication

## Authenticating Requests

To authenticate requests, include an `Authorization` header with the value `"Bearer {YOUR_AUTH_KEY}"`.

!!! info
    All authenticated endpoints are marked with a <span class="badge badge-auth">requires authentication</span> badge in the documentation below.

### Retrieving Your Token

You can retrieve your token by visiting your dashboard and clicking **Generate API token**.

### Example Request

```bash
curl -X GET \
  https://api.example.com/v1/users \
  -H "Authorization: Bearer your_api_token_here"


