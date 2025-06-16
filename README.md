#include <stdio.h>
#include <stdlib.h>
#include <time.h>
#include <string.h>

typedef struct {
    char nome[50];
    int ataque;
    int defesa;
    int magia;
} Carta;

void preencherBaralho(Carta baralho[], int tamanho) {
    strcpy(baralho[0].nome, "Dragao Vermelho");
    baralho[0].ataque = 95;
    baralho[0].defesa = 80;
    baralho[0].magia = 70;

    strcpy(baralho[1].nome, "Gigante de Pedra");
    baralho[1].ataque = 80;
    baralho[1].defesa = 98;
    baralho[1].magia = 30;

    strcpy(baralho[2].nome, "Mago Eletrico");
    baralho[2].ataque = 75;
    baralho[2].defesa = 60;
    baralho[2].magia = 99;

    strcpy(baralho[3].nome, "Elfo Arqueiro");
    baralho[3].ataque = 88;
    baralho[3].defesa = 70;
    baralho[3].magia = 65;
    
    strcpy(baralho[4].nome, "Goblin Ladrao");
    baralho[4].ataque = 60;
    baralho[4].defesa = 40;
    baralho[4].magia = 20;

    strcpy(baralho[5].nome, "Hidra de Lerna");
    baralho[5].ataque = 92;
    baralho[5].defesa = 85;
    baralho[5].magia = 50;
}

void mostrarCarta(Carta carta) {
    printf("\nSua carta:\n");
    printf("Nome: %s\n", carta.nome);
    printf("1. Ataque: %d\n", carta.ataque);
    printf("2. Defesa: %d\n", carta.defesa);
    printf("3. Magia: %d\n", carta.magia);
}

int main() {
    Carta baralho[6];
    Carta cartaJogador, cartaComputador;
    int escolha, indiceJogador, indiceComputador;
    
    srand(time(NULL));

    preencherBaralho(baralho, 6);

    indiceJogador = rand() % 6;
    cartaJogador = baralho[indiceJogador];

    do {
        indiceComputador = rand() % 6;
    } while (indiceComputador == indiceJogador);
    cartaComputador = baralho[indiceComputador];

    printf("--- Super Trunfo Batalha ---\n");
    
    mostrarCarta(cartaJogador);
    
    printf("\nEscolha um atributo (1-3): ");
    scanf("%d", &escolha);
    
    printf("\nCarta do Computador:\n");
    printf("Nome: %s\n", cartaComputador.nome);
    printf("Ataque: %d\n", cartaComputador.ataque);
    printf("Defesa: %d\n", cartaComputador.defesa);
    printf("Magia: %d\n", cartaComputador.magia);

    int valorJogador, valorComputador;

    if (escolha == 1) {
        valorJogador = cartaJogador.ataque;
        valorComputador = cartaComputador.ataque;
        printf("\nVoce escolheu Ataque!\n");
    } else if (escolha == 2) {
        valorJogador = cartaJogador.defesa;
        valorComputador = cartaComputador.defesa;
        printf("\nVoce escolheu Defesa!\n");
    } else if (escolha == 3) {
        valorJogador = cartaJogador.magia;
        valorComputador = cartaComputador.magia;
        printf("\nVoce escolheu Magia!\n");
    } else {
        printf("\nOpcao invalida. Voce perdeu a rodada.\n");
        return 1;
    }
    
    printf("Seu valor: %d vs Valor do Computador: %d\n", valorJogador, valorComputador);

    if (valorJogador > valorComputador) {
        printf("\n*** Voce venceu! ***\n");
    } else if (valorJogador < valorComputador) {
        printf("\n--- Voce perdeu! ---\n");
    } else {
        printf("\n=== Empate! ===\n");
    }

    return 0;
}
