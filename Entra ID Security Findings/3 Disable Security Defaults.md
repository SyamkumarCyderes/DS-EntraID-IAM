To configure security defaults using Microsoft Graph PowerShell:

Connect to the Microsoft Graph service using Connect-MgGraph -Scopes 'Policy.ReadWrite.ConditionalAccess'.

Run the following Microsoft Graph PowerShell command:

$params = @{ IsEnabled = $false }
Update-MgPolicyIdentitySecurityDefaultEnforcementPolicy -BodyParameter $params
