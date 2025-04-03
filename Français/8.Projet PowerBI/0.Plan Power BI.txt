# ** Plan Intensif et Avancé pour le Mini-Projet Power BI : Analyse des Ventes et Promotions**  
 **Objectif : Créer une application Power BI complète avec des analyses avancées, une interactivité maximisée et une ergonomie soignée**.

---

## ** Étape 1 : Importation & Transformation des Données (Power Query Editor & M)**  
 **Objectif : Nettoyer, transformer et optimiser les données pour la modélisation et améliorer la performance**.  

### **1.1. Optimisations avancées dans Power Query**  
 **DimProduct** :  
- Supprimer la colonne inutile **Column1**.  
- Ajouter une colonne **"Type de produit"** en classifiant automatiquement les produits (Power Query M).  
- Nettoyer les noms de produits en supprimant les caractères spéciaux.  
- Gérer les valeurs manquantes dans `Manufacturer` en remplaçant par `"Unknown"`.  

 **DimPromotion** :  
- Convertir **StartDate** et **EndDate** en format date et créer une **colonne calculée pour la durée** de la promotion.  
- Supprimer les promotions expirées automatiquement.  

 **FactSales** :  
- Filtrer les valeurs aberrantes (`UnitPrice > 0`).  
- Ajouter une colonne `Revenue` = `[UnitPrice] * [SalesQuantity]`.  
- Gérer les valeurs nulles dans `DiscountAmount`.  
- Ajouter un **flag** `IsReturned` (`1` si `ReturnQuantity` > `0`, sinon `0`).  

 **DimDate (génération automatique avec Power Query M)**  
- Générer une **table de dates dynamiques**.  
- Ajouter des **indicateurs de périodes** (`IsCurrentMonth`, `IsLastYear`).  

---

## ** Étape 2 : Création de Tables et Colonnes Calculées (DAX)**  
 **Objectif : Améliorer le modèle en créant des tables simplifiées, des colonnes calculées et des mesures avancées**.  

### **2.1. Tables calculées pour simplifier le modèle**  
 **Table Produits-Promotions Agrégée**  
```DAX
TableProductPromo = 
SELECTCOLUMNS(
    NATURALINNERJOIN(DimProductPromotion, DimPromotion),
    "ProductKey", [ProductKey],
    "Promotion", [PromotionLabel],
    "DiscountPercent", [DiscountPercent],
    "Category", RELATED(DimCategory[CategoryName])
)
```
 **Table Agrégée des Ventes par Mois et par Canal**  
```DAX
SalesByMonth = 
SUMMARIZE(
    FactSales, 
    DimDate[YYYYMM], DimChannel[ChannelName],
    "Total Ventes", SUM(FactSales[SalesQuantity] * FactSales[UnitPrice])
)
```

---

### **2.2. Colonnes calculées en DAX**  
 **Marge bénéficiaire**  
```DAX
ProfitMargin = DIVIDE(([Total Sales] - [Total Cost]), [Total Sales], 0)
```
 **Classification des promotions**  
```DAX
DiscountCategory = 
SWITCH(TRUE(),
    [DiscountPercent] >= 50, "Très forte",
    [DiscountPercent] >= 30, "Forte",
    [DiscountPercent] >= 10, "Modérée",
    "Faible"
)
```

 **Statut du produit (Gamme de prix basée sur le prix unitaire)**  
```DAX
PriceCategory = 
IF([UnitPrice] > 400, "Premium",
    IF([UnitPrice] > 200, "Moyenne Gamme", "Entrée de Gamme")
)
```

---

### **2.3. Mesures avancées en DAX**  
 **Ventes après remise**  
```DAX
NetSales = [Total Sales] - SUM(FactSales[DiscountAmount])
```
 **Croissance des ventes par année**  
