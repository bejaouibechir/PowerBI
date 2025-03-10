# Exercices Progressifs sur Power Query Editor et le Langage M

## **1. Suppression des lignes vides**
### **Problématique**
Votre table contient des lignes vides et vous souhaitez les supprimer pour améliorer la qualité des données.

### **Échantillon**
| Nom  | Âge | Ville  |
|------|----|------|
| Alice | 25 | Paris  |
|       |    |       |
| Bob   | 30 | Lyon   |
|       |    |       |
| Eve   | 22 | Nantes |

### **Solution**
- Ouvrir Power Query Editor.
- Aller dans l'onglet "Accueil" et cliquer sur "Supprimer les lignes" > "Supprimer les lignes vides".

### **Explication**
Power Query identifie et supprime automatiquement les lignes sans valeurs.

### **Code Power Query M**
```m
let
    Source = Excel.CurrentWorkbook(){[Name="Table1"]}[Content],
    FiltrerLignesVides = Table.SelectRows(Source, each not List.IsEmpty(Record.FieldValues(_)))
in
    FiltrerLignesVides
```

---

## **2. Suppression des doublons**  
### **Problématique**  
Votre table contient des doublons, et vous devez conserver uniquement les valeurs uniques.

### **Échantillon**  
| Nom  | Âge | Ville  |
|------|----|------|
| Alice | 25 | Paris  |
| Bob   | 30 | Lyon   |
| Alice | 25 | Paris  |
| Eve   | 22 | Nantes |

### **Solution**  
- Sélectionner les colonnes concernées.
- Aller dans "Accueil" et cliquer sur "Supprimer les doublons".

### **Explication**  
Power Query compare toutes les valeurs des colonnes et garde uniquement les lignes uniques.

### **Code Power Query M**  
```m
let
    Source = Excel.CurrentWorkbook(){[Name="Table1"]}[Content],
    SupprimerDoublons = Table.Distinct(Source)
in
    SupprimerDoublons
```

---

## **3. Conversion du type de données**
### **Problématique**
Certaines colonnes de votre table sont mal formatées (ex. : texte au lieu de nombre). Vous devez corriger cela.

### **Échantillon**
| Nom  | Âge   | Salaire |
|------|------|--------|
| Alice | "25" | "3000" |
| Bob   | "30" | "4000" |
| Eve   | "22" | "2500" |

### **Solution**
- Sélectionner la colonne concernée.
- Aller dans "Transformer" > "Type de données" et choisir "Nombre entier".

### **Explication**
Power Query convertit les valeurs en type numérique pour permettre des calculs.

### **Code Power Query M**
```m
let
    Source = Excel.CurrentWorkbook(){[Name="Table1"]}[Content],
    ConvertirAge = Table.TransformColumnTypes(Source,{{"Âge", Int64.Type}, {"Salaire", Int64.Type}})
in
    ConvertirAge
```

---

## **4. Fractionner une colonne texte**
### **Problématique**
Une colonne contient des données combinées que vous souhaitez séparer (ex. : noms et prénoms dans la même colonne).

### **Échantillon**
| Nom Complet |
|-------------|
| Alice Dupont |
| Bob Martin  |
| Eve Lambert |

### **Solution**
- Sélectionner la colonne.
- Aller dans "Transformer" > "Fractionner la colonne" > "Par délimiteur" (espace).

### **Explication**
Power Query crée deux nouvelles colonnes en divisant la chaîne à l'espace.

### **Code Power Query M**
```m
let
    Source = Excel.CurrentWorkbook(){[Name="Table1"]}[Content],
    FractionnerColonne = Table.SplitColumn(Source, "Nom Complet", Splitter.SplitTextByDelimiter(" ", QuoteStyle.None), {"Prénom", "Nom"})
in
    FractionnerColonne
```

---

## **5. Création d'une requête imbriquée**
### **Problématique**
Vous souhaitez utiliser une sous-requête pour filtrer des données avant de les joindre à une autre table.

### **Échantillon**
**Table Employés**
| ID  | Nom   | Salaire |
|-----|------|--------|
| 1   | Alice | 3000   |
| 2   | Bob   | 4000   |
| 3   | Eve   | 2500   |

**Table Salaires Minimums**
| Seuil |
|-------|
| 2800  |

### **Solution**
- Créer une requête distincte pour le seuil de salaire.
- Filtrer les employés en fonction de ce seuil.

### **Explication**
L'utilisation d'une requête imbriquée permet de rendre le filtrage dynamique.

### **Code Power Query M**
```m
let
    Source = Excel.CurrentWorkbook(){[Name="Employés"]}[Content],
    SeuilSalaire = Excel.CurrentWorkbook(){[Name="Salaires Minimums"]}[Content],
    Seuil = SeuilSalaire{0}[Seuil],
    FiltrerEmployes = Table.SelectRows(Source, each [Salaire] > Seuil)
in
    FiltrerEmployes
```

---

## **6. Filtrer les valeurs nulles**
### **Problématique**
Votre table contient des valeurs nulles dans certaines colonnes et vous souhaitez les filtrer pour ne conserver que les lignes complètes.

### **Échantillon**
| Nom  | Âge  | Ville  |
|------|-----|-------|
| Alice | 25  | Paris  |
| Bob   | null | Lyon   |
| Eve   | 22  | null   |

### **Solution**
- Sélectionner la colonne concernée.
- Aller dans "Accueil" > "Supprimer les valeurs nulles".

### **Explication**
Power Query supprime automatiquement les lignes contenant des valeurs nulles.

### **Code Power Query M**
```m
let
    Source = Excel.CurrentWorkbook(){[Name="Table1"]}[Content],
    SupprimerNulls = Table.SelectRows(Source, each not List.Contains(Record.FieldValues(_), null))
in
    SupprimerNulls
```

---
