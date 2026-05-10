# Stroke Prediction

Podstawowa struktura notebooka do projektu klasyfikacji ryzyka udaru.

## Wczytanie i opis danych

- Źródło danych.
- Wymiary zbioru i opis kolumn.
- Typy zmiennych (numeryczne / kategoryczne).
- Krótki komentarz o jakości danych.

Dane zostały wzięte z ogólno dostępnego zbioru danych na kaggle - *Stroke Prediction Dataset*. Zbiór danych zawiera 11 atrybutów (z czego jedna opisuje czy pacjent miał zawał) oraz 5110 pacjentów.

| Nazwa kolumny | Opis | Rodzaj danych |
|---|---|---|
| `gender` | Płeć pacjenta | Kategoryczna |
| `age` | Wiek pacjenta | Numeryczna |
| `hypertension` | Czy pacjent ma nadciśnienie | Kategoryczna (binarna) |
| `heart_disease` | Czy pacjent ma jakieś choroby serca | Kategoryczna (binarna) |
| `ever_married` | Czy pacjent był/jest w małżeństwie | Kategoryczna |
| `work_type` | Rodzaj pracy wykonywany | Kategoryczna |
| `Residence_type` | Miejsce zamieszkania | Kategoryczna |
| `avg_glucose_level` | Średni poziom glukozy we krwi | Numeryczna |
| `bmi` | 'Body mass index' | Numeryczna |
| `smoking_status` | Relacja pacjenta z papierosami | Kategoryczna |

Zbiór danych jest silnie niezbalansowany, około 5% danych to przypadki pozytywne (zawał wystąpił). Do jak najleszego wytrenowania modelu będzie dobrze zrobiony podział danych.

## Czyszczenie i sanity check danych

- Braki danych i strategia ich uzupełniania.
- Outliery

Sprawdźmy jak wygląda brak BMI w kontekście tego czy ktoś miał udar czy nie

20% przypadków z udarem to osoby, które nie mają BMI w danych. Brakujące dane w kolumnie BMI zastąpimy medianą zbioru, ponieważ model mógłby się nauczyć, że jeżeli ta kolumna jest pusta to mamy udar.


## Eksploracyjna analiza danych (EDA)

- Rozkład zmiennej celu (`stroke`).
- Analiza cech względem celu.
- Korelacje i zależności między cechami.
- Najważniejsze obserwacje z EDA.

### Wnioski z EDA

- Zmienna celu `stroke` jest silnie niezbalansowana (klasa pozytywna to niewielki odsetek danych), dlatego sama accuracy nie bedzie wystarczajaca do oceny modelu.
- Braki `bmi` niosa informacje: dla `bmi_missing=1` odsetek `stroke=1` jest wyraznie wyzszy niz dla `bmi_missing=0` (ok. `19.9%` vs `4.26%`).
- Wiek ma silny zwiazek z ryzykiem: wraz ze wzrostem grupy wiekowej rosnie odsetek `stroke=1`.
- Dla cech kategorycznych (`gender`, `ever_married`, `work_type`, `Residence_type`, `smoking_status`) widoczne sa roznice miedzy grupami, wiec warto zachowac, np. przez użycie OneHotEncoding.

## 6. Podział danych i metodologia

- Kodowanie zmiennych kategorycznych
- Skalowanie zmiennych numerycznych
- Podział na zbiory: treningowy / walidacyjny / testowy.
- Strategia walidacji (np. cross-validation).
- Metryki oceny (np. accuracy, precision, recall, F1, ROC-AUC).
- Kryteria wyboru modelu końcowego.

**Dlaczego używamy SMOTE?**

Zmienna celu `stroke` jest silnie niezbalansowana (klasa pozytywna jest rzadka), co może powodować, że model będzie faworyzował klasę `0`.

SMOTE (Synthetic Minority Oversampling Technique) tworzy syntetyczne przykłady klasy mniejszościowej tylko na zbiorze treningowym, dzięki czemu model ma lepszą szansę nauczyć się wzorców dla `stroke=1` i nie ignorować przypadków pozytywnych.

Ważne: SMOTE stosujemy wyłącznie na danych treningowych (`X_train`, `y_train`), a zbiór testowy pozostaje bez zmian, aby ocena modelu była uczciwa.

## 7. Trenowanie modeli

- Model bazowy (baseline).
- Modele porównawcze.
- Strojenie hiperparametrów.
- Tabela porównawcza wyników.

### 7.1 Baseline

- Wnioski:
Wyniki są fatalne, co świadczy o silnie niezbalansowanym zbiorze. Implikuje to potrzebę użycia modelu z tuningiem hiperparametrów. W związku z czym należy stroić drzewa decyzyjne pod F2 i balans klas.

### 7.2 Drzewa decyzyjne oraz tuning

- Wnioski:
Na zbiorze testowym model osiągnał recall = 0.84, precision = 0.097, F2 = 0.332. Model stosunkowo dobrze wykrywa zagrożenie życia, kosztem zwiększoną ilościa alarmów.

- Wnioski:
W porówaniu z innymi modelami `decision_tree_best` realizuje najlepiej cel projektu.
Mimo, że `logreg_deafult` ma wyższe `ROC-AUC` oraz `PR-AUC`, jego niski recall go dyskwalifikuje.

### 7.3 Analiza progu decyzyjnego

- Wnioski:
Dla progów do 0.7 metryki są stałe, powyżej jest nagły spadek do zera, co świadczy o tym, że model przestaje przewidywać klasę pozytywną.

## 8. Ewaluacja modelu końcowego

- Macierz pomyłek i interpretacja.
- Klasyfikacja błędów.
- Analiza cech ważnych dla predykcji.
- Ograniczenia modelu.

W część C porównano baseline i model drzewa decyzyjnego. Najlepszy okazał się `decision_tree_best`. Metryki testowe: recall = 0.84, precision = 0.097, F2 = 0.332, ROC-AUC=0.719, PR-AUC=0.089.

```diff
- TO DO: Wyniki logreg, SVM, Kuba, Wiktoria

## 9. Wnioski i dalsze kroki

- Podsumowanie najważniejszych rezultatów.
- Odpowiedź na pytanie badawcze.
- Propozycje ulepszeń.
- Pomysły na dalszą pracę.

Drzewo dało najwyższy recall i F2 spośród modeli osoby C.
Koszt to niska precyzja, czyli dużo fałszwych alarmów, lecz zgodnie z metryką biznesową model predykuje zagrożenie życia.
Analiza thresholdu ukazała skokowy charakter predykcji, czyli prostotę modelu drzewa.

```diff
- TO DO : Wyniki i wnioski z SVM oraz Logreg
