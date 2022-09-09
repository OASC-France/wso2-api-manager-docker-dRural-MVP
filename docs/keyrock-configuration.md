# WSO2 API Manager configuration for Keyrock

## Keyrock configuration

Go to Keyrock and sign in as admin

Create a new application (Home > Applications > Register)

![Application Information](/images/keyrock/create-application-step-1.png | width=600)

Enable **Json Web Token** (OAuth2 Credentials > Token types)

![Json Web Token](/images/keyrock/json-web-token.png | width=600)

## API Manager configuration

Go to the management console

### Identity provider

Create a new identity provider (Identity Providers > Add)

**Basic Information**

![Basic Information](/images/api-manager/identity-provider/basic-information.png | width=600)

**Claim Configuration > Basic Claim Configuration**

![Basic Claim Configuration](/images/api-manager/identity-provider/basic-claim-configuration.png | width=600)

**Role Configuration**

![Role Configuration](/images/api-manager/identity-provider/role-configuration.png | width=600)

**Federated Authenticators > OAuth2/OpenID Connect Configuration**

![OAuth2/OpenID Connect Configuration](/images/api-manager/identity-provider/oauth2-openid-connect-configuration.png | width=600)

**Just-in-Time Provisioning**

![Just-in-Time Provisioning](/images/api-manager/identity-provider/just-in-time-provisioning.png | width=600)

### Service provider

List identity providers (Identity Providers > List)

Edit **apim_publisher**

> You must have accessed the publisher at least once for it to appear in this list.

**Local & Outbound Authentication Configuration**

![Local & Outbound Authentication Configuration](/images/api-manager/service-providers/local-outbound-authentication-configuration.png | width=600)

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
