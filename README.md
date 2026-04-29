# 🫀 Zawał serca, system wczesnego ostrzegania

Miniprojekt zespołu `Wiki-Maliniki` w ramach przedmiotu **Uczenie Maszynowe**.


> Jest to dokumentacja miniprojektu, która będzie ewaluować wraz z koniecznymi zmianami.Służy jako skrypt potrzebny do stworzenia projektu. Wraz ze zmianą, będę edytować plan wedle potrzeb i ustaleniu zgody współautorów.

> **UWAGA**: Jak będziemy tworzyć skrypty Pythonowe, to wrzucajmy, to do katalogu `src`. I do `init.py` dodawajmy, to co będzie potrzebne, żeby można było importować funkcje, bez konieczności podawania całej ścieżki. 

> **UWAGA**: Jak zrobimy swój segemnt to dodajcie to do Readme.md

## Spis treści
- [Cel projektu](#cel-projektu)
- [Zbiór danych](#zbiór-danych)
- [Modele](#modele)
- [Kluczowe metryki](#kluczowe-metryki)
- [Struktura repozytorium](#struktura-repozytorium)
- [Instalacja i uruchomienie](#instalacja-i-uruchomienie)
- [Podział pracy](#podział-pracy)
- [Wyniki](#wyniki)
- [Autorzy](#autorzy)



## Cel projektu

Zbudowanie systemu wczesnego ostrzegania na podstawie zbioru **Stroke Prediction Dataset** (~5 000 pacjentów, klasyfikacja binarna: zawał / brak zawału). System ma wykryć jak najwięcej osób zagrożonych przy jak najmniejszej liczbie fałszywych alarmów.

> Założenie biznesowe: co czwarty fałszywie zaalarmowany pacjent odinstalowuje aplikację.

## Zbiór danych

| Cecha | Wartość |
|-------|---------|
| Źródło | [Kaggle – Stroke Prediction Dataset](https://www.kaggle.com/datasets/fedesoriano/stroke-prediction-dataset) |
| Liczba rekordów | ~5 110 |
| Klasa pozytywna | `stroke = 1` |
| Problem | Silna niezbalansowanie klas (~5% pozytywnych) |

## Modele

| # | Model | Osoba | Zakres tuningu |
|---|-------|-------|----------------|
| 1 | **Drzewa decyzyjne** | Dawid (C) | głębokość, pruning, ważenie klas |
| 2 | **Regresja logistyczna** | Wiktoria (B) | regularyzacja, selekcja cech |
| 3 | **SVM** | Kuba (A) | regularyzacja, kernel, ważenie klas |

## Kluczowe metryki

- **Recall** klasy pozytywnej — priorytet (uratować życie)
- **Precision / FPR** — minimalizacja fałszywych alarmów
- **F2-score** — waga recall > precision
- **Custom utility score** — `+1` za uratowanego, `−0.25` za fałszywy alarm
- **ROC-AUC** / **PR-AUC** — pomocniczo, próg decyzyjny dobrany pod metrykę biznesową

## Struktura repozytorium

```
.
├── data/                   # Dane wejściowe (CSV)
│   └── healthcare-dataset-stroke-data.csv
├── notebooks/              # Notebooki Jupyter (EDA, modele, eksperymenty)
│   └── Stroke_Prediction.ipynb
├── src/                    # Moduły Pythona (preprocessing, modele, metryki)
│   └── __init__.py
├── reports/                # Raport końcowy
├── slides/                 # Slajdy do prezentacji
├── requirements.txt
├── .gitignore
└── README.md
```

## Instalacja i uruchomienie

```bash
# 1. Sklonuj repozytorium
git clone "https://github.com/Zajecia-na-PWr-LR/lista-4-projekt-grupowy-malinki-wiki.git"
cd lista-4-projekt-grupowy-malinki-wiki

# 2. Utwórz środowisko wirtualne
python -m venv .venv
source .venv/bin/activate  

# 3. Zainstaluj zależności
pip install -r requirements.txt

# 4. Uruchom notebook
jupyter notebook notebooks/Stroke_Prediction.ipynb
```

## Podział pracy

| Faza | Osoba A (Kuba) | Osoba B (Wiktoria) | Osoba C (Dawid) |
|------|---------------|-------------------|-----------------|
| 1. Setup + EDA | Metryki + funkcja kosztu | Wczytanie danych, EDA | Setup repo + README |
| 2. Preprocessing | Strategia CV | Preprocessing, niezbalansowane klasy | Baseline |
| 3. Modele + tuning | SVM + porównanie modeli | Regresja logistyczna | Drzewa decyzyjne |
| 4. Finalizacja | Finalny pipeline `predict()` | Wykresy + analiza błędów | Raport + slajdy |
| 5. Oddanie | Final QA | Próba prezentacji | Oddanie projektu |

## Wyniki

<-- TODO: uzupełnić po zakończeniu eksperymentów -->

| Model | Recall | Precision | F2 | Utility | ROC-AUC |
|-------|--------|-----------|----|---------|---------|
| Drzewa decyzyjne | – | – | – | – | – |
| Regresja logistyczna | – | – | – | – | – |
| SVM | – | – | – | – | – |

## Autorzy

- **Kuba** — SVM, metryki, walidacja krzyżowa, porównanie modeli, finalny pipeline, QA
- **Wiktoria** — wczytanie danych, EDA, preprocessing, niezbalansowane klasy, regresja logistyczna, wykresy
- **Dawid** — setup repo, baseline, drzewa decyzyjne, raport, slajdy
