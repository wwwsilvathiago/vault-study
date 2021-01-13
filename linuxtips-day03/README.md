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
