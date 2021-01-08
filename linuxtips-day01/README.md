# Introcuction to Vault

Vault is a tool for securely accessing secrets. A secret is anything that you want to tightly control access to, such as API keys, passwords, or certificates. Vault provides a unified interface to any secret, while providing tight access control and recording a detailed audit log.

A modern system requires access to a multitude of secrets: database credentials, API keys for external services, credentials for service-oriented architecture communication, etc. Understanding who is accessing what secrets is already very difficult and platform-specific. Adding on key rolling, secure storage, and detailed audit logs is almost impossible without a custom solution. This is where Vault steps in.

## Notes

* Extra reading:
  * https://www.vaultproject.io/docs/what-is-vault

## Commands used

* vault server -dev
* vault status
* export VAULT_ADDR='http://CIDR:PORT'
* vault list
* vault kv list
* vault kv put
  * example: vault kv put secret/giropops key=value
* vault kv get
  * example: vault kv get -version=1 secret/giropops
  * example: vault kv get -field=key secret/giropops
* vault kv metadata get 
  * example: vault kv metadata get secret/giropops
* vault kv destroy
  * example: vault kv destroy -versions 1 secret/giropops
  * note: destroyed secrets can not be restored
* vault kv delete
  * example: vault kv delete -versions 1 secret/giropops
  * note: deleted secrets can be restore with the undelete command
* vault kv undelete
  * example: vault kv undelete -versions 1 secret/giropops
# KV Secrets Engine

The kv secrets engine is used to store arbitrary secrets within the configured physical storage for Vault. This backend can be run in one of two modes. It can be a generic Key-Value store that stores one value for a key. Versioning can be enabled and a configurable number of versions for each key will be stored.

## Notes

* Extra reading:
  * https://www.vaultproject.io/docs/secrets/kv
* There can be two versions of the KV Engine:
  * Version 1: https://www.vaultproject.io/docs/secrets/kv/kv-v1
  * Version 2: https://www.vaultproject.io/docs/secrets/kv/kv-v2

## Commands used

* vault secrets disable
* vault secrets enable
  * vault secrets enable -version=2 $PATHOFSECRET