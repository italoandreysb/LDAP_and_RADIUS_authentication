 
## Habilitando RADIUS client no PFSense:

1. Para habilitar a autenticação RADIUS acesse o menu SYSTEM >> USER MANAGER >> AUTHENTICATION SERVERS:
 Clique em +Add;  
 Description name: (Defina um nome);  
 Type: Radius;  
 Protocol: MS-CHAPv2;  
 Hostname or IP address: (selecione o IP do NPS);  
 Shared Secret: (Defina uma senha complexa);  
 Service offered: Authentication;  
 Authentication port: 1813;  
 Athentication Timeout: 10;  
 Racius NAS IP Attrbute: (selecione a interface que aceitará as solicitações), ex: LANSERVIDORES;  
 
2. Para permitir autenticações radius acesse o menu SYSTEM >> USER MANAGER >> SETTINGS:  
 Auhtentication Server: Selecione a autenticação cadastrada acima.
 
Obs: 
 Para validar o procedimento de login pode-se utilizar do serviço de TESTE DE AUTENTICAÇÃO, ele pode ser encontrado em: DIAGNOSTICS >> AUTHENTICATION.



## Adicionando Host e política do client Radius no NPS (Windows server):
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

### Definindo permissões de acesso no PFSENSE  
Prossiga apenas se o vendor for PFSENSE, caso contrário ignore esta etapa.  

Ainda editando a política, acesse a aba SETTINGS >> STANDARD e adicionar uma classe/class com o valor/value "admins" para vincular ao grupo admin do PF Sense.   
