# Klasyfikacja Poziomu Stresu u Nastolatków

## Cel projektu
Automatyczna klasyfikacja poziomu stresu u nastolatków na podstawie cech demograficznych oraz codziennych nawyków przy użyciu algorytmów uczenia nadzorowanego.

## Opis danych
Zbiór obejmuje dane ankietowe 1200 osób. Zawiera cechy: wiek, płeć, liczbę godzin snu, czas w mediach społecznościowych, najczęściej używaną aplikację oraz wskaźnik stanów depresyjnych. Zmienna docelowa (poziom stresu pierwotnie w skali 1-10) została pogrupowana na 3 klasy:
* **Klasa 0 (niski stres):** wyniki 1-4
* **Klasa 1 (średni stres):** wyniki 5-7
* **Klasa 2 (wysoki stres):** wyniki 8-10

## Przygotowanie danych
* Podział danych na zbiór treningowy (80%) i testowy (20%) przed nałożeniem transformacji w celu uniknięcia wycieku danych (*Data Leakage*).
* Budowa potoku przetwarzania (*Pipeline*): standaryzacja cech numerycznych (*StandardScaler*) oraz przekształcenie zmiennych tekstowych na postać binarną (*OneHotEncoder*).

## Testowanie modeli
Porównano dwa algorytmy na zbiorze treningowym:
* **Regresja Logistyczna:** uzyskała celność 38% i całkowicie ignorowała klasę 0.
* **Las Losowy:** uzyskał celność 100% (błąd przetrenowania - *overfitting*). W celu poprawy generalizacji zastosowano strojenie hiperparametrów (*RandomizedSearchCV* z 3-krotną walidacją krzyżową), nakładając ograniczenia na głębokość i podziały drzew w Lesie Losowym.

## Wyniki i wnioski
* **Wynik ostateczny:** Zoptymalizowany model Lasu Losowego osiągnął na zbiorze testowym dokładność na poziomie **45%**.
* **Wskazanie lepszego modelu:** Las Losowy okazał się skuteczniejszy od Regresji Logistycznej, ponieważ poprawnie klasyfikował próby ze wszystkich trzech grup, realizując podział nieliniowy.
* **Przyczyna niskiej celności:** Analiza macierzy korelacji wykazała, że zmienne wejściowe (np. wiek, czas przed ekranem) mają korelację bliską 0.00 ze zmienną docelową. Dane są silnie zaszumione informacyjnie, co uniemożliwia modelom dokładniejsze typowanie.
* **Poprawność metodologiczna:** Spadek dokładności na zbiorze testowym w porównaniu do idealnego wyniku treningowego potwierdza szczelność potoku przetwarzania danych i brak sztucznego zawyżenia wyników poprzez wyciek danych.