```DAX
SalesGrowth = 
VAR PreviousYearSales = 
    CALCULATE([Total Sales], SAMEPERIODLASTYEAR(DimDate[DateKey]))
RETURN 
    DIVIDE([Total Sales] - PreviousYearSales, PreviousYearSales, 0)
```
 **Taux de retour**  
```DAX
ReturnRate = DIVIDE(SUM(FactSales[ReturnQuantity]), SUM(FactSales[SalesQuantity]), 0)
```
 **Indice de performance des canaux de vente**  
```DAX
ChannelPerformanceIndex = 
RANKX(ALL(DimChannel), [Total Sales], , DESC, DENSE)
```

---

## ** Étape 3 : Création des Rapports et Visualisations**  
 **Objectif : Construire 3 rapports interactifs, organisés par pages et fonctionnalités UX avancées**.  

### ** Rapport 1 : Vue d’ensemble des ventes**
 **Page 1 : Dashboard Global**  
 **KPI Cards interactifs** :  
- **Total Ventes**, **Profit**, **Réductions appliquées**, **Taux de retour**  
- **Gauge KPI** pour la **croissance des ventes annuelle**  
 **Graphiques avancés** :  
- **Carte géographique des ventes**  
- **Heatmap des ventes par jour et canal**  
 **Interactivité** :  
- **Slicer dynamique** pour l’année/mois  
- **Boutons de navigation** entre les pages  

 **Page 2 : Analyse des ventes par catégorie**  
 **Diagramme en anneau** : Répartition des ventes par catégorie  
 **Bar chart avec drillthrough** : Produits les plus vendus  

 **Page 3 : Évolution des ventes**  
 **Graphique en courbes** : Ventes mensuelles  
 **Slicer dynamique** pour filtrer par canal de vente  

---

### ** Rapport 2 : Impact des promotions**  
 **Page 1 : Aperçu des promotions**  
 **Histogramme avancé** : Nombre de ventes par promotion  
 **Heatmap des remises appliquées**  

 **Page 2 : Comparaison avec et sans promotion**  
 **Stacked Column Chart** : Ventes avec et sans promotion  
 **Bouton Toggle** pour filtrer uniquement les produits en promo  

 **Page 3 : Analyse détaillée par produit**  
 **Tableau dynamique** des ventes par produit et par promo  
 **Drillthrough pour voir les transactions d’un produit spécifique**  

 **Interactivité avancée** :  
- **Cross-report Drillthrough** : Analyse promotion d’un produit en cliquant sur le rapport global  
- **Grouping et Bins pour segmenter les réductions en tranches**  

---

### ** Rapport 3 : Analyse des canaux de distribution**  
 **Page 1 : Ventes par canal**  
 **Graphique en colonnes empilées** : Comparaison entre Online, Reseller, Store  
 **Carte interactive** : Répartition des ventes  

 **Page 2 : Performance des canaux**  
 **Ranking des canaux par ventes (RANKX)**  
 **Filtering conditionnel** : Masquer les canaux sous-performants  

 **Page 3 : Comparaison année par année**  
 **Graphique en courbes** : Évolution des ventes par canal  
 **Boutons de navigation pour comparer différentes périodes**  

---

## ** Étape 4 : Publication, Optimisation et Sécurité**  
 **Objectif : Assurer un déploiement efficace et sécurisé sur Power BI Service**.  

 **Optimisation avec DAX Studio**  
 **Row-Level Security (RLS) par canal de vente**  
 **Actualisation planifiée sur Power BI Service**  
 **Création d’une Application Power BI avec rapports et dashboards**  

---

## ** Résumé Final : Pourquoi ce plan est ultra-intensif ?**  
 **Utilisation avancée de Power Query & Power Query M**  
 **Création de nombreuses mesures et tables calculées DAX**  
 **Rapports ultra-interactifs (Drillthrough, Cross-report, Buttons, KPIs, Slicers, Filters, Grouping, Bins, Gauges)**  
 **Expérience utilisateur soignée et optimisée**  

