---
layout: post
title:  "Domínios customizados no Heroku com PointDNS"
description: "Há tempos uso o heroku para testar algumas ideias mas nunca havia chegado ao ponto de apontar um domínio relevante pra lá, até hoje."
cover: /assets/images/poindns-exemplo.png
categories: dicas
---

Há tempos uso o heroku para testar algumas ideias mas nunca havia chegado ao ponto de apontar um domínio relevante pra lá, até hoje.

Bom, o heroku permite que você coloque um domínio próprio(~~desde que deixe seu cartão de crédito lá~~), mesmo no plano gratuito, o problema é que como o apontamento só pode ser feito para o domínio do app no heroku(algo como *app-exemplo.herokuapp.com*), não temos como fazer apontamentos que não sejam do tipo CNAME. Para termos mais flexibilidade nas configurações de DNS o heroku oferece alguns *addons*, entre eles o PointDNS, que no plano gratuito oferece a possibilidade de várias configurações para um domínio. Então vamos lá configurar. 

## Adicionando o PointDNS ao projeto

O primeiro passo para configurar o serviço de DNS é vincular o PointDNS ao projeto. Isso pode ser feito tanto pela [página do addon no heroku](https://elements.heroku.com/addons/pointdns) quanto pela linha de comando, usando o heroku toolbelt:

```
$ heroku addons:create pointdns:developer -a nome_do_app
Creating pointdns:developer on ⬢ nome_do_app...
```

Feito isso você pode adicionar um domínio customizado pelo dashboard ou pela linha de comando:

```
$ heroku domains:add dominio-exemplo.com -a nome_do_app
$ heroku domains:add *.dominio-exemplo.com -a nome-do-app
```

Para abrir o painel do PointDNS você pode seguir usando a linha de comando:

```
$ heroku addons:open pointdns
```

O painel abrirá no seu browser padrão. Na aba "Nameservers" você pode encontrar uma lista de endereços de DNS que você pode usar para fazer o apontamento do domínio(no painel da godaddy, registro.br, etc).

A partir daí você pode ir na aba "Domains" e selecionar o seu domínio criado nos passo anteriores, podendo criar registros de redirect , CNAME, A e fazer configurações mais complexas. 

![poindns-exemplo](/assets/images/poindns-exemplo.png)

Bom, é isso. Até onde deu pra ver as únicas limitações do PointDNS na versão gratuita é a quantidade de domínios(apenas 1) e a quantidade de registros(no máximo 10) e resolve bem o problema que se propõe.

Abraço!