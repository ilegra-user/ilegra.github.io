---
layout: post
comments: true
title: Programação funcional - Composição
author: Joel Correa
categories: functional-programming
---

O paradigma de programação funcional e suas diversas ramificações tem se mostrado uma área de estudo relevante no que diz respeito a escalabilidade de software, produtividade e simplicidade. Porém há uma série de discussões a respeito da aplicação do paradigma a problemas concretos de negócio, ou seja, até que ponto deve-se considerar aplicar as soluções funcionais a questões fora o domínio matemático, e, de que forma isto se dá? Para isto é necessário desmitificar certas questões.

Funções
=========
Enquanto no mundo programação imperativa tipos primitivos são valores e funções são componentes que realizam operações modificando o estado da aplicação, o paradigma funcional dá uma posição privilegiada às funções, a ponto de serem consideradas <b>cidadãos de primeira classe</b> (Higher order functions). 

Isto significa dizer que no paradigma funcional uma função tem <b>o mesmo peso ou importância que um tipo primitivo</b> (funções são valores). Sendo assim elas aparecerão de forma muito mais frequente no decorrer dos seus blocos de código (funções podem receber ou retornar outras funções, por exemplo).

Complexidade
=========
Como podemos resolver problemas de negócio complexos com funções? O ponto central aqui é complexidade.

Gerenciar a <b>complexidade</b> é um dos maiores problemas encontrados durante o desenvolvimento de software e um ponto crucial no implementação de aplicações sustentáveis. Para tal o paradigma funcional tem uma solução: Composição. Através de composição é possível criar complexidade a partir da simplicidade, de acordo com a necessidade.

<blockquote>
Princípio da composicionalidade: Propriedade das expressões <b>complexas</b> cujo sentido é determinado pelos sentidos dos seus <b>constituintes</b> e pelas regras usadas para os <b>combinar</b> - Infopédia 
</blockquote>

Composição de funções - Classificação
=========
A partir da idéia de composição, tem-se uma série de nomenclaturas utilizadas para <b>classificar os tipos de relações</b> possíveis entre as funções. Surge aí a noção de Applicatives, Functors, Monoids, Monads (dentre outros), frequentemente citados em diversas fontes de pesquisa relacionadas a linguagens funcionais. 

O conhecimento dos termos em si não é estritamente necessário para quem está iniciando com o paradigma funcional, porém na medida em que se buscam soluções mais robustas, sem que para isto haja perda de simplicidade, faz-se necessário cada vez mais o conhecimento e a familiaridade com estes conceitos para que se possa tirar um melhor proveito da linguagem e paradigma com o qual se está desenvolvendo.

Links úteis
=========
http://en.wikipedia.org/wiki/Function_composition_(computer_science)

http://www.slideshare.net/calewis/jax-func-4557288

http://channel9.msdn.com/Shows/Going+Deep/Brian-Beckman-Dont-fear-the-Monads

http://aeflash.com/2013-06/monads.html

http://www.codecommit.com/blog/ruby/monads-are-not-metaphors


