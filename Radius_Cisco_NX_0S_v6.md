## Hablitando RADIUS client no CISCO:
SO: NX-OS, v6.0  
Antes de iniciar a configração, certifique-se de que o acesso via console está funcional.

Show running-config
```
radius-server key 7 "SenhaAleatoria"                                                 # Altere a senha entre áspas
radius-server host 192.168.X.X key 7 "SenhaDoHost" authentication accounting         # Altere para ip do NPS e senha cadastrada no server
#
radius-server key 7 "SenhaAleatoria"
radius-server timeout 5
radius-server retransmit 1
radius-server deadtime 0
 ### utilize apenas caracteres alfanumericos abaixo
radius-server host 192.168.X.X key 0 "AlterarSenha" auth-port 1812 acct-port 1813 authentication accounting           # Altere para ip do NPS e senha cadastrada no server
```
show running-config radius all
```
aaa group server radius radius 
    server 192.168.X.X         # Altere para o IP do servidor (NPS)
    deadtime 0
    use-vrf default
    no source-interface

ip radius source-interface mgmt0          # altere a "mgmt0" para a interface de comunicação com o radius desejada.
```
show running-config aaa all 
```
logging level aaa 5
aaa authentication login default group radius local 
aaa authentication login console local 
aaa authorization ssh-publickey default local 
aaa authorization ssh-certificate default local 
aaa accounting default local 
aaa user default-role 
aaa authentication login default fallback error local 
aaa authentication login console fallback error local 
aaa authentication login error-enable
```
Após isso, valide as configurações e posteriormente salve as alterações.


### Definindo permissões para CISCO NX-OS no Servidor NPS:
Prossiga apenas se o vendor for CISCO, NX-OS caso contrário ignore esta etapa.

 Configurando o VSAs (Vendors Specific Atributes) conseguimos através do NPS informar qual o grupo local que o usuário radius vai se enquadrar, para definirmos isso, precisamos:

 1. Ter o host configurado;
 2. Ter a policy configurada;
 3. Estar logando normalmente, mesmo sem permissões administrativas;

 Acesse o Servidor NPS, vá em **POLICIES >> NETWOKR POLICIES**, selecione a policy e vá em propriedades;
 Clique na aba **SETTINGS** >> **VENDOR SPECIFIC** >> **ADD**;
 Selecione o vendor "**Cisco-AV-Pair**", adicione exatamente o attribute vaue: **"shell:roles=network-admin"** (sem àspas) para dar permissão administrativa.

Desta forma todos os usuários que se enquadrarem nesta política (e consequentemente nos grupos previamente selecionados) farão parte do grupo interno "network-admin" e poderão exercer funcões administrativas.

Referência: [Cisco.com](https://www.cisco.com/c/en/us/td/docs/switches/datacenter/nexus9000/sw/6-x/security/configuration/guide/b_Cisco_Nexus_9000_Series_NX-OS_Security_Configuration_Guide/b_Cisco_Nexus_9000_Series_NX-OS_Security_Configuration_Guide_chapter_0111.html)

