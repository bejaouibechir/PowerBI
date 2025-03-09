# Dax démos

## **1. Formule DAX : Total Sales Measure**
### **Définition**  
```DAX
Total Sales Measure = SUMX(FactSales, FactSales[UnitPrice] * FactSales[SalesQuantity])
```
- **Objectif** : Calculer le chiffre d'affaires total.
- **Explication** : `SUMX` parcourt chaque ligne et fait `UnitPrice * SalesQuantity`, puis additionne le tout.

### **Données Test**
#### **Table `FactSales`**
| SalesKey | ProductKey | DateKey  | UnitPrice | SalesQuantity |
|----------|-----------|----------|----------|--------------|
| 1        | 101       | 20240101 | 10       | 5            |
| 2        | 102       | 20240101 | 20       | 3            |
| 3        | 103       | 20240102 | 15       | 2            |

#### **Table `DimProduct`**
| ProductKey | ProductName |
|------------|------------|
| 101        | Laptop     |
| 102        | Phone      |
| 103        | T-Shirt    |

### **Résultat**
**Total Sales Measure = (10 × 5) + (20 × 3) + (15 × 2) = 50 + 60 + 30 = 140**

---

## **2. Formule DAX : Total Sales by Product**
### **Définition**
```DAX
Total Sales by Product = CALCULATE(
    [Total Sales Measure],
    ALLEXCEPT(DimProduct, DimProduct[ProductKey])
)
```
- **Objectif** : Calculer le total des ventes par produit, sans être influencé par d'autres filtres.

### **Données Test**
#### **Table `FactSales`**
| SalesKey | ProductKey | DateKey  | UnitPrice | SalesQuantity |
|----------|-----------|----------|----------|--------------|
| 1        | 101       | 20240101 | 10       | 5            |
| 2        | 102       | 20240101 | 20       | 3            |
| 3        | 101       | 20240102 | 15       | 2            |

#### **Table `DimProduct`**
| ProductKey | ProductName | Category     |
|------------|------------|-------------|
| 101        | Laptop     | Electronics |
| 102        | Phone      | Electronics |
| 103        | T-Shirt    | Clothing    |

### **Sans `ALLEXCEPT` (Filtrage classique)**
Si un filtre est appliqué sur `Category = Electronics`, alors seules les ventes de Laptop et Phone sont prises en compte.

### **Avec `ALLEXCEPT`**
Même si on applique un filtre sur la catégorie, **les ventes restent calculées par produit indépendamment des autres filtres** :
| ProductKey | ProductName | Total Sales |
|------------|------------|------------|
| 101        | Laptop     | (10×5) + (15×2) = 80 |
| 102        | Phone      | 20×3 = 60 |

---

## **3. Formule DAX : Total Sales After Discount**
### **Définition**
```DAX
Total Sales After Discount = 
SUMX(FactSales, (FactSales[UnitPrice] - FactSales[DiscountAmount]) * FactSales[SalesQuantity])
```
- **Objectif** : Calculer les ventes après réduction.

### **Données Test**
#### **Table `FactSales`**
| SalesKey | ProductKey | DateKey  | UnitPrice | DiscountAmount | SalesQuantity |
|----------|-----------|----------|----------|----------------|--------------|
| 1        | 101       | 20240101 | 10       | 2              | 5            |
| 2        | 102       | 20240101 | 20       | 5              | 3            |

#### **Table `DimProduct`**
| ProductKey | ProductName |
|------------|------------|
| 101        | Laptop     |
| 102        | Phone      |

### **Résultat**
**Total Sales After Discount = (10 - 2) × 5 + (20 - 5) × 3 = 40 + 45 = 85**

---

## **4. Formule DAX : Previous Year Sales**
### **Définition**
```DAX
Previous Year Sales = 
CALCULATE([Total Sales Measure], SAMEPERIODLASTYEAR(DimDate[Year]))
```
- **Objectif** : Calculer le total des ventes de l’année précédente.

### **Données Test**
#### **Table `FactSales`**
| SalesKey | ProductKey | DateKey  | UnitPrice | SalesQuantity |
|----------|-----------|----------|----------|--------------|
| 1        | 101       | 20230101 | 10       | 5            |
| 2        | 102       | 20240101 | 20       | 3            |

