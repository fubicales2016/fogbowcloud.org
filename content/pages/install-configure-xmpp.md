Title: Install and configure XMPP
url: install-configure-xmpp
save_as: install-configure-xmpp.html
section: install-configure
index: 2

Install and configure XMPP
==========

Extensible Messaging and Presence Protocol (XMPP) é um protocolo aberto para sistemas de mensagens instantâneas, desenvolvido originalmente para mensagens instantâneas e informação de presença formalizado pelo IETF.
Esse protocolo é utilizado para a comunicação entre os componentes do FOGBOW.

## Instalação:
Nós aconselhamos usar o prosody e para instalar devemos seguir os seguintes passos.
``` shell
$ apt-get update
$ apt-get install prosody
```

## Configuração
No arquivo de configuração do prosody(/etc/prosody/prosody.cfg.lua), adicione os componentes que estarão vinculados a esse XMPP caso necessário. 
 
```bash
# Manager component
Component "manager.name"
        component_secret = "manager.password"

# Rendezvous component
# Required when your deployment use a Rendezvous own
Component "rendezvous.name"
        component_secret = "rendezvous.password"
```

## Iniciar
``` shell
$ service prosody start
```
