---
layout: post
title:  "Não somos tão bons assim"
categories: projetos  python  c#
---
Há alguns meses precisei desenvolver uma aplicação para um dispositivo da motorola que roda Windows CE 5. Conversei com um amigo que já tinha experiência com esses dispositivos e ele me falou que havia uma versão do python e do tk que rodava perfeitamente, logo optei por desenvolver dessa forma. Era perfeito, trabalho com python há bastante tempo e resolveria o problema rapidamente.

Aplicação desenvolvida e testada, encomendamos mais dispositivos e após meses de espera fui colocar a aplicação para rodar, aí começou o problema. Os novos dispositivos vieram atualizados com Windows CE 6.0 e o python só rodava até a versão 5.x. Todo o trabalho foi por água a baixo.

Pesquisei alternativas e aí tive que desenvolver a aplicação utilizando C#, no .Net Compact Framework e DB4O para o banco de dados. Tentamos fazer a implantação por duas vezes e falhamos. Não deu erro, não tinha bugs(até tinha, mas não veio ao caso) e tudo havia sido implementado do "jeito certo". Então qual o problema? Bom, entendíamos bastante de tecnologia, conversamos bastante sobre o problema que o cliente precisava resolver, mas não entendíamos como as coisas funcionavam no dia-a-dia da empresa, e isso fez com que todo o trabalho fosse em vão e com quem precisássemos reescrever a aplicação com base na experiência que tivemos nas tentativas anteriores.

Com isso aprendi algumas coisas que são interessantes de serem apontadas, acredito que pelo menos uma vez você já tenha passado por algo parecido.

## Nem tudo é o que parece

Essa foi minha primeira experiência com um aplicativo desktop, trabalho com web há muito tempo e achei que não poderia ser tão diferente. Nunca me enganei tanto. Os problemas são diferentes, suportar 10mil req/s não é a mesma coisa que 10mil pessoas numa loja, por exemplo.

## Se você não é especialista no assunto, não suponha, teste

Em alguns casos achei que uma requisição pela rede wifi demoraria demais e no fim das contas se mostrou mais rápido que fazer a leitura em disco(sério). Errei em não estudar a fundo o dispositivo usado e tivemos vários problemas desse tipo.

## Conheça bem os pontos de falha

O dispositivo é específico para coleta de dados(leitura de código de barras) e em vários casos a leitura demorou mais do que a checagem(no banco). Isso não passou pela minha cabeça em momento algum, afinal, o dispositivo é feito para isso, e como é de se imaginar, não nos preparamos para esse problema.

## Ambiente de implantação

Numa aplicação desktop, onde praticamente toda a infraestrutura é local, é importantíssimo que se conheça muito bem os possíveis problemas. Essa foi a maior das falhas. Quando trabalhamos com web, é muito fácil reproduzir o ambiente dos usuários do sistema, há inúmeras ferramentas e geralmente ter várias versões dos principais browsers já cobre 90% dos casos. No caso dessa aplicação local, fazer suposições e testes com o que imaginávamos do cenário da implantanção foi longe de ser uma boa ideia.

## Converse com quem entende

Mesmo conhecendo profissionais que já fizeram esse tipo de projeto, não nos preocupamos em perguntar mais sobre como funciona, quais os problemas comuns ou quais as melhores abordagens. Poderíamos ter poupado muito tempo e esforço se tivéssemos nos atentado para isso.

----

Dá pra resumir tudo isso em excesso de confiança e inexperiência. Agora é recolher os cacos e transformar tudo isso em experiência, até porque no fim das contas não somos tão bons assim.
