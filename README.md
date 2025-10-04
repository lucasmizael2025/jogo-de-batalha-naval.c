#include <stdio.h>
#include <stdlib.h> 

#define TAM 10       
#define NAVIO 3      
#define AGUA 0       
#define HABILIDADE 5 
#define TAM_HAB 5   


void exibirTabuleiro(int tabuleiro[TAM][TAM]) {
    for (int i = 0; i < TAM; i++) {
        for (int j = 0; j < TAM; j++) {
            if (tabuleiro[i][j] == AGUA) printf("~ ");
            else if (tabuleiro[i][j] == NAVIO) printf("N ");
            else if (tabuleiro[i][j] == HABILIDADE) printf("* ");
            else printf("%d ", tabuleiro[i][j]); // fallback
        }
        printf("\n");
    }
}

/* --- gera um "cone" apontando para baixo em uma matriz 5x5 --- */
void gerarHabilidadeCone(int hab[TAM_HAB][TAM_HAB]) {
    for (int i = 0; i < TAM_HAB; i++) {
        for (int j = 0; j < TAM_HAB; j++) {
            if (j >= TAM_HAB/2 - i && j <= TAM_HAB/2 + i) hab[i][j] = 1;
            else hab[i][j] = 0;
        }
    }
}


void gerarHabilidadeCruz(int hab[TAM_HAB][TAM_HAB]) {
    for (int i = 0; i < TAM_HAB; i++) {
        for (int j = 0; j < TAM_HAB; j++) {
            if (i == TAM_HAB/2 || j == TAM_HAB/2) hab[i][j] = 1;
            else hab[i][j] = 0;
        }
    }
}


void gerarHabilidadeOctaedro(int hab[TAM_HAB][TAM_HAB]) {
    int centro = TAM_HAB / 2;
    for (int i = 0; i < TAM_HAB; i++) {
        for (int j = 0; j < TAM_HAB; j++) {
            if (abs(i - centro) + abs(j - centro) <= centro) hab[i][j] = 1;
            else hab[i][j] = 0;
        }
    }
}


void aplicarHabilidade(int tab[TAM][TAM], int hab[TAM_HAB][TAM_HAB], int origemLinha, int origemColuna) {
    int meio = TAM_HAB / 2;
    for (int i = 0; i < TAM_HAB; i++) {
        for (int j = 0; j < TAM_HAB; j++) {
            int linha = origemLinha - meio + i;
            int coluna = origemColuna - meio + j;
            if (linha >= 0 && linha < TAM && coluna >= 0 && coluna < TAM) {
                if (hab[i][j] == 1 && tab[linha][coluna] == AGUA) {
                    tab[linha][coluna] = HABILIDADE;
                }
            }
        }
    }
}

int main(void) {
    
    int matriz3[3][3] = {
        {1,2,3},
        {4,5,6},
        {7,8,9}
    };

    printf("=== MATRIZ 3x3 ===\n");
    printf("O elemento na posicao [0][0] e %d\n", matriz3[0][0]);
    printf("O elemento na posicao [1][1] e %d\n", matriz3[1][1]);
    printf("O elemento na posicao [2][2] e %d\n\n", matriz3[2][2]);

    int tabuleiro[TAM][TAM];
    int i, j;

    for (i = 0; i < TAM; i++)
        for (j = 0; j < TAM; j++)
            tabuleiro[i][j] = AGUA;

    int linhaNavioHorizontal = 2, colunaNavioHorizontal = 4;
    for (j = 0; j < 3; j++)
        tabuleiro[linhaNavioHorizontal][colunaNavioHorizontal + j] = NAVIO;

    int linhaNavioVertical = 5, colunaNavioVertical = 7;
    for (i = 0; i < 3; i++)
        tabuleiro[linhaNavioVertical + i][colunaNavioVertical] = NAVIO;

    int cone[TAM_HAB][TAM_HAB], cruz[TAM_HAB][TAM_HAB], octaedro[TAM_HAB][TAM_HAB];
    gerarHabilidadeCone(cone);
    gerarHabilidadeCruz(cruz);
    gerarHabilidadeOctaedro(octaedro);

    aplicarHabilidade(tabuleiro, cone, 2, 2);     
    aplicarHabilidade(tabuleiro, cruz, 5, 5);     
    aplicarHabilidade(tabuleiro, octaedro, 7, 2); 

    printf("=== TABULEIRO FINAL ===\n\n");
    exibirTabuleiro(tabuleiro);

    return 0;
}
