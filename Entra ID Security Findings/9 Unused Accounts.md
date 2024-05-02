Detect inactive user accounts with Microsoft Graph
==================================================

You can detect inactive accounts by evaluating several properties, some of which are available on the beta endpoint of the Microsoft Graph API. We don't recommend using the beta endpoints in production, but invite you to try them out.

The lastSignInDateTime property exposed by the signInActivity resource type of the Microsoft Graph API. The lastSignInDateTime property shows the last time a user attempted to make an interactive sign-in attempt in Microsoft Entra ID. Using this property, you can implement a solution for the following scenarios:

Last sign-in date and time for all users: In this scenario, you need to generate a report of the last sign-in date of all users. You request a list of all users, and the last lastSignInDateTime for each respective user:

https://graph.microsoft.com/v1.0/users?$select=displayName,signInActivity
Users by name: In this scenario, you search for a specific user by name, which enables you to evaluate the lastSignInDateTime:

https://graph.microsoft.com/v1.0/users?$filter=startswith(displayName,'Isabella Simonsen')&$select=displayName,signInActivity
Users by date: In this scenario, you request a list of users with a lastSignInDateTime before a specified date:

https://graph.microsoft.com/v1.0/users?$filter=signInActivity/lastSignInDateTime le 2019-06-01T00:00:00Z
Last successful sign-in date and time (beta): This scenario is available only on the beta endpoint of the Microsoft Graph API. You can request a list of users with a lastSuccessfulSignInDateTime before a specified date:

https://graph.microsoft.com/beta/users?$filter=signInActivity/lastSuccessfulSignInDateTime le 2019-06-01T00:00:00Z


NOTES:

Considerations for the lastSignInDateTime property
==================================================

The following details relate to the lastSignInDateTime property.

The lastSignInDateTime property is exposed by the signInActivity resource type of the Microsoft Graph API.

The property is not available through the Get-MgAuditLogDirectoryAudit cmdlet.

Each interactive sign-in attempt results in an update of the underlying data store. Typically, sign-ins show up in the related sign-in report within 6 hours.

To generate a lastSignInDateTime timestamp, you must attempt a sign-in. Either a failed or successful sign-in attempt, as long as it's recorded in the Microsoft Entra sign-in logs, generates a lastSignInDateTime timestamp. The value of the lastSignInDateTime property might be blank if:

The last attempted sign-in of a user took place before April 2020.
The affected user account was never used for a sign-in attempt.
The last sign-in date is associated with the user object. The value is retained until the next sign-in of the user. It might take up to 24 hours to update.

