#Async em  Scala 
	
Um dos conceitos que podemos considerar chave para a escalabilidade é a programação assíncrona.
Ela consiste em realizar qualquer tarefa mais pesada em termos de recursos computacionais em um processo ou até mesmo máquina separada, fora do workflow natural de um aplicativo.	
Em uma requisição assíncrona, não existe sincronismo entre as requisições, sendo assim, podemos enviar diversas requisições em paralelo, onde cada resposta retorna quando estiver pronta.
A programação assíncrona é a que permite a delegação de processo de aplicação a outros tópicos , sistemas e ou dispositivos. 
Programas síncronos é executado de forma sequencial enquanto aplicativos assíncronos pode iniciar uma nova operação e sem esperar a conclusão dos novos que ele mesmo pode continuar a trabalhar no seu próprio fluxo .

Exemplo:

	Vamos imaginar um caso em que uma pessoa envie um e-mail e não pode fazer nada até que a resposta recebida de 
	um remetente. Ou seja as tarefas para qualquer coisa além do envio de e-mail são bloqueados pela resposta que 
	você não tem controle se pode tomar muito tempo. Na maneira assíncrona é possível enviar o e-mail e continuar 
	a trabalhar em outras tarefas enquanto espera a resposta do remetente.

Quando uma requisição web chega ao servidor a aplicação precisa executar vários processos até montar o HTML de resposta, esse tempo precisa ser o menor possível para que o mesmo servidor possa responder o máximo possível de requisições por um período de tempo.
	Se uma requisição demora mais porque precisa ficar esperando operações de leitura e escrita (I/0), como salvar um registro no Banco de dados, executar uma query pesada , chamar um web service, podemos dizer que a requisição tem um I/O bound.
	Mas se a requisição demora mais por processamento, como por exemplo processar um array de objetos, realizar transformação de uma estrutura em outra, gerar um PDF, montar um HTML, podemos dizer que a requisição tem um CPU bound.
	Ou seja temos dois tipos de possíveis gargalos, no caso do CPU bound a única maneira de resolver isso é ter CPU  mais rápidos, ou CPUs em paralelo,  ou seja a aplicação precisa suportar multithread ou multi- processos para utilizar todas as CPUs. No caso do I/O bound o sistema precisa suportar chamadas assíncronas, ou seja o sistema terá chamadas de notificações de eventos que informarão quando por exemplo uma chamada ao banco de dados foi concluída.
O Scala tem uma abordagem em sua API de Concurrency muito simples para implementar a idéia de programação assíncrona. Usando essa API de Concurrency o  compilador faz o trabalho difícil, e o aplicativo mantém a estrutura lógica que se assemelha ao código síncrono. Como resultado, obtemos todas as vantagens da programação assíncrona com pouco do esforço.
Hoje na ilegra temos algumas aplicações 100% do conceito assíncrono, deixando as aplicações com uma grande escalabilidade que indica sua habilidade de manipular uma porção crescente de trabalho de forma uniforme, ou estar preparado para crescer ou se tornar distribuído com pouco esforço.

##Funture/Promise em Scala#

Future e uma API de scala que proporciona uma maneira de executar operações em paralelo de forma não bloqueante. 
 A ideia é que um Future seja uma espécie que objeto que tenha um espaço reservado que podemos trabalhar a espera de um resultado que ainda não possuímos. Com isso podemos compor tarefas simultâneas de forma assíncrona e sem bloqueio de uma forma rápida.
Em scala podemos combinar o Future com maps, for-comprehensions e filters de uma forma não bloqueante, e imutável.
Então definindo Future podemos dizer que é um objeto que contem um valor que pode se tornar disponível . 
Um Future pode ser dizer completo ou concluído de duas formas:
- Concluído com sucessos e tem valor.
- Concluído com Falha e possui uma exceção como valor.

**Exemplo:** 
 
    import scala.concurrent._
    import ExecutionContext.Implicits.global
    
    val f : Future[List[User]] =  future{
      dao.getUsers()
    }



**Onde:**

- `import ExecutionContext.Implicits.global` -> importa um contexto de execução global padrão do scala , que fornece pools de threads para lidar com assíncronos . 
- em seguida estamos fazendo uma chamada hipotética ao Banco de Dados e com sabemos que isso pode levar algum tempo, faremos uma chamada assíncrona para não bloquear o resto do programa, e no quando estiver pronto teremos a resposta na variável `f`.


##Callbacks:##

Mas para interagir com esses valores  do Future precisamos associar a um Callback. Este callback é chamado de forma assíncrona quando o Future for concluído. Se o Future  foi concluído o registro é associado a um callback, o retorno pode tanto ser executado de forma assíncrona, ou sequencial.
A forma mais comum de registra um call-back é usando o método onComplete, que aplica seu resultado a um Success se foi concluída com êxito e Failure se for concluída com um exceção.
        
**Exemplo:**

    import scala.concurrent._
    import ExecutionContext.Implicits.global
    import scala.util.Success
    import scala.util.Failure
    
    case class User(name: String, age: Int)
    
    object TestFuture {
      val f: Future[List[User]] = future {
       dao.getUsers() 
      }
    
      f onComplete {
        case Success(result) => result.map(f=> println("Users: "+f.name))
        case Failure(t) => println("Ocorreu um erro no Future: " + t.getMessage)
      }
    }
	
---

        Os métodos OnComplete , Success e Failure são do tipo Unit, então esse métodos não podem ser 
        encadeados com outros ou seja tudo que é feito dentro desses métodos morre ali mesmo.


##Promises##

