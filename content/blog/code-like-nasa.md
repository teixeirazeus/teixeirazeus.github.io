---
title: "Como programar como a NASA"
description: "10 regras usadas pela NASA para programar"
dateString: Jun 2023
draft: false
tags: ["programação", "nasa"]
weight: 106
cover:
    image: "/blog/code-like-nasa/nasa-logo.webp"
---

Atualmente estou pesquisando maneiras de deixar meu código cada vez mais robusto. Em uma dessas pesquisas, encontrei um técnica utilizada pela NASA para programar sistemas criticos. Achei bem interessante e resolvi compartilhar aqui.

A NASA, conhecida por seus rigorosos padrões de segurança e confiabilidade, segue uma série de diretrizes de codificação conhecidas como "The Power of 10 Rules". Essas regras foram concebidas por Gerard J. Holzmann do Laboratório de Propulsão a Jato da NASA.

## 1.Evite construções de fluxo complexas, como goto e recursão
O fluxo do código deve ser o mais simples possível, eliminando a utilização de goto e recursão.
A recursão e o comando goto podem tornar o código confuso e mais difícil de entender e manter. Isso pode levar a erros mais facilmente.

```c
// Ruim
void recursiveFunction(int n) {
    if (n == 0) return;
    recursiveFunction(n-1);
}

// Bom
void iterativeFunction(int n) {
    for(int i = 0; i < n; i++) {
        // Faça algo
    }
}
```

## 2. Todos os loops devem ter limites fixos. Isso previne a execução descontrolada do código
Loops com limites fixos ajudam a evitar loops infinitos que podem travar o sistema.

Digamos que você tenha uma lista ligada, e você está procurando um elemento nela. Uma abordagem padrão seria percorrer a lista até encontrar o elemento ou até atingir o final da lista.

```c
struct Node {
    int data;
    struct Node* next;
};

void searchLinkedList(struct Node* head, int target) {
    struct Node* current = head;
    
    while(current != NULL) {
        if(current->data == target) {
            printf("Elemento encontrado.\n");
            return;
        }
        current = current->next;
    }
    printf("Elemento não encontrado.\n");
}
```

No entanto, essa abordagem pode levar a um loop infinito se houver um erro no código que cria a lista ligada, como um loop na lista. Para evitar isso, podemos adicionar um limite superior no número de iterações.

```c
void searchLinkedList(struct Node* head, int target) {
    struct Node* current = head;
    int counter = 0;
    
    while(current != NULL && counter < 1000) {
        if(current->data == target) {
            printf("Elemento encontrado.\n");
            return;
        }
        current = current->next;
        counter++;
    }
    
    if(counter == 1000) {
        printf("Limite superior atingido. Verifique a lista.\n");
    } else {
        printf("Elemento não encontrado.\n");
    }
}
```

Este código irá parar de procurar o elemento depois de 1000 iterações, independentemente de ter encontrado o elemento ou não. Isso pode ajudar a prevenir um loop infinito caso haja um problema com a lista ligada.


## 3. Evite a alocação de memória no heap
Muito dos bugs vem do vazamento de memória. A alocação de memória no heap deve ser evitada sempre que possível. Utilize sempre alocação estática.

```c
// Ruim
int *p = (int*)malloc(sizeof(int)*10);

// Bom
int p[10];
```


## 4. Restrinja funções a uma única página impressa
Restringir as funções a uma única página impressa torna o código mais legível e mantém cada função focada em uma única tarefa. Aproximadamente 60 linhas de código.
Cada função deve fazer somente uma coisa.

## 5. Use no mínimo duas afirmações de tempo de execução por função
As afirmações de tempo de execução ajudam a identificar erros no código.

```c
void someFunction(int n) {
    assert(n > 0);
    // Faça algo
    assert(n == 0); // por exemplo, n deve ser zero depois de realizar a operação
}
```

## 6. Restrinja o escopo dos dados ao menor possível
Restringir o escopo dos dados pode prevenir erros e tornar o código mais seguro. Com isso você restringe o acesso a variáveis e funções somente onde elas são necessárias, diminuindo a chance de erros.

```c
void someFunction() {
    int localVar = 10; // só pode ser acessado dentro desta função
}
```

## 7. Verifique o valor de retorno de todas as funções não-void
Essa regra garante que erros não sejam ignorados e possam ser tratados adequadamente.

```c
// Bom
int result = someFunction();
if (result == ERROR) {
    // trate o erro
}
```

## 8. Use o pré-processador com moderação
O uso excessivo do pré-processador pode tornar o código difícil de ler, ofuscando o código real e confunde análises estáticas.

```c
// Ruim
#define SQUARE(x) ((x) * (x))

// Bom
int square(int x) {
    return x * x;
}
```

## 9. Limite o uso de ponteiros a uma única desreferência e não use ponteiros de função
Os ponteiros podem ser uma fonte de erros, portanto, seu uso deve ser minimizado. Eles não podem referenciar mais de uma camada de indireção e não devem ser usados para ponteiros de função.
Não use ponteiros de função, eles ofuscam o código e podem ser substituídos por funções.

```c
// Ruim
int** pp;
*(*pp) = 10;

// Bom
int* p;
*p = 10;
```

## 10. Todos os avisos devem ser tratados antes da liberação do software
Isso ajuda a identificar e corrigir possíveis problemas antes que o software seja lançado.

## Conclusão

Na minha visão as regras focam em uma coisa, simplicidade. Quando mais simples e objetivo for o código, mais fácil será de entender e manter. Estas regras com a prática de testes unitários, podem ser uma grande arma no arsenal de qualquer desenvolvedor que se importa com a qualidade do seu código.

Se quiser se aprofundar no tópico, você pode ler o [documento da NASA](https://github.com/stanislaw/awesome-safety-critical/blob/master/Backup/JPL_Coding_Standard_C.pdf) que contém maiores detalhes sobre a sua padronização de código.

## Referências
https://github.com/stanislaw/awesome-safety-critical/blob/master/Backup/JPL_Coding_Standard_C.pdf

https://en.wikipedia.org/wiki/The_Power_of_10:_Rules_for_Developing_Safety-Critical_Code

https://betterprogramming.pub/the-power-of-10-nasas-rules-for-coding-43ae1764f73d