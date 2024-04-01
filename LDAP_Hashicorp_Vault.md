# Autenticando Hashicorp Vault com Active directory

### 1. No vault, via cli, habilite a autenticação LDAP:

```
vault auth enable ldap
```

### 2. Grave as informações do AD no arquivo de configurações:

```
vault write auth/ldap/config \
    url="ldap://teste-com-br.intranet:7389" \
    binddn="uid=bind,cn=Users,dc=teste-com-br,dc=intranet" \
    bindpass="senha@123" \
    userdn="cn=Users,dc=teste-com-br,dc=intranet" \
    userattr="uid" \
    groupdn="cn=groups,dc=teste-com-br,dc=intranet" \
    groupfilter="(&(objectClass=posixGroup)(memberUid={{.Username}}))"
```

### (Opcional) Comandos úteis para validação:

Testando usuário LDAP via CLI:

```
vault login -method=ldap username=bind
```

Lendo as configurações do vault:

```
vault read auth/ldap/config
```

Caso obtenha um erro "Vault Error, Server gave HTTP response to HTTPS client", rode o comando abaixo para configurar o cliente do Vault para conversar com o servidor de desenvolvimento.:

export VAULT_ADDR='http://127.0.0.1:8200'
