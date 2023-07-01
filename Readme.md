## Purpose:
The purpose of the system is to connect food donors with local charities and food banks to minimize food waste and ensure that surplus food is redistributed to those in need.

## System Components:
The backend service will consist of several key components:

a. User Management: Implementing user registration, authentication, and authorization functionalities to manage the profiles of food donors, charities, and food banks.

b. Donation Management: Allowing food donors to input information about surplus food, including quantity, type, expiration date, and pickup location. Donors should be able to schedule pickups and track the status of their donations.

c. Inventory Management: Maintaining a centralized inventory of available food donations, including details such as food type, quantity, quality, and expiry dates.

d. Matching Algorithm: Designing an algorithm that matches available food donations with the requirements and capacity of local charities and food banks. Consider factors like proximity, dietary restrictions, and storage capabilities to optimize the matching process.

e. Delivery Coordination: Implementing features to facilitate the coordination of food pickups and deliveries, including scheduling, routing, and notifications to ensure timely and efficient redistribution of surplus food.

f. Reporting and Analytics: Generating reports and provide analytics on food waste reduction, donation trends, impact assessment, and other relevant metrics to measure the effectiveness of the system.

## Technology Stack:
To build the backend service, you can consider using the following technologies and frameworks:

a. Programming Language: Java for the backend logic.

b. Framework: Spring Boot for rapid development and efficient management of RESTful APIs.

c. Database: MySQL for storing user profiles, food donation information, and other relevant data.

d. Authentication and Authorization: Using JWT (JSON Web Tokens) for secure user authentication and authorization.

e. RESTful API Design: Implementing RESTful APIs using Spring MVC to expose the necessary endpoints for communication with the frontend and other services.

f. Data Persistence: Utilizing an Object-Relational Mapping (ORM) tool like JPA to interact with the database.

g. Messaging System: Considering the use of a messaging system like Apache Kafka to handle real-time notifications and event-driven communication between system components.

## Integration:
Integration of the backend service with frontend applications (web or mobile) that allow food donors, charities, and food banks to interact with the system. Ensure smooth data flow and provide a user-friendly interface for seamless user experiences.

## Security Considerations:
Implementing a proper security measures to protect user data and prevent unauthorized access. Utilize encryption techniques, secure communication protocols (HTTPS), and follow best practices for data protection.

## Testing and Deployment:
Testing of the backend service, including unit testing, integration testing, and end-to-end testing. Deployment of the service to a reliable hosting environment or cloud platform that ensures scalability, availability, and performance.



---
## User Management - DB Schema:

Table: users

id: INT (Primary Key)
username: VARCHAR(255)
password: VARCHAR(255)
email: VARCHAR(255)
first_name: VARCHAR(255)
last_name: VARCHAR(255)
created_at: DATETIME
updated_at: DATETIME

Table: roles

id: INT (Primary Key)
name: VARCHAR(255)
created_at: DATETIME
updated_at: DATETIME

Table: privileges

id: INT (Primary Key)
name: VARCHAR(255)
created_at: DATETIME
updated_at: DATETIME
Table: user_roles

id: INT (Primary Key)
user_id: INT (Foreign Key referencing users.id)
role_id: INT (Foreign Key referencing roles.id)
created_at: DATETIME
updated_at: DATETIME
Table: user_privileges

id: INT (Primary Key)
user_id: INT (Foreign Key referencing users.id)
privilege_id: INT (Foreign Key referencing privileges.id)
created_at: DATETIME
updated_at: DATETIME
Table: user_groups

id: INT (Primary Key)
name: VARCHAR(255)
created_at: DATETIME
updated_at: DATETIME
Table: user_group_membership

id: INT (Primary Key)
user_id: INT (Foreign Key referencing users.id)
group_id: INT (Foreign Key referencing user_groups.id)
created_at: DATETIME
updated_at: DATETIME


Table: permissions

id: INT (Primary Key)
name: VARCHAR(255)
description: VARCHAR(255)
created_at: DATETIME
updated_at: DATETIME

Table: user_permissions

