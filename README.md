# Dátová analýza pacientov so synkopou

Tento repozitár obsahuje kód k bakalárskej práci zameranej na analýzu klinických údajov pacientov so synkopou, identifikáciu rizikových faktorov a predikciu opakovania synkopy pomocou metód strojového učenia.

Práca bola riešená podľa metodológie **KDD (Knowledge Discovery in Databases)**, ktorá zahŕňa výber dát, predspracovanie, transformáciu, modelovanie a interpretáciu výsledkov.

---

## Cieľ práce

Cieľom práce bolo:

- spracovať existujúci klinický dataset pacientov so synkopou,
- vytvoriť cieľovú premennú `recidiva`,
- spracovať chýbajúce hodnoty,
- natrénovať a porovnať klasifikačné modely,
- vyhodnotiť riziko opakovania synkopy,
- identifikovať najdôležitejšie rizikové faktory.

---


## Dataset

V práci bol použitý existujúci klinický dataset **data_full.csv**, ktorý vznikol ako výstup inej bakalárskej práce. Dataset obsahoval údaje o pacientoch so synkopou, napríklad vek, pohlavie, anamnestické údaje, ochorenia srdca, neurologické ochorenia a hodnoty krvného tlaku z HUT testu.

Cieľová premenná `recidiva` bola vytvorená z premennej `C2`, ktorá vyjadrovala počet epizód synkopy. Dáta obsahovali demografické, anamnestické a klinické premenné, napríklad vek, pohlavie, ochorenia srdca, arytmie, neurologické ochorenia, diabetes, anémiu, rodinnú anamnézu a hodnoty krvného tlaku z HUT testu.

Cieľová premenná `recidiva` bola vytvorená zo stĺpca `C2`:

- `0` – pacient mal jednu epizódu synkopy,
- `1` – pacient mal viac ako jednu epizódu synkopy.

---

## Predspracovanie dát

V rámci predspracovania boli vykonané najmä tieto kroky:

- odstránenie záznamov bez známej cieľovej premennej,
- nahradenie hodnôt `-1` hodnotou `NaN`,
- úprava premennej `Pohlavie`,
- úprava premennej `O1` pre náhle úmrtie v rodine,
- rozdelenie hodnôt krvného tlaku na samostatné premenné `SBP_before`, `DBP_before`, `SBP_tilt`, `DBP_tilt`,
- rozdelenie dát na trénovaciu a testovaciu množinu v pomere 80/20,
- použitie jednoduchej imputácie a MICE imputácie.

---

## Použité modely

V projekte boli použité tieto klasifikačné modely:

- **XGBoost** – trénovaný na datasete s chýbajúcimi hodnotami, keďže tento model dokáže s neúplnými údajmi pracovať priamo,
- **Logistic Regression** – použitý ako jednoduchší referenčný model; pred trénovaním bolo použité škálovanie pomocou `StandardScaler`,
- **Random Forest** – ansámblový model založený na rozhodovacích stromoch,
- **Gradient Boosting** – boostingový model založený na postupnom učení rozhodovacích stromov.

Modely Logistic Regression, Random Forest a Gradient Boosting boli testované pri dvoch spôsoboch dopĺňania chýbajúcich hodnôt:

- jednoduchá imputácia (`SimpleImputer`),
- MICE imputácia (`IterativeImputer`).

Okrem toho bol vytvorený aj kontrolný experiment **Random Forest bez HUT premenných**, kde boli odstránené premenné `SBP_before`, `DBP_before`, `SBP_tilt` a `DBP_tilt`.

---

## Vyhodnotenie

Modely boli vyhodnotené pomocou metrík:

- Accuracy,
- AUC,
- Precision,
- Recall,
- F1-score,
- Confusion matrix,
- ROC curve.

Najlepšiu celkovú výkonnosť dosiahol model  **Random Forest bez HUT premenných** trénovaný na dátach po MICE imputácii bez premenných súvisiacich s HUT testom. Tento variant dosiahol najvyššiu hodnotu AUC a zároveň ukázal, že model dokáže predikovať recidívu synkopy aj bez špecializovaných parametrov z HUT vyšetrenia.

---

## Použité knižnice

Práca bola naprogramovaná v jazyku **Python** v prostredí **Jupyter Notebook**.

Použité knižnice:

- numpy==1.26.4
- pandas==2.2.2
- scikit-learn==1.5.1
- xgboost==3.2.0
- seaborn==0.13.2
- matplotlib==3.9.2


Inštalácia knižníc:

```bash
pip install pandas numpy matplotlib seaborn scikit-learn xgboost
```

---

## Spustenie projektu

1. Nainštalujte potrebné knižnice.
2. Otvorte notebook `prakticka_cast.ipynb`.
3. Skontrolujte cestu k dátovému súboru.
4. Spustite bunky notebooku postupne od začiatku.

Odporúčaná štruktúra repozitára:

```text
.
├── prakticka_cast.ipynb
├── data_full.csv
└── README.md
```

---

## Dôležité poznámky

- Hodnota `-1` v pôvodnom datasete označovala chýbajúce údaje.
- XGBoost bol trénovaný na dátach s chýbajúcimi hodnotami.
- Ostatné modely boli trénované na dátach po imputácii.
- Pri MICE imputácii boli binárne premenné zaokrúhlené na hodnoty 0 a 1.
- Výsledky slúžia ako výskumný výstup bakalárskej práce, nie ako samostatný diagnostický nástroj.