#### **Table `DimDate`**
| DateKey  | Year |
|----------|------|
| 20230101 | 2023 |
| 20240101 | 2024 |

### **Résultat**
Si nous sommes en **2024**, alors `Previous Year Sales = Total Sales en 2023 = 10 × 5 = 50`.

---

## **5. Formule DAX : Sales Category**
### **Définition**
```DAX
Sales Category = 
IF([Total Sales Measure] > 100000, "High Sales", "Low Sales")
```
- **Objectif** : Catégoriser les ventes en "High Sales" ou "Low Sales".

### **Données Test**
#### **Table `FactSales`**
| SalesKey | ProductKey | DateKey  | UnitPrice | SalesQuantity |
|----------|-----------|----------|----------|--------------|
| 1        | 101       | 20240101 | 1200     | 100          |
| 2        | 102       | 20240101 | 800      | 50           |

#### **Table `DimProduct`**
| ProductKey | ProductName |
|------------|------------|
| 101        | Laptop     |
| 102        | Phone      |

### **Résultat**
| Product  | Total Sales | Sales Category |
|----------|------------|---------------|
| Laptop   | 120000     | High Sales    |
| Phone    | 40000      | Low Sales     |

---

Très bien ! On va continuer avec des formules DAX plus avancées en conservant le même style explicatif avec des données de test et des comparaisons détaillées. 🚀  

---

## **6. `CALCULATE` vs. `CALCULATETABLE`**  
### **Définition**  
- `CALCULATE` retourne une **seule valeur** modifiée par des filtres.
- `CALCULATETABLE` retourne **une table entière** filtrée.

### **Données Test**
#### **Table `FactSales`**
| SalesKey | ProductKey | DateKey  | UnitPrice | SalesQuantity |
|----------|-----------|----------|----------|--------------|
| 1        | 101       | 20240101 | 10       | 5            |
| 2        | 102       | 20240101 | 20       | 3            |
| 3        | 101       | 20240102 | 15       | 2            |

#### **Table `DimProduct`**
| ProductKey | ProductName |
|------------|------------|
| 101        | Laptop     |
| 102        | Phone      |

### **Exemples**
```DAX
-- Utilisation de CALCULATE pour obtenir la somme des ventes du produit 101
Sales for Laptop = 
CALCULATE(SUM(FactSales[SalesQuantity]), FactSales[ProductKey] = 101)
```
🔹 Résultat : **7 (5+2)** (seulement les ventes du Laptop)

```DAX
-- Utilisation de CALCULATETABLE pour retourner une table filtrée
Sales Table for Laptop =
CALCULATETABLE(FactSales, FactSales[ProductKey] = 101)
```
🔹 Résultat : Une table contenant uniquement les ventes du Laptop.  

| SalesKey | ProductKey | DateKey  | UnitPrice | SalesQuantity |
|----------|-----------|----------|----------|--------------|
| 1        | 101       | 20240101 | 10       | 5            |
| 3        | 101       | 20240102 | 15       | 2            |

---

## **7. `ISBLANK` vs. `BLANK`**  
### **Définition**  
- `ISBLANK` vérifie si une expression retourne **BLANK**.
- `BLANK` représente **une valeur vide**.

### **Données Test**
#### **Table `FactSales`**
| SalesKey | ProductKey | DateKey  | ReturnAmount |
|----------|-----------|----------|-------------|
| 1        | 101       | 20240101 | 50          |
| 2        | 102       | 20240101 | NULL        |

### **Exemples**
```DAX
-- Vérifier si une valeur est BLANK
Check Blank = IF(ISBLANK(FactSales[ReturnAmount]), "No Return", FactSales[ReturnAmount])
```
🔹 Résultat :
| SalesKey | ReturnAmount | Check Blank |
|----------|-------------|-------------|
| 1        | 50          | 50          |
| 2        | NULL        | No Return   |

