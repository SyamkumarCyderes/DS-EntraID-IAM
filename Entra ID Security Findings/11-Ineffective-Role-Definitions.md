PowerShell
==========

Follow these steps to list Microsoft Entra roles using PowerShell.

    Open a PowerShell window. If necessary, use Install-Module to install Microsoft Graph PowerShell. For more information, see Prerequisites to use PowerShell or Graph Explorer.
    PowerShell 

Install-Module Microsoft.Graph -Scope CurrentUser

In a PowerShell window, use Connect-MgGraph to sign in to your tenant.
PowerShell

Connect-MgGraph -Scopes "RoleManagement.Read.All"

Use Get-MgRoleManagementDirectoryRoleDefinition to get all roles.
PowerShell

Get-MgRoleManagementDirectoryRoleDefinition

To view the list of permissions of a role, use the following cmdlet.
PowerShell

    # Do this avoid truncation of the list of permissions
    $FormatEnumerationLimit = -1

    (Get-MgRoleManagementDirectoryRoleDefinition -Filter "displayName eq 'Conditional Access Administrator'").RolePermissions | Format-list

Microsoft Graph API
===================

Follow these instructions to list Microsoft Entra roles using the Microsoft Graph API in Graph Explorer.

    Sign in to the Graph Explorer.

    Select GET as the HTTP method from the dropdown.

    Select the API version to v1.0.

    Add the following query to use the List unifiedRoleDefinitions API.
    HTTP 

GET https://graph.microsoft.com/v1.0/roleManagement/directory/roleDefinitions

Select Run query to list the roles.

To view permissions of a role, use the following API.
HTTP

    GET https://graph.microsoft.com/v1.0/roleManagement/directory/roleDefinitions?$filter=DisplayName eq 'Conditional Access Administrator'&$select=rolePermissions

