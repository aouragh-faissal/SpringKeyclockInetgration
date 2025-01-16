# Integrating Keycloak with Spring Boot

This guide demonstrates how to integrate Keycloak with a simple Spring Boot RESTful application to manage user authentication and authorization for **user** and **admin** roles.

## Prerequisites

1. Keycloak server installed and running. (in our case we will run a keycloak container via docker)
2. Keycloak Admin Console access.
3. Java Development Kit (JDK) 17 or later.
4. Maven or Gradle build tool installed.
5. Basic understanding of Spring Boot and RESTful APIs.

## Setting Up Keycloak

From a terminal, enter the following command to start Keycloak:

> docker run -p 8080:8080 -e KC_BOOTSTRAP_ADMIN_USERNAME=admin -e KC_BOOTSTRAP_ADMIN_PASSWORD=admin quay.io/keycloak/keycloak:26.1.0 start-dev

in your browser log into the Admin Console. [link] (http://localhost:8080) *username admin and password admin*

### Create a Realm:
Create a new realm (e.g., demo-realm). the default one is Master

### Create a Client:
In your realm, create a client (e.g., spring-boot-app).
Set the client protocol to openid-connect.
Set the access type to public access.
Make this URLs Configuration

- Root URL: http://localhost:8081
- Home URL: http://localhost:8081
- Redirect URL: http://localhost:8081/*
- Post Logout: http://localhost:8081
- Web Origins: *
  
### Define Roles:
Navigate to the Roles tab in the Keycloak Admin Console.
Create realm roles user and admin.

### Create Users:
Navigate to the Users tab and create users.
in the credentials pane enter the user password.
Create client roles client_user and client_admin.
within the realm roles, you can associate the realm admin role with the client_admin role of the client.

> Realm roles are defined at the realm level. They are global and can be used across the entire realm, regardless of a specific client.
Client roles are defined at the level of a specific client within the realm. They only apply to that particular client's resources or permissions.

Assign roles (client_user or client_admin) to each user under the "Role Mappings" tab.

## Well-known Endpoint

Keycloak exposes a set of endpoints that applications and services can use to authenticate and authorize their users. 
The most important endpoint to understand is the well-known configuration endpoint. 
It lists endpoints and other configuration options relevant to the OpenID Connect implementation in Keycloak. 
The endpoint is:
http://localhost:8080/realms/{realm-name}/.well-known/openid-configuration
in the response you will find a token-endpoint attribute, copy the link into postman.
launch a post request with this information in the body of the request (type x-www-form-urlencoded):

![generate token](https://github.com/user-attachments/assets/0d433596-594b-4d3c-9fc1-cd1a3730a697)

In the response you will find a token *token-access* that you can use to access the client:

## Testing

Run the Spring boot Application
Use tools like Postman or curl to access the secured endpoints:

- /api/v1/demo/admin – Accessible only by client_admin.
- /api/v1/demo/user – Accessible only by client_user.

Add the Bearer token from Keycloak to the Authorization header in your requests.

![demo admin](https://github.com/user-attachments/assets/f178f35f-062d-482f-925c-7e55ee2c6b87)

## Happy Coding!