```DAX
-- Remplacer une valeur NULL par BLANK
Replace Null with Blank =
IF(FactSales[ReturnAmount] = 0, BLANK(), FactSales[ReturnAmount])
```
🔹 Résultat : Lignes avec `NULL` deviennent `BLANK`.

---

## **8. `TOTALYTD` - Ventes cumulées depuis le début de l'année**  
### **Définition**  
`TOTALYTD` calcule la somme cumulative depuis le début de l'année.

### **Données Test**
#### **Table `FactSales`**
| SalesKey | ProductKey | DateKey  | UnitPrice | SalesQuantity |
|----------|-----------|----------|----------|--------------|
| 1        | 101       | 20240101 | 10       | 5            |
| 2        | 101       | 20240201 | 10       | 3            |
| 3        | 101       | 20240301 | 10       | 4            |

#### **Table `DimDate`**
| DateKey  | Year | Month |
|----------|------|-------|
| 20240101 | 2024 | Jan   |
| 20240201 | 2024 | Feb   |
| 20240301 | 2024 | Mar   |

### **Exemple**
```DAX
Total Sales YTD =
TOTALYTD(SUM(FactSales[SalesQuantity]), DimDate[DateKey])
```
🔹 Résultat (cumul des ventes mois par mois) :
| Month | SalesQuantity | YTD Sales |
|-------|--------------|-----------|
| Jan   | 5            | 5         |
| Feb   | 3            | 8         |
| Mar   | 4            | 12        |

---

## **9. `EOMONTH` - Fin du mois courant**  
### **Définition**  
Retourne la **dernière date** du mois pour une date donnée.

### **Données Test**
#### **Table `DimDate`**
| DateKey  | Year | Month |
|----------|------|-------|
| 20240101 | 2024 | Jan   |
| 20240201 | 2024 | Feb   |

### **Exemple**
```DAX
End of Month Date = EOMONTH(DimDate[DateKey], 0)
```
🔹 Résultat :
| DateKey  | End of Month |
|----------|-------------|
| 20240101 | 2024-01-31  |
| 20240201 | 2024-02-29  |

Si on met `EOMONTH(DimDate[DateKey], 1)`, on obtient la **fin du mois suivant**.

---

## **10. `FILTER` - Filtrage avancé**  
### **Définition**  
Retourne une table filtrée selon une condition.

### **Données Test**
#### **Table `FactSales`**
| SalesKey | ProductKey | DateKey  | UnitPrice | SalesQuantity |
|----------|-----------|----------|----------|--------------|
| 1        | 101       | 20240101 | 10       | 5            |
| 2        | 102       | 20240101 | 20       | 3            |
| 3        | 101       | 20240102 | 15       | 2            |

#### **Exemple**
```DAX
Filtered Sales = FILTER(FactSales, FactSales[UnitPrice] > 10)
```
Résultat : Retourne seulement les ventes avec `UnitPrice > 10`.

| SalesKey | ProductKey | UnitPrice |
|----------|-----------|----------|
| 2        | 102       | 20       |
| 3        | 101       | 15       |

---

### 11. Utilisation avancée de `ALL` pour supprimer les filtres

#### **Exemple avec données**

**Table `FactSales` :**
| ProductKey | DateKey | SalesAmount |
|------------|---------|------------|
| 1          | 202401  | 500        |
| 2          | 202401  | 300        |
| 1          | 202402  | 700        |
| 2          | 202402  | 400        |

**Table `DimProduct` :**
| ProductKey | ProductName  |
|------------|-------------|
| 1          | Laptop      |
| 2          | Tablet      |

#### **Formule et application**
```DAX
Total Sales All Products = CALCULATE(SUM(FactSales[SalesAmount]), ALL(DimProduct))
```
**Explication des arguments :**
- `CALCULATE(expression, filters...)` : Modifie le contexte d'évaluation d'une expression.
- `SUM(FactSales[SalesAmount])` : Calcule la somme des ventes.
- `ALL(DimProduct)` : Supprime tous les filtres sur `DimProduct`.

**Effet** : Cette formule ignore tous les filtres sur `DimProduct`, renvoyant **2000** même si un produit est sélectionné.

---

### 12. Comparaison `FILTER` vs `ISFILTERED` vs `LOOKUPVALUE`

