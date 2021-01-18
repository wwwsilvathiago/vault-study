# Vault Server

Installing Vault again, but with the intention of running in prod (not -dev)

Outside of development mode, Vault servers are configured using a file. The format of this file is HCL or JSON. An example configuration is shown below:

After the configuration is written, use the -config flag with vault server to specify where the configuration is.

## Notes

* Extra reading:
  * https://www.vaultproject.io/docs/configuration

## Commands and setup used

* mkdir /var/log/vault
* mkdir /var/lib/vault/data
* ln -s /var/log/vault/ /var/lib/vault/logs
* mkdir /etc/vault
* vim /etc/vault/config.json
  * see file in this folder
* useradd -r vault
  * -r, --system > create a system account
* chown -Rv vault:vault /var/lib/vault
  * changes owner of the /var/lib/vault dir
* chown -Rv vault:vault /etc/vault
* chown -Rv vault:vault /var/log/vault/
* vim /etc/systemd/system/vault.service
  * see file in this folder
  * whenever you alter this configuration, run:
    * systemctl daemon-reload
* systemctl enable vault.service
  * enable service
* systemctl status vault
  * check if service is running
* systemctl start vault
  * start vault service
* journalctl -u vault.service
  * to check logs
* export VAULT_ADDR=http://127.0.0.1:8210
  * setting a local vault address
  * echo "export VAULT_ADDR='http://127.0.0.1:8210'" >> /root/.bashrc
    * To automatically set the vault address
* vault operator init
  * Initialize a new Vault cluster
  * Outputs unseal tokens
* vault operator unseal ${token}
  * Uses tokens received on the init operation to unseal the vault
* export VAULT_TOKEN=${roottoken/usertoken}
* vault secrets enable -version=2 -path=gft kv

# New Root Token

Previously, we ran the server on default configurations, at this point we'll erase all files from /var/lib/vault/data so that we can start over, and generate new tokens and set a different -key-shares and -key-threshold parameter

The operator generate-root command generates a new root token by combining a quorum of share holders. With the -dr-token or -recovery-token options, it can generate a DR operational token or a recovery token in the same way.

## Notes

* Extra reading:
  * https://www.vaultproject.io/docs/commands/operator/generate-root
  * https://learn.hashicorp.com/tutorials/vault/generate-root

## Commands used

* sudo systemctl stop vault
  * to start the process, we first stop the current vault service
* rm -rf /var/lib/vault/data/*
  * remove current data
* sudo systemctl start vault
  * at this point, we want to remove old data, so we can start over
* systemctl start vault
  * starting the service again
* vault operator init -key-shares=3 -key-threshold=2 
  * redefines amount of key-shares (used to unseal) and threshold to unseal 
* vault operator generate-root
  * Generates a new root token by combining a quorum of share holders.
* vault operator generate-root -init
  * *it's important to keep the data output*
* vault operator generate-root -nonce {nonce-value-here}
  * it's important to keep the encoded token output data
* vault operator generate-root -decode {encoded-token-value} -otp {otp-from-the-init-operation}
  * decrypts the token created by the generate-root operation
  * the new root token, doesn't disable the initial root token

# Rekey

The operator rekey command generates a new set of unseal keys. This can optionally change the total number of key shares or the required threshold of those key shares to reconstruct the master key. This operation is zero downtime, but it requires the Vault is unsealed and a quorum of existing unseal keys are provided.

## Notes

* Extra reading:
  * https://www.vaultproject.io/docs/commands/operator/rekey
  * https://en.wikipedia.org/wiki/Cryptographic_nonce
  * https://www.vaultproject.io/docs/internals/rotation

## Commands used

* vault operator rekey - init -key-shares 5 -key-threshold 3
  * it's important to keep the data output
* vault operator rekey -nonce {nonce-value-output-from-above-command}
  * outputs a new set of keyshares, as usual, it's important to keep it