id: INT (Primary Key)
user_id: INT (Foreign Key referencing users.id)
permission_id: INT (Foreign Key referencing permissions.id)
created_at: DATETIME
updated_at: DATETIME

Table: group_permissions

id: INT (Primary Key)
group_id: INT (Foreign Key referencing user_groups.id)
permission_id: INT (Foreign Key referencing permissions.id)
created_at: DATETIME
updated_at: DATETIME

Table: audit_logs

id: INT (Primary Key)
user_id: INT (Foreign Key referencing users.id)
action: VARCHAR(255)
description: VARCHAR(255)
created_at: DATETIME
updated_at: DATETIME

Table: sessions

id: INT (Primary Key)
user_id: INT (Foreign Key referencing users.id)
token: VARCHAR(255)
expires_at: DATETIME
created_at: DATETIME

Table: password_resets

id: INT (Primary Key)
user_id: INT (Foreign Key referencing users.id)
token: VARCHAR(255)
expires_at: DATETIME
created_at: DATETIME

Table: user_attributes

id: INT (Primary Key)
user_id: INT (Foreign Key referencing users.id)
attribute_name: VARCHAR(255)
attribute_value: VARCHAR(255)
created_at: DATETIME
updated_at: DATETIME

Table: group_roles

id: INT (Primary Key)
group_id: INT (Foreign Key referencing user_groups.id)
role_id: INT (Foreign Key referencing roles.id)
created_at: DATETIME
updated_at: DATETIME

Table: group_attributes

id: INT (Primary Key)
group_id: INT (Foreign Key referencing user_groups.id)
attribute_name: VARCHAR(255)
attribute_value: VARCHAR(255)
created_at: DATETIME
updated_at: DATETIME

Table: mfa_methods

id: INT (Primary Key)
name: VARCHAR(255) (e.g., "SMS", "Email", "Authenticator App")
created_at: DATETIME
updated_at: DATETIME

Table: user_mfa_methods

id: INT (Primary Key)
user_id: INT (Foreign Key referencing users.id)
mfa_method_id: INT (Foreign Key referencing mfa_methods.id)
mfa_identifier: VARCHAR(255) (e.g., Phone number for SMS, Email address for Email)
is_verified: BOOLEAN (Flag to indicate if the MFA method is verified)
created_at: DATETIME
updated_at: DATETIME

Table: user_mfa_tokens

id: INT (Primary Key)
user_id: INT (Foreign Key referencing users.id)
mfa_method_id: INT (Foreign Key referencing mfa_methods.id)
token: VARCHAR(255) (The MFA token generated and sent to the user)
expires_at: DATETIME (Expiration time for the token)
created_at: DATETIME
updated_at: DATETIME

We can manage multi-factor authentication for users in the User Management system. Here's a brief explanation of each table:

mfa_methods: This table stores the available multi-factor authentication methods, such as "SMS," "Email," or "Authenticator App."

user_mfa_methods: This table associates users with their selected MFA methods. It includes information about the MFA identifier (e.g., phone number or email address) and whether the method is verified.

user_mfa_tokens: This table stores the temporary MFA tokens generated and sent to users during the authentication process. It includes an expiration time for the tokens.


---

## APIs


Create User

HTTP Method: POST
API Path: /users
Request Body: User object (containing user details)
Response Body: Created User object or success message

Get User by ID

HTTP Method: GET
API Path: /users/{id}
Request Body: None
Response Body: User object

Update User

HTTP Method: PUT or PATCH
API Path: /users/{id}
Request Body: Updated User object
Response Body: Updated User object or success message

Delete User

HTTP Method: DELETE
API Path: /users/{id}
Request Body: None
Response Body: Deleted User object or success message
Get All Users

HTTP Method: GET
API Path: /users
Request Body: None
Response Body: List of User objects

Assign Role to User

HTTP Method: POST
API Path: /users/{id}/roles
Request Body: Role object or Role ID
Response Body: Updated User object or success message

Remove Role from User

HTTP Method: DELETE
API Path: /users/{id}/roles/{roleId}
Request Body: None
Response Body: Updated User object or success message

