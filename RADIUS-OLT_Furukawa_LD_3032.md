# AUTENTICAÇÃO RADIUS - OLT FURUKAWA - LD3032 
### SO: NOS version 2.02 #0048

## Comandos Úteis:

#### Mostrando informações de login:
```# show running-config login```

## Processo simplificado de configuração:
```
en
 conf t
  login remote radius primary 
  login remote radius enable
  login radius server <ip_do_server> <senha_do_host> auth_port 1812 acct_port 1813 
    (Ex: login radius server 192.168.0.x aa47asMO1662s auth_port 1812 acct_port 1813)
  login radius interface management
```

## Observação:

Este vendor permite que você faça login com ambos métodos (local e radius) simultaneamente, recomendo que altere as credenciais do usuário admin, isso pode ser feito com o comando abaixo:

```passwd```



# Adicionando Host e política do client Radius no NPS (Windows server).

Utilize a ferramenta Network Policy Server para prosseguir.

1. Crie o host:
 Acesse **Radius clients and Servers** >> **RADIUS Clients**, Adicione um novo host:
 Aba Settings:
 Friendly name: (defina um nome);
 Address: (defina um IP);
 Shared secret manual: (defina uma passphrase/senha forte ou gere uma automática);

2. Crie a política:
 Acesse **Policies** >> **Network policies,** Adicione uma:
 Aba Overview:
 Policy name: (Defina um nome);
 Marque a opção "ignore user account dial-in properties";
	
Aba condition:
 Adicione um **WINDOWS GROUP** com o nome do grupo que terá acesso, ex: GG_Infraestrutura.
 Neste caso, apenas que estiver no grupo "GG_Infraestrutura" do Active Directory fará login.
 Adicione um **NAS IPv4 Address** onde o valor é o IP do host.

### Definindo permissões de acesso na OLT Furukawa LD-3032:

 Configurando o VSAs (Vendors Specific Atributes) conseguimos através do NPS informar qual o grupo local que o usuário radius vai se enquadrar, para definirmos isso, precisamos:

 1. Ter o host configurado;
 2. Ter a policy configurada;
 3. Estar logando normalmente, mesmo sem permissões administrativas;

 Acesse o Servidor NPS, vá em **POLICIES >> NETWOKR POLICIES**, selecione a policy e vá em propriedades;
 Clique na aba **SETTINGS** >> **VENDOR SPECIFIC** >> **ADD**;
 Selecione o vendor "**Cisco-AV-Pair**", adicione exatamente o attribute vaue: **"shell:roles=admin"** (sem àspas) para dar permissão administrativa.

Desta forma todos os usuários que se enquadrarem nesta política (e consequentemente nos grupos previamente selecionados) farão parte do grupo interno "admin" e poderão exercer funcões administrativas.
.
