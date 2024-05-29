# Proiect-Proiectarea-Algoritmilor
#include <stdio.h>
#include <string.h>
#include <stdlib.h>

// Funcție pentru a calcula distanța Levenshtein
int calculeaza_distanta_levenshtein(const char *sursa, const char *tinta) {
    int lungime_sursa = strlen(sursa);
    int lungime_tinta = strlen(tinta);
    
    // Alocare matrice pentru distanțe
    int **matrice_distante = (int **)malloc((lungime_sursa + 1) * sizeof(int *));
    for (int i = 0; i <= lungime_sursa; i++) {
        matrice_distante[i] = (int *)malloc((lungime_tinta + 1) * sizeof(int));
    }

    // Inițializarea matricei
    for (int i = 0; i <= lungime_sursa; i++) {
        matrice_distante[i][0] = i;
    }
    for (int j = 0; j <= lungime_tinta; j++) {
        matrice_distante[0][j] = j;
    }

    // Calcularea distanței de editare
    for (int i = 1; i <= lungime_sursa; i++) {
        for (int j = 1; j <= lungime_tinta; j++) {
            if (sursa[i - 1] == tinta[j - 1]) {
                matrice_distante[i][j] = matrice_distante[i - 1][j - 1];
            } else {
                matrice_distante[i][j] = matrice_distante[i - 1][j - 1] + 1;  // Substituire
                if (matrice_distante[i][j] > matrice_distante[i - 1][j] + 1) {  // Ștergere
                    matrice_distante[i][j] = matrice_distante[i - 1][j] + 1;
                }
                if (matrice_distante[i][j] > matrice_distante[i][j - 1] + 1) {  // Inserție
                    matrice_distante[i][j] = matrice_distante[i][j - 1] + 1;
                }
            }
        }
    }

    int distanta = matrice_distante[lungime_sursa][lungime_tinta];

    // Eliberarea memoriei alocate
    for (int i = 0; i <= lungime_sursa; i++) {
        free(matrice_distante[i]);
    }
    free(matrice_distante);

    return distanta;
}

int main() {
    char cod_gresit[256];
    char sintaxa_valida[256];

    // Introducerea fragmentului de cod
    printf("Introdu fragmentul de cod: ");
    fgets(cod_gresit, sizeof(cod_gresit), stdin);
    // Eliminarea newline-ului de la final, dacă există
    cod_gresit[strcspn(cod_gresit, "\n")] = 0;

    // Introducerea regulii de sintaxă
    printf("Introdu regula de sintaxa: ");
    fgets(sintaxa_valida, sizeof(sintaxa_valida), stdin);
    // Eliminarea newline-ului de la final, dacă există
    sintaxa_valida[strcspn(sintaxa_valida, "\n")] = 0;

    // Calcularea numărului minim de operații
    int numar_operatii_minime = calculeaza_distanta_levenshtein(cod_gresit, sintaxa_valida);
    printf("Numărul minim de operații necesare: %d\n", numar_operatii_minime);

    return 0;
}
