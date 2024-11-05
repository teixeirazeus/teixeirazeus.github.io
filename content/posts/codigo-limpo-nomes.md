---
author:
  name: "Thiago S. Teixeira"
date: 2023-01-01
linktitle: codigo-limpo-nomes
type:
- post
- posts
tags: ["Clean Code", "Python"]
title: "Código limpo: nomes"
description: "Como nomear variáveis, funções e classes"
weight: 106
---

![Código limpo: nomes](/static//blog/codigo-limpo-nomes/cover.jpg)

Este é meu primeiro post, onde pretendo explorar um pouco sobre a questão do código limpo. Uma mistura da literatura da computação e minhas reflexões como programador.

### Comunicação

Vivi grande parte da minha vida em São Paulo capital, tenho o sotaque e até utilizo algumas gírias paulistas. Quando me empolgo, fico parecendo o Boça do Hermes e Renato. _Puta minigame da hora, meu!_.

Estava na fase do vestibular e acabei passando no curso de ciência da computação na UTFPR de Ponta Grossa, Paraná. Novos ares, nova cultura e nova linguagem?!

Em um dos meus primeiros contatos com os pontagrossenses, ouvia frases como “_Tinha dois piá ali, descendo a ladeira!_”. E eu pensava comigo “_Pia? Aquelas de banheiro? Como assim?!_”.

Não cheguei a questionar as pessoas na hora, fui compreendendo o que elas queriam dizer **através do contexto**. 

Repare como a mente humana é incrível:

No meio de toda uma conversa, ouvi uma frase que não fazia sentido logico. Rapidamente consegui levantar uma suposição que meu interlocutor estava correto, e eu, por estar em outro Estado, devo ter me deparado com uma gíria que ainda não conheço. Um modo de nomear algo diferente da forma que eu utilizo.

Após continuar ouvindo variadas utilizações da palavra “piá”, finalmente consegui chegar a conclusão que se tratava da palavra “menino”.

A mensagem foi interpretada mesmo com ruído. Porém, repare como minha mente precisou dar toda uma volta, perder energia para compreender uma simples mensagem. E se minha conclusão estivesse errada? O único modo de saber se seu estava correto, era perguntar ao meu interlocutor o que era o tal “piá”. Não seria melhor utilizar a palavra “menino”?

### Intenção

O equivalente em código seria eu não compreender o nome de algo, como uma variável, e ser obrigado a ler todo o código para entender o contexto de aplicação daquele nome.

Como programadores, temos que escrever código que sejam fáceis de ler e fácil compreensão para todos. Isto inclui seguir um padrão de nomes daquela linguagem que seja amplamente aceita.

Em python temos que todas a variáveis deve ser em minusculo, separado por underline.

```python
full_name = "Fulano da Silva" # correto
fullName = "Fulano da Silva" # errado
```

Mas é preciso mais do que isso, temos que escolher bem o nome para ser clara nossa intenção. Qual é o objetivo dessa variável? Para que ela é utilizada? Quem ler vai entender o porquê dela estar ali?

```python
enable = False
i = 0 # index do loop
index = 0 # index da lista
l = [1,2,3]
```

Enable? Eu estou ativando o que alí?

Index do loop? Que loop? Para fazer o que? Para que colocar o comentário? Não é mais fácil já nomear loop_index?

Se você precisa colocar um comentário ao lado para explicar o nome da variável, é bem provável que você tenha falhado na escolha do nome.

```python
has_even_number = False
loop_index = 0
numbers_index = 0
numbers = [1, 2, 3]
```

Repare que agora, mesmo não olhando o resto do código, é possível entender como será o programa. Tem dois inteiros, um para um loop e outro para mapear a lista numbers, junto com um boleano que deve ser acionado ao detectar um número par na lista.

Tudo isso sem ler a lógica. Já é possível ter uma boa noção de como essas variáveis serão utilizadas pelo programa.

### Pronunciável

O código bem escrito possui a mesma fluidez da leitura de um texto bem estruturado. Para isto é necessário escolher nomes pronunciáveis, que possibilitem esse tipo de leitura.
```python
qty_pokeball = 10 
n_pk = 10
```

QTY pokeball? Não é melhor colocar em extenso quantity?

n_pk? Deve ser algo como _number of pokeball_, mas eu só consigo supor isto através do contexto. Poderia ser algo como _number of players killed_, ou algo assim.

```python
if wild_pokemon.is_rare and player.has_pokeball:
    player.throw_pokeball(wild_pokemon)
```

Repare como é possível ler o código como se fosse um texto simples. Se o pokemon selvagem for raro e o jogador tiver pokebolas, o jogador joga uma pokebola no pokemon.

Esta é umas das razões que eu prefiro sempre utilizar nomes em inglês, pois se trata de uma língua universal e fica muito mais harmônica no código. Já que a maioria dos módulos são escritos em inglês e a própria sintaxe da linguagem de programação é em inglês.

### Encoding

Encoding é uma técnica para nomeação de variáveis com tags, normalmente dizendo seu tipo. Hoje, com todas as facilidades de uma interface de desenvolvimento, fica muito mais fácil saber o tipo de cada variável. Deixando o encoding apenas um ruído.

Saber o tipo de cada variável é muito importante para evitar erros. O programador tem uma maior noção de como manipular aquele pedaço de código.

No caso do python, todas as variáveis são dinâmicas. Porém, há um modo de especificar o tipo de variável em funções.

No caso a baixo, a função recebe um argumento do tipo Pokemon e retorna um booleano.

```python
def is_eletric(pokemon: Pokemon) -> bool:
    return pokemon.type == 'eletric'
```


O interpretador python não irá fazer nenhuma checagem de tipo, esse tipo de notação serve apenas para o programador. Exceto seja explicitamente usando a função _isinstance_.

### Tamanho

Qual é o tamanho ideal para nomes? Qual o limite descritivo que ele suporta? A chave está no escopo.

Variáveis podem ser pequenas se o escopo delas for pequeno.

```python
for i in range(10):
    print(i*"Ha", end="!\n")
```

Em um caso de um for pequeno, logo ao bater o olho já é possível entender a utilização da variável. Ela está próxima de sua declaração. Então, no final não tem muita importância a escolha de um nome mais longo.

Variáveis com grande escopo devem ter nomes grandes.

Já uma variável com um grande escopo, será utilizada por várias partes do programa. Será inserida em vários contextos, varias funções. Logo ela deve ter um bom longo nome, principalmente para uma variável global.

Para função e métodos, a regra é invertida. Nome pequeno para grandes escopos e grande para pequenos escopos. O mesmo vale para nome de classes.

Funções publicas verão chamadas por várias partes do programa, então ter um nome pequeno as torna muito mais convenientes.

Funções privadas são chamadas apenas dentro de sua classe, então ela tem uma folga maior para o tamanho do nome. Normalmente elas que fazem todo o trabalho duro e mais específico para as funções públicas, algo para discutir no próximo post sobre funções.

### Conclusão

Nomes são uma importante ferramenta para comunicação, que quando utilizados de maneira correta deixam o código muito mais legível.

Se você deseja se aprofundar mais no tópico, recomendo o livro Código limpo, que está sendo minha referência para este post.

Código Limpo: [https://amzn.to/3joqHkd](https://amzn.to/3joqHkd)