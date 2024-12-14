# #Definição

Fila é uma estrutura de dado que funciona com base em FIFO first in first out. Dessa maneira, ao colocar um elemento na fila ele sempre será colocado na extremidade denominada de final. De forma análoga, caso um elemento seja retirado da lista ele sairá de uma extremidade chamada de começo.

# #Estrutura

A estrutura da fila é muito semelhante com a estrutura da pilha e os métodos implementados aqui realizam funções semelhantes que os métodos da pilha, mas claro que possuem suas diferenças em relação ao funcionamento dessa estrutura no código em si.

#  #CODE
## *fila.h*
	
```
##define MAX 100

typedef struct fila
{
    int inicio;
    int fim;
    int vetor[MAX];
} Fila;

Fila* cria_fila();
int inserir(Fila* f, int v);
int retirar(Fila* f, int* v);
int esta_vazio(Fila* f);
int esta_cheio(Fila* f);
void libera_fila(Fila* f);
```

* esta_vazio -> Será verdade caso começo e o final estejam na mesma posição.
- esta_cheio -> Será verdade caso o final esteja na última posição.
- inserir:
	-  Verifica se a fila está cheia;
	-  Se não, adiciona um item a fila na posição indica pelo final;
	-  Incrementa final;
	-  Retorna 1 se inseriu e 0 caso não.
- retirar:
	- Verificar se a fila está vazia;
	- Se não, retira o elemento indicado pelo começo;
	- incrementa inicio;
	-  Retorna 1 se retirou e 0 caso não.
## *fila.c*

```
#include <stdio.h>
#include <stdlib.h>
#include "test.h"

Fila* cria_fila() {
    Fila* f = (Fila*)malloc(sizeof(Fila)){
    if (f != NULL) {
        f->inicio = 0;
        f->fim = 0;
    }
    return f;
}

int inserir(Fila* f, int v)
{
    if (!esta_cheio(f))
    {
        f -> vetor[f->fim++] = v;
        return 1;      
    }
    return 0;
}

int retirar(Fila* f, int* v)
{
    if (!esta_vazio(f))
    {
        *v = f -> vetor[f->inicio++];
        return 1;
    }
    return 0;
}

int esta_vazio(Fila* f)
{
    return(f->inicio == f->fim);
}

int esta_cheio(Fila* f)
{
    return(f->fim == MAX);
}

void libera_fila(Fila* f)
{
    free(f);
}
```

## *main.c*

```
#include <stdio.h>
#include <stdlib.h>
#include "test.h"

int main()
{
    Fila* fila = cria_fila();
    
    inserir(fila, 10);
    inserir(fila, 20);
    inserir(fila, 30);
    
    int x;
    retirar(fila, &x);

    printf("Elemento %d retirado\n", x);
    system("PAUSE");

    return 0;

}
```

# Fila Circular

Diferentemente da fila normal, aqui é como se dobrassemos um vetor unindo suas pontas (começo e final). O ponto positivo dessa ação é reutilizar eficientemente o espaço quando elementos são removidos, ao invés de só deixar posições "vazias". A parte importante é entender a forma como tratar uma fila circular bem como sua organização física, pois a interface é praticamente a mesma da fila normal.
A única diferença estrutural entre as duas filas pode ser resumida em uma variável size (tamanho). Essa variável vai ser responsável por:
1. Limitar o uso de memória: Indica quantos elementos podem ser armazenados ao mesmo tempo;
2. Detectar as condições de fila cheia e vazia: 
- A fila está cheia quando:

```
	(fim + 1) % tamanho == inicio;	
```
Isso ocorre porque o próximo espaço de inserção está no mesmo índice do início, ou seja, não há mais espaço disponível.

- A fila está vazia quando:
```
	inicio == fim;
```

3. Controlar a alocação de memória fixa
4. Calculo eficiente do número de elementos da fila:
```
	int currentsize = (fim - inicio + tamanho ) % tamanho;   
```

## Implementação de uma fila circular

### fila.h
```
#ifndef CIRCULAR_QUEUE_H 
#define CIRCULAR_QUEUE_H 
#define tamanho 5 

typedef struct { 
	int items[tamanho]; 
	int inicio, fim, tamanho; 
} fila;


void inicializarFila(Fila *f); // Inicializa a fila 
int filaCheia(Fila *f); // Verifica se a fila está cheia 
int filaVazia(Fila *f); // Verifica se a fila está vazia 
void enfileirar(Fila *f, int valor); // Insere um elemento na fila 
int desenfileirar(Fila *f); // Remove um elemento da fila 
void exibirFila(Fila *f); // Exibe os elementos da fila

#endif // CIRCULAR_QUEUE_H



```

### fila.c
```
#include <stdio.h>
#include "circular_queue.h"

// Inicializa a fila
void inicializarFila(Fila *f) {
    f->inicio = 0;
    f->fim = 0;
    f->quantidade = 0;
}

// Verifica se a fila está cheia
int filaCheia(Fila *f) {
    return f->quantidade == TAMANHO;
}

// Verifica se a fila está vazia
int filaVazia(Fila *f) {
    return f->quantidade == 0;
}

// Insere um elemento na fila
void enfileirar(Fila *f, int valor) {
    if (filaCheia(f)) {
        printf("A fila está cheia!\n");
        return;
    }
    f->itens[f->fim] = valor;
    f->fim = (f->fim + 1) % TAMANHO;
    f->quantidade++;
}

// Remove um elemento da fila
int desenfileirar(Fila *f) {
    if (filaVazia(f)) {
        printf("A fila está vazia!\n");
        return -1; // Retorna um valor indicando erro
    }
    int valor = f->itens[f->inicio];
    f->inicio = (f->inicio + 1) % TAMANHO;
    f->quantidade--;
    return valor;
}

// Exibe os elementos da fila
void exibirFila(Fila *f) {
    if (filaVazia(f)) {
        printf("A fila está vazia!\n");
        return;
    }
    int i = f->inicio;
    for (int contador = 0; contador < f->quantidade; contador++) {
        printf("%d ", f->itens[i]);
        i = (i + 1) % TAMANHO;
    }
    printf("\n");
}

```

### main.c
```
#include <stdio.h>
#include "circular_queue.h"

int main() {
    Fila fila;
    inicializarFila(&fila);

    // Testando a fila circular
    enfileirar(&fila, 10);
    enfileirar(&fila, 20);
    enfileirar(&fila, 30);
    exibirFila(&fila); // Deve exibir: 10 20 30

    printf("Removido: %d\n", desenfileirar(&fila)); // Remove 10
    exibirFila(&fila); // Deve exibir: 20 30

    enfileirar(&fila, 40);
    enfileirar(&fila, 50);
    exibirFila(&fila); // Deve exibir: 20 30 40 50

    enfileirar(&fila, 60); // Tentativa de inserir com a fila cheia
    printf("Removido: %d\n", desenfileirar(&fila)); // Remove 20
    exibirFila(&fila); // Deve exibir: 30 40 50

    return 0;
}

```

## Operações Básicas 
### Inserção: 
1. Verificar se a fila está cheia: 

2. Caso não esteja cheia:
	- Incrementar `fim`.
	- Adicionar o elemento no índice `fim`.

### Remoção:
1. Verifica se tá vazia
2. Caso não:
	- incrementa inicio
	- Retorna o elemento do indice inicio
