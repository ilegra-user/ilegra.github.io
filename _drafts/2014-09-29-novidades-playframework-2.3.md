---
layout: post
comments: true
title: Novidades do Play! 2.3
author: Christophe Marchal
categories: playframework
---


Play Framework tem por ambição ser o melhor web framework full-stack da JVM. Ele está sendo cada vez mais modularizado para conseguir usar os seus componentes fora do Play e também visa trocar os componentes default. Por exemplo: hoje é fácil usar um template engine diferente do default (twirl). Já para a parte de tela, é normal usar bibliotecas javascripts para deixar estas mais dinâmicas.

O Play! já apresentava algumas funcionalidades para facilitar o desenvolvimento javascript e a criação de CSS. Funcionando em cima do SBT, ele compilava os arquivos escritos usando Cofeescript, gerava os .css baseado nos arquivo usando Less. Porém, ele tinha um suporte limitado a algumas bibliotecas. No Play! 2.3 foi modificada completamente essa parte.


Sbt-Web
---------------------

Os desenvolvedores do Play! 2.3 externalizaram toda parte que gerenciava CSS e javascript numa família de plugins: [sbt-web][1]. O plugin facilita a criação de plugins sbt orientado a desenvolvimento web. O que principalmente significa criar plugins sbt baseados em bibliotecas que normalmente rodam em cima do Node.js. A vantagem é não precisar aprender a usar o [Grunt][2] para automatizar as tarefas do código de tela (compilação, minification, testes, validação ...). Os plugins podem ser usados em projetos que não usam o Play!. O SBT consegue rodar esses plugins usando a JVM via [Trireme][3] (default) ou [Node.js][4] (mais rápido).

Webjars
---------------------

Sbt-web tem suporte para [Webjars][5]. Webjars permitem gerenciar as dependências do seu código cliente como Jquery e Bootstrap da mesma maneira que se gerencia as dependências java: seja com seu gerenciador de dependência preferido (SBT, maven, gradle ...). Webjars tem suporte para dependências transitivas. A maioria dos webjar têm suporte para "Source Map",  ou seja, vocês conseguem debugar o código escrito em Coffeescript usando as ferramentas de desenvolvedor do seu navagador.

<center>
![Play Framework][7]
</center>


Javascript testing
---------------------

Por default, qualquer arquivo javascript na pasta test/assets com sufixo: Spec.js ou Test.js será executado na fase de teste da aplicação.


Asset pipeline
---------------------

Depois de compilados e ter passados os testes, arquivos javascripts e css precisam ser transformados para melhorar a performance do web site. Esse pipeline de transformação deve ser definido no seu build.sbt

{% highlight scala %}
pipelineStages := Seq(rjs, digest, gzip)
{% endhighlight %}

Cada stage do pipeline depende de um plugin:
RequireJS plugin (sbt-rjs) para otimizar o load dos javascripts.
digest plugin (sbt-digest) gera o hash de cada asset e para eles será cacheado.
gzip plugin (sbt-gzip) para a compressão dos assets antes de deployar a aplicação.

RequireJS
---------------------

Play! tem um melhor suporte para as otimizações do [RequireJS][6]. Essa biblioteca otimiza o load dos javascripts. Por exemplo: se todo código javascript necessário para sua aplicação rodar cabe num arquivo pequeno, ele vai servir um arquivo só com todo javascript. Assim, o navegador baixa um arquivo só no lugar de um arquivo por biblioteca usada.

Asset Fingerprinting
---------------------

Os assets são estáticos e só mudam a cada deploy. Logo, os navegadores podem cachear eles no lugar de baixar em cada pagina. É desejável que o navegador baixe um asset somente quando este for modificado. Uma técnica para isso é gerar um hash do arquivo e botar esse hash no nome. Assim, o navegador pode detectar se ele precisa baixar o arquivo de novo ou não. A lógica para gerar o nome do arquivo está agora facilitada:

Parte do template

{% highlight html %}
<link rel=”stylesheet” media=”screen” href=”@routes.Assets.versioned(“stylesheets/main.css”)”>
{% endhighlight %}

HTML Gerado:

{% highlight html %}
<link rel=”stylesheet” media=”screen” href=”/assets/public/84a01dc6c53f0d2a58a2f7ff9e17a294-main.css”>
{% endhighlight %}


WebSocket
---------------------

O código para websocket foi muito simplificado. Não é mais necessário escrever código usando os Iteratee e Enumerator. Agora, o código do servidor é um ator que espera as mensagens que o cliente javascript vai mandar.

[1]: https://github.com/sbt/sbt-web#sbt-web "sbt-web"
[2]: http://gruntjs.com/ "Grunt"
[3]: https://github.com/apigee/trireme#trireme "Trireme"
[4]: http://nodejs.org/ "Node"
[5]: http://www.webjars.org/ "Webjars"
[6]: http://requirejs.org/ "RequireJS"
[7]: /public/play.png "Play Framework"