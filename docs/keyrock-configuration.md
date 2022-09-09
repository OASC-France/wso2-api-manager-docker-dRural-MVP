# WSO2 API Manager configuration for Keyrock

## Keyrock configuration

Go to Keyrock and sign in as admin

Create a new application (Home > Applications > Register)

<img src="../images/keyrock/create-application-step-1.png" alt="Application Information" width="600"/>

Enable **Json Web Token** (OAuth2 Credentials > Token types)

<img src="../images/keyrock/json-web-token.png" alt="Json Web Token" width="600"/>

## API Manager configuration

Go to the management console

### Identity provider

Create a new identity provider (Identity Providers > Add)

**Basic Information**

<img src="../images/api-manager/identity-provider/basic-information.png" alt="Basic Information" width="600"/>

**Claim Configuration > Basic Claim Configuration**

<img src="../images/api-manager/identity-provider/basic-claim-configuration.png" alt="Basic Claim Configuration" width="600"/>

**Role Configuration**

<img src="../images/api-manager/identity-provider/role-configuration.png" alt="Role Configuration" width="600"/>

**Federated Authenticators > OAuth2/OpenID Connect Configuration**

<img src="../images/api-manager/identity-provider/oauth2-openid-connect-configuration.png" alt="OAuth2/OpenID Connect Configuration" width="600"/>

**Just-in-Time Provisioning**

<img src="../images/api-manager/identity-provider/just-in-time-provisioning.png" alt="Just-in-Time Provisioning" width="600"/>

### Service provider

List identity providers (Identity Providers > List)

Edit **apim_publisher**

> You must have accessed the publisher at least once for it to appear in this list.

**Local & Outbound Authentication Configuration**

<img src="../images/api-manager/service-providers/local-outbound-authentication-configuration.png" alt="Local & Outbound Authentication Configuration" width="600"/>

### Roles

Create roles (Users and Roles > Add > Add New Role)

### Configuration file

wso2am-4.1.0/repository/conf/deployment.toml

```
[user_store]
…
username_java_script_regex = '(.*?)'
username_java_regex = '(.*?)'
```
