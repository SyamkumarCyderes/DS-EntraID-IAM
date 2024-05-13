Complete an access review of groups and applications in access reviews
======================================================================

You can retrieve the results of an access review using Microsoft Graph or PowerShell.

You will first need to locate the instance of the access review. If the accessReviewScheduleDefinition is a recurring access review, instances represent each recurrence. A review that does not recur will have exactly one instance. Instances also represent each unique group being reviewed in the schedule definition. If a schedule definition reviews multiple groups, each group will have a unique instance for each recurrence. Every instance contains a list of decisions that reviewers can take action on, with one decision per identity being reviewed.

Once you have identified the instance, to retrieve the decisions using Graph, call the Graph API to list decisions from an instance. If this is a multi-stage review, call the Graph API to list decisions from a multi-stage access review. The caller must either be a user in an appropriate role with an application that has the delegated 

     AccessReview.Read.All or 
     AccessReview.ReadWrite.All permission

or an application with the 

     AccessReview.Read.All or 
     AccessReview.ReadWrite.All application permission 

You can retrieve the decisions In PowerShell with the 

     Get-MgIdentityGovernanceAccessReviewDefinitionInstanceDecision 

cmdlet from the Microsoft Graph PowerShell cmdlets for Identity Governance module. Note that the default page size of this API is 100 decision items.
![image](https://github.com/SyamkumarCyderes/DS-EntraID-IAM/assets/166405206/97de3ced-aedd-4f37-b99e-4ada51587689)
