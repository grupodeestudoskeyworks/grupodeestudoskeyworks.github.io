---
layout: post
title: "Introdução ao Node.js"
description: "Criação de um servidor REST básico utilizando Node.js."
author: augusto_rodrigues
tags: [nodejs]
---

## Instalação

Acesse a [página de download](https://nodejs.org/en/download/) do site oficial, você pode baixar tanto a versão [LTS](https://en.wikipedia.org/wiki/Long-term_support "Long-term support"), como a versão corrente.

> A versão corrente contém funcionalidades que ainda estão sendo testadas e podem estar instáveis.

#### Linux

Para prosseguir com a instalação no Linux, a melhor opção é baixar dos repositórios oficiais de sua distribuição, você pode encontrar as instruções [aqui](https://nodejs.org/en/download/package-manager/).

#### Windows

Baixe o executável conforme sua arquitetura, após o download prossiga com o next, next, finish e estará instalado se tudo ocorrer como esperado.


Após o download você tera o node e o [npm](https://www.npmjs.com/ "Node Package Manager")*(posteriormente iremos ver o que ele é)* instalados. Verifique se ambos foram instalados corretamente através dos comandos:

{% highlight bash %}
$ node --version
v8.4.0
$ npm --version
5.4.0
{% endhighlight %}

Se aparecer **outro resultado**, que não a informação da versão algo ocorreu errado durante a instalação, tente repetir o processo de instalação. Se mesmo assim não funcionar entre em contato para podermos ajuda-lo(a).

### O que é o node?

Um tempo de execução **JavaScript** contruido em cima do [motor JavaScript V8 do Chrome](https://developers.google.com/v8/). Ele usa um modelo [**orientado a eventos**](https://pt.wikipedia.org/wiki/Programa%C3%A7%C3%A3o_orientada_a_eventos) sem bloqueio de I/O, o que o torna leve e eficiente. [npm](https://www.npmjs.com/), o ecosistema de pacotes do Node.js, é o maior ecosistema de bibliotecas de código aberto no mundo.

### Hello World

Crie a pasta `hello-word`:

{% highlight bash %}
$ mkdir hello-world
{% endhighlight %}

Crie o arquilo `hw.js` dentro da pasta `hello-world`:

{% highlight bash %}
$ cd hello-world
$ touch hw.js
{% endhighlight %}

Abra o arquivo `hw.js` com seu editor de código preferido e digite o seguitne comando:

{% highlight javascript %}
console.log('Hello World!')
{% endhighlight %}

> Caso você não possua um editor de código, você pode baixar o VS Code [aqui](https://code.visualstudio.com/).

Salve o arquivo e execute o commando:

{% highlight bash %}
$ node hw.js
Hello World!
{% endhighlight %}

### Criando um servidor HTTP

Crie a pasta `http-server`:

{% highlight bash %}
$ mkdir http-server
{% endhighlight %}

Crie o arquilo `index.js` dentro da pasta `http-server`:

{% highlight bash %}
$ cd http-server
$ touch index.js
{% endhighlight %}

Abra o arquivo `index.js` e copie o seguinte código:

{% highlight javascript %}
// importação do módulo http, que faz parte do núcleo do node
const http = require('http')  

const port = 3000

// função para tratar todas as requisições recebidas
const requestHandler = (request, response) => {  
  console.log(request.url)
  // envia a resposta 200 OK, com a mensagem 'Hello Node.js Server!'
  response.end('Hello Node.js Server!')
}

const server = http.createServer(requestHandler)

server.listen(port, (err) => {  
  // se ocorrer algum erro ao iniciar o servidor, este callback será chamado
  // o erro mais comum que pode acontecer é a porta já estar em uso
  if (err) {
    return console.log('something bad happened', err)
  }

  console.log(`servidor escutando na porta ${port}`)
})
{% endhighlight %}

Salve o arquivo e execute o commando:

{% highlight bash %}
$ node index.js
{% endhighlight %}