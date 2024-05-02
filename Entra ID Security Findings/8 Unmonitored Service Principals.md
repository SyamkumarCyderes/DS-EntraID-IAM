Access reports using Microsoft Graph Explorer

With all the prerequisites configured, you can run activity log queries in Microsoft Graph. For more information on Microsoft Graph queries for activity logs, see Activity reports API overview.

Start Microsoft Graph Explorer tool.

Select your profile and then select Modify permissions.

Consent to the following required permissions:

AuditLog.Read.All
Directory.Read.All
Use one of the following queries to start using Microsoft Graph for accessing activity logs:

GET https://graph.microsoft.com/v1.0/auditLogs/directoryAudits
GET https://graph.microsoft.com/v1.0/auditLogs/signIns
GET https://graph.microsoft.com/v1.0/auditLogs/provisioning

Fine-tune your queries
To search for specific activity log entries, use the $filter and createdDateTime query parameters with one of the available properties. Some of the following queries use the beta endpoint. The beta endpoint is subject to change and isn't recommended for production use.

Sign-in log properties
Audit log properties
Try using the following queries:

For sign-in attempts where Conditional Access failed:

GET https://graph.microsoft.com/v1.0/auditLogs/signIns?&$filter=conditionalAccessStatus eq 'failure'
To find sign-ins to a specific application:

GET https://graph.microsoft.com/v1.0/auditLogs/signIns?&$filter=(createdDateTime ge 2024-01-13T14:13:32Z and createdDateTime le 2024-01-14T17:43:26Z) and appId eq 'APP ID'
For non-interactive sign-ins:

GET https://graph.microsoft.com/beta/auditLogs/signIns?&$filter=(createdDateTime ge 2024-01-13T14:13:32Z and createdDateTime le 2024-01-14T17:43:26Z) and signInEventTypes/any(t: t eq 'nonInteractiveUser')
For service principal sign-ins:

GET https://graph.microsoft.com/beta/auditLogs/signIns?&$filter=(createdDateTime ge 2024-01-13T14:13:32Z and createdDateTime le 2024-01-14T17:43:26Z) and signInEventTypes/any(t: t eq 'servicePrincipal')
For managed identity sign-ins:

GET https://graph.microsoft.com/beta/auditLogs/signIns?&$filter=(createdDateTime ge 2024-01-13T14:13:32Z and createdDateTime le 2024-01-14T17:43:26Z) and signInEventTypes/any(t: t eq 'managedIdentity')
To get the authentication method of a user:

GET https://graph.microsoft.com/beta/users/{userObjectId}/authentication/methods
Requires UserAuthenticationMethod.Read.All permission
To see the user registration details report:

GET https://graph.microsoft.com/beta/reports/authenticationMethods/userRegistrationDetails
Requires UserAuthenticationMethod.Read.All permission
For the registration details of specific user:

GET https://graph.microsoft.com/beta/reports/authenticationMethods/userRegistrationDetails/{userId}
Requires UserAuthenticationMethod.Read.All permission