Enquanto Future são definidas como um tipo de somente leitura objeto com espaço reservado para um resultado que ainda não existe, o Promises pode ser pensado como um recipiente onde se atribui o valor de um Future completo.  Ou seja um Promise pode ser usado para completar um future com valor quando tem sucesso no método success ou pode ser uma Promise de exeção no método de Failure. E por padrão um Promise completo retorna um Future.	
Cria-se uma promessa que é o lugar onde você vai colocar o resultado da computação e da promessa de que você terá um futuro que vai ser usado para ler o resultado que foi colocado na promessa. Quando você completar uma promessa, seja por falha ou sucesso, você irá acionar todo o comportamento que foi anexado ao futuro associado.

**Exemplo:**

    import scala.concurrent._
    import ExecutionContext.Implicits.global
    import scala.util.Success
    import scala.util.Failure
    
    case class User(name: String, age: Int)
    
    object TestFuture {
      val f: Future[List[User]] = future {
        dao.getUsers() 
      }
    
      val resultPromise: Promise[List[User]] = Promise[List[User]]
    
      f onComplete {
        case Success(result) => resultPromise.success(result)
        case Failure(t) => resultPromise.failure(t)
      }
    
      resultPromise.future.map { f =>
        f.map(user => println("value = " + user.name))
      }
    }



**Onde:**
- A variável resultPromises é literalmente a promessa que teremos uma Lista de usuários 
- No success é atribuído o valor do Future com sucesso para o promises
- No Failure  é atribuído o valor de falha ao promises
- Esses promise continua nos devolvendo um Future, mas uma das formas de eu interagir com esse future é utilizando um map, com um map eu acesso os valores do future e com o segundo map percorremos a Lista de Usuários.


##For-comprehensions/Maps/FlatMap##

Interarir com os futures com callback e promisses é simples, mas exigem que se escreva muito código quando na verdade quermos alguma forma mais rápida e simples. Combinando com outras funções é aonde está o maior ganho dos futures. 
O for-comprehension é uma das maneiras de iteratir com funções que estão dentro de um future, dentro dele podemos avaliar processamentos em paralelo e no final agregar em um só resultado.


**Exemplo:**

	val result1:Future[List[User]] = future(dao.getUsers())
	val result2:Future[List[Pessoa]] = future(dao.getPessoas())
	
	val res = for {
	   r1 <- result1
	   r2 <- result2
	} yield (r1+r2)

O mesmo podemos fazer com a função de Map, interagir com processamentos que estão dentro de um future.
Quando você está escrevendo a função que você passa para um map, você está com o futuro, ou melhor, em um futuro possível. Essa função de mapeamento é executado assim que o seu futuro [de List]  for concluída com êxito. No entanto, dessa forma estamos esperando apenas o resultado de sucesso de uma lista de números, caso essa lista venha a falhar vamos receber um Future[List] com uma falha, o mesmo caso podemos considera no exemplo anterior com o for-comprehension.

**Exemplo:**

	var listNumbers:Future[List] = Future(List(1,2,3,4,5))
	
	listNumbers.map(list=> list.map(number=> println(number)))
	resultado = 1234 

Se o cálculo de um futuro dependee do resultado de outra, podemos provavelmente recorrer a flatMap para evitar uma estrutura profundamente aninhada de futuros.
FlatMap funciona aplicando uma função que retorna uma seqüência para cada elemento da lista, e achatando os resultados na lista original, e tem a grande vantagem de fazer o mesmo com funções que estão dentro de um future.

**Exemplo:**

	val f1 = Future ( "Hello" + "World" )
	
	val f2 = Future(3)
	
	val f3 = f1.flatMap (x ⇒ f2.map (y => x.length * y))
	
	f3.value = Some(Success(30))

**Conclusão**

Usar o conceito de programação assíncrona é certamente um benefício dentro do código. Um conceito simples como um gerenciador de fila de procesos pode mudar totalmente a forma como se constrói aplicativos.
Se fila está grande e os jobs estiverem acumulando, podemos adicionar mais consumidores. Pois eles já estão configurados para ler da fila.
Então podemos  pensar que um Future são os produtores e Promises são os consumidores.  E um Future é essencial para uma referencia read-only para um valor que ainda deve ser processado e um Promise é praticamente o mesmo, exceto que podemos escrever nele,  em resumo os dois você pode realizar leitura mas apenas no promise você pode escrever. 
        A principal vantagem de Future e (programação assíncrona) , é a tarefa  atuar em segmento diferente, por isso thread principal não fica bloqueada até a tarefa ser concluída. É possível executar outras tarefas ao mesmo tempo. O modelo assíncrono em caso de tarefa de longa duração é muito útil, para que o seu aplicativo reaja a outras ações realizadas pelo  usuário.


###Referências:###

- http://danielwestheide.com/blog/2013/01/09/the-neophytes-guide-to-scala-part-8-welcome-to-the-future.html

- <http://arild.github.io/scala-workshop>

- <http://docs.scala-lang.org/overviews/core/futures.html>

- <http://www.slideshare.net/AlexandruNedelcu/prezentare-25762173>

- <http://doc.akka.io/docs/akka/2.0.1/scala/futures.html>

- <http://stackoverflow.com/questions/13381134/what-are-the-use-cases-of-scala-concurrent-promise>

- <http://www.akitaonrails.com/2013/12/23/solucoes-para-um-mundo-assincrono-concorrente?utm_source=feedburner&utm_medium=feed&utm_campaign=Feed%3A+AkitaOnRails+%28Akita+On+Rails%29#links>

- <http://blogs.technet.com/b/meamcs/archive/2012/09/08/why-a-how-to-asynchronous-programming.aspx>


