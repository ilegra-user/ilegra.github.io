---
layout: post
title: Coherence
author: Leonardo Amarilho e Christophe Marchal
categories: data grid
---

Vamos falar sobre a experiência que tivemos na implantação do Coherence em um dos nossos projetos, aqui na empresa.

Um dos nossos clientes tinha um processo que podia demorar de alguns minutos até varias horas para acabar. O resultado do processamento podia variar de alguns kilobytes até 800Mb. Melhorar a performance desse processo não era uma opção. O resultado desse processo era agregado a um resultado de pesquisa. Era aceitável a pesquisa demorar, mas reordenar os resultados da pesquisa ou aplicar qualquer filtro em cima tinha que ser rápido. Por tanto não podia chamar o processo de novo.

A solução escolhida foi de usar um data grid para cachear o resultado do processo e conseguir depois acessar esse resultado em alguns milisegundos. Normalmente, se usa um data grid quando precisa de mais memória do que uma máquina só tem. Um data grid roda em cima de um cluster de máquinas e vai distribuir os dados entre os servidores. A ideia é que qualquer nodo consegue saber aonde achar qualquer informação no cluster. A ilegra costuma usar soluções open source e livres, por isso que primeiro usamos a solução do Terracotta. Porém tivemos algumas dificuldades em configurar ele para descartar as chaves menos usadas. Como o cliente tinha uma licença para usar Coherence, acabamos migrando para Coherence. 

Abaixo listarei algumas estratégias que testamos e nossa opinião:

<strong>* Replicate Cache</strong> - Todos os seus nodos de seu cache vão possuir os mesmos dados e de rápido acesso. 
<br/><em>Ideal:</em> Se você precisa de uma resposta rápida, afinal todos os nodos possuem todas as informações e você possui memória suficiente para colocar nos nodos. 
<br/><em>Observação:</em> Se o seu cache contiver muitos dados, e o acesso a esses dados não seja constante, tornasse custoso manter todos seus nodos com todos os dados replicados.

<strong>* Distributed Cache</strong> - Todos seus nodos possuem uma área aonde guardam lógicamente em qual dos nodos está a informação que você procura.
<br/><em>Ideal:</em> Tipologia ideal quando você possui muitas informações, mas ao mesmo tempo é custoso demais, em relação a memória, replicar esses dados em todos os nodos.
<br/><em>Observação:</em> Quanto o maior número de nodos, menor o espaço em memória necessário. Isso acontece porque cada nodo além de saber logicamente aonde encontrar a informação, possui uma área primária para guardar os dados e um espaço de backup, onde usa para recuperar as informações caso um nodo desconecte.

<strong>* Near Cache</strong> - Cada nodo além de possuir a informação de onde os dados estão, igual ao Distributed Cache, possui uma área local em memória configurável que guarda alguns dados já acessados.
<br/><em>Ideal:</em> Considerado o mais balanceado entre os dois anteriores, em relação ao custo benefício. Além de ter informações locais já acessadas e de rápida resposta, não guarda fisicamente todos os dados.
<br/><em>Observação:</em> Esse tipo de tipologia tem um ganho de performance, quando o número de nodos do seu cache é alto e você possui um espaço considerável, para guardar no cache local algumas informações, assim evitando tráfego de rede.