#### **Exemple avec données**

**Table `DimProduct` :**
| ProductKey | ProductName | Category |
|------------|------------|----------|
| 1          | Laptop     | Tech     |
| 2          | Tablet     | Tech     |
| 3          | Desk       | Office   |

#### **Formules et applications**
```DAX
Filtered Sales = CALCULATE(SUM(FactSales[SalesAmount]), FILTER(DimProduct, DimProduct[Category] = "Tech"))
```
**Explication des arguments :**
- `FILTER(table, condition)` : Applique un filtre ligne par ligne.
- `DimProduct[Category] = "Tech"` : Sélectionne uniquement les produits de la catégorie Tech.

Renvoie les ventes **de la catégorie Tech** (Laptop + Tablet).

```DAX
IsFilteredCategory = IF(ISFILTERED(DimProduct[Category]), "Filtered", "Not Filtered")
```
**Explication des arguments :**
- `ISFILTERED(column)` : Vérifie si une colonne est filtrée.
- `IF(condition, true_value, false_value)` : Retourne "Filtered" si un filtre est appliqué sur `Category`.

```DAX
FindProduct = LOOKUPVALUE(DimProduct[ProductName], DimProduct[ProductKey], 2)
```
**Explication des arguments :**
- `LOOKUPVALUE(result_column, search_column, search_value)` : Cherche la valeur correspondant au `ProductKey` donné.

Renvoie `Tablet` car `ProductKey = 2`.

---

### 13. `INDEX` pour récupérer un élément spécifique

#### **Exemple avec données**

| ProductKey | SalesAmount |
|------------|------------|
| 1          | 500        |
| 2          | 300        |
| 3          | 700        |
| 4          | 400        |

```DAX
SecondHighestSale = INDEX(2, ALL(FactSales), ORDERBY(FactSales[SalesAmount], DESC))
```
**Explication des arguments :**
- `INDEX(position, table, ORDERBY(...))` : Renvoie la valeur à une position donnée.
- `ORDERBY(FactSales[SalesAmount], DESC)` : Trie les ventes par ordre décroissant.

Renvoie `500` (deuxième plus grand `SalesAmount`).

---

### 14. `WINDOW` avec `PARTITIONBY` vs `ORDERBY`

#### **Exemple avec données**

| ProductKey | Category | SalesAmount |
|------------|----------|------------|
| 1          | Tech     | 500        |
| 2          | Tech     | 300        |
| 3          | Office   | 700        |
| 4          | Office   | 400        |

```DAX
MovingAvg = WINDOW(-1, 0, ALL(FactSales), ORDERBY(FactSales[SalesAmount]), PARTITIONBY(DimProduct[Category]))
```
**Explication des arguments :**
- `WINDOW(start, end, table, ORDERBY(...), PARTITIONBY(...))` : Définit une plage de lignes à analyser.
- `ORDERBY(FactSales[SalesAmount])` : Trie les ventes par montant.
- `PARTITIONBY(DimProduct[Category])` : Effectue les calculs par catégorie de produit.

**Effet** : Calcule la moyenne mobile par `Category`, triée par `SalesAmount`.

---

### 15. `SELECTEDVALUE` pour récupérer la sélection actuelle

#### **Formule et application**
```DAX
SelectedCategory = SELECTEDVALUE(DimProduct[Category], "Multiple or None")
```
**Explication des arguments :**
- `SELECTEDVALUE(column, alternate_value)` : Renvoie la valeur sélectionnée si une seule est filtrée.
- `"Multiple or None"` : Valeur alternative si plusieurs valeurs sont sélectionnées.

Renvoie :
- `"Tech"` si un seul filtre sur `Tech`.
- `"Multiple or None"` si plusieurs catégories sont sélectionnées ou aucune.

---

# Démos des Fonctions Financières en DAX

Nous allons illustrer les principales fonctions financières DAX avec des échantillons de données et des exemples concrets.

---

## **1. XIRR - Taux de Rentabilité Interne Irrégulier**
### **Démo : Calcul du rendement d'un investissement à flux irréguliers**

