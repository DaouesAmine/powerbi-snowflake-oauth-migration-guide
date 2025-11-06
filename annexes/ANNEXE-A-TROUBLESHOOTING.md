# Troubleshooting Guide for OAuth Migration Errors

This document serves as a guide to help troubleshoot common issues encountered during the OAuth migration process for Power BI and Snowflake.

## Common Errors and Solutions

### 1. Invalid Client ID Error
**Error Message:** `Invalid client ID`
- **Cause:** This usually occurs when the client ID provided does not exist or has been deleted.
- **Solution:** Double-check the client ID in your application settings. Make sure it matches the one you received from Snowflake.

### 2. Redirect URI Mismatch
**Error Message:** `Redirect URI does not match`
- **Cause:** The redirect URI specified in your application may not match the one configured in your OAuth provider settings.
- **Solution:** Ensure that the redirect URI in your application matches exactly with the one registered in Snowflake.

### 3. Expired Token
**Error Message:** `Token has expired`
- **Cause:** The access token you are trying to use is no longer valid because it has exceeded its expiration time.
- **Solution:** Request a new access token using the refresh token or re-authenticate the user to get a new access token.

### 4. Insufficient Privileges
**Error Message:** `Insufficient privileges to perform this operation`
- **Cause:** The user associated with the access token does not have sufficient permissions.
- **Solution:** Check the user's roles and permissions in Snowflake and adjust them as needed.

### 5. Network Issues
**Error Message:** `Network error`
- **Cause:** There could be an issue with your internet connection or a problem with the Snowflake service.
- **Solution:** Check your internet connection and try accessing Snowflake through a browser to rule out service issues.

## Additional Resources
- [Snowflake Documentation](https://docs.snowflake.com/en/user-guide/oauth.html)
- [Power BI Documentation](https://docs.microsoft.com/en-us/power-bi/connect-data/desktop-connect-snowflake)