# Lab-06 - Broken Object Level Authorization

In the previous labs we have successfully configure Azure APIM to be able to protect our API using 
OAuth2.0. However, we have not done the fine-grain authorization yet. You can notice that every users in our system has the same privileges, it can access to the same piece of information through the API.

In the real-world situation, not every users can access to every data. For example, `user-a` should be able to access to his own `user profile` information, but MUST NOT able to access to `user-profile` of other users.

In this scenario, we called `Broken Object Level Authorization`, Because the authorization process has not done in the level of the object.

In the old days, the only way to fix this is by application logic. However, with Azure APIM, we can off-load some of the task to Azure APIM


### Exercise 1 - Examine the problem
In this exercise we will setup `Profile API V1` which will show you how the `Broken Object Level Authorization` works

- Create API Name `Profile API`
    + Backend: `https://workshop-api.z-unified.com/profile-service`
    + Prefix: `profile-service`

- Configure `cors` (All Operations)
- Configure `Validate JWT` (All Operations)
- Add Operation `Get Profile` with path `/v1/profiles/{id}`, Method `GET`
- Use the `Postman` tool or `curl` command line to get the user profile for these user id
    +
    +
    +

NOTE: We can see that even we protect the API with OAuth2.0, any valid users are able to access other's users information. This is really dangerous vulnerabilities

### Exercise 2 - Discussion for Solution
Let explore, the user profile data
```json
{
    "id": "string",
    "username": "string",
    "firstname": "string",
    "avatar": "string",
    "dob": "string",
    "address": "string",
    "mobile": "string",
}
```

The solution is to use the JWT token which is signed from Azure AD to protect the data, We have to create the link between JWT token and the user profile data for each user

NOTE
- Claims `oid` in JWT token is the unique with in the same tanent
- Claims `sub` cannot be used since it is unique within only the same client application, e.g. SPA, So it's not recommended to use it


### Exercise 3 - OID link with user data
Application may need to adjust a little bit by adding `oid` fields to user profile data so that we can relate between JWT and the data, so the data would be

```json
{
    "id": "string",
    "username": "string",
    "firstname": "string",
    "avatar": "string",
    "dob": "string",
    "address": "string",
    "mobile": "string",
    "oid": "string
}
```

### Exercise 4 - Update profile api
In the version 1, `/v1/profiles/{id}`, we get user profile based on the user id, 

