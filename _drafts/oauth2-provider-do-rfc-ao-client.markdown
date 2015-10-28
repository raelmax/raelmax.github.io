---
layout: post
title: Introdução ao oAuth2
cover: /assets/images/oauth-intro.jpg
categories: series oauth2
---

O oAuth2 é um padrão de autenticação que permite aplicações de terceiros terem
acesso a dados de um usuário(ou seus próprios dados) em um determinado serviço
do protocolo HTTP. Atualmente esse é o padrão usado para integração de aplicações
nos mais diversos serviços, como quando criamos uma aplicação que se comunica
com o Facebook ou quando implementamos alguma aplicação que utilize a API da
DigitalOcean, por exemplo.

## Que problema o oAuth resolve?

O foco do oAuth2 é a autenticação de aplicações de terceiros. Em um modelo de
autenticação comum, cliente-servidor, caso o usuário precisa-se dar acesso a
seus dados de determinada aplicação à uma aplicação de terceiros, o processo
seria basicamente esse:

- Usuário compartilha seus dados de acesso(usuário e senha) com a aplicação terceira;
- A aplicação terceira requisita os dados a aplicação principal usando os dados
de acesso do usuário.

Esse modelo apresenta alguns problemas, como:

- A necessidade da aplicação terceira armazenar os dados de acesso do usuário;
- A aplicação terceira teria acesso a tudo que o usuário tem no sistema, sem
restrições;
- Caso o usuário use mais de uma aplicação terceira, a única forma de remover
o acesso de uma aplicação aos seus dados seria mudando os dados de acesso,
afetando todas as outras aplicações.

## Como ele resolve?

Para resolver esses problemas o oAuth propõe uma camada de autenticação, com
fluxo bem definido e que evita o acesso direto aos dados do usuário.

[falta mais explicações]

### Roles
