activityBasedTimeoutPolicy resource type
========================================

Represents a policy that can control the idle timeout for web sessions for applications that support activity-based timeout functionality. Applications enforce automatic sign out after a period of inactivity. This type of policy can only be applied at the organization level (by setting the isOrganizationDefault property to true).

<https://learn.microsoft.com/en-us/graph/api/resources/activitybasedtimeoutpolicy?view=graph-rest-1.0>




Properties of an activity-based timeout policy definition
The properties below form the JSON object that represents an activity-based timeout policy. This JSON object must be converted to a string with quotations escaped to be inserted into the definition property. An example is shown below in JSON format:

{
  "definition":["{\"ActivityBasedTimeoutPolicy\":{\"Version\":1,\"ApplicationPolicies\":[{\"ApplicationId\":\"default\",\"WebSessionIdleTimeout\":\"01:00:00\"},{\"ApplicationId\":\"c44b4083-3bb0-49c1-b47d-974e53cbdf3c\",\"WebSessionIdleTimeout\":\"00:15:00\"}]}}"]
}

Note: All time durations in these properties are specified in the format "dd.hh:mm:ss".

Note: Max values for properties denoted in "days" are 1 second short of the denoted number of days. For example, the max value of 1 days is specified as "23:59:59".