#### **Données d'exemple :**
| Date       | Montant |
|------------|---------|
| 01/01/2020 | -10000  |
| 01/06/2021 | 2000    |
| 01/12/2022 | 3000    |
| 01/12/2023 | 5000    |
| 01/06/2024 | 7000    |

#### **Formule DAX :**
```DAX
XIRR_Value = XIRR(Investments[Montant], Investments[Date])
```
#### **Explication :**
- `Investments[Montant]` : Liste des flux de trésorerie.
- `Investments[Date]` : Dates des transactions.
- **Sans XIRR**, il serait difficile d'estimer un rendement lorsque les flux ne sont pas réguliers.

---

## **2. IRR - Taux de Rentabilité Interne**
### **Démo : Investissement avec flux réguliers**

#### **Données d'exemple :**
| Période | Montant |
|---------|---------|
| 0       | -10000  |
| 1       | 3000    |
| 2       | 3000    |
| 3       | 4000    |

#### **Formule DAX :**
```DAX
IRR_Value = IRR(Investment[Montant])
```
#### **Explication :**
- `Investment[Montant]` : Liste des flux périodiques.
- IRR fonctionne uniquement si les flux sont équidistants.

---

## **3. NPV - Valeur Actuelle Nette**
### **Démo : Calcul de la rentabilité d'un projet**

#### **Données d'exemple :**
| Période | Flux |
|---------|------|
| 0       | -10000 |
| 1       | 3000  |
| 2       | 4000  |
| 3       | 5000  |

#### **Formule DAX :**
```DAX
NPV_Value = NPV(0.08, Investment[Flux])
```
#### **Explication :**
- `0.08` : Taux d'actualisation (8%).
- `Investment[Flux]` : Flux de trésorerie.

---

## **4. PV - Valeur Actuelle d'un investissement**
### **Démo : Calcul de la valeur actuelle d'un paiement futur**

#### **Formule DAX :**
```DAX
PV_Value = PV(0.05, 10, -1000)
```
#### **Explication :**
- `0.05` : Taux d'intérêt (5%).
- `10` : Nombre de périodes.
- `-1000` : Paiement à chaque période.

---

## **5. FV - Valeur Future**
### **Démo : Prévision de l'épargne à long terme**

#### **Formule DAX :**
```DAX
FV_Value = FV(0.05, 10, -1000, 0, 0)
```
#### **Explication :**
- `0.05` : Taux d'intérêt.
- `10` : Nombre de périodes.
- `-1000` : Montant investi à chaque période.
- `0` : Valeur actuelle.

---

## **6. RATE - Calcul du Taux d'Intérêt**
### **Démo : Trouver le taux d'un prêt**

#### **Formule DAX :**
```DAX
Rate_Value = RATE(10, -1000, 0, 15000, 0, 0.1)
```
#### **Explication :**
- `10` : Nombre de périodes.
- `-1000` : Paiement à chaque période.
- `15000` : Valeur future.
- `0.1` : Estimation du taux initial.

---

## **7. PMT - Calcul de mensualités**
### **Démo : Calculer le paiement d'un emprunt**

#### **Formule DAX :**
```DAX
PMT_Value = PMT(0.05 / 12, 60, -20000, 0, 0)
```
#### **Explication :**
- `0.05 / 12` : Taux mensuel.
- `60` : Nombre de paiements (5 ans).
- `-20000` : Montant emprunté.

---

## **8. CUMIPMT - Total des intérêts payés**
### **Démo : Coût total des intérêts sur 12 mois**

#### **Formule DAX :**
```DAX
Cum_Interest = CUMIPMT(0.05 / 12, 60, 20000, 1, 12, 0)
```
#### **Explication :**
- `1,12` : Intérêts payés de la 1ère à la 12e période.

---

## **9. INTRATE - Calcul du taux d'une obligation**
### **Démo : Déterminer le rendement d'un titre financier**

#### **Formule DAX :**
```DAX
InterestRate = INTRATE(365, -1000, 1100)
```
#### **Explication :**
- `365` : Durée en jours.
- `-1000` : Montant investi.
- `1100` : Valeur de remboursement.

---




