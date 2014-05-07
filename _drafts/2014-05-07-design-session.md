---
layout: post
comments: true
title: Design Session
author: Rafael Souza
categories: extreme-programming
---


Desing Session, também conhecida como [Quick Design Session][1], é uma das práticas do [Extreme Programming][2] presente no dia a dia do desenvolvimento de software da ilegra.

A Design Session é um processo colaborativo de aprendizado e problematização no qual o objetivo é evoluir, iniciar ou afirmar um [design de software][10]. É importante ressaltar que, ao contrário da [fase de Design proposta pelo modelo Waterfall][3], a Desing Session não se propõe a entender e resolver todo o problema antes da sua implementação, e sim levantar, de forma rápida, as principais variáveis envolvidas e propor um esboço que funcione como norte para o desenvolvimento.


Motivação
---------------------

Essa é uma prática que lida diretamente com os aspectos de manutenção e flexibilidade do software, melhorando a qualidade e, por consequência, a velocidade de entrega do software. Além disso, a Design Session trabalha sob uma visão diferente da codificação e é um ótimo momento para os desenvolvedores buscarem um melhor entendimento do que será desenvolvido e proporem possíveis soluções para o problema em questão.

As Design Sessions, seguidas de revisões, também podem ser utilizadas como um meio de melhorar algumas habilidades dos desenvolvedores, tais como: capacidade analítica e de abstração, comunicação com PO e com o time, entendimento geral do sistema e [problematização][4]. 

Como aplicamos na ilegra
---------------------

As Design Sessions são aplicadas por um desenvolvedor ou em equipe, de forma rápida, em no máximo 30 minutos, embora por vezes despendemos mais tempo. 

Quando é feita por um desenvolvedor, este centraliza a comunicação com o Product Owner (afim de elucidar requisitos) e submete o esboço de sua solução à uma revisão (Design Review). Além disso, esse desenvolvedor fica incumbido de fazer um briefing explicando sua solução para a equipe que irá trabalhar no problema proposto. 

Quando é realizada em equipe, a Design Session é mais dinâmica e colaborativa dispensando revisão e briefing, e nesse caso, os requisitos já devem estar mais claros ou o Product Owner estar junto à equipe.

<center>
![Design Session colaborativa][12]
</center>

A prática de Design Session, geralmente é aplicada por User Story e trabalhada em duas [etapas cíclicas][11]. Em uma dessas etapas olhamos para o contexto do problema: variáveis, requisitos, natureza da informação, componentes envolvidos, participantes e etc. A outra etapa envolve a parte mais técnica do design de software e o ponto fundamental é esboçar a solução que vai nortear o desenvolvimento, nesta etapa estão envolvidos: [design patterns][5], [SOLID][6], trade-off, algoritmo, estrutura de dados, [composição de componentes][7], entre outros.

O que aprendemos
---------------------

Com a nossa experiência aplicando essa prática aprendemos que o nível de abstração que deve-se chegar no esboço do problema está mais relacionado ao próprio problema do que a alguma regra ou técnica. O que deve ser buscado é que o time "pegue a ideia" da solução proposta e, para tanto, algumas caixas desenhadas em um caderno mostrando relações e responsabilidades de componentes, por vezes, é o suficiente.

Embora seja comum vermos diagramas de classes e componentes como resultados das Design Sessions, o uso ou padronização de linguagens de modelagem e ferramentas não é obrigatório. O principal objetivo dessa prática não é documentar software, e sim, visualizar o problema e comunicar um esboço de solução ao time. Aqui na ilegra,  geralmente utilizamos cadernos, quadros e [cacoo][8] como ferramentas. Já a linguagem de modelagem gráfica surge sozinha a partir dos gostos e experiência dos times e, na maioria das vezes, ela é um misto de UML, [CRC][9] e muita criatividade.

[1]: http://guide.agilealliance.org/guide/quickdesign.html "Quick Design Session"
[2]: http://www.extremeprogramming.org "Extreme Programming"
[3]: http://en.wikipedia.org/wiki/Big_Design_Up_Front "Big Design Up Front"
[4]: http://en.wikipedia.org/wiki/How_to_Solve_It "How to Solve it!"
[5]: http://sourcemaking.com/design_patterns "Design Patterns"
[6]: http://butunclebob.com/ArticleS.UncleBob.PrinciplesOfOod "SOLID"
[7]: http://www.infoq.com/presentations/Simple-Made-Easy "Simple Made Easy"
[8]: https://cacoo.com "Cacoo"
[9]: http://www.agilemodeling.com/artifacts/crcModel.htm "Class Responsibility Collaborator"
[10]: http://martinfowler.com/articles/designDead.html "Software Design"
[11]: https://www.youtube.com/watch?v=f84n5oFoZBc#t=320 "Hammock Driven Development"
[12]: /public/design-session.jpg "Design Session Colaborativa"