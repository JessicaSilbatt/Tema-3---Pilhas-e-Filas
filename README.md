#include <stdio.h>
#include <stdlib.h>
#include <time.h>

#define TAM 5

typedef struct {
    char tipo;
    int id;
} Peca;

typedef struct {
    Peca fila[TAM];
    int inicio, fim, qtd;
} Fila;

void inicializar (Fila *f) {
    f->inicio = 0;
    f->fim = 0;
    f->qtd = 0;
}

int estaCheia (Fila *f) {
    return f->qtd == TAM;
}

int estaVazia (Fila *f) {
    return f->qtd == 0;
}

void inserirPeca (Fila *f, Peca p) {
    if (estaCheia(f)) {
        printf("A fila está cheia!\n");
        return;
    }
    f->fila[f->fim] = p;
    f->fim = (f->fim + 1) % TAM;
    f->qtd++;
}

Peca jogarPeca (Fila *f) {
    Peca p = {'-', -1};
    if (estaVazia(f)) {
        printf("A fila está vazia!\n");
        return p;
    }
    p = f->fila[f->inicio];
    f->inicio = (f->inicio + 1) % TAM;
    f->qtd--;
    return p;
}

void mostrarFila (Fila *f) {
    printf("\nFila de peças:\n");
    if (estaVazia(f)) {
        printf("[vazia]\n");
        return;
    }
    for (int i = 0; i < f->qtd; i++) {
        int pos = (f->inicio + i) % TAM;
        printf("[%c %d] ", f->fila[pos].tipo, f->fila[pos].id);
    }
    printf("\n");
}

Peca gerarPeca (int id) {
    char tipos[] = {'I', 'O', 'T', 'L'};
    Peca p;
    p.tipo = tipos[rand() % 4];
    p.id = id;
    return p;
}

int main () {
    Fila fila;
    inicializar(&fila);
    srand(time(NULL));
    int id = 0, opcao;
    Peca p;

    for (int i = 0; i < TAM; i++) {
        inserirPeca(&fila, gerarPeca(id++));
    }

    do {
        mostrarFila(&fila);
        printf("\n1 - Jogar peça\n");
        printf("2 - Inserir nova peça\n");
        printf("0 - Sair\n");
        printf("Escolha uma opção: ");
        scanf("%d", &opcao);

        if (opcao == 1) {
            p = jogarPeca(&fila);
            if (p.id != -1)
                printf("Peça jogada: [%c %d]\n", p.tipo, p.id);
        } else if (opcao == 2) {
            if (!estaCheia(&fila)) {
                p = gerarPeca(id++);
                inserirPeca(&fila, p);
                printf("Nova peça inserida: [%c %d]\n", p.tipo, p.id);
            } else {
                printf("A fila está cheia!\n");
            }
        }
    } while (opcao != 0);

    printf("Programa encerrado.\n");

    return 0;
}
