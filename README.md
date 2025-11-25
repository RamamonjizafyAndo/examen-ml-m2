# **Rapport de Projet \- PoketraFinday**

## **Examen Final Machine Learning & Data Science**

Réalisé au sein de ISPM - Madagascar (www.ispm-edu.com)

### **1\. Informations sur le Groupe**

Merci de lister tous les membres de l'équipe ayant participé au Hackathon.

#### Membre 1 : 
* nom : RAMAMONJIZAFY 
* prénoms : Manitra Andonirina
* classe :  IGGLIA 5 
* numéro : 35
* rôle : Développeur

#### Membre 2 : 
* nom : ANDRIAMANARIVO 
* prénoms : Tahiana Nomena
* classe : IGGLIA 5
* numéro : 28
* rôle : Présentatrice

#### Membre 3 : 
* nom : ANDRIAMPARANY
* prénom(s) : Safidiniaina Jocelyn
* classe : IGGLIA 5
* numéro : 39
* rôle : développeur

#### Membre 4 : 
* nom : RABEARISOA
* prénom(s) : Ramanandraibe Germain
* classe : IGGLIA 5
* numéro : 16
* rôle : Data analyst

#### Membre 5 : 
* nom : EMASINDRA 
* prénom(s) : Eddy Nicolas
* classe : IGGLIA5
* numéro : 15
* rôle : développeur


### **2\. Résumé du Travail**

Problématique :  
PoketraFinday fait face à une recrudescence de fraudes sophistiquées (vols de comptes nocturnes, ingénierie sociale ciblant les seniors, transactions suspectes) qui fragilise la confiance des utilisateurs et menace la viabilité de la plateforme. Avec un taux de fraude d'environ 2% mais un impact financier et réputationnel considérable, il est critique de développer un système de détection automatique en temps réel capable d'identifier les transactions frauduleuses avant qu'elles ne causent des dommages irréversibles, tout en minimisant les fausses alertes qui pénaliseraient les utilisateurs légitimes.  
Méthodologie Adoptée :  
Notre approche s'articule en quatre étapes principales : (1) Une EDA approfondie orientée fraude révélant un déséquilibre critique des classes (ratio 49:1) et identifiant des patterns temporels et transactionnels suspects. (2) Un feature engineering avancé créant des variables d'interaction (montant/type, montant/âge), des features statistiques par groupe (moyennes par jour/heure), un encodage cyclique pour les variables temporelles, et des indicateurs de seuil. (3) Une modélisation progressive : baseline avec Régression Logistique (class_weight='balanced'), puis exploration de modèles avancés (Decision Tree, Random Forest, Gradient Boosting, XGBoost) avec SMOTE pour gérer le déséquilibre. (4) Une stratégie de validation avec division train/validation stratifiée (80/20) et hyperparameter tuning via GridSearchCV optimisant le F1-Score, métrique principale adaptée aux problèmes déséquilibrés.  
Résultats Obtenus :  
Notre meilleur modèle (Random Forest optimisé avec hyperparameter tuning) atteint un F1-Score de 0.7879 sur le jeu de validation, représentant une amélioration significative par rapport à la baseline de Régression Logistique. Une découverte clé de notre analyse révèle que les fraudes présentent des patterns temporels distincts (notamment une concentration accrue pendant certaines heures de la journée et jours de la semaine), et que certaines interactions entre variables (comme le ratio montant/type de transaction et les montants anormalement élevés par rapport aux moyennes temporelles) sont particulièrement discriminantes pour identifier les transactions frauduleuses.  
Mots-clés :  
Fraude, Imbalanced Data, SMOTE, Random Forest, Feature Engineering

### **3\. Contenu du Repository**

Voici la liste des fichiers et liens importants pour évaluer notre travail :

* **01_EDA_Preparation_Donnees.ipynb** : Analyse exploratoire complète (EDA) et préparation des données avec feature engineering basique.  
* **02_Baseline_Regression_Logistique.ipynb** : Modèle baseline avec Régression Logistique et évaluation des performances.  
* **03_Modeles_Avances.ipynb** : Exploration de modèles avancés (Decision Tree, Random Forest, Gradient Boosting, XGBoost), feature engineering avancé, SMOTE, et hyperparameter tuning.  
* **04_Generation_Soumission.ipynb** : Génération du fichier submission.csv avec le meilleur modèle.  
* **submission.csv** : Nos prédictions sur le fichier test.csv.  

**Liens Utiles :**

