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
```
# 7.3.17
Napisz funkcję usun o dwóch argumentach Lista typu element*
i a typu int. Funkcja powinna usuwać z listy reprezentowanej
przez zmienną Lista element o wartości a pola i (o ile taki element
znajduje się na liście).
```
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
```
# 7.3.18
Napisz funkcję usunw o dwóch argumentach Lista i elem typu
element*. Funkcja powinna usuwać z listy reprezentowanej
przez zmienną Lista element wskazywany przez zmienną elem.
```
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
# 7.3.20
Napisz funkcję zeruj, która dostaje jako argument listę wskaźnikową o elementach typu element i nadaje wartość 0 polom i we wszystkich elementach listy. Napisz dwie wersje funkcji: dla list z głową
i dla list bez głowy.
```
#include <stdio.h>
#include <stdlib.h>

typedef struct element {
    int wartosc;
    struct element* nast;
} element;

void zeruj_bez_glowy(element* lista) {
    while (lista != NULL) {
        lista->wartosc = 0;
        lista = lista->nast;
    }
}

void zeruj_z_glowa(element* glowa) {
    if (glowa == NULL) return;
    zeruj_bez_glowy(glowa->nast);
}

int main() {
    element* glowa = malloc(sizeof(element));
    glowa->nast = malloc(sizeof(element));
    glowa->nast->wartosc = 10;
    glowa->nast->nast = NULL;

    zeruj_z_glowa(glowa);
    printf("Po wyzerowaniu: %d\n", glowa->nast->wartosc);
    free(glowa->nast);
    free(glowa);
    return 0;
}
```
# 7.3.21
Napisz funkcję bezwzględna, która dostaje jako argument listę wskaźnikową o elementach typu element i zapisuje do pól i wszystkich
elementów listy wartość bezwzględną ich pierwotnej wartości. Napisz
dwie wersje funkcji: dla list z głową i dla list bez głowy.
```
#include <stdio.h>
#include <stdlib.h>

typedef struct element {
    int wartosc;
    struct element* nast;
} element;

void bezwzgledna_bez_glowy(element* lista) {
    while (lista != NULL) {
        if (lista->wartosc < 0)
            lista->wartosc = -lista->wartosc;
        lista = lista->nast;
    }
}

void bezwzgledna_z_glowa(element* glowa) {
    if (glowa == NULL) return;
    bezwzgledna_bez_glowy(glowa->nast);
}

int main() {
    element* glowa = malloc(sizeof(element));
    glowa->nast = malloc(sizeof(element));
    glowa->nast->wartosc = -7;
    glowa->nast->nast = NULL;

    bezwzgledna_z_glowa(glowa);
    printf("Po bezwzglednej: %d\n", glowa->nast->wartosc);
    free(glowa->nast);
    free(glowa);
    return 0;
}
```
# 7.3.22
Zdefiniuj strukturę trojkat mogącą służyć jako typ elementów listy
jednokierunkowej. Struktura trojkat powinna posiadać pola służące
do przechowywania wszystkich boków trójkąta oraz jego pola.
Napisz funkcję pole, która otrzymuje w argumencie listę wskaźnikową
o elementach typu trojkat i we wszystkich elementach listy do odpowiedniego pola wstawia wartość pola trójkąta o bokach, których długość przechowuje dana struktura. Napisz dwie wersje funkcji: dla list
z głową i dla list bez głowy.
```
#include <stdio.h>
#include <stdlib.h>
#include <math.h>

typedef struct trojkat {
    double a, b, c, pole;
    struct trojkat* nast;
} trojkat;

double oblicz_pole(double a, double b, double c) {
    double p = (a + b + c) / 2;
    return sqrt(p * (p - a) * (p - b) * (p - c));
}

void pole_bez_glowy(trojkat* lista) {
    while (lista != NULL) {
        lista->pole = oblicz_pole(lista->a, lista->b, lista->c);
        lista = lista->nast;
    }
}

void pole_z_glowa(trojkat* glowa) {
    if (glowa == NULL) return;
    pole_bez_glowy(glowa->nast);
}

int main() {
    trojkat* glowa = malloc(sizeof(trojkat));
    glowa->nast = malloc(sizeof(trojkat));
    glowa->nast->a = 3;
    glowa->nast->b = 4;
    glowa->nast->c = 5;
    glowa->nast->nast = NULL;

    pole_z_glowa(glowa);
    printf("Pole trojkata: %.2f\n", glowa->nast->pole);
    free(glowa->nast);
    free(glowa);
    return 0;
}
```
# 7.3.24
Napisz funkcję suma, która dostaje jako argument listę wskaźnikową o elementach typu element i zwraca jako wartość sumę pól i ze wszystkich elementów listy. Napisz dwie wersje funkcji: dla list z głową i dla list bez głowy.
```
#include <stdio.h>
#include <stdlib.h>

typedef struct element {
    int wartosc;
    struct element* nast;
} element;

int suma_bez_glowy(element* lista) {
    int suma = 0;
    while (lista != NULL) {
        suma += lista->wartosc;
        lista = lista->nast;
    }
    return suma;
}

int suma_z_glowa(element* glowa) {
    if (glowa == NULL) return 0;
    return suma_bez_glowy(glowa->nast);
}

int main() {
    element* glowa = malloc(sizeof(element));
    glowa->nast = malloc(sizeof(element));
    glowa->nast->wartosc = 2;
    glowa->nast->nast = malloc(sizeof(element));
    glowa->nast->nast->wartosc = 3;
    glowa->nast->nast->nast = NULL;

    printf("Suma: %d\n", suma_z_glowa(glowa));
    return 0;
}
```
# 7.3.26
Napisz funkcję minimum, która dostaje jako argument Lista listę wskaźnikową o elementach typu element i zwraca jako wartość
wskaźnik do elementu listy o najmniejszej wartości pola i. Napisz
dwie wersje funkcji: dla list z głową i dla list bez głowy
```
#include <stdio.h>
#include <stdlib.h>

typedef struct element {
    int wartosc;
    struct element* nast;
} element;

element* minimum_bez_glowy(element* lista) {
    if (!lista) return NULL;
    element* min = lista;
    while (lista != NULL) {
        if (lista->wartosc < min->wartosc)
            min = lista;
        lista = lista->nast;
    }
    return min;
}

element* minimum_z_glowa(element* glowa) {
    if (!glowa) return NULL;
    return minimum_bez_glowy(glowa->nast);
}

int main() {
    element* glowa = malloc(sizeof(element));
    glowa->nast = malloc(sizeof(element));
    glowa->nast->wartosc = 7;
    glowa->nast->nast = malloc(sizeof(element));
    glowa->nast->nast->wartosc = 3;
    glowa->nast->nast->nast = NULL;

    element* min = minimum_z_glowa(glowa);
    printf("Minimum: %d\n", min->wartosc);
    return 0;
}
```
# 7.3.28
Napisz funkcję, która dostaje jako argument listę o elementach typu element i zwraca jako wartość największą na wartość bezwzględną
spośród różnic pomiędzy polami i w różnych elementach listy otrzymanej w argumencie. Zakładamy, że otrzymana w argumencie funkcji lista jest co najmniej dwuelementowa. Napisz dwie wersje funkcji:
dla list z głową i dla list bez głowy
```
#include <stdio.h>
#include <stdlib.h>

typedef struct element {
    int wartosc;
    struct element* nast;
} element;

int max_roznica_bez_glowy(element* lista) {
    int max = 0;
    for (element* i = lista; i != NULL; i = i->nast) {
        for (element* j = lista; j != NULL; j = j->nast) {
            int roznica = i->wartosc - j->wartosc;
            if (roznica < 0) roznica = -roznica;
            if (roznica > max)
                max = roznica;
        }
    }
    return max;
}

int max_roznica_z_glowa(element* glowa) {
    if (!glowa) return 0;
    return max_roznica_bez_glowy(glowa->nast);
}

int main() {
    element* glowa = malloc(sizeof(element));
    glowa->nast = malloc(sizeof(element));
    glowa->nast->wartosc = 1;
    glowa->nast->nast = malloc(sizeof(element));
    glowa->nast->nast->wartosc = 9;
    glowa->nast->nast->nast = NULL;

    printf("Max roznica: %d\n", max_roznica_z_glowa(glowa));
    return 0;
}
```
# 7.3.29
Napisz funkcję kopiuj, która jako argument otrzymuje jednokierunkową listę wskaźnikową o elementach typu element, tworzy kopię
otrzymanej w argumencie listy i zwraca jako wartość wskaźnik do kopii. Napisz dwie wersje funkcji: dla list z głową i dla list bez głowy
```
#include <stdio.h>
#include <stdlib.h>

typedef struct element {
    int wartosc;
    struct element* nast;
} element;

element* kopiuj_bez_glowy(element* lista) {
    if (lista == NULL) return NULL;
    element* kopia = malloc(sizeof(element));
    kopia->wartosc = lista->wartosc;
    kopia->nast = NULL;

    element* ogon = kopia;
    lista = lista->nast;
    while (lista != NULL) {
        ogon->nast = malloc(sizeof(element));
        ogon = ogon->nast;
        ogon->wartosc = lista->wartosc;
        ogon->nast = NULL;
        lista = lista->nast;
    }
    return kopia;
}

element* kopiuj_z_glowa(element* glowa) {
    if (!glowa) return NULL;
    element* nowa_glowa = malloc(sizeof(element));
    nowa_glowa->nast = kopiuj_bez_glowy(glowa->nast);
    return nowa_glowa;
}

int main() {
    element* glowa = malloc(sizeof(element));
    glowa->nast = malloc(sizeof(element));
    glowa->nast->wartosc = 4;
    glowa->nast->nast = NULL;

    element* nowa = kopiuj_z_glowa(glowa);
    printf("Kopia: %d\n", nowa->nast->wartosc);
    return 0;
}
```
# 7.3.31
Napisz funkcję, która dostaje w argumentach wskaźnik do listy
wskaźnikowej o elementach typu element i odwraca kolejność elementów listy. Napisz dwie wersje funkcji: dla list z głową i dla list bez głowy. W wersji dla list bez głowy funkcja powinna zwracać wskaźnik
do pierwszego elementu odwróconej listy.
```
#include <stdio.h>
#include <stdlib.h>

typedef struct element {
    int wartosc;
    struct element* nast;
} element;

element* odwroc_bez_glowy(element* lista) {
    element* prev = NULL;
    element* curr = lista;
    while (curr != NULL) {
        element* next = curr->nast;
        curr->nast = prev;
        prev = curr;
        curr = next;
    }
    return prev;
}

element* odwroc_z_glowa(element* glowa) {
    if (!glowa) return NULL;
    glowa->nast = odwroc_bez_glowy(glowa->nast);
    return glowa;
}

int main() {
    element* glowa = malloc(sizeof(element));
    glowa->nast = malloc(sizeof(element));
    glowa->nast->wartosc = 1;
    glowa->nast->nast = malloc(sizeof(element));
    glowa->nast->nast->wartosc = 2;
    glowa->nast->nast->nast = NULL;

    glowa = odwroc_z_glowa(glowa);
    printf("Po odwroceniu: %d\n", glowa->nast->wartosc);
    return 0;
}
