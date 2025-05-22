# 7.2.10
 Zdefiniuj unię super_int, w której będzie można przechowywać
zarówno zmienne typu int, jak i unsigned int.
```
#include <stdio.h>

typedef union {
    int i;
    unsigned int u;
} super_int;

int main() {
    super_int x;

    x.i = -42;
    printf("Wartość jako int: %d\n", x.i);
    printf("Ta sama wartość jako unsigned int: %u\n", x.u);

    x.u = 3000000000U;
    printf("Wartość jako unsigned int: %u\n", x.u);
    printf("Ta sama wartość jako int: %d\n", x.i);

    return 0;
}
```
# 7.2.11
Zdefiniuj unię Liczba, która może służyć w zależności od potrzeb
do przechowywania liczby wymiernej lub liczby całkowitej. Zdefiniuj
strukturę Dane, o dwóch polach polu tp typu int oraz polu zaw typu
Liczba.
```
#include <stdio.h>
#include <stdlib.h>

typedef union {
    int calkowita;
    float wymierna;
} Liczba;

typedef struct {
    int typ;      // 0 = int, 1 = float
    Liczba val;
} Dane;

Dane wczytaj_dane() {
    Dane d;
    printf("Wczytać (0) int czy (1) float? ");
    scanf("%d", &d.typ);

    if (d.typ == 0) {
        printf("Podaj liczbę całkowitą: ");
        scanf("%d", &d.val.calkowita);
    } else {
        printf("Podaj liczbę wymierną: ");
        scanf("%f", &d.val.wymierna);
    }

    return d;
}

int main() {
    Dane dane = wczytaj_dane();

    if (dane.typ == 0)
        printf("Wczytano int: %d\n", dane.val.calkowita);
    else
        printf("Wczytano float: %f\n", dane.val.wymierna);

    return 0;
}
```
# 7.3.1 - 7.3.10
7.3.1
Napisz funkcję utworz zwracającą wskaźnik do pustej listy bez głowy o elementach typu element
7.3.2
Napisz funkcję wyczysc, która dostaje jako argument wskaźnik
do pierwszego elementu listy wskaźnikowej bezgłowy o elementach typu element i usuwa wszystkie elementy listy.
7.3.4
Napisz funkcję dodajk o dwóch argumentach Lista typu element*
i a typu int zwracającą wskaźnik do typu element. Funkcja powinna
dodawać na koniec listy reprezentowanej przez zmienną Lista nowy
element o wartości a pola i oraz zwracać wskaźnik do pierwszego
elementu tak powiększonej listy.
7.3.6
Napisz funkcję znajdz o dwóch argumentach Lista typu
element* i a typu int zwracającą wskaźnik do typu element. Funkcja powinna sprawdzać, czy na liście reprezentowanej przez zmienną
Lista znajduje się element o polu i równym a. Jeżeli tak, to funkcja
powinna zwrócić wskaźnik do tego elementu. W przeciwnym wypadku
funkcja powinna zwrócić wartość NULL.
7.3.7
Napisz funkcję usun o dwóch argumentach Lista typu element*
i a typu int zwracającą wskaźnik do typu element. Funkcja powinna
usuwać z listy reprezentowanej przez zmienną Lista element o wartości a pola i (o ile taki element znajduje się na liście) oraz zwracać
wskaźnik do pierwszego elementu zmodyfikowanej listy (jeżeli po usunięciu elementu lista będzie pusta, to funkcja powinna zwrócić wartość
NULL).
7.3.10
Napisz funkcję utworz tworzącą pustą listę z głową o elementach
typu element i zwracającą jako wartość wskaźnik do głowy utworzonej
listy.

```
// Plik: zad7_3_1.c
#include <stdio.h>
#include <stdlib.h>

typedef struct element {
    int wartosc;
    struct element* nast;
} element;

element* utworz() {
    return NULL;
}

int main() {
    element* lista = utworz();
    if (lista == NULL) {
        printf("Lista jest pusta.\n");
    }
    return 0;
}

// Plik: zad7_3_2.c
#include <stdio.h>
#include <stdlib.h>

typedef struct element {
    int wartosc;
    struct element* nast;
} element;

void wyczysc(element* lista) {
    element* tmp;
    while (lista != NULL) {
        tmp = lista;
        lista = lista->nast;
        free(tmp);
    }
}

int main() {
    element* lista = malloc(sizeof(element));
    lista->wartosc = 10;
    lista->nast = malloc(sizeof(element));
    lista->nast->wartosc = 20;
    lista->nast->nast = NULL;

    wyczysc(lista);
    printf("Lista wyczyszczona.\n");
    return 0;
}

// Plik: zad7_3_4.c
#include <stdio.h>
#include <stdlib.h>

typedef struct element {
    int wartosc;
    struct element* nast;
} element;

element* dodajk(element* Lista, int a) {
    element* nowy = malloc(sizeof(element));
    nowy->wartosc = a;
    nowy->nast = NULL;

    if (Lista == NULL)
        return nowy;

    element* p = Lista;
    while (p->nast != NULL)
        p = p->nast;

    p->nast = nowy;
    return Lista;
}

int main() {
    element* lista = NULL;
    lista = dodajk(lista, 5);
    lista = dodajk(lista, 10);
    lista = dodajk(lista, 15);

    element* p = lista;
    while (p != NULL) {
        printf("%d ", p->wartosc);
        p = p->nast;
    }
    printf("\n");
    return 0;
}

// Plik: zad7_3_6.c
#include <stdio.h>
#include <stdlib.h>

typedef struct element {
    int wartosc;
    struct element* nast;
} element;

element* znajdz(element* Lista, int a) {
    while (Lista != NULL) {
        if (Lista->wartosc == a)
            return Lista;
        Lista = Lista->nast;
    }
    return NULL;
}

int main() {
    element* lista = malloc(sizeof(element));
    lista->wartosc = 1;
    lista->nast = malloc(sizeof(element));
    lista->nast->wartosc = 2;
    lista->nast->nast = NULL;

    element* wynik = znajdz(lista, 2);
    if (wynik)
        printf("Znaleziono: %d\n", wynik->wartosc);
    else
        printf("Nie znaleziono.\n");
    return 0;
}

// Plik: zad7_3_7.c
#include <stdio.h>
#include <stdlib.h>

typedef struct element {
    int wartosc;
    struct element* nast;
} element;

element* usun(element* Lista, int a) {
    if (Lista == NULL) return NULL;

    element *p = Lista, *poprz = NULL;

    while (p != NULL && p->wartosc != a) {
        poprz = p;
        p = p->nast;
    }

    if (p == NULL) return Lista;

    if (poprz == NULL) {
        element* nowyStart = p->nast;
        free(p);
        return nowyStart;
    }

    poprz->nast = p->nast;
    free(p);
    return Lista;
}

int main() {
    element* lista = NULL;
    lista = malloc(sizeof(element));
    lista->wartosc = 1;
    lista->nast = malloc(sizeof(element));
    lista->nast->wartosc = 2;
    lista->nast->nast = NULL;

    lista = usun(lista, 1);

    element* p = lista;
    while (p != NULL) {
        printf("%d ", p->wartosc);
        p = p->nast;
    }
    printf("\n");
    return 0;
}

// Plik: zad7_3_10.c
#include <stdio.h>
#include <stdlib.h>

typedef struct element {
    int wartosc;
    struct element* nast;
} element;

element* utworz_z_glowa() {
    element* glowa = malloc(sizeof(element));
    glowa->nast = NULL;
    return glowa;
}

int main() {
    element* glowa = utworz_z_glowa();
    if (glowa != NULL && glowa->nast == NULL)
        printf("Utworzono liste z glowa.\n");
    free(glowa);
    return 0;
}
```

# 3.1.13
Napisz funkcję dodajk o dwóch argumentach Lista typu element*
i a typu int. Funkcja powinna dodawać na koniec listy reprezentowanej przez zmienną Lista nowy element o wartości a pola i.
```
#include <stdio.h>
#include <stdlib.h>

typedef struct element {
    int wartosc;
    struct element* nast;
} element;

element* dodajk(element* Lista, int a) {
    element* nowy = malloc(sizeof(element));
    nowy->wartosc = a;
    nowy->nast = NULL;

    if (Lista == NULL)
        return nowy;

    element* p = Lista;
    while (p->nast != NULL)
        p = p->nast;

    p->nast = nowy;
    return Lista;
}

int main() {
    element* lista = NULL;
    lista = dodajk(lista, 5);
    lista = dodajk(lista, 10);
    element* p = lista;
    while (p != NULL) {
        printf("%d ", p->wartosc);
        p = p->nast;
    }
    printf("\n");
    return 0;
}
```
# 7.3.14
Napisz funkcję dodajw o trzech argumentach Lista oraz elem typu
element* i a typu int. Funkcja powinna dodawać element o wartości
a pola i do listy reprezentowanej przez zmienną Lista na miejscu tuż
za elementem wskazywanym przez elem.
```
#include <stdio.h>
#include <stdlib.h>

typedef struct element {
    int wartosc;
    struct element* nast;
} element;

element* dodajw(element* Lista, element* elem, int a) {
    element* nowy = malloc(sizeof(element));
    nowy->wartosc = a;
    nowy->nast = NULL;

    if (elem == NULL) return Lista;

    nowy->nast = elem->nast;
    elem->nast = nowy;
    return Lista;
}

int main() {
    element* lista = malloc(sizeof(element));
    lista->wartosc = 1;
    lista->nast = NULL;

    lista = dodajw(lista, lista, 2); // dodaj po pierwszym elemencie

    element* p = lista;
    while (p != NULL) {
        printf("%d ", p->wartosc);
        p = p->nast;
    }
    printf("\n");
    return 0;
}
```
# 7.3.15
Napisz funkcję znajdz o dwóch argumentach Lista typu element*
i a typu int zwracającą wskaźnik do typu element. Funkcja powinna
sprawdzać, czy na liście reprezentowanej przez zmienną Lista, znajduje się element o polu i równym a. Jeżeli tak, to funkcja powinna
zwrócić wskaźnik do tego elementu. W przeciwnym wypadku funkcja
powinna zwrócić wartość NULL.
```
// Plik: zad7_3_15.c
#include <stdio.h>
#include <stdlib.h>

typedef struct element {
    int wartosc;
    struct element* nast;
} element;

element* znajdz(element* Lista, int a) {
    while (Lista != NULL) {
        if (Lista->wartosc == a)
            return Lista;
        Lista = Lista->nast;
    }
    return NULL;
}

int main() {
    element* lista = malloc(sizeof(element));
    lista->wartosc = 3;
    lista->nast = malloc(sizeof(element));
    lista->nast->wartosc = 5;
    lista->nast->nast = NULL;

    element* wynik = znajdz(lista, 5);
    if (wynik)
        printf("Znaleziono: %d\n", wynik->wartosc);
    else
        printf("Nie znaleziono.\n");
    return 0;
}

// Plik: zad7_3_17.c
#include <stdio.h>
#include <stdlib.h>

typedef struct element {
    int wartosc;
    struct element* nast;
} element;

element* usun(element* Lista, int a) {
    if (Lista == NULL) return NULL;

    element *p = Lista, *poprz = NULL;

    while (p != NULL && p->wartosc != a) {
        poprz = p;
        p = p->nast;
    }

    if (p == NULL) return Lista;

    if (poprz == NULL) {
        element* nowyStart = p->nast;
        free(p);
        return nowyStart;
    }

    poprz->nast = p->nast;
    free(p);
    return Lista;
}

int main() {
    element* lista = malloc(sizeof(element));
    lista->wartosc = 1;
    lista->nast = malloc(sizeof(element));
    lista->nast->wartosc = 2;
    lista->nast->nast = NULL;

    lista = usun(lista, 1);

    element* p = lista;
    while (p != NULL) {
        printf("%d ", p->wartosc);
        p = p->nast;
    }
    printf("\n");
    return 0;
}

// Plik: zad7_3_18.c
#include <stdio.h>
#include <stdlib.h>

typedef struct element {
    int wartosc;
    struct element* nast;
} element;

element* usun_element(element* Lista, element* elem) {
    if (Lista == NULL || elem == NULL) return Lista;

    element *p = Lista, *poprz = NULL;

    while (p != NULL && p != elem) {
        poprz = p;
        p = p->nast;
    }

    if (p == NULL) return Lista;

    if (poprz == NULL) {
        element* nowyStart = p->nast;
        free(p);
        return nowyStart;
    }

    poprz->nast = p->nast;
    free(p);
    return Lista;
}

int main() {
    element* lista = malloc(sizeof(element));
    lista->wartosc = 1;
    lista->nast = malloc(sizeof(element));
    lista->nast->wartosc = 2;
    lista->nast->nast = NULL;

    lista = usun_element(lista, lista); // usuwamy pierwszy

    element* p = lista;
    while (p != NULL) {
        printf("%d ", p->wartosc);
        p = p->nast;
    }
    printf("\n");
    return 0;
}
```
