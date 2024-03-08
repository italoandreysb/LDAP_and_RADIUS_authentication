VRP (R)  v8.190 (NE40E V800R011C10SPC100)
Modelo: HUAWEI NE40E-F1A-14H24Q

Comandos úteis
  ```display domain```
  ```display radius-server configuration group gerencia_radius```
  ```commit trial```

Habilitei os logs:
``` 
info-center source default channel logbuffer log level debugging state on
commit trial
```

Testando o radius:
```
aaa
 authentication-scheme gerencia_radius
  test-aaa peter.parker adminadmin radius-group gerencia_radius
  # Após isso cheque os logs do NPS
```

## Habilite o RADIUS:
```
radius enable
commit
```

## Crie o server group:
```
radius-server group gerencia_radius
 radius-server shared-key-cipher <senha_definida>
 radius-server authentication <ip_RADIUS_SERVER> source LoopBack1 1812 weight 0
 radius-server accounting <ip_RADIUS_SERVER> source LoopBack1 1813 weight 0
 radius-server retransmit 5 timeout 20
 radius-server user-name original
 quit
```
 Observação: o comando "undo radius-server user-name domain-included" não pode estar aplicado, caso contrário, ao definir o domínio padrão, é exibido o erro:
  Error: Configuring devices in a RADIUS server group or TACACS server template bound to the admin domain to send user names without domain names brings security risks.


## Crie os esquemas de autenticaçãoe e domínio:
```
aaa
 authentication-scheme gerencia_radius
  authentication-mode radius local
  quit

 accounting-scheme gerencia_radius
  accounting-mode radius
  accounting start-fail online
  quit
 
 authorization-scheme gerencia_radius
  authorization-mode if-authenticated

 domain gerencia_radius
  authentication-scheme gerencia_radius
  authorization-scheme gerencia_radius
  accounting-scheme gerencia_radius
  radius-server group gerencia_radius
  quit
 quit
```
## Confgure o a interface VTY:
```
user-interface vty 0 7
 authentication-mode aaa
 user privilege level 3
 history-command max-size 256
 idle-timeout 60 0
 protocol inbound ssh
```
## Defina o dominio criado como padrão:
```
aaa
 default-domain admin gerencia_radius 
```