Get User Roles

HTTP Method: GET
API Path: /users/{id}/roles
Request Body: None
Response Body: List of Role objects or Role IDs

Create User Group

HTTP Method: POST
API Path: /user-groups
Request Body: User Group object (containing group details)
Response Body: Created User Group object or success message

Get User Group by ID

HTTP Method: GET
API Path: /user-groups/{id}
Request Body: None
Response Body: User Group object

Update User Group

HTTP Method: PUT or PATCH
API Path: /user-groups/{id}
Request Body: Updated User Group object
Response Body: Updated User Group object or success message

Delete User Group

HTTP Method: DELETE
API Path: /user-groups/{id}
Request Body: None
Response Body: Deleted User Group object or success message

Get All User Groups

HTTP Method: GET
API Path: /user-groups
Request Body: None
Response Body: List of User Group objects

Add User to User Group

HTTP Method: POST
API Path: /user-groups/{groupId}/users/{userId}
Request Body: None
Response Body: Updated User Group object or success message

Remove User from User Group

HTTP Method: DELETE
API Path: /user-groups/{groupId}/users/{userId}
Request Body: None
Response Body: Updated User Group object or success message

Get User Privileges

HTTP Method: GET
API Path: /users/{id}/privileges
Request Body: None
Response Body: List of Privilege objects or Privilege IDs assigned to the user

Assign Privilege to User

HTTP Method: POST
API Path: /users/{id}/privileges
Request Body: Privilege object or Privilege ID
Response Body: Updated User object or success message

Remove Privilege from User

HTTP Method: DELETE
API Path: /users/{id}/privileges/{privilegeId}
Request Body: None
Response Body: Updated User object or success message

Create Role

HTTP Method: POST
API Path: /roles
Request Body: Role object (containing role details)
Response Body: Created Role object or success message

Get Role by ID

HTTP Method: GET
API Path: /roles/{id}
Request Body: None
Response Body: Role object

Update Role

HTTP Method: PUT or PATCH
API Path: /roles/{id}
Request Body: Updated Role object
Response Body: Updated Role object or success message

Delete Role

HTTP Method: DELETE
API Path: /roles/{id}
Request Body: None
Response Body: Deleted Role object or success message

Get All Roles

HTTP Method: GET
API Path: /roles
Request Body: None
Response Body: List of Role objects

Assign Privilege to Role

HTTP Method: POST
API Path: /roles/{roleId}/privileges
Request Body: Privilege object or Privilege ID
Response Body: Updated Role object or success message

Remove Privilege from Role

HTTP Method: DELETE
API Path: /roles/{roleId}/privileges/{privilegeId}
Request Body: None
Response Body: Updated Role object or success message

Get User by Username

HTTP Method: GET
API Path: /users/username/{username}
Request Body: None
Response Body: User object

Get User by Email

HTTP Method: GET
API Path: /users/email/{email}
Request Body: None
Response Body: User object

Change User Password

HTTP Method: PUT or PATCH
API Path: /users/{id}/password
Request Body: New password
Response Body: Success message or updated User object

Reset User Password

HTTP Method: POST
API Path: /users/{id}/password/reset
Request Body: None
Response Body: Success message or updated User object

Activate User Account

HTTP Method: POST
API Path: /users/{id}/activate
Request Body: None
Response Body: Success message or updated User object

Deactivate User Account

HTTP Method: POST
API Path: /users/{id}/deactivate
Request Body: None
Response Body: Success message or updated User object

Search Users

HTTP Method: GET
API Path: /users/search
Request Body: Search criteria (e.g., name, email)
Response Body: List of matching User objects

Get User Groups of User

HTTP Method: GET
API Path: /users/{id}/user-groups
Request Body: None
Response Body: List of User Group objects

Get Users in User Group

HTTP Method: GET
API Path: /user-groups/{groupId}/users
Request Body: None
Response Body: List of User objects

Get User's Roles and Privileges

HTTP Method: GET
API Path: /users/{id}/roles-privileges
Request Body: None
Response Body: Roles and Privileges assigned to the User
