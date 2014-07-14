## Reactive Streams and Play 3 ##

### Stream ###

Um Stream é um fluxo de dados, seja, tens um inicio, porm tu no sabe quando vai acabar. Pode pensar num stream como uma lista e um iterator. A cada iteraço tu tens que chamar um metodo para saber se vai ter um próximo elemento. 

**Caso de uso** 
Nesse exemplo vamos considerar o envio de arquivos muito grandes. Criamos um servidor que vai receber esses arquivos e aplicar algum tipo de tratamento em cima. Carregar o documento inteiro na memoria do servidor no é aceitavel porque dependando do tamanho do arquivo, poderiamos chegar ao limite da memoria que podemos alocar.  Logo, o tratamento deve ser feito em cima de pedaços do arquivo. Nosso servidor tem que nos fornecer o arquivo na forma de stream e processar o documento linha por linha. Assim, no precisamos ter o documento inteiro na memoria. 

**Vou precisar disso mesmo?**
Esse tipo de feature sao mais usadas pelos framework para optimizar o IO. Porem, como desenvolvedor, voc vai precisar usar Stream quando voce vai precisar fazer o parsing de um xml de varios Giga. Também pode chegar a usar quando voc precisa trabalhar com stream de valores: valores da bolsa, valor de sensores, dados financeiros, lendo campos muito grande a partir do banco de dados. 

**Problema**
O que acontece se o tempo de processar uma linha é maior do que o tempo de enviar uma linha? É o classico problema do consumidor lento. Tem varias estrategias para resolver isso. No caso de um reactive stream, em vez de ter um nodo enviando os dados e um outro aceitando eles, um nodo vai pedir um certa quantidade de dados (Consumer) e o outro nodo vai fornecer (Publisher). Seja, passamos de um modo PUSH para um modo PULL. Assim, o nodo processando os dados só vai pedir mais dados quando ele acabar o processamento.

### Play ###
Play framework, [já vem que uma implementaço de stream][3]. Essa implementaço vem do Haskell com Iteratee e Enumerator. Um Enumerator produce os dados e um Iteratee consume eles. O problema é que essa impelementaço é muito complicada e a API dificil de entender. Para a verso 2.4, que vai sair no final do ano 2014, Play terá dois modulos experimentais: um usando [akka-http][4] para gerenciar toda stack http com suporte a reactive stream e um outro modulo permitira o uso [akka-stream][5], que também implementa reactive streams. Em 2015, Play 3, usara reactive stream no lugar do Iteratee/Enumerator e tera metodos para transformar um Iteratee em Consumer e um Enumerator em Publisher.

### References ###
http://www.reactive-streams.org/
[Introducing Reactive Streams video][1]
[Play roadmap][2]


  [1]: https://www.youtube.com/watch?v=khmVMvlP_QA
  [2]: https://docs.google.com/document/d/11sVi1-REAIDFVHvwBrfRt1uXkBzROHQYgmcZNGJtDnA/pub
  [3]: http://playframework.com/documentation/2.3.x/Iteratees
  [4]: http://doc.akka.io/docs/akka-stream-and-http-experimental/0.4/scala/index-http.html
  [5]: https://typesafe.com/activator/template/akka-stream-scala
