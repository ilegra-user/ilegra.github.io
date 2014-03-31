---
layout: post
title: Coherence
author: Leonardo Amarilho e Christophe Marchal
categories: datagrid
comments: true
---

Vamos falar sobre a experiência que tivemos na implantação do <strong><a  href="http://www.oracle.com/technetwork/middleware/coherence/overview/index.html" target="_blank">Coherence</a></strong> em um dos nossos projetos, aqui na empresa.

Um dos nossos clientes tinha um processo que podia demorar de alguns minutos até varias horas para acabar. O resultado do processamento podia variar de alguns kilobytes até 800Mb. Melhorar a performance desse processo não era uma opção. O resultado desse processo era agregado a um resultado de pesquisa. Era aceitável a pesquisa demorar, mas reordenar os resultados da pesquisa ou aplicar qualquer filtro em cima tinha de ser rápido. 

A solução escolhida foi de usar um data grid para cachear o resultado do processo e conseguir acessar esse resultado em alguns milisegundos. Normalmente, se usa um data grid quando precisa de mais memória do que uma única máquina. Um data grid roda em cima de um cluster de máquinas e vai distribuir os dados entre os servidores. A ideia é que qualquer nodo consiga saber aonde achar qualquer informação no cluster. A ilegra costuma usar soluções open source, por isso inicialmente optamos pelo <strong><a href="http://www.terracotta.org/" target="_blank">Terracota</a></strong>. Porém tivemos algumas dificuldades em configurar ele para descartar as chaves menos usadas. Como o cliente tinha uma licença para usar Coherence, acabamos migrando para essa solução. 

Abaixo listarei algumas estratégias de cache que testamos, e a nossa opinião.

<strong>* Replicate Cache</strong> - Todos os nodos do cache possuirão os mesmos dados com um rápido acesso. 
<br/><em>Ideal:</em> Quando a necessidade é uma resposta rápida e existe memória suficiente para colocar em todos os nodos.
<br/><em>Observação:</em> Quando o seu cache contiver muitos dados e o acesso a esses dados não é constante, torna-se custoso manter todos seus nodos com todos os dados replicados.

<strong>* Distributed Cache</strong> - Todos seus nodos possuem uma área onde guardam lógicamente qual dos nodos contém a informação requisitada.
<br/><em>Ideal:</em> Tipologia ideal quando você possui muitas informações, mas ao mesmo tempo é custoso demais, em relação a memória, replicar esses dados em todos os nodos.
<br/><em>Observação:</em> Quanto o maior número de nodos, menor o espaço em memória necessário. Isso acontece porque cada nodo além de saber logicamente onde encontrar a informação, possui uma área primária para guardar os dados e um espaço de backup usado para recuperar as informações caso um nodo desconecte.

<strong>* Near Cache</strong> - Cada nodo além de possuir a informação de onde os dados estão, igual ao Distributed Cache, possui uma área local em memória configurável que guarda alguns dados previamente acessados.
<br/><em>Ideal:</em> É considerado mais balanceado dos três citados, principalmente em relação ao custo benefício. Além de possuir informações locais já acessadas e de rápida resposta, não guarda fisicamente todos os dados.
<br/><em>Observação:</em> Esse tipo de tipologia tem um ganho de performance, quando o número de nodos do cache é alto e possui um espaço considerável para guardar no cache local algumas informações, evitando assim tráfego desnecessário de rede.
