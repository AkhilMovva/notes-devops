# Vault

## COMMANDS

```bash
# Get docker image: 
docker pull vault

# Running image: --rm
docker run -d -p 8200:8200 --restart unless-stopped --name vault-server --cap-add=IPC_LOCK -e 'VAULT_DEV_ROOT_TOKEN_ID=tdc-token' -e 'VAULT_DEV_LISTEN_ADDRESS=0.0.0.0:8200' vault

#Get IP Address: 
docker inspect vault-server | grep IPAddress

# Set environment variable: 
export VAULT_ADDR='http://172.17.0.2:8200'

# Vault CLI - Ubuntu
curl -fsSL https://apt.releases.hashicorp.com/gpg | sudo apt-key add -
sudo apt-add-repository "deb [arch=amd64] https://apt.releases.hashicorp.com $(lsb_release -cs) main"
sudo apt-get update && sudo apt-get install vault

#Authenticate to server from CLI: 
vault login

# Write secret (CLI): 
vault kv put secret/tdc tdcpassword=test1234

#Read secret (CLI): 
vault kv get secret/tdc 
```

## Python Script To Test

```bash
pip install hvac
```

```python
import hvac

client = hvac.Client(url='http://172.17.0.2:8200',token="tdc-token")
print(client.is_authenticated())
read_response = client.secrets.kv.read_secret_version(path='tdc')

print(read_response['data']['data']['tdcpassword'])
```

## INFO

- `vault info`: <https://vaultproject.io>
- `vault CLI download`: <https://vaultproject.io/downloads>
- `docker image`: <https://hub.docker.com/_/vault>
- `hvac info`: <https://hvac.readthedocs.io/en/stable/overview.html#getting-started>
