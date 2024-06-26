id: ee55dc85-d2da-48c1-a6c0-3eaee62a8d56
name: User Account Created Using Incorrect Naming Format
description: |
  'This query looks for accounts being created where the name does not match a defined pattern.
    Attackers may attempt to add accounts as a means of establishing persistant access to an environment, looking for anomalies in created accounts may help identify illegitimately created accounts.
    Created accounts should be investigated to ensure they were legitimated created.
    The user_regex field in the query needs to be populated with the expected pattern for the environment before deployment.
    Ref: https://docs.microsoft.com/azure/active-directory/fundamentals/security-operations-user-accounts#accounts-not-following-naming-policies'
severity: Low
requiredDataConnectors:
  - connectorId: AzureActiveDirectory
    dataTypes:
      - AuditLogs
queryFrequency: 1d
queryPeriod: 1d
triggerOperator: gt
triggerThreshold: 0
tactics:
  - Persistence
relevantTechniques:
  - T1136.003
tags:
  - AADSecOpsGuide
query: |
  // Add the environments expected username format regex below before deploying
  let user_regex = "";
  AuditLogs
  | where OperationName =~ "Add user"
  | where Result =~ "success"
  | extend userAgent = tostring(AdditionalDetails[0].value)
  | extend InitiatingAppName = tostring(InitiatedBy.app.displayName)
  | extend InitiatingAppServicePrincipalId = tostring(InitiatedBy.app.servicePrincipalId)
  | extend InitiatingUserPrincipalName = tostring(InitiatedBy.user.userPrincipalName)
  | extend InitiatingAadUserId = tostring(InitiatedBy.user.id)
  | extend InitiatingIPAddress = tostring(InitiatedBy.user.ipAddress)
  | extend InitiatedBy = tostring(iff(isnotempty(InitiatingUserPrincipalName),InitiatingUserPrincipalName, InitiatingAppName))
  | extend AddedUser = tostring(TargetResources[0].userPrincipalName)
  | where AddedUser matches regex user_regex
  | extend InitiatingAccountName = tostring(split(InitiatingUserPrincipalName, "@")[0]), InitiatingAccountUPNSuffix = tostring(split(InitiatingUserPrincipalName, "@")[1])
  | extend TargetAccountName = tostring(split(AddedUser, "@")[0]), TargetAccountUPNSuffix = tostring(split(AddedUser, "@")[1])
entityMappings:
  - entityType: Account
    fieldMappings:
      - identifier: Name
        columnName: InitiatingAppName
      - identifier: AadUserId
        columnName: InitiatingAppServicePrincipalId
  - entityType: Account
    fieldMappings:
      - identifier: FullName
        columnName: InitiatingUserPrincipalName
      - identifier: Name
        columnName: InitiatingAccountName
      - identifier: UPNSuffix
        columnName: InitiatingAccountUPNSuffix
  - entityType: Account
    fieldMappings:
      - identifier: AadUserId
        columnName: InitiatingAadUserId
  - entityType: Account
    fieldMappings:
      - identifier: FullName
        columnName: AddedUser
      - identifier: Name
        columnName: TargetAccountName
      - identifier: UPNSuffix
        columnName: TargetAccountUPNSuffix
  - entityType: IP
    fieldMappings:
      - identifier: Address
        columnName: InitiatingIPAddress
version: 1.1.0
kind: Scheduled
metadata:
    source:
        kind: Community
    author:
        name: Microsoft Security Research
    support:
        tier: Community
    categories:
        domains: [ "Security - Others" ]
