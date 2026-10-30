# Cyfrowe Archiwum Walk i Sparingów w Pamięci Obiektowej (S3 + DynamoDB)

Projekt semestralny realizowany w ramach przedmiotu chmurowego (Temat 1).

---

## 1. Cel projektu

Celem projektu jest wdrożenie bezpiecznego, cyfrowego archiwum nagrań wideo z walk, sparingów i treningów sztuk walki. Rozwiązanie oparte jest na standardzie pamięci obiektowej Amazon S3 oraz bezserwerowej bazie DynamoDB.

Projekt realizuje:
- Przechowywanie plików wideo w płaskiej strukturze pamięci obiektowej z hierarchicznym nazewnictwem prefiksów.
- Wersjonowanie materiałów chroniące przed utratą surowych ujęć podczas wgrywania wersji po montażu / ze zwolnionym tempem.
- Automatyczne indeksowanie metadanych (przeciwnik, data, formuła walki/kategoria wagowa, rezultat, czas trwania) w bazie DynamoDB.
- Szybkie wyszukiwanie materiałów według kategorii/formuły oraz bezpieczne udostępnianie klipów za pomocą podpisanych adresów URL (`presigned URLs`) ważnych przez 10 minut.
- Pełną persystencję stanu (eksport i import) pozwalającą na odtworzenie bazy i plików po restarcie środowiska Colab.

Projekt uruchamiany jest w bezpłatnym środowisku Google Colab przy użyciu emulatora `moto`, bez użycia karty płatniczej.

---

## 2. Skład zespołu i podział pracy

*Projekt realizowany jednoosobowo.*

| Kacper Korbecki / Keroczkaq | Rola | Zakres odpowiedzialności |
|---|---|---|
| **Keroczkaq** (@Keroczkaq) | Pełna realizacja (Full-stack Cloud) | Konfiguracja emulatora S3 i DynamoDB, implementacja skryptów uploadu, wersjonowania, indeksowania metadanych walk, backupu oraz przygotowanie dokumentacji i kosztorysu |

---

## 3. Układ kluczy (struktura prefiksów)

W pamięci obiektowej nie występują fizyczne foldery – strukturę hierarchiczną symuluje się za pomocą prefiksów oddzielonych ukośnikiem (`/`):

```text
walki/{rok}/{kategoria}/{przeciwnik_lub_wydarzenie}/{nazwa_nagrania}.mp4
