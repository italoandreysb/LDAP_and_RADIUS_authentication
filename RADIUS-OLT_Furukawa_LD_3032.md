# AUTENTICAÇÃO RADIUS - OLT FURUKAWA - LD3020 
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
## Comandos Úteis:

Referência: [Helpcenter Furukawa](file:///C:/Users/italo.bezerra.INTRA.BKTELE/Downloads/MFPC000368-User%20Manual%20GPON%20OLT%20LD3032-rev02_SOLUTIONS_ok.pdf)