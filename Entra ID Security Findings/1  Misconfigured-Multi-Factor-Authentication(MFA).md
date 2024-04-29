
PowerShell reporting on users registered for MFA
First, ensure that you have the Install the Microsoft Graph PowerShell SDK installed.

Identify users who have registered for MFA using the PowerShell that follows. This set of commands excludes disabled users since these accounts can't authenticate against Microsoft Entra ID:

#Powershell Script

    Get-MgUser -All | Where-Object {$_.StrongAuthenticationMethods -ne $null -and $_.BlockCredential -eq $False} | Select-Object -Property UserPrincipalName

#Identify users who aren't registered for MFA by running the following PowerShell commands. This set of commands excludes disabled users since these accounts can't authenticate against Microsoft Entra ID:

    Get-MgUser -All | Where-Object {$_.StrongAuthenticationMethods.Count -eq 0 -and $_.BlockCredential -eq $False} | Select-Object -Property UserPrincipalName

#Identify users and output methods registered:

    Get-MgUser -All | Select-Object @{N='UserPrincipalName';E={$_.UserPrincipalName}},@{N='MFA Status';E={if ($_.StrongAuthenticationRequirements.State){$_.StrongAuthenticationRequirements.State} else {"Disabled"}}},@{N='MFA Methods';E={$_.StrongAuthenticationMethods.methodtype}} | Export-Csv -Path c:\MFA_Report.csv -NoTypeInformation


Additional MFA reports

The following additional information and reports are available for MFA events, including those for MFA Server:

Cloud MFA sign-in events from an on-premises AD FS adapter or NPS extension won't have all fields in the sign-in logs populated due to limited data returned by the on-premises component. You can identify these events by the resourceID adfs or radius in the event properties. They include:

resultSignature
appID
deviceDetail
conditionalAccessStatus
authenticationContext
isInteractive
tokenIssuerName
riskDetail, riskLevelAggregated,riskLevelDuringSignIn, riskState,riskEventTypes, riskEventTypes_v2
authenticationProtocol
incomingTokenType
Organizations that run the latest version of NPS extension or use Microsoft Entra Connect Health will have location IP address in events.
