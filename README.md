# Analisi del dataset McDonald's

Analisi delle percezioni dei consumatori sul brand McDonald's, svolta seguendo la pipeline di data analysis del corso di **Fondamenti di Scienza dei Dati** (Università di Palermo).

## Dataset

`data/mcdonalds.csv` – 1453 risposte a un sondaggio, 15 attributi:

| Attributi | Tipo | Descrizione |
|---|---|---|
| `yummy`, `convenient`, `spicy`, `fattening`, `greasy`, `fast`, `cheap`, `tasty`, `expensive`, `healthy`, `disgusting` | binari (Yes/No) | percezione del brand |
| `Like` | ordinale (−5 … +5) | gradimento complessivo |
| `Age` | numerico | età del rispondente |
| `VisitFrequency` | ordinale | frequenza di visita (da *Never* a *More than once a week*) |
| `Gender` | binario | Female / Male |

### Fonte

Il dataset proviene da:

> Dolnicar, S., Grün, B., Leisch, F. (2018). *Market Segmentation Analysis: Understanding It, Doing It, and Making It Useful*. Springer. https://doi.org/10.1007/978-981-10-8818-6

ed è distribuito anche come `mcdonalds` nel pacchetto R [`MSA`](https://cran.r-project.org/package=MSA). Il file è incluso qui solo a scopo didattico.

## Pipeline

1. **Data collection** – caricamento ed esplorazione
2. **Preprocessing** – data cleaning (missing values, duplicati, outlier), feature extraction (codifica binaria e ordinale), analisi esplorativa e correlazioni, z-score normalization
3. **Riduzione della dimensionalità** – PCA, scree plot e biplot
4. **Learning non supervisionato** – segmentazione con K-means (scelta di K con metodo del gomito e silhouette)
5. **Learning supervisionato** – classificazione di `Like > 0` con kNN e regressione logistica, model selection con `GridSearchCV` e cross-validation 5-fold
6. **Evaluation** – accuracy, precision/recall, matrice di confusione sul test set
7. **Prediction** – modello finale applicato a nuovi clienti

## Risultati principali

- **PCA**: PC1 (25.5% della varianza) è l'asse del *gusto* (`yummy`, `tasty` vs `disgusting`), PC2 (14.8%) quello del *prezzo* (`cheap` vs `expensive`). Servono 8 componenti su 11 per spiegare il 90% della varianza.
- **Segmentazione (K = 4)**:

  | Segmento | Quota | Profilo | Like medio |
  |---|---|---|---|
  | Fan attenti alla salute | ~12% | gustoso, non ingrassante, ritenuto sano | +2.7 |
  | Fan consapevoli | ~51% | gustoso e comodo, ma fa ingrassare | +2.3 |
  | Detrattori | ~8% | scomodo, disgustoso, caro | −3.3 |
  | Critici ma utilizzatori | ~29% | comodo ma non gustoso | −1.6 |

  I silhouette score bassi (≈ 0.23) indicano che i segmenti non sono nettamente separati.
- **Classificazione**: regressione logistica ≈ **85%** di accuratezza sul test set, kNN (k = 9) ≈ 84%. Gli attributi più influenti sono `yummy`, `tasty`, `disgusting` e la frequenza di visita.

![Biplot PCA](figures/pca_biplot.png)
![Profilo dei segmenti](figures/segment_profile.png)

## Struttura

```
mcdonalds-analysis/
├── data/mcdonalds.csv
├── notebooks/analisi_mcdonalds.ipynb
├── figures/                 # grafici generati dal notebook
├── requirements.txt
└── README.md
```

## Esecuzione

```bash
pip install -r requirements.txt
cd notebooks
jupyter notebook analisi_mcdonalds.ipynb
```

Il notebook va eseguito dalla cartella `notebooks/` (usa percorsi relativi `../data` e `../figures`).
