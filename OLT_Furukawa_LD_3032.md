# AUTENTICAÇÃO RADIUS - OLT FURUKAWA - LD3020 
Referência: [Helpcenter Furukawaq](file:///C:/Users/italo.bezerra.INTRA.BKTELE/Downloads/MFPC000368-User%20Manual%20GPON%20OLT%20LD3032-rev02_SOLUTIONS_ok.pdf)
## Comandos Úteis:

### Mostrando o método inicial de login
show running-config login

1. Processo de configuração:

### Ingressando no modo de configuração global:
``
en
 conf t
``
### Resumo da configuração:
```
login remote radius primary 
login remote radius enable
login radius server <ip_do_server> <senha_do_host> auth_port 1812 acct_port 1813 
    (Ex: login radius server 192.168.0.x aa47asMO1662s auth_port 1812 acct_port 1813)
login radius interface management
```
## Comandos Úteis:

### Mostrando o método inicial de login
show running-config login
