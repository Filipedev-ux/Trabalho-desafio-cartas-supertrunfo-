#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <time.h>
#include <unistd.h>

typedef struct {
    char nome[50];
    int forca;
    int velocidade;
    int agilidade;
    char grupo;
} Carta;

void limparTela() {
#ifdef _WIN32
    system("cls");
#else
    system("clear");
#endif
}

int main() {
    Carta baralho[20] = {
        {"Superman", 95, 90, 85, 'A'},
        {"Flash", 70, 100, 95, 'A'},
        {"Batman", 85, 70, 90, 'A'},
        {"Mulher-Maravilha", 90, 80, 88, 'A'},
        {"Coringa", 75, 65, 80, 'A'},
        {"Lex Luthor", 80, 60, 70, 'A'},
        {"Homem-Aranha", 88, 85, 98, 'B'},
        {"Hulk", 98, 60, 50, 'B'},
        {"Capitao America", 86, 75, 92, 'B'},
        {"Thanos", 99, 50, 60, 'B'},
        {"Homem de Ferro", 92, 88, 85, 'B'},
        {"Loki", 82, 70, 88, 'B'},
        {"Goku", 97, 96, 94, 'C'},
        {"Vegeta", 96, 95, 93, 'C'},
        {"Freeza", 94, 90, 89, 'C'},
        {"Cell", 93, 89, 91, 'C'},
        {"Majin Boo", 95, 85, 80, 'C'},
        {"Pikachu", 70, 80, 90, 'D'},
        {"Charizard", 85, 78, 82, 'D'},
        {"Mewtwo", 92, 90, 88, 'D'}
    };
    baralho[9].grupo = 'S'; // Thanos é a Super Trunfo

    srand(time(NULL));
    for (int i = 19; i > 0; i--) {
        int j = rand() % (i + 1);
        Carta temp = baralho[i];
        baralho[i] = baralho[j];
        baralho[j] = temp;
    }

    Carta maoJogador[20], maoComputador[20];
    int nJogador = 10, nComputador = 10;
    
    for(int i = 0; i < 10; i++) {
        maoJogador[i] = baralho[i];
        maoComputador[i] = baralho[i + 10];
    }
    
    int rodada = 1;
    int escolha;
    int turnoDoJogador = 1;

    while (nJogador > 0 && nComputador > 0) {
        limparTela();
        printf("--- RODADA %d ---\n", rodada);
        printf("PLACAR: Jogador (%d cartas) vs Computador (%d cartas)\n", nJogador, nComputador);
        printf("--------------------------------------------------\n");

        Carta cartaJogador = maoJogador[0];
        Carta cartaComputador = maoComputador[0];

        if(turnoDoJogador) {
            printf("Sua carta: %s [Grupo: %c]\n", cartaJogador.nome, cartaJogador.grupo);
            printf("1. Forca: %d\n", cartaJogador.forca);
            printf("2. Velocidade: %d\n", cartaJogador.velocidade);
            printf("3. Agilidade: %d\n", cartaJogador.agilidade);
            printf("Escolha um atributo (1-3): ");
            scanf("%d", &escolha);
            while (getchar() != '\n');
        } else {
            printf("O computador esta escolhendo...\n");
            escolha = (rand() % 3) + 1;
            sleep(2);
        }

        printf("\n-- COMPARACAO --\n");
        printf("Sua carta: %s (%s: %d)\n", cartaJogador.nome,
               (escolha == 1) ? "Forca" : (escolha == 2) ? "Velocidade" : "Agilidade",
               (escolha == 1) ? cartaJogador.forca : (escolha == 2) ? cartaJogador.velocidade : cartaJogador.agilidade);
        printf("Carta do Computador: %s (%s: %d)\n", cartaComputador.nome,
               (escolha == 1) ? "Forca" : (escolha == 2) ? "Velocidade" : "Agilidade",
               (escolha == 1) ? cartaComputador.forca : (escolha == 2) ? cartaComputador.velocidade : cartaComputador.agilidade);
        printf("----------------\n");

        int jogadorVenceu = 0;
        int empate = 0;
        
        if (cartaJogador.grupo == 'S' && cartaComputador.grupo != 'S') {
            jogadorVenceu = 1;
        } else if (cartaComputador.grupo == 'S' && cartaJogador.grupo != 'S') {
            jogadorVenceu = 0;
        } else if (cartaJogador.grupo == cartaComputador.grupo || cartaJogador.grupo == 'S' || cartaComputador.grupo == 'S') {
            int atrJogador = (escolha == 1) ? cartaJogador.forca : (escolha == 2) ? cartaJogador.velocidade : cartaJogador.agilidade;
            int atrComputador = (escolha == 1) ? cartaComputador.forca : (escolha == 2) ? cartaComputador.velocidade : cartaComputador.agilidade;
            if (atrJogador > atrComputador) {
                jogadorVenceu = 1;
            } else if (atrJogador < atrComputador) {
                jogadorVenceu = 0;
            } else {
                empate = 1;
            }
        } else if (cartaJogador.grupo == 'A' && cartaComputador.grupo == 'B') {
            jogadorVenceu = 1;
        } else if (cartaJogador.grupo == 'B' && cartaComputador.grupo == 'C') {
            jogadorVenceu = 1;
        } else if (cartaJogador.grupo == 'C' && cartaComputador.grupo == 'D') {
            jogadorVenceu = 1;
        } else if (cartaJogador.grupo == 'D' && cartaComputador.grupo == 'A') {
            jogadorVenceu = 1;
        } else {
            jogadorVenceu = 0;
        }

        if (empate) {
            printf("Resultado: EMPATE!\n");
            turnoDoJogador = !turnoDoJogador; 
            
            maoJogador[nJogador] = cartaJogador;
            maoComputador[nComputador] = cartaComputador;
        } else if (jogadorVenceu) {
            printf("Resultado: VOCE VENCEU A RODADA!\n");
            turnoDoJogador = 1;
            
            maoJogador[nJogador] = cartaJogador;
            maoJogador[nJogador + 1] = cartaComputador;
            nJogador++;
            nComputador--;
        } else {
            printf("Resultado: O COMPUTADOR VENCEU A RODADA!\n");
            turnoDoJogador = 0;

            maoComputador[nComputador] = cartaComputador;
            maoComputador[nComputador + 1] = cartaJogador;
            nComputador++;
            nJogador--;
        }
        
        for (int i = 0; i < nJogador; i++) maoJogador[i] = maoJogador[i+1];
        for (int i = 0; i < nComputador; i++) maoComputador[i] = maoComputador[i+1];

        printf("\nPressione Enter para continuar...");
        while (getchar() != '\n');
        rodada++;
    }
    
    limparTela();
    printf("========== FIM DE JOGO ==========\n");
    if (nJogador > 0) {
        printf("PARABENS! VOCE VENCEU O JOGO!\n");
    } else {
        printf("QUE PENA! O COMPUTADOR VENCEU.\n");
    }
    printf("=================================\n");

    return 0;
}
