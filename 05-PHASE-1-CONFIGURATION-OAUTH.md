# Phase 1 OAuth Configuration Guide

## Introduction
This guide provides a step-by-step process for configuring OAuth for Power BI with Snowflake using Azure AD. The setup will take approximately 3-5 days to complete, depending on your familiarity with the involved technologies.

---

## Creating Azure AD App Registration
1. **Log into the Azure Portal**: 
   Access the Azure Portal at [Azure Portal](https://portal.azure.com).
2. **Navigate to Azure Active Directory**: Click on 'Azure Active Directory' from the left-hand menu.
3. **Select App registrations**: Click on 'App registrations' in the sidebar.
4. **Create a new registration**:  
   - Click on 'New registration'. 
   - Enter your app name.
   - Select who can use this application or access this API.
   - Set the Redirect URI (optional).
   ![App Registration](link_to_screenshot)
5. **Complete Registration**.

### Troubleshooting:
- Ensure the app name is unique.
- Verify the selected account type matches your needs.

---

## Creating Client Secret
1. **Navigate to Certificates & secrets**: In the app registration pane, click on 'Certificates & secrets'.
2. **Create a new secret**:  
   - Click 'New client secret'.
   - Add a description and expiration.
   ![Client Secret](link_to_screenshot)
3. **Save the secret**: Copy the secret value; you won't be able to see it again.

---

## Configuring API Permissions
1. **Select API permissions**: In your app registration, click on 'API permissions'.
2. **Add permissions**:  
   - Click on 'Add a permission'.
   - Choose the APIs your app will access and set the necessary permissions.
   ![API Permissions](link_to_screenshot)
3. **Grant Admin Consent**:  
   - Ensure you grant admin consent for the permissions added.

---

## Creating Snowflake Security Integration
```sql
-- SQL script to create security integration in Snowflake
CREATE OR REPLACE SECURITY INTEGRATION oauth_integration
  TYPE = OAUTH
  OAUTH_TYPE = 'EXTERNAL'
  ENABLED = TRUE
  OAUTH_CLIENT_ID = '<your-client-id>'
  OAUTH_CLIENT_SECRET = '<your-client-secret>'
  OAUTH_REDIRECT_URI = '<your-redirect-uri>';
```

### Troubleshooting:
- Check for existing integrations that may conflict with configuration.
- Ensure credentials are correct.

---

## Mapping Users in Snowflake
1. **Define user mapping**: Outline how your Azure AD users will map to Snowflake users.
2. **SQL for user creation**:  
   ```sql
   CREATE USER some_user PASSWORD='password';
   GRANT ROLE some_role TO USER some_user;
   ```
3. **Testing user access**: Validate that the mapping works as expected.

---

## Enabling SSO in Power BI
1. **Go to Power BI Service**: Log into Power BI at [Power BI Service](https://app.powerbi.com).
2. **Setup SSO**: 
   - Navigate to the dataset settings.
   - Configure the SSO settings with the Azure AD app.
   ![SSO Configuration](link_to_screenshot)
3. **Test SSO functionality**: Ensure that the users can access data seamlessly.

### Troubleshooting:
- Check whether Azure AD is set up for SSO correctly.
- Ensure the correct credentials are configured.

---

## Validation Checklists
- Verify Azure AD app registrations.
- Confirm API permissions are granted.
- Ensure Snowflake integration is set up correctly.
- Test access and permissions.

---

## Troubleshooting Section
- Common issue #1: [Description and solutions]
- Common issue #2: [Description and solutions]

---