Powershell script to find the number of privileged accounts in Entra ID. 
========================================================================

Get-MgBetaRoleManagementDirectoryRoleDefinition -Filter "isPrivileged eq true" | Format-List

To list privileged permissions, use the Get-MgBetaRoleManagementDirectoryResourceNamespaceResourceAction command.
=================================================================================================================

Get-MgBetaRoleManagementDirectoryResourceNamespaceResourceAction -UnifiedRbacResourceNamespaceId "microsoft.directory" -Filter "isPrivileged eq true" | Format-List

To list privileged role assignments, use the Get-MgBetaRoleManagementDirectoryRoleAssignment command.
=====================================================================================================

Get-MgBetaRoleManagementDirectoryRoleAssignment -ExpandProperty "roleDefinition" -Filter "roleDefinition/isPrivileged eq true" | Format-List


