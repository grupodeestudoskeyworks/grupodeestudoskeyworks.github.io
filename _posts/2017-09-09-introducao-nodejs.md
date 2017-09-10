---
layout: post
title: "Introdução ao Node.js"
description: "Criação de um servidor REST básico utilizando Node.js."
author: augusto_rodrigues
tags: [nodejs, npm, express]
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

// se ocorrer algum erro ao iniciar o servidor este evento será invocado
server.on('error', (e) => {
  if (e.code === 'EADDRINUSE') {
    console.log(`A porta ${port} já está em uso`)
  } else {
    console.error('Ocorreu um erro ao tentar iniciar o servidor', e)
  }
});

server.listen(port, (err) => {
  console.log(`servidor escutando na porta ${port}`)
})

{% endhighlight %}

Salve o arquivo e execute o commando:

{% highlight bash %}
$ node index.js
servidor escutando na porta 3000
{% endhighlight %}

Abra o browser e acesse o endereço `http://localhost:3000/`, a mensagem "`Hello Node.js Server!`" deverá aparecer na tela.

O módulo **http** é bem baixo nível. Construir uma aplicação web complexa utilizando o código acima iria consumir muito tempo. Este é o motivo pelo qual normalmente escolhemos um framework para abstrair tais comportamentos. Existem vários cujo você pode escolher, aqui estão os mais usados:

  * [express](http://expressjs.com/)
  * [hapi](https://hapijs.com/)
  * [koa](http://koajs.com/)
  * [restify](http://restify.com/)

Para o nosso servidor utilizaremos o express, por ser o mais comum.

### Inicializano o projeto

Dentro da pasta `http-server` execute o comando:

{% highlight bash %}
$ npm init -y
{% endhighlight %}

Isso irá adicionar o **arquivo de configuração** básico do npm, o `package.json`.
Ele possui a seguinte estrutura:

{% highlight json %}
{
  "name": "http-server",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "keywords": [],
  "author": "",
  "license": "ISC"
}
{% endhighlight %}

  * **name**: Nome do projeto, segue o padrão camel-dashed-case.
  * **version**: Versão do pacote (este projeto é considerado um pacote do npm ainda não publicado).
  * **description**: Breve descrição sobre o que se trata o projeto.
  * **main**: Ponto de partida de sua aplicação (somente um referencial, não é utilizado pelo node).
  * **scripts**: Aqui você pode informar qualquer script que desejar e executá-lo pelo comando `npm run <nome-do-script>`, ele irá rodar como bash a partir do mesmo diretório que o arquivo `package.json`.

  > O script `start` é uma exeção, ele pode ser executado simplesmente como `npm start`.

### Instalando o Express

Dentro da pasta `http-server` execute:

{% highlight bash %}
$ npm install express --save
{% endhighlight %}

O comando `npm install` instala todos os módulos informados no diretório atual dentro da pasta node_modules. A flag `--save` diz para o npm adicionar a informação da dependência no `package.json`:

{% highlight json %}
{
  "name": "http-server",
  "version": "1.0.0",
  ...
  "dependencies": {
      "express": "^4.15.4"
  }
}
{% endhighlight %}

> A pasta `node_modules` deve ser adicionada ao arquivo `.gitignore`. É uma má prática versioná-la assim como com qualquer gereciador de pacotes.

Assim toda vez que você baixar o projeto, será necessário executar o commando `npm install`, ele irá restaurar todos os pacotes definidos em `package.json`.

#### Aplicando o Express

Abra o arquivo `index.js` e substitua seu conteúdo pelo seguinte:

{% highlight javascript %}
// importação do módulo recem instalado
const express = require('express')  

const app = express()  
const port = 3000

// callback para a rota http://localhost:3000/
app.get('/', (request, response) => {  
  response.send('Hello from Express!')
})

app.listen(port, () => {  
  console.log(`servidor escutando na porta ${port}`)
})
{% endhighlight %}

A maior diferença que você deve notar é que o Express por padrão lhe retorna um roteador. Você não precisa fazer uma verificação manual da rota e decidir o que fazer com ela, ao invés disso pode definí-las com `app.get`, `app.post`, `app.put`, etc. Eles são traduzidos para os verbos HTTP correspondentes.

Um dos conceitos mais poderosos que o Express implementa é o **padrão middleware**.

### Middlewares

Você pode fazer uma analogia de middlewares para pipelines Unix, porém para requisições HTTP:

{% highlight text %}
1º middleware -> 2º middleware -> 3º middleware -> Manipulador opcional da rota
{% endhighlight %}

Acima você pode ver como uma requisição passa por uma aplicação Express. Ela atravessa três middlewares. Cada um pode modificá-la, então baseado na regra de negócio, o terceiro middleware pode mandar de volta uma resposta ou passá-la para um manipulador de rota.

Na prática, você pode fazer assim:

{% highlight javascript %}
const express = require('express')  
const app = express()

app.use((request, response, next) => {  
  console.log(request.headers)
  next()
})

app.use((request, response, next) => {  
  request.chance = Math.random()
  next()
})

app.get('/', (request, response) => {  
  response.json({
    chance: request.chance
  })
})

app.listen(3000)  
{% endhighlight %}

Coisas para serem notadas aqui:

  * `app.use`: é assim que você define middlewares - ela recebe uma função com três parametros, o primeiro sendo a requisição, o segundo sendo a resposta e o terceiro sendo o callback para o **próximo**(`next`). Chamando `next` o Express saberá que ele pode ir para o próximo middleware ou gerenciador de rota.
  * o primeiro middleware irá logar os cabeçalhos e imeditamente chamar o próximo.
  * o segundo adiciona uma propriedade a mais - **isso é uma das melhores características do padrão middleware**. Seus middlewares podem adicionar informações a mais ao objeto da requisição que os próximos middlewares poderão ler/alterar.

### Manipulação de erros

Assim como em todos os frameworks, ter uma manipulação correta de erros é crucial. No Express você tem que criar uma função middleware especial para fazê-lo - um middleware com quatro paramêtros.

{% highlight javascript %}
const express = require('express')  
const app = express()

app.get('/', (request, response) => {  
  throw new Error('boom explodiu')
})

app.use((err, request, response, next) => {  
  // loga o erro, por enquanto somente console.log
  console.log(err)
  response.status(500).send('Deu ruim!')
})
{% endhighlight %}

Coisas para serem notadas aqui:

  * A função manipuladora de erro deve ser a última a ser adiocionada com `app.use`.
  * O manipulador de erro tem um callback `next` que pode ser usado para encadear vários manipuladores de erro.
