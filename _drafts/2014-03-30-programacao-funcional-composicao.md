O paradigma de programação funcional, e suas diversas ramificações tem se mostrado uma área de estudo relevante no que diz respeito a escalabilidade de software, produtividade e simplicidade. Porém há uma série de discussões a respeito da aplicabilidade do paradigma a problemas concretos de negócio, ou seja, até que ponto deve-se considerar aplicar as soluções funcionais a questões fora o domínio matemático, e de que forma isto se dá? Para isto é necessário desmistificar certas questões.

Programação funcional -> Funções
=========
Enquanto no mundo programação imperativa, tipos primitivos são valores, e funções são componentes que realizam operações modificando o estado da aplicação, o paradigma funcional dá uma posição privilegiada às funções, a ponto de serem consideradas cidadãos de primeira classe (Higher order functions). 

Isto significa dizer que no paradigma funcional uma função tem o mesmo peso ou importância que um tipo primitivo, e sendo assim elas aparecerão de forma muito mais frequente no decorrer dos seus blocos de código. 

Complexidade de negócio vs Simplicidade das funções
=========
E de que forma podemos resolver problemas de negócio complexos com funções? O ponto central aqui é complexidade.

Gerenciar a complexidade é um dos maiores problemas encontrados durante o desenvolvimento de software, e um ponto crucial no implementação de aplicações sustentáveis. Para tal o paradigma funcional tem uma solução: Composição (princípio da composicionalidade*). Através de composição é possível criar complexidade a partir da simplicidade, de acordo com a necessidade.

* "Propriedade das expressões complexas cujo sentido é determinado pelos sentidos dos seus constituintes e pelas regras usadas para os combinar
Infopédia 


Composição de funções - Classificação
=========
A partir da idéia de composição, se tem uma série de nomenclaturas utilizadas para classificar os tipos de relações possíveis entre as funções. Surge aí a noção de Applicatives, Functors, Monoids, Monads (dentre outros) frequentemente citados em diversas fontes de pesquisa relacionadas a linguagens funcionais. 

O conhecimento dos termos em si não é estritamente necessário para quem está iniciando com o paradigma funcional, porém na medida em que se buscam soluções mais robustas, sem que para isto haja perda de simplicidade, se faz necessário cada vez mais o conhecimento e a familiaridade com estes conceitos para que se possa tirar um melhor proveito da linguagem e paradigma com o qual se está desenvolvendo.

