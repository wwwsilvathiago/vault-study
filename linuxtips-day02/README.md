# Secrets Engines

Secrets engines are components which store, generate, or encrypt data. Secrets engines are incredibly flexible, so it is easiest to think about them in terms of their function. Secrets engines operate by receiving API calls that meet [this interface](https://github.com/hashicorp/vault/blob/061bc8ed2bbed49eb4f54c3a04647fbbefb13e48/sdk/framework/backend.go#L105). The calls receive data from the caller, take some action on that data, and they return a result.

Some secrets engines simply store and read data - like encrypted Redis/Memcached. Other secrets engines connect to other services and generate dynamic credentials on demand. Other secrets engines provide encryption as a service, totp generation, certificates, and much more.

Secrets engines are enabled at a "path" in Vault. When a request comes to Vault, the router automatically routes anything with the route prefix to the secrets engine. In this way, each secrets engine defines its own paths and properties. To the user, secrets engines behave similar to a virtual filesystem, supporting operations like read, write, and delete.

## Notes

* Extra reading:
  * https://www.vaultproject.io/docs/secrets