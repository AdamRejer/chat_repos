# 6.2.1
Napisz funkcję, która dostaje jako argument dodatnie liczby całkowite n i m, tworzy dynamiczną dwuwymiarową tablicę tablic elementów typu int o wymiarach n na m, i zwraca jako wartość wskaźnik
do niej.
```
#include <stdio.h>
#include <stdlib.h>

int** utworz_tablice_2d(int n, int m) {
    int **tab = malloc(n * sizeof(int*));
    for (int i = 0; i < n; i++) {
        tab[i] = malloc(m * sizeof(int));
        for (int j = 0; j < m; j++) {
            tab[i][j] = i * m + j; // przykładowe wartości
        }
    }
    return tab;
}

void wypisz_tablice_2d(int **tab, int n, int m) {
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < m; j++) {
            printf("%3d ", tab[i][j]);
        }
        printf("\n");
    }
}

void zwolnij_tablice_2d(int **tab, int n) {
    for (int i = 0; i < n; i++) {
        free(tab[i]);
    }
    free(tab);
}

int main() {
    int n = 3, m = 4;
    int **tablica = utworz_tablice_2d(n, m);
    wypisz_tablice_2d(tablica, n, m);
    zwolnij_tablice_2d(tablica, n);
    return 0;
}
```
6.2.2
 Napisz funkcję, która dostaje jako argument dodatnie liczby
całkowite n i m, tworzy dynamiczną dwuwymiarową tablicę elementów
typu int o wymiarach n na m i zwraca jako wartość wskaźnik do niej.
```
#include <stdio.h>
#include <stdlib.h>

int*** utworz_tablice_3d(int n, int m) {
    int ***tab = malloc(n * sizeof(int**));
    for (int i = 0; i < n; i++) {
        tab[i] = malloc(n * sizeof(int*));
        for (int j = 0; j < n; j++) {
            tab[i][j] = malloc(m * sizeof(int));
            for (int k = 0; k < m; k++) {
                tab[i][j][k] = i * 100 + j * 10 + k; // przykładowe wartości
            }
        }
    }
    return tab;
}

void wypisz_tablice_3d(int ***tab, int n, int m) {
    for (int i = 0; i < n; i++) {
        printf("Warstwa %d:\n", i);
        for (int j = 0; j < n; j++) {
            for (int k = 0; k < m; k++) {
                printf("%4d ", tab[i][j][k]);
            }
            printf("\n");
        }
        printf("\n");
    }
}

void zwolnij_tablice_3d(int ***tab, int n) {
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < n; j++) {
            free(tab[i][j]);
        }
        free(tab[i]);
    }
    free(tab);
}

int main() {
    int n = 2, m = 3;
    int ***tablica = utworz_tablice_3d(n, m);
    wypisz_tablice_3d(tablica, n, m);
    zwolnij_tablice_3d(tablica, n);
    return 0;
}
```
6.2.7
Napisz funkcję, która dostaje jako argument dodatnią liczbę całkowitą n, tworzy dynamiczną dwuwymiarową trójkątną tablicę tablic
elementów typu int o wymiarach n na n i zwraca jako wartość wskaźnik do niej.
```
#include <stdio.h>
#include <stdlib.h>

int** utworz_trojkatna(int n) {
    int **tab = malloc(n * sizeof(int*));
    for (int i = 0; i < n; i++) {
        tab[i] = malloc((i + 1) * sizeof(int));
        for (int j = 0; j <= i; j++) {
            tab[i][j] = i + j; // dowolne wartości
        }
    }
    return tab;
}

void wypisz_trojkatna(int **tab, int n) {
    for (int i = 0; i < n; i++) {
        for (int j = 0; j <= i; j++) {
            printf("%3d ", tab[i][j]);
        }
        printf("\n");
    }
}

void zwolnij_trojkatna(int **tab, int n) {
    for (int i = 0; i < n; i++) {
        free(tab[i]);
    }
    free(tab);
}

int main() {
    int n = 5;
    int **trojkatna = utworz_trojkatna(n);
    wypisz_trojkatna(trojkatna, n);
    zwolnij_trojkatna(trojkatna, n);
    return 0;
}
```
6.2.20
 Napisz funkcję, która dostaje jako argumenty dwie dwuwymiarowe
tablice tablic o elementach typu int oraz ich wymiary, i zamienia
zawartości obu tablic.
```
#include <stdio.h>
#include <stdlib.h>

void zamien_tablice(int **a, int **b, int n, int m) {
    for (int i = 0; i < n; i++)
        for (int j = 0; j < m; j++) {
            int temp = a[i][j];
            a[i][j] = b[i][j];
            b[i][j] = temp;
        }
}

int** nowa_tablica(int n, int m) {
    int **tab = malloc(n * sizeof(int*));
    for (int i = 0; i < n; i++) {
        tab[i] = malloc(m * sizeof(int));
    }
    return tab;
}

void wypelnij_sekwencyjnie(int **tab, int n, int m, int start) {
    for (int i = 0, val = start; i < n; i++)
        for (int j = 0; j < m; j++)
            tab[i][j] = val++;
}

void wypisz_tablice(int **tab, int n, int m) {
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < m; j++)
            printf("%3d ", tab[i][j]);
        printf("\n");
    }
}

void zwolnij_tablice(int **tab, int n) {
    for (int i = 0; i < n; i++)
        free(tab[i]);
    free(tab);
}

int main() {
    int n = 2, m = 3;
    int **a = nowa_tablica(n, m);
    int **b = nowa_tablica(n, m);
    wypelnij_sekwencyjnie(a, n, m, 1);
    wypelnij_sekwencyjnie(b, n, m, 100);

    printf("Przed zamianą:\n");
    wypisz_tablice(a, n, m);
    wypisz_tablice(b, n, m);

    zamien_tablice(a, b, n, m);

    printf("Po zamianie:\n");
    wypisz_tablice(a, n, m);
    wypisz_tablice(b, n, m);

    zwolnij_tablice(a, n);
    zwolnij_tablice(b, n);
    return 0;
}

```
6.2.23
Napisz funkcję, która dostaje jako argumenty dwuwymiarową
tablicę tablic o elementach typu int oraz jej wymiary, i zmienia kolejność wierszy w tablicy w taki sposób, że wiersz pierwszy ma się znaleźć
na miejscu drugiego, wiersz drugi ma się znaleźć na miejscu trzeciego
itd., natomiast ostatni wiersz ma się znaleźć na miejscu pierwszego
(przyjmujemy, że dwa elementy tablicy leżą w tym samym wierszu
jeżeli mają taką samą pierwszą współrzędną).
```
#include <stdio.h>
#include <stdlib.h>

// Funkcja tworzy nową tablicę o wymiarach n x m
int** utworz_tablice(int n, int m) {
    int** tab = malloc(n * sizeof(int*));
    for (int i = 0; i < n; i++) {
        tab[i] = malloc(m * sizeof(int));
    }
    return tab;
}

// Wypełnia tablicę wartościami sekwencyjnymi od start
void wypelnij_tablice(int** tab, int n, int m, int start) {
    for (int i = 0, val = start; i < n; i++) {
        for (int j = 0; j < m; j++) {
            tab[i][j] = val++;
        }
    }
}

// Wypisuje zawartość tablicy
void wypisz_tablice(int** tab, int n, int m) {
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < m; j++) {
            printf("%3d ", tab[i][j]);
        }
        printf("\n");
    }
}

// Zmienia kolejność wierszy w tablicy
void odwroc_wiersze(int** tab, int n, int m) {
    for (int i = 0; i < n / 2; i++) {
        int* temp = tab[i];
        tab[i] = tab[n - 1 - i];
        tab[n - 1 - i] = temp;
    }
}

// Zwalnia pamięć
void zwolnij_tablice(int** tab, int n) {
    for (int i = 0; i < n; i++) {
        free(tab[i]);
    }
    free(tab);
}

int main() {
    int n = 4, m = 5;
    int** tablica = utworz_tablice(n, m);
    wypelnij_tablice(tablica, n, m, 1);

    printf("Przed odwróceniem wierszy:\n");
    wypisz_tablice(tablica, n, m);

    odwroc_wiersze(tablica, n, m);

    printf("Po odwróceniu wierszy:\n");
    wypisz_tablice(tablica, n, m);

    zwolnij_tablice(tablica, n);
    return 0;
}
```
6.2.24
Napisz funkcję, która dostaje jako argumenty dwuwymiarową tablicę
tablic o elementach typu int oraz jej wymiary, i zmienia kolejność
kolumn w tablicy w taki sposób, że kolumna pierwsza ma się znaleźć
na miejscu drugiej, kolumna druga ma się znaleźć na miejscu trzeciej
itd., natomiast ostatnia kolumna ma się znaleźć na miejscu pierwszej
(przyjmujemy, że dwa elementy tablicy leżą w tej samej kolumnie,
jeżeli mają taką samą drugą współrzędną).
```
#include <stdio.h>
#include <stdlib.h>

// Wszystkie pomocnicze funkcje są takie same jak wyżej

void odwroc_kolumny(int** tab, int n, int m) {
    for (int j = 0; j < m / 2; j++) {
        for (int i = 0; i < n; i++) {
            int temp = tab[i][j];
            tab[i][j] = tab[i][m - 1 - j];
            tab[i][m - 1 - j] = temp;
        }
    }
}

int main() {
    int n = 4, m = 5;
    int** tablica = utworz_tablice(n, m);
    wypelnij_tablice(tablica, n, m, 10);

    printf("Przed odwróceniem kolumn:\n");
    wypisz_tablice(tablica, n, m);

    odwroc_kolumny(tablica, n, m);

    printf("Po odwróceniu kolumn:\n");
    wypisz_tablice(tablica, n, m);

    zwolnij_tablice(tablica, n);
    return 0;
}
```
