---
layout: post
title:  "Bottle: começando na web com python"
categories: python bottle
---
> Se você já tem uma experiência mais avançada em desenvolvimento web e/ou python, recomendo ler o tutorial oficial do Bottle, aqui o ritmo será bem mais lento pois o foco são iniciantes!

Trabalho com web há alguns anos, passei rapidamente por PHP e logo em seguida comecei a trabalhar com python e por consequência com Django. O início foi bem doloroso, não sei se todos tem a mesma impressão, mas acredito que python(para a web) tem uma curva de aprendizado muito grande se comparado ao PHP(que ainda é a grande porta de entrada da web), são muitas coisas a aprender para colocar um simples projeto em python no ar  enquanto em PHP tudo que se precisa fazer é "escrever um arquivinho e jogar no ftp".

Pensando nisso resolvi escrever uma série de artigos para ajudar iniciantes em desenvolvimento web a darem os primeiros passos utilizando python. Vamos lá?

## Requisitos

- Saber o básico de python (import, funções, tipos de dados...)
- Conhecer um pouco de html será bastante útil também
- Um computador com linux(pra facilitar as coisas pra mim. :)

## Configurando o ambiente

Assumindo que você esteja usando Linux e que o Python já vem por padrão na maioria das distribuições, vamos configurar nosso ambiente para depois partirmos para as explicações.

Primeiro, crie um diretório na sua home com o nome "meu_projeto" e em seguida baixe o arquivo [bottle.py](https://raw.githubusercontent.com/bottlepy/bottle/master/bottle.py) para esse diretório. Caso queria fazer isso no terminal execute:

{% highlight bash linenos %}
mkdir meu_projeto && cd meu_projeto && wget https://raw.githubusercontent.com/bottlepy/bottle/master/bottle.py
{% endhighlight %}

Com isso você já estará pronto para desenvolver seu projeto em python, utilizando o Bottle. Mais na frente aprenderemos a isolar nosso ambiente de desenvolvimento, gerir dependências e muito mais, por hora isso é mais que o suficiente.

## Hello World

Com o Bottle "instalado" no passo anterior, vamos agora escrever nossa primeira aplicação web, será um exemplo clássico de "Hello World!". Para isso, crie um arquivo "app.py" no diretório "meu_projeto".

Abra o arquivo com seu editor de código preferido e digite o seguinte:

{% highlight python linenos %}
from bottle import route, run

@route('/')
def index():
    return 'Hello World!'

run(host='localhost', port=8080)
{% endhighlight %}

Nesse pequeno trecho de código temos uma aplicação web básica. Na primeira linha temos os "imports". No python, quando precisamos buscar coisas(variáveis, funções, classes...) que estão em outros arquivos usamos essa sintaxe. Como no passo anterior fizemos download de um arquivo "bottle.py" que se encontra no mesmo diretório do nosso arquivo "app.py", podemos importar funções do módulo bottle[.py]. Nesse caso importamos as funções "route" e "run".

Na linha 3 temos a definição da "rota". Podemos chamar de rotas todos os endereços acessíveis da sua aplicação web, como "/" que é a raíz ou "/contato" que seria sua página de contato. Para definir uma rota no Bottle usamos a função "route", que importamos na primeira linha. Essa função é usada como um decorator nas funções que representam uma página da sua aplicação, o conceito de decorator é um pouco complexo e podemos deixar para depois, o importante é você entender qual o papel do "route". Note que o decorator "route" recebe um parâmetro, nesse caso "/", que significa que estamos mapeando a rota da url raíz da aplicação. Resumindo: o "route("/")" faz com que ao acessar a url raíz o Bottle invoque a função que está abaixo do "route", no caso, a função "index".

Na linha 4 temos a definição da função "index" que é a função usada para responder à url raíz da nossa aplicação. O nome "index" poderia ser trocado por qualquer outro, pois o que define a qual rota responderá essa função é o decorator "route" que está logo acima da definição dela.

Na linha 5 temos o retorno da função "index". Nesse caso estamos retornando uma string com o nosso "Hello World". Essa é fácil, né. :)

Na última linha usamos o comando "run"(importado na primeira linha) para dizer ao python "hey, executa esse código aqui!" e nele passamos dois parâmetros, o "host" que seria a url da aplicação e "port", que será a porta usada para rodar a aplicação.

Simples, não? :)

## Executando a aplicação

O último passo dessa etapa é rodar nossa aplicação, para isso execute no terminal:  

{% highlight bash linenos %}
cd meu_projeto && python app.py
{% endhighlight %}

Agora, acesse no seu navegador o endereço: [http://localhost:8080](http://localhost:8080) e veja o seu primeiro app em python rodando! \o/

## Concluindo

Esse é um exemplo básico de como desenvolver uma aplicação simples, nesse primeiro artigo abordamos alguns princípios que fazem parte de qualquer aplicação web em Bottle e nos demais daremos uma incrementada nesse projeto.

Dúvidas, críticas e sugestões nos comentários! :)

### Links úteis

- [Python para Zumbis (curso online de python)](http://pycursos.com/python-para-zumbis/)
- [Bottle - Site Oficial](http://bottlepy.org)
- [Bottle - Fontes de Estudo](https://ericstk.wordpress.com/2014/12/03/bottle-framework-fontes-de-estudos/)
- [Bottle - Desenvolvendo com Bottle - Parte 1](http://pythonclub.com.br/desenvolvendo-com-bottle-parte-1.html)