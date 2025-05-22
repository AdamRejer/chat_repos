```
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <wchar.h>
```
#Zad 5.2.1 wyczysc
Napisz funkcję wyczysc, która usuwa z tablicy przechowywany
w niej napis (w sensie: umieszcza w niej poprawny napis o długości
0). Napisz dwie wersje funkcji wyczysc dla napisów składających się
ze znaków typu char i wchar_t.
```
void wyczysc_char(char *napis) {
    napis[0] = '\0';
}

// wersja dla wchar_t
void wyczysc_wchar(wchar_t *napis) {
    napis[0] = L'\0';
}

int main() {
    char tekst1[] = "Przykladowy tekst";
    wchar_t tekst2[] = L"Przykładowy tekst szeroki";

    printf("Przed czyszczeniem: '%s'\n", tekst1);
    wyczysc_char(tekst1);
    printf("Po czyszczeniu: '%s'\n", tekst1);

    wprintf(L"\nPrzed czyszczeniem: '%ls'\n", tekst2);
    wyczysc_wchar(tekst2);
    wprintf(L"Po czyszczeniu: '%ls'\n", tekst2);

    return 0;
}
```
#Zad 5.2.2 dlugosc
Napisz funkcję dlugosc, która jako argument otrzymuje napis
i zwraca jako wartość jego długość. Napisz dwie wersje funkcji dlugosc
dla napisów składających się ze znaków typu char i wchar_t.

```
// wersja dla char
int dlugosc_char(const char *napis) {
    int i = 0;
    while (napis[i] != '\0') i++;
    return i;
}

// wersja dla wchar_t
int dlugosc_wchar(const wchar_t *napis) {
    int i = 0;
    while (napis[i] != L'\0') i++;
    return i;
}

int main() {
    char tekst1[] = "Test";
    wchar_t tekst2[] = L"Tekst szeroki";

    printf("Dlugosc (char): %d\n", dlugosc_char(tekst1));
    wprintf(L"Dlugosc (wchar_t): %d\n", dlugosc_wchar(tekst2));

    return 0;
}

```
# Zad 5.2.3 porownaj
Napisz funkcję porownaj, która jako argumenty otrzymuje dwa napisy
i zwraca 1 gdy napisy są równe i 0 w przeciwnym przypadku. Napisz
dwie wersje funkcji porownaj dla napisów składających się ze znaków
typu char i wchar_t.
```
#include <stdio.h>
#include <stdlib.h>
#include <wchar.h>

// wersja dla char
int porownaj_char(const char *a, const char *b) {
    int i = 0;
    while (a[i] && b[i]) {
        if (a[i] != b[i]) return 0;
        i++;
    }
    return a[i] == b[i];
}

// wersja dla wchar_t
int porownaj_wchar(const wchar_t *a, const wchar_t *b) {
    int i = 0;
    while (a[i] && b[i]) {
        if (a[i] != b[i]) return 0;
        i++;
    }
    return a[i] == b[i];
}

int main() {
    char tekst1[] = "abc";
    char tekst2[] = "abc";
    char tekst3[] = "xyz";

    wchar_t wtekst1[] = L"kot";
    wchar_t wtekst2[] = L"kot";
    wchar_t wtekst3[] = L"pies";

    printf("Porownanie (char): %d\n", porownaj_char(tekst1, tekst2)); // 1
    printf("Porownanie (char): %d\n", porownaj_char(tekst1, tekst3)); // 0

    wprintf(L"Porownanie (wchar_t): %d\n", porownaj_wchar(wtekst1, wtekst2)); // 1
    wprintf(L"Porownanie (wchar_t): %d\n", porownaj_wchar(wtekst1, wtekst3)); // 0

    return 0;
}
```
# Zad 5.2.5
Napisz funkcję przepisz, która otrzymuje dwie tablice znaków i przepisuje napis znajdujący się w pierwszej tablicy do drugiej tablicy. Zakładamy, że w drugiej tablicy jest wystarczająco dużo miejsca. Napisz
dwie wersje funkcji przepisz dla napisów składających się ze znaków
typu char i wchar_t.
```
#include <stdio.h>
#include <wchar.h>

void przepisz_char(char *dst, const char *src) {
    while (*src) {
        *dst++ = *src++;
    }
    *dst = '\0';
}

void przepisz_wchar(wchar_t *dst, const wchar_t *src) {
    while (*src) {
        *dst++ = *src++;
    }
    *dst = L'\0';
}

int main() {
    char a[] = "Ala";
    char b[100];
    przepisz_char(b, a);
    printf("Wynik (char): %s\n", b);

    wchar_t wa[] = L"Kot";
    wchar_t wb[100];
    przepisz_wchar(wb, wa);
    wprintf(L"Wynik (wchar_t): %ls\n", wb);
    return 0;
}

```
# 5.2.6
Napisz funkcję kopiujn, która dostaje w argumentach dwie tablice
znaków nap1, nap2 oraz liczbę n i przekopiowuje n pierwszych znaków
z napisu przechowywanego w tablicy nap1 do tablicy nap2. W przypadku gdy napis w tablicy nap1 jest krótszy niż n znaków, funkcja
powinna po prostu przepisać cały napis. Funkcja powinna zadbać o to,
żeby na końcu napisu w tablicy nap2 znalazł się znak o kodzie 0.
Zakładamy, że w docelowej tablicy jest wystarczająco dużo miejsca.
Napisz dwie wersje funkcji: dla napisów składających się ze znaków
typu char i wchar_t.
```
#include <stdio.h>
#include <wchar.h>

void kopiuj_char(char *dst, const char *src, int n) {
    for (int i = 0; i < n && src[i]; i++) {
        dst[i] = src[i];
    }
    dst[n] = '\0';
}

void kopiuj_wchar(wchar_t *dst, const wchar_t *src, int n) {
    for (int i = 0; i < n && src[i]; i++) {
        dst[i] = src[i];
    }
    dst[n] = L'\0';
}

int main() {
    char src1[] = "abcdefgh";
    char dst1[100];
    kopiuj_char(dst1, src1, 4);
    printf("Wynik (char): %s\n", dst1);

    wchar_t src2[] = L"napis szeroki";
    wchar_t dst2[100];
    kopiuj_wchar(dst2, src2, 6);
    wprintf(L"Wynik (wchar_t): %ls\n", dst2);
    return 0;
}

```
# 5.2.7
Napisz funkcję sklej otrzymującą jako argumenty trzy tablice
znaków i zapisującą do trzeciej tablicy konkatenację napisów znajdujących się w dwóch pierwszych tablicach (czyli dla napisów ”Ala m”
i ”a kota” znajdujących się w pierwszych dwóch argumentach do trzeciej tablicy powinien zostać zapisany napis ”Ala ma kota”). Zakładamy, że w trzeciej tablicy jest wystarczająco dużo miejsca.
Napisz dwie wersje funkcji sklej dla napisów składających się ze znaków typu char i wchar_t.
```
#include <stdio.h>
#include <wchar.h>

void sklej_char(char *wynik, const char *a, const char *b) {
    while (*a) *wynik++ = *a++;
    while (*b) *wynik++ = *b++;
    *wynik = '\0';
}

void sklej_wchar(wchar_t *wynik, const wchar_t *a, const wchar_t *b) {
    while (*a) *wynik++ = *a++;
    while (*b) *wynik++ = *b++;
    *wynik = L'\0';
}

int main() {
    char a[] = "Ala ";
    char b[] = "ma kota";
    char c[100];
    sklej_char(c, a, b);
    printf("Wynik (char): %s\n", c);

    wchar_t wa[] = L"Kot ";
    wchar_t wb[] = L"w butach";
    wchar_t wc[100];
    sklej_wchar(wc, wa, wb);
    wprintf(L"Wynik (wchar_t): %ls\n", wc);
    return 0;
}

```
# 5.2.8
Napisz funkcję, która otrzymuje w argumencie napis i podmienia
w nim wszystkie małe litery na duże litery.
Zakładamy, że napis przechowywany jest w tablicy o elementach typu char, składa się wyłącznie z liter łacińskich i białych znaków,
oraz że system ,na którym jest kompilowany i uruchamiany program,
używa standardowego kodowania ASCII.
```
#include <stdio.h>
#include <wchar.h>

void zmien_char(char *napis) {
    for (int i = 0; napis[i]; i++) {
        if (napis[i] >= 'a' && napis[i] <= 'z')
            napis[i] = napis[i] - 32;
        else if (napis[i] >= 'A' && napis[i] <= 'Z')
            napis[i] = napis[i] + 32;
    }
}

void zmien_wchar(wchar_t *napis) {
    for (int i = 0; napis[i]; i++) {
        if (napis[i] >= L'a' && napis[i] <= L'z')
            napis[i] = napis[i] - 32;
        else if (napis[i] >= L'A' && napis[i] <= L'Z')
            napis[i] = napis[i] + 32;
    }
}

int main() {
    char tekst[] = "Ala MA Kota";
    zmien_char(tekst);
    printf("Wynik (char): %s\n", tekst);

    wchar_t tekst2[] = L"KoT Ma aLa";
    zmien_wchar(tekst2);
    wprintf(L"Wynik (wchar_t): %ls\n", tekst2);
    return 0;
}
```
# 5.2.9
Napisz funkcję wytnij, która dostaje jako argumenty napis
oraz dwie liczby całkowite n i m, i wycina z otrzymanego napisu znaki
o indeksach od n do m ( n 6 m). Otrzymany w argumencie napis
może mieć dowolną liczbę znaków (w tym mniejszą od n lub m)
Napisz dwie wersje funkcji wytnij dla napisów składających się ze znaków typu char i wchar_t.
```
#include <stdio.h>
#include <wchar.h>

void wytnij_char(char *dst, const char *src, int m, int n) {
    int i, j = 0;
    for (i = m; i <= n && src[i]; i++) {
        dst[j++] = src[i];
    }
    dst[j] = '\0';
}

void wytnij_wchar(wchar_t *dst, const wchar_t *src, int m, int n) {
    int i, j = 0;
    for (i = m; i <= n && src[i]; i++) {
        dst[j++] = src[i];
    }
    dst[j] = L'\0';
}

int main() {
    char tekst[] = "abcdefghij";
    char wynik[100];
    wytnij_char(wynik, tekst, 2, 5);
    printf("Wytnij (char): %s\n", wynik);

    wchar_t tekst2[] = L"Programowanie";
    wchar_t wynik2[100];
    wytnij_wchar(wynik2, tekst2, 3, 8);
    wprintf(L"Wytnij (wchar_t): %ls\n", wynik2);
    return 0;
}
```
# 5.2.10
Napisz funkcję wytnij2, która dostaje jako argument dwa napisy nap1 i nap2, i wycina z napisu nap1 pierwsze wystąpienie w nim
napisu nap2. Napisz dwie wersje funkcji wytnij2 dla napisów składających się ze znaków typu char i wchar_t.
```
#include <stdio.h>
#include <wchar.h>

void wytnij2_char(char *dst, const char *src1, const char *src2) {
    int i = 0, j = 0;
    while (src1[i]) {
        int found = 0;
        for (int k = 0; src2[k]; k++) {
            if (src1[i] == src2[k]) {
                found = 1;
                break;
            }
        }
        if (!found)
            dst[j++] = src1[i];
        i++;
    }
    dst[j] = '\0';
}

void wytnij2_wchar(wchar_t *dst, const wchar_t *src1, const wchar_t *src2) {
    int i = 0, j = 0;
    while (src1[i]) {
        int found = 0;
        for (int k = 0; src2[k]; k++) {
            if (src1[i] == src2[k]) {
                found = 1;
                break;
            }
        }
        if (!found)
            dst[j++] = src1[i];
        i++;
    }
    dst[j] = L'\0';
}

int main() {
    char a[] = "abrakadabra";
    char b[] = "ab";
    char wynik[100];
    wytnij2_char(wynik, a, b);
    printf("Wytnij2 (char): %s\n", wynik);

    wchar_t wa[] = L"programowanie";
    wchar_t wb[] = L"aeiou";
    wchar_t wynik2[100];
    wytnij2_wchar(wynik2, wa, wb);
    wprintf(L"Wytnij2 (wchar_t): %ls\n", wynik2);
    return 0;
}
```
# 5.2.14
 Napisz funkcję, która wypisuje na standardowym wyjściu otrzymany w argumencie napis. Napisz dwie wersje funkcji: dla napisów
składających się ze znaków typu char i wchar_t.
```
#include <stdio.h>
#include <wchar.h>

// wersja dla char
void wypisz_char(const char *napis) {
    printf("%s\n", napis);
}

// wersja dla wchar_t
void wypisz_wchar(const wchar_t *napis) {
    wprintf(L"%ls\n", napis);
}

int main() {
    char tekst[] = "Witaj swiecie!";
    wchar_t wtekst[] = L"Pozdrowienia szerokie";

    wypisz_char(tekst);
    wypisz_wchar(wtekst);

    return 0;
}
```
# 5.2.16
Napisz funkcję, która dostaje jako argument tablicę znaków i wczytuje do niej napis ze standardowego wejścia. Napisz dwie wersje funkcji: dla tablicy składających się ze znaków typu char i wchar_t.
```
#include <stdio.h>
#include <wchar.h>

// wersja dla char
void wczytaj_char(char *napis, int max_len) {
    fgets(napis, max_len, stdin);
    // usuń znak nowej linii
    for (int i = 0; napis[i]; i++) {
        if (napis[i] == '\n') {
            napis[i] = '\0';
            break;
        }
    }
}

// wersja dla wchar_t
void wczytaj_wchar(wchar_t *napis, int max_len) {
    fgetws(napis, max_len, stdin);
    // usuń znak nowej linii
    for (int i = 0; napis[i]; i++) {
        if (napis[i] == L'\n') {
            napis[i] = L'\0';
            break;
        }
    }
}

int main() {
    char tekst[100];
    wchar_t wtekst[100];

    printf("Podaj napis (char): ");
    wczytaj_char(tekst, 100);
    printf("Wczytano: %s\n", tekst);

    printf("Podaj napis (wchar_t): ");
    wczytaj_wchar(wtekst, 100);
    wprintf(L"Wczytano: %ls\n", wtekst);

    return 0;
}
```
# 5.2.18
Napisz funkcję, która otrzymuje w argumencie tablicę napisów
(tablicę tablic a więc typ char** lub wchar_t**) oraz jej rozmiar
i zwraca jako wartość pierwszy leksykograficznie spośród przechowywanych w tablicy napisów (funkcja powinna zwrócić kopię znajdującego się w tablicy napisu).
Zakładamy, że napisy zawierają wyłącznie małe litery łacińskie. Napisz dwie wersje funkcji: dla napisów składających się ze znaków typu
char i wchar_t.
```
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <wchar.h>

// wersja dla char
char* pierwszy_lekso_char(char **tab, int n) {
    if (n == 0) return NULL;
    int min_idx = 0;
    for (int i = 1; i < n; i++) {
        if (strcmp(tab[i], tab[min_idx]) < 0) {
            min_idx = i;
        }
    }
    char *wynik = malloc(strlen(tab[min_idx]) + 1);
    if (wynik) strcpy(wynik, tab[min_idx]);
    return wynik;
}

// wersja dla wchar_t
wchar_t* pierwszy_lekso_wchar(wchar_t **tab, int n) {
    if (n == 0) return NULL;
    int min_idx = 0;
    for (int i = 1; i < n; i++) {
        if (wcscmp(tab[i], tab[min_idx]) < 0) {
            min_idx = i;
        }
    }
    wchar_t *wynik = malloc((wcslen(tab[min_idx]) + 1) * sizeof(wchar_t));
    if (wynik) wcscpy(wynik, tab[min_idx]);
    return wynik;
}

int main() {
    // test dla char
    char *napisy1[] = {"krowa", "werala", "kot", "pies"};
    int n1 = 4;
    char *wynik1 = pierwszy_lekso_char(napisy1, n1);
    printf("Pierwszy (char): %s\n", wynik1);
    free(wynik1);

    // test dla wchar_t
    wchar_t *napisy2[] = {L"pies", L"zebra", L"werala", L"balon"};
    int n2 = 4;
    wchar_t *wynik2 = pierwszy_lekso_wchar(napisy2, n2);
    wprintf(L"Pierwszy (wchar_t): %ls\n", wynik2);
    free(wynik2);

    return 0;
}
```
# 5.2.20
 Napisz funkcję godzina, która dostaje w argumentach trzy liczby
całkowite godz, min i sek, zawierające odpowiednio godziny, minuty
oraz sekundy, i zwraca jako wartość napis zawierający godzinę w formacie godz:min:sek, w którym wartości poszczególnych pól pochodzą
ze zmiennych podanych w argumentach.
Napisz dwie wersje funkcji godzina: zwracające napisy będące tablicami znaków typu char i typu wchar_t.
```
#include <stdio.h>
#include <stdlib.h>
#include <wchar.h>

void godzina_char(int godz, int min, int sek, char *wynik) {
    sprintf(wynik, "%02d:%02d:%02d", godz, min, sek);
}

void godzina_wchar(int godz, int min, int sek, wchar_t *wynik) {
    swprintf(wynik, 100, L"%02d:%02d:%02d", godz, min, sek);
}

int main() {
    char buf[20];
    wchar_t wbuf[20];

    godzina_char(5, 9, 7, buf);
    godzina_wchar(12, 30, 45, wbuf);

    printf("Godzina (char): %s\n", buf);
    wprintf(L"Godzina (wchar_t): %ls\n", wbuf);

    return 0;
}
```
# 5.2.22
Napisz funkcję sklej, która dostaje w argumentach trzy napisy
i zwraca jako wartość napis powstały ze sklejenia napisów otrzymanych w argumentach.
Napisz dwie wersje funkcji sklej operujące na napisach składających
się ze znaków typu char i typu wchar_t.
```
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <wchar.h>

void sklej_char(char *wynik, const char *a, const char *b, const char *c) {
    strcpy(wynik, a);
    strcat(wynik, b);
    strcat(wynik, c);
}

void sklej_wchar(wchar_t *wynik, const wchar_t *a, const wchar_t *b, const wchar_t *c) {
    wcscpy(wynik, a);
    wcscat(wynik, b);
    wcscat(wynik, c);
}

int main() {
    char wynik[100];
    wchar_t wwynik[100];
    sklej_char(wynik, "ala", "-", "kot");
    sklej_wchar(wwynik, L"pies", L"+", L"mysz");

    printf("Sklej (char): %s\n", wynik);
    wprintf(L"Sklej (wchar_t): %ls\n", wwynik);

    return 0;
}
```
# 5.2.24
Napisz funkcję kopiuj, która dostaje jako argumenty napis oraz tablicę znaków i przepisuje napis do otrzymanej w argumencie tablicy
znaków.
Napisz dwie wersje funkcji kopiuj operujące na napisach składających
się ze znaków typu char i typu wchar_t.
```
#include <stdio.h>
#include <wchar.h>

void kopiuj_char(char *dst, const char *src) {
    while (*src) {
        *dst++ = *src++;
    }
    *dst = '\0';
}

void kopiuj_wchar(wchar_t *dst, const wchar_t *src) {
    while (*src) {
        *dst++ = *src++;
    }
    *dst = L'\0';
}

int main() {
    char zrodlo[] = "hello";
    char cel[100];
    kopiuj_char(cel, zrodlo);
    printf("Kopiuj (char): %s\n", cel);

    wchar_t wzrodlo[] = L"swiat";
    wchar_t wcel[100];
    kopiuj_wchar(wcel, wzrodlo);
    wprintf(L"Kopiuj (wchar_t): %ls\n", wcel);

    return 0;
}
```
# 5.2.25
Napisz funkcję kopiuj, która dostaje jako argumenty napis oraz wskaźnik do napisu (czyli wskaźnik do wskaźnika), tworzy nową tablicę znaków, kopiuje do niej napis zawarty w pierwszym argumencie, i przypisuje wskaźnik do nowo utworzonej tablicy do zmiennej wskazywanej
przez drugi argument.
Napisz dwie wersje funkcji kopiuj operujące na napisach składających
się ze znaków typu char i typu wchar_t.
```
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <wchar.h>

void kopiuj_char2(char **dst, const char *src) {
    *dst = malloc(strlen(src) + 1);
    if (*dst) strcpy(*dst, src);
}

void kopiuj_wchar2(wchar_t **dst, const wchar_t *src) {
    *dst = malloc((wcslen(src) + 1) * sizeof(wchar_t));
    if (*dst) wcscpy(*dst, src);
}

int main() {
    char *nowy;
    kopiuj_char2(&nowy, "dynamiczny tekst");
    printf("Nowy (char): %s\n", nowy);
    free(nowy);

    wchar_t *wnowy;
    kopiuj_wchar2(&wnowy, L"napis szeroki");
    wprintf(L"Nowy (wchar_t): %ls\n", wnowy);
    free(wnowy);

    return 0;
}
```
# 5.2.26
Napisz funkcję, która dostaje w argumencie napis i zamienia wszystkie występujące w nim małe litery na odpowiadające im duże litery.
Napisz dwie wersje funkcji operujące na napisach składających się
ze znaków typu char i typu wchar_t.
```
#include <stdio.h>
#include <wchar.h>

void zamien_na_male_char(char *napis) {
    for (int i = 0; napis[i]; i++) {
        if (napis[i] >= 'A' && napis[i] <= 'Z') {
            napis[i] += 32;
        }
    }
}

void zamien_na_male_wchar(wchar_t *napis) {
    for (int i = 0; napis[i]; i++) {
        if (napis[i] >= L'A' && napis[i] <= L'Z') {
            napis[i] += 32;
        }
    }
}

int main() {
    char tekst[] = "HELLO WoRLd";
    zamien_na_male_char(tekst);
    printf("Male (char): %s\n", tekst);

    wchar_t wtekst[] = L"KOTEK i MYSZKA";
    zamien_na_male_wchar(wtekst);
    wprintf(L"Male (wchar_t): %ls\n", wtekst);

    return 0;
}
```