* [**LIEN VERS LA VIDÉO DE PRÉSENTATION** https://www.loom.com/share/8d86a96478094b4eb518b5c68f827256]

### **4\. Réponses aux Questions d'Analyse**

*Répondez de manière précise aux questions posées dans le sujet. Utilisez des chiffres ou des références à vos graphiques pour justifier vos réponses.*

**Q1. Pourquoi on utilise F1-Score au lieu de accuracy ?**

Dans notre dataset, les classes sont fortement déséquilibrées avec un ratio de 49:1 (98% de transactions légitimes vs 2% de fraudes). L'accuracy serait une métrique trompeuse car un modèle naïf qui prédit toujours "légitime" obtiendrait une accuracy de 98% sans détecter aucune fraude. Le F1-Score combine la précision (Precision) et le rappel (Recall) en une seule métrique, ce qui permet d'évaluer la capacité du modèle à identifier correctement les fraudes (classe minoritaire) tout en minimisant les fausses alertes. Cette métrique est particulièrement adaptée aux problèmes de classification déséquilibrés où la classe d'intérêt (fraude) est rare mais critique à détecter.

**Q2. Qu'est ce qui est plus grave ici, les Faux Positifs ou les Faux Négatifs ?**

Les **Faux Négatifs** (fraudes non détectées) sont nettement plus graves que les Faux Positifs dans ce contexte. Un Faux Négatif signifie qu'une transaction frauduleuse est laissée passer, causant une perte financière directe pour PoketraFinday et ses utilisateurs, ainsi qu'un préjudice réputationnel. À l'inverse, un Faux Positif (transaction légitime bloquée) est certes gênant pour l'utilisateur mais peut être résolu rapidement par un appel au service client, sans perte financière irréversible. C'est pourquoi notre modèle privilégie le rappel (Recall) pour maximiser la détection des fraudes, même si cela augmente légèrement le nombre de fausses alertes nécessitant une vérification manuelle.

**Q3. Stratégie de Modélisation : Quelles nouvelles variables (Feature Engineering) ont le plus amélioré votre modèle par rapport à la Baseline ?**

Les features qui ont le plus contribué à l'amélioration du modèle sont : (1) **Les variables d'interaction** : `amount_ratio` (montant relatif à la moyenne par type de transaction) et `amount_age_ratio` qui capturent des patterns non-linéaires. (2) **Les features statistiques par groupe** : `amount_vs_day_avg` et `amount_vs_hour_avg` qui identifient les transactions anormalement élevées par rapport aux moyennes temporelles, révélant des comportements suspects. (3) **L'encodage cyclique temporel** : `day_sin`, `day_cos`, `hour_sin`, `hour_cos` qui capturent les patterns circulaires des fraudes (par exemple, les fraudes nocturnes). (4) **Les indicateurs de seuil** : `is_high_amount` (montants au-dessus du 95e percentile) qui identifient les transactions exceptionnellement élevées souvent associées aux fraudes. Ces features ont permis au Random Forest d'augmenter le F1-Score de manière significative en capturant des interactions complexes que la baseline linéaire ne pouvait pas détecter.

**Q4. Enoncez tous les types de fraudes que vous avez décelé lors de votre analyse**

* **Fraudes temporelles nocturnes** : Concentration accrue des fraudes pendant les heures nocturnes (22h-6h), suggérant des vols de comptes pendant que les utilisateurs dorment.
* **Fraudes par type de transaction** : Certains types de transactions (notamment TRANSFER et CASH_OUT) présentent un taux de fraude significativement plus élevé que les autres, indiquant que ces canaux sont privilégiés par les fraudeurs.
* **Fraudes avec montants anormalement élevés** : Transactions avec des montants très supérieurs aux moyennes observées pour le type de transaction et la période, suggérant des tentatives de siphonner rapidement les comptes.
* **Fraudes ciblant les populations vulnérables** : Patterns suggérant des fraudes ciblant les seniors (ingénierie sociale) et les jeunes utilisateurs, avec des montants et comportements transactionnels atypiques pour ces groupes d'âge.
* **Fraudes temporelles par jour de la semaine** : Variations du taux de fraude selon les jours de la semaine, avec certains jours présentant des pics de fraudes, possiblement liés à des patterns d'activité des fraudeurs.

**Q5. Selon vous, quelle décision prendre si une transaction *en cours* est détectée comme *fraude* par le modèle ?**

Si une transaction en cours est détectée comme fraude par le modèle, nous recommandons une approche en plusieurs niveaux selon le score de confiance : (1) **Blocage immédiat** pour les transactions avec un score de probabilité très élevé (>0.9) et/ou des montants critiques, avec notification automatique au service sécurité. (2) **Mise en attente avec vérification manuelle** pour les transactions avec un score modéré (0.7-0.9), permettant une validation rapide par un agent avant validation ou blocage. (3) **Notification préventive** pour les scores plus faibles (0.5-0.7), avec envoi d'un SMS/email de confirmation au client pour valider la transaction. Cette approche graduée permet de bloquer les fraudes évidentes tout en minimisant l'impact sur l'expérience utilisateur, avec un mécanisme d'appel pour les cas litigieux. Le système doit également enregistrer toutes les transactions suspectes pour améliorer continuellement le modèle.

### **5\. Bibliographie**
