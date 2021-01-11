---
layout: post
title: "Bancos de dados Chave e Valor"
description: "Entenda o que são, onde vivem e do que se alimentam"
date: 01/01/1970
feature_image: images/kv-database/lockers.jpg
language: pt-BR
tags: [data%pt-BR%, nosql%pt-BR%]
---

Bancos de dados chave e valor são uma categoria que abrange uma série de bancos de dados NoSQL com caracteristicas similares. Apesar disto, nem todos são iguais, aqui vou explicar um pouco algumas destas caracteriscas sem entrar a fundo nas especificidades de cada um.

<!--more-->

Para saber mais sobre a definição de um banco de dados NoSQL, as diferenças que eles tem de um banco de dados relacional sobre o teorema de CAP entre outras coisas, um ótimo livro chamado [NoSQL Distilled](https://www.amazon.com.br/NoSQL-Distilled-Emerging-Polyglot-Persistence-ebook/dp/B0090J3SYW/ref=tmm_kin_swatch_0?_encoding=UTF8&qid=&sr=). Este livro abrange conceitos iniciais e gerais de bancos de dados NoSQL e serve como uma ótima base para quem quer aprender um pouco mais.

## Como assim chave e valor?

Um banco de dados chave valor é uma estrutura para armazenamento de dados que funciona de forma muito similar a uma estrutura de dados do tipo mapa ou um dicionário, onde nós temos uma chave. Esta chave é um identificador para um registro é como se ela guardasse o endereço para o valor que procuramos. 


Utilizando uma analogia simples, pode se dizer que uma estrutura deste tipo é como a recepção de uma pousada. Ao chegar na pousada e informar a hospedagem a recepção nos entrega uma chave e esta chave contem o numero do nosso quarto e nos permite acessá-lo. De posse desta chave sempre conseguirmos ter um acesso direto ao nosso quarto.

{% include image_caption.html imageurl="/images/kv-database/reception.jpg" title="Hotel Reception" caption="" %}

Trazendo isto um pouco mais perto da linguagem técnica podemos pensar também em um *[JSON](https://pt.wikipedia.org/wiki/JSON)*:

```json
{
    "quartos": [
        {"306": {"camas": 2, "tv": "plasma"}},
        {"206": {"camas": 1, "tv": "plasma"}}
    ]
}
```

## Tá mas onde entra o banco de dados?

Certo, se chegamos até aqui, provavelmente você já entendeu um pouco de como tudo isto funciona, e talvez até já tenha lidado com mapas e dicionários com algumas linguagens de programação, mas fica a pergunta, o que diabos isso tudo tem a ver com bancos de dados e o que são bancos de dados chave e valor, certo?

Bom tem tudo a ver e você já vai ver porquê.

De acordo com a definição da [Oracle](https://www.oracle.com/br/database/what-is-database/):

> Um banco de dados é uma coleção organizada de informações - ou dados - estruturadas, normalmente armazenadas eletronicamente em um sistema de computador

Nesta categoria se encaixam softwares cujo objetivo é manter dados de forma organizada:

- [Redis](https://redis.io)
- [DynamoDB](https://aws.amazon.com/dynamodb/)
- [Voldemort](https://www.project-voldemort.com/voldemort/)

Aqui trago somente 3 exemplos de bancos chave e valor mas esta lista não tem o objetivo de de ser exaustiva, Este [artigo](https://en.wikipedia.org/wiki/Key–value_database) possui uma lista mais completa caso seja do seu interesse.

Estes bancos em geral, embora não trabalhem exclusivamente como chave e valor desempenham bem este papel.

Bancos de dados Chave e Valor podem operar de duas formas: Em memória ou de forma híbrida.

### Bancos de dados em memória

{% include image_caption.html imageurl="/images/kv-database/memory.jpg" title="Memory" caption="" %}

Quando um banco de dados está configurado para trabalhar em memória o que ocorre é que toda a informação é armazenada na memória RAM do servidor. Como tudo, quando falamos de software (e até em outras situações) existem pontos positivos e negativos.

Como principal ponto positivo temos a velocidade. Bancos de dados operando exclusivamente em memória não precisam escrever a informação em disco e o acesso a memória é muito mais rápido que o acesso a disco portanto são bancos onde a escrita e a leitura de informações é muito rápida.

Em se tratando de bancos chave e valor a leitura de informações se torna muito rápida também pela forma como é armazenda pois similar a um mapa, a partir da chave temos acesso direto ao valor então na maior parte dos casos podemos dizer que teremos uma complexidade O(1) para a leitura de informações destes bancos de dados.

Mas como [não existe almoço grátis](https://pt.wikipedia.org/wiki/There_ain%27t_no_such_thing_as_a_free_lunch) existem também alguns lados negativos desta abordagem.

Como as informações estão em memória e a memória RAM é volátil, caso ocorra alguma falha, queda de energia, etc. esta informação pode ser inteiramente perdida. Outro ponto importante é que usualmente temos uma quantidade muito menor de memória RAM do que disco nos servidores, e esta não é facilmente expansivel a ponto de podermos ter memória de forma elástica, inclusive pelo seu custo, sendo assim ficamos limitados a quantidade disponível de memória RAM, portanto é preciso tomar cuidado, tanto para garantir que estamos usando esta abordagem com dados que realmente valham a pena pois não teremos muito espaço, bem como, que esta informação não represente problema caso perdida ou possa ser realimentada de outra fonte.

### Armazenamento Híbrido

Estes bancos geralmente não trabalham somente com armazenamento em disco, especialmente visto que o objetivo deles em geral é ter ótima performance, sendo assim normalmente eles trabalham com uma abordagem mais híbrida. Esta abordagem funciona, em geral, de forma que toda informação quando é gravada é persistida no disco porém uma parte dela é mantida em memória para que o acesso seja rápido.

O que é ou não mantido em memória vai depender da política de cada banco de dados e geralmente pode ser configurada de acordo com o seu use case para definir a forma que melhor se adequa, podendo ser por exemplo os registros mais recentes, os registros mais acessados, etc.


