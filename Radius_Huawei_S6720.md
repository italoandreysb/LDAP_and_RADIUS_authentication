 
## Hablitando RADIUS client no HUAWEI S6720:

### Antes de configurarmos o RADIUS é interessante ter as credenciais de acesso console, caso não tenha, adicione uma senha para acesso futuro. Alterando a senha do console:
```
user-interface console 0
 set authentication password cipher <nova_senha>
```

### Crie o template:
```
radius-server template teste
 radius-server authentication <ip-do-NPS> 1812 weight 80
 radius-server accounting <ip-do-NPS> 1813 weight 80
```
### Exponha a senha:
``` 
 radius-server shared-key cipher <insira-senha-do-host>
 radius-server retransmit 2
 undo radius-server user-name domain-included
 quit
```
### Configure a autenticação
``` 
aaa
  authentication-scheme auth
  authentication-mode radius local
  quit

accounting-scheme teste-scheme
 accounting-mode radius
 accounting start-fail online
 quit

domain teste
  authentication-scheme auth
  accounting-scheme teste-scheme
  radius-server teste
  quit
 quit

domain teste
domain teste admin
```
### Ajustando prioridade 
```
user-interface vty 0 4
 user privilege level 15
```
### Configurando  autenticação local (Caso o host perca comunicação com o AD):
```
aaa
 local-user testeadmin password irreversible-cipher <digite a senha desejada>
 local-user testeadmin service-type ssh
 local-user testeadmin privilege level 15
 quit
```
### Checando as configurações RADIUS:
```
display radius-server configuration template intra.teste
```


## Adicionando Host e política do client Radius no NPS (Windows server).
Este procedimento é similar para a maioria dos hosts que serão adicionados, podendo conter pequenas alterações de acordo com o vendor que especifico ao fim deste tópico.


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



### Definindo permissões de acesso na OLT HUAWEI MA5800:
Prossiga apenas se o vendor for HUAWEI MA5800 caso contrário ignore esta etapa.

Antes da configuração abaixo os usuários dos grupos designados na política conseguem acessar a OLT sem permissões admnistrativas, após esta configuração os usuários terão acesso administrativos.

 Acesse o Servidor NPS, vá em **POLICIES >> NETWOKR POLICIES**, selecione a policy e vá em propriedades;
 Clique na aba **SETTINGS** >> **VENDOR SPECIFIC** >> **ADD**;
 Selecione o vendor "**Vendor-Specific**", clique em **ADD**, depoi em **ADD** novamente para adicionar o atributo;
 Selecione a opção **ENTER VALOR CODE**, digite **"2011"**, selecione **"YES, IT CONFORMS"**, clique em **CONFIGURE ATTRIBUTE**;
 Em **"VENDOR-ASSIGNED ATTRIBUTE NUMBER"**, digite **"29"**, em "ATTRIBUTE FORMAT", selecione **"Decimal"** e em **"ATTRIBUTE VALUE"**, digite **"15"**. 
 Confirme as alterações.