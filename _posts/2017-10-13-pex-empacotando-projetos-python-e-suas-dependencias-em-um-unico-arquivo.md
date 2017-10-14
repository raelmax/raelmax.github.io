---
layout: post
title:  "PEX: Empacotando projetos python e suas dependências em um único arquivo"
description: "Uma das grandes vantagens das linguagens compiladas é a geração de um único arquivo binário com todas as dependências do seu projeto, facilitando a distribuição e deploy. Com o PEX podemos criar algo um pouco parecido mas para pacotes python, uma espécie de 'virtualenv zipado'."
categories: python
---

Antes de começar a falar do PEX é importante conhecer um comportamento pouco explorado do interpretador do Python. Para isso vamos criar um arquivo de exemplo com o seguinte conteúdo e chamá-lo de `teste.py`:
{% highlight python linenos %}
print("Tudo ok! Hello World!")
{% endhighlight %}

O código pode ser executado com o comando `python teste.py`. Mas para o nosso exemplo tentaremos executá-lo assim:
{% highlight bash linenos %}
~/Code/me/pex $ python .
{% endhighlight %}

Mas receberemos um erro que diz que não existe um módulo "main" no diretório atual, o que é verdade já que nosso único o arquivo se chama `teste.py`.
{% highlight bash linenos %}
/Users/raelmax/.pyenv/versions/2.7.13/bin/python: can't find '__main__' module in '.'
{% endhighlight %}

Nesse momento você já deve ter percebido qual é o tal comportamento pouco explorado. Se mudarmos o nome do nosso arquivo de `teste.py` para `__main__.py` ao executarmos o comando `python .` o arquivo `__main__.py` será executado, exemplo:

{% highlight bash linenos %}
~/Code/me/pex $ mv teste.py __main__.py
~/Code/me/pex $ python .
Tudo ok! Hello World!
{% endhighlight %}

Indo um pouco além podemos "zipar" esse arquivo `__main__.py` e o comportamento do interpretador não muda:
{% highlight bash linenos %}
~/Code/me/pex $ zip teste.zip __main__.py
  adding: __main__.py (stored 0%)
~/Code/me/pex $ python teste.zip
Tudo ok! Hello World!
{% endhighlight %}

Podemos também criar um diretório contendo nosso arquivo `__main__.py` e executá-lo da mesma forma:
{% highlight bash linenos %}
~/Code/me/pex $ mkdir teste
~/Code/me/pex $ mv __main__.py teste/
~/Code/me/pex $ python teste
Tudo ok! Hello World!
{% endhighlight %}

Esses comportamentos são padrões do interpretador do python, foram melhorados com a [PEP 441](https://www.python.org/dev/peps/pep-0441/) e o PEX usa para criar ambientes python independentes, que é o nosso objetivo.

### O que é o PEX

O PEX é o responsável por gerar arquivos independentes que contém um ambiente python completo, como um virtualenv. Ao gerar um arquivo PEX ele cuida dos detalhes necessários para criar um arquivo "zip" que atenda aos padrões do interpretador do python, adicionando um arquivo `__main__.py` e o header `#!/usr/bin/env python`.

A documentação do projeto é bem completa e dá mais detalhes sobre como tudo funciona, você pode conferir aqui: [https://pex.readthedocs.io/](https://pex.readthedocs.io/en/stable/index.html).

### Instalando o PEX

A instalação é feita usando o pip, como qualquer outro pacote python:
{% highlight bash linenos %}
~/Code/me/pex $ pip install pex
{% endhighlight %}

Eu encontrei um pequeno probleminha nessa abordagem de instalação porque ela requer que você tenha um virtualenv ativo ou permissão para instalar como um pacote global. Para conseguir fugir desses dois pontos eu fiz um pequeno script que baixa, instala no `/tmp/` e cria um executável PEX usando o próprio PEX:
{% highlight bash linenos %}
~/Code/me/pex $ curl -L https://bit.ly/pex-standalone | bash
{% endhighlight %}

Caso você tenha usado a primeira abordagem você terá disponível o comando `pex`, se você optou por usar o meu script você precisará executar com `./pex`. Nas dicas a seguir eu usarei o arquivo gerado pelo meu script.

### Ambientes virtuais temporários

Com o PEX instalado podemos criar ambientes virtuais tanto temporários quanto permanentes. Para exemplificar vamos criar um ambiente virtual com a biblioteca [requests](http://docs.python-requests.org/en/master/):
{% highlight bash linenos %}
~/Code/me/pex $ ./pex -v requests
  urllib3 1.22pex :: Resolving distributions
  requests 2.18.4
  certifi 2017.7.27.1
  chardet 3.0.4
  idna 2.6
pex: Building pex: 1249.6ms
pex:   Resolving distributions: 1243.7ms
Running PEX file at /var/folders/lp/bl8845jd6td_f_f99jfm034r0000gn/T/tmpQvgSXG with args []
pex: PEX.run invoking /Users/raelmax/.pyenv/versions/2.7.13/bin/python2.7 /var/folders/lp/bl8845jd6td_f_f99jfm034r0000gn/T/tmpQvgSXG
Python 2.7.13 (default, Oct 10 2017, 14:59:59)
[GCC 4.2.1 Compatible Apple LLVM 9.0.0 (clang-900.0.37)] on darwin
Type "help", "copyright", "credits" or "license" for more information.
(InteractiveConsole)
>>> import requests
>>> r = requests.get('https://raelmax.github.io/')
>>> r.status_code
200
{% endhighlight %}

Com esse comando o PEX executa o interpretador do python com o pacote especificado no PYTHONPATH, o que pode ser bem útil no momento do desenvolvimento. Caso o pacote que você queria instalar possua um executável você pode usar a flag `-c` para dizer para o PEX executá-lo. Vamos usar o [thumbor](http://thumbor.org) nesse exemplo:
{% highlight bash linenos %}
~/Code/me/pex $ ./pex thumbor -c thumbor -- --version
Thumbor v6.3.2 (10-Apr-2017)
{% endhighlight %}

O `--` é usado para dizer para o PEX que os próximo parâmetros serão passados para o programa que você está executando(no caso, o thumbor) e não para o próprio PEX.

### Salvando ambientes PEX

Apesar de ser útil durante o desenvolvimento, os ambientes temporários não são tão interessantes quando você costumar usar sempre os mesmos softwares, nesse caso é interessante persistir o seu ambiente. Para esse exemplo vamos usar o [httpie](https://httpie.org/):
{% highlight bash linenos %}
~/Code/me/pex $ ./pex httpie -c http -o httpie.pex
~/Code/me/pex $ ./httpie.pex --version
0.9.9
~/Code/me/pex $ ./httpie.pex --print h https://raelmax.github.io
HTTP/1.1 200 OK
Accept-Ranges: bytes
Access-Control-Allow-Origin: *
Age: 0
Cache-Control: max-age=600
Connection: keep-alive
Content-Encoding: gzip
Content-Length: 3055
Content-Type: text/html; charset=utf-8
Date: Sat, 14 Oct 2017 14:15:29 GMT
Expires: Sat, 14 Oct 2017 14:11:09 GMT
Last-Modified: Thu, 22 Sep 2016 22:55:15 GMT
Server: GitHub.com
Vary: Accept-Encoding
Via: 1.1 varnish
X-Cache: MISS
X-Cache-Hits: 0
X-Fastly-Request-ID: ce8dfb821c7e25940ebd20819c14f602f828558d
X-GitHub-Request-Id: BC2E:18C4E:826E725:BD29B8D:59E218A4
X-Served-By: cache-gru17128-GRU
X-Timer: S1507990529.067934,VS0,VE142
{% endhighlight %}

### Conclusão

O PEX é uma ótima alternativa para quem quer testar um novo pacote sem "sujar" o ambiente de desenvolvimento do seu projeto e também para quem  usa projetos python que contém scripts no dia a dia, como o thumbor, httpie e outros. Nesse segundo caso a grande vantagem é não precisar ter um virtualenv apenas para esses projetos e ter que ficar alternando entre o virtualenv do projeto e o virtualenv que contém esses scripts, nem mesmo "sujar" sua instalação global do Python.

Uma última dica é criar um diretório para abrigar todos esses executáveis e colocar no seu `PATH`, com isso você não precisará chamar o executável com o `PATH` completo e poderá fazer isso de dentro do seu projeto mesmo. Para isso eu criei um diretório `~/.pybin` e coloquei lá o arquivo `pex` gerado pelo meu script e sempre que crio um executável novo também mando pra lá. E adicionei esse diretório ao meu `.bash_profile`:

{% highlight bash linenos %}
~/Code/me/pex $ echo 'export PATH="~/.pybin:$PATH"' > ~/.bash_profile
{% endhighlight %}

É isso! :)

- [PEX](https://pex.readthedocs.io)
- [WTF is PEX](https://www.youtube.com/watch?v=NmpnGhRwsu0)
