## Account

An Account is a unique identity within a Directory, with a unique username and/or email address. An account can log in to applications using either the email address or username associated with it. Accounts can represent people end-users, but they can also be used to represent services, machines, or any ‘entity’ that needs to login to a Stormpath-enabled application.

### Properties

Properties on a instance resource can be read or set.  There are a set of properties that are read-only that are set during Stormpath operations, like creation or modification.  

| Name  | Description   | REST  | Java  | Python    | Node  | PHP   | Ruby  |
| ----  | -----------   | ----  | ----  | ------    | ----  | ---   | ----  |
| Created At | The created at time for the resource in ISO 8601 datetime format |  |  |  |  |  |  |
| Email | The email for the account |  |  |  |  |  |  |
| Full Name | The derived full name from the Given Name and Surname.  This is a read-only property |  |  |  |  |  |  |
| Given Name | The given name (first name) of the account |  |  |  |  |  |  |
| Href | The reference to the resource in Stormpath. This property is read-only |  |  |  |  |  |  |
| Middle Name | The middle name of the account |  |  |  |  |  |  |
| Modified At | The last modified time for tthe resource in ISO 8601 datetime format.  This is a read-only property |  |  |  |  |  |  |
| Status | The status of the account.  The status of the account can be `ENABLED` `DISABLED` `UNVERIFIED` |  |  |  |  |  |  |
| Surname | The surname (last name) of the account |  |  |  |  |  |  |
| Username | The username of the account |  |  |  |  |  |  |

### Links

Links represent properties on an instance resource that point to another resource.  These must be obtainable from the resource.

| Name  | Description   | REST  | Java  | Python    | Node  | PHP   | Ruby  |
| ----  | -----------   | ----  | ----  | ------    | ----  | ---   | ----  |
| API Keys | An expandable link to the account's API Keys collection  |  |  |  |  |  |  |
| Applications | An expandable link to the account's application collection.  This collection represents the applications the account is associated with through account store mappings  |  |  |  |  |  |  |
| Custom Data | An expandable link to the account's customData  |  |  |  |  |  |  |
| Directory | An exandable link to the directory that the account was created in  |  |  |  |  |  |  |
| Email Verification Token | 
| Group Memberships | An expandable link to the group memberships collection for the account  |  |  |  |  |  |  |
| Groups | An expandable link to the groups for the account  |  |  |  |  |  |  |
| Provider Data | An expandable link to the account's provider data  |  |  |  |  |  |  |
| Tenant | An expandable link to the tenant that the account was created in  |  |  |  |  |  |  |

### Functions

Functions are methods that can be invoked from the resource. 

| Name  | Description   | Input | Output  | REST    | Java  | Python    | Node  | PHP   | Ruby  |
| ----  | -----------   | ----- | ------  | ----    | ----  | ------    | ----  | ---   | ----  |
| Add Group | A function used to add the account to a group | Group | Group Membership |  |  |  |  |  |  |
| Create API Key | A function used to create an API Key for the account | | API Key |  |  |  |  |  |  |
| Is Member of Group | A function used to check if the account is associated with a group | boolean | String (name or href for the Group |  |  |  |  |  |  |