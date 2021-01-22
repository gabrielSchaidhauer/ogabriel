---
layout: post
title: "Bancos de dados Chave e Valor"
description: "Entenda o que são, onde vivem e do que se alimentam"
date: 21/01/2021
feature_image: images/kv-database/lockers.jpg
language: pt-BR
tags: [data%pt-BR%, nosql%pt-BR%]
---

Bancos de dados chave e valor são uma categoria que abrange uma série de bancos de dados NoSQL com caracteristicas similares. Apesar disto, nem todos são iguais, aqui vou explicar um pouco algumas destas caracteriscas sem entrar a fundo nas especificidades de cada um.

<!--more-->

Para saber mais sobre a definição de um banco de dados NoSQL, as diferenças que eles tem de um banco de dados relacional sobre o teorema de CAP entre outras coisas, um ótimo livro chamado [NoSQL Distilled](https://www.amazon.com.br/NoSQL-Distilled-Emerging-Polyglot-Persistence-ebook/dp/B0090J3SYW/ref=tmm_kin_swatch_0?_encoding=UTF8&qid=&sr=). Este livro abrange conceitos iniciais e gerais de bancos de dados NoSQL e serve como uma ótima base para quem quer aprender um pouco mais.

## Como assim chave e valor?

Um banco de dados chave valor é uma estrutura para armazenamento de dados que funciona de forma muito similar a uma estrutura de dados do tipo mapa ou um dicionário, onde nós temos uma chave. Esta chave é um identificador para um registro é como se ela guardasse o endereço para o valor que procuramos. 


Utilizando uma analogia simples, pode se dizer que uma estrutura deste tipo é como a recepção de uma pousada. Ao chegar na pousada e informar a hospedagem a recepção nos entrega uma chave e esta chave contem o numero do nosso quarto e nos permite acessá-lo. De posse desta chave sempre conseguimos ter um acesso direto ao nosso quarto.

{% include image_caption.html imageurl="/images/kv-database/reception.jpg" title="Hotel Reception" caption="" %}

Trazendo isto um pouco mais perto da linguagem técnica podemos pensar também em um *[JSON](https://pt.wikipedia.org/wiki/JSON)*:

```json
{
    "quartos": [
        {"306": {"camas": 2, "tv": "LCD"}},
        {"206": {"camas": 1, "tv": "LCD"}}
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

{% include image_caption.html imageurl="/images/kv-database/bookshelf.jpg" title="Bookshelf" caption="" %}

Estes bancos geralmente não trabalham somente com armazenamento em disco, especialmente visto que o objetivo deles em geral é ter ótima performance, sendo assim normalmente eles trabalham com uma abordagem mais híbrida. Esta abordagem funciona, em geral, de forma que toda informação quando é gravada é persistida no disco porém uma parte dela é mantida em memória para que o acesso seja rápido.

O que é ou não mantido em memória vai depender da política de cada banco de dados e geralmente pode ser configurada de acordo com o seu caso de uso para definir a forma que melhor se adequa, podendo ser por exemplo os registros mais recentes, os registros mais acessados, etc.

Ao utilizar esta abordagem não temos mais a limitação da quantidade de informação com base na memória RAM e sim no disco que geralmente é bem maior, bem como, caso ocorra alguma falha de hardware ou algo que ocasione a perda dos dados em memória, as informações não serão perdidas, visto que poderão ser recuperadas do disco. O ponto negativo disto é que em situações que fujam ao que foi configurado teremos o tempo de acesso ao disco adicionado as nossas buscas.

Á exceção de cenários específicos onde realmente podemos estar dispostos a perder as informações esta abordagem geralmente é preferível, visto que os tempos de acesso a disco vem se tornando cada vez mais rápidos a medida que tecnologias como SSD vão surgindo e sendo aprimoradas o que torna os contras desta abordagem cada vez menos relevantes face as vantagens presentes.

## Modelagem

{% include image_caption.html imageurl="/images/kv-database/modeling.jpg" title="Modeling" caption="" %}

Quando já temos uma bagagem de um banco relacional, como geralmente é comum, temos a tendência de pensar a modelagem de dados de uma forma e as queries que iremos fazer decidimos depois. Isto ocorre, pois os bancos relacionais, trabalhando com sql, nos oferecem uma infinidade de formas de localizar um registro ou uma série de registros.

Em geral isto não ocorre com bancos do tipo chave e valor, o modelo ideal de acesso é quando temos acesso direto a chave sem precisar realizar nenhum tipo de consulta. Na maior parte dos bancos, não é possivel realizar uma busca dentro do valor. Para exemplificar imagine uma biblioteca que organiza os livros de forma que sempre é possivel localizá-los se souber a estante e a prateleira.

```json
{
    "biblioteca": {
        "A10:3": [
            {
                "titulo": "Harry Potter e a Ordem da Fenix",
                "autor": "J.K. Rowling"
            },
            {
                "titulo": "Harry Potter e a Pedra Filosofal",
                "autor": "J.K. Rowling"
            }
        ],
        "A10:12": [
            {
                "titulo": "O Hobbit",
                "autor": "J.R.R. Tolkien"
            },
            {
                "titulo": "O Senhor dos Anéis: A Sociedade do Anel",
                "autor": "J.R.R. Tolkien"
            }
        ]
    }
}
```

Caso saibamos qual a estante e a prateleira podemos localizar os livros da série Harry Potter por exemplo, porém náo conseguimos realizar uma consulta para saber em quais estantes estão os livros de J.R.R. Tolkien.

Portanto, com este tipo de banco de dados é importante que realizemos a modelagem dos dados já pensando na maneira como irá ser o acesso deles. É muito importante que se pense muito bem em qual será a chave usada para estes dados pois ela será essencial na forma de acessá-los.

Outro ponto importante sobre modelagem é o uso de aggregates, porém como isto não é um tema especifico de bancos chave e valor não vai ser alvo deste artigo, quem tiver interesse pode ler um pouco mais sobre isto [aqui](https://arleypadua.medium.com/porque-armazenar-agregados-com-nosql-b2a460ffe18a).

## Quando usar?

Como já vimos, bancos chave e valor, em geral, possuem um modelo de acesso as informações que estão salvas que é dependende do conhecimento prévio da chave que guarda estas informações. Este modelo de acesso faz com que existam cenários onde não conseguimos utilizar este banco de dados da melhor forma possível. Imagine buscar uma lista de produtos onde para exibir a informação de cada produto precisariamos consultar novamente o banco de dados? Então, essa seria a combinação de uma má modelagem com um use case que não combina com o tipo de banco.

Comumente bancos de dados chave e valor são usados para armazenar informações que sejam uma unidade, tais como dados de sessão de um usuário, preferências de um usuário, carrinho de compras em e-commerce, etc.

Obviamente o que indica se o seu caso de uso vai se adequar a um banco de dados deste tipo depende do seu modelo de negócio e da modelagem do banco e do sistema, pode ser que no seu cenário seja possivel ter uma lista de produtos por exemplo associada a uma chave especifica, mas é sempre importante estar atento a forma como vai se buscar os dados face as limitações deste tipo de banco.

## Conclusão

Bancos de dados chave e valor são ferramentas poderosas e não vou serei eu que irei fugir do clichê de dizer que com grandes poderes vem grandes responsabilidades. Ao trabalhar com este tipo de banco precisamos avaliar com muita calma se ele é o ideal para o nosso caso, Qual a forma de armazenamento que vamos usar, se o banco que estamos pensando oferece mais de uma opção, se temos uma ideia da modelagem que iremos aplicar com base nos usos que vamos ter, etc. Mesmo com tudo isso eles tem muitas vantagens em performance, volume de acessos e dados, então se ainda não conhecem vale a pena experimentar!

Se tiver um tempinho me diga o que achou e vamos conversar!

Happy Coding!
