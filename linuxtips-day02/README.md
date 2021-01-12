# Secrets Engines

Secrets engines are components which store, generate, or encrypt data. Secrets engines are incredibly flexible, so it is easiest to think about them in terms of their function. Secrets engines operate by receiving API calls that meet [this interface](https://github.com/hashicorp/vault/blob/061bc8ed2bbed49eb4f54c3a04647fbbefb13e48/sdk/framework/backend.go#L105). The calls receive data from the caller, take some action on that data, and they return a result.

Some secrets engines simply store and read data - like encrypted Redis/Memcached. Other secrets engines connect to other services and generate dynamic credentials on demand. Other secrets engines provide encryption as a service, totp generation, certificates, and much more.

Secrets engines are enabled at a "path" in Vault. When a request comes to Vault, the router automatically routes anything with the route prefix to the secrets engine. In this way, each secrets engine defines its own paths and properties. To the user, secrets engines behave similar to a virtual filesystem, supporting operations like read, write, and delete.

## Notes

* Extra reading:
  * https://www.vaultproject.io/docs/secrets
  * https://www.vaultproject.io/docs/secrets/aws

## Commands used

* vault secrets enable aws
* vault secrets enable -path=gft kv
* vault secrets disable gft
* vault secrets move ssh/ karl_lab_ssh/
* vault path-help aws

# Auth Methods

Auth methods are the components in Vault that perform authentication and are responsible for assigning identity and a set of policies to a user.

Having multiple auth methods enables you to use an auth method that makes the most sense for your use case of Vault and your organization.

For example, on developer machines, the GitHub auth method is easiest to use. But for servers the AppRole method is the recommended choice.

To learn more about authentication, see the [authentication concepts page](https://www.vaultproject.io/docs/concepts/auth).

## Notes

* Extra reading:
  * https://www.vaultproject.io/docs/auth

## Commands used

* vault auth enable userpass
  * vault auth enable -path=gft_users/ userpass
* vault auth disable userpass
* vault write auth/gft_users/users/karl password=typeithere
* vault read auth/gft_users/users/karl
* vault login -method=userpass -path=gft_users username=karl password=typeithere
* vault write cubbyhole/my-secret foo=bar
* vault login token=yourtokenhere

# Policies

Everything in Vault is path based, and policies are no exception. Policies provide a declarative way to grant or forbid access to certain paths and operations in Vault. This section discusses policy workflows and syntaxes.

Policies are deny by default, so an empty policy grants no permission in the system.

## Notes

* Extra reading:
  * https://www.vaultproject.io/docs/concepts/policies

## Commands used

* vault policy write gft first-policy.yml
* 
## Policies created

* First policy:
``` json
### its a prank!
path "gft/treinamentos/vault/plataforma"{
	capabilities = ["read"]
}

### correct way:

path "gft/data/treinamentos/vault/plataforma"{
	capabilities = ["read"]
}

path "gft/*" {
  capabilities = ["list"]
}
```
* Second policy:

``` json

path "gft/data/treinamentos/+/plataforma"{
	capabilities = ["read","update", "create", "delete"]
}

path "gft/*" {
  capabilities = ["list"]
}
```
