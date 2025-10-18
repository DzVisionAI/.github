# 🚘 Système Automatisé de Reconnaissance de Plaques d'Immatriculation (ALPR) pour Véhicules Algériens

**Rapport de Projet de Deuxième Année, Deuxième Cycle — Intelligence Artificielle et Science des Données**  
*École Supérieure d'Informatique 08 Mai 1945 — Sidi Bel Abbès*  
**Étudiants :** Djaber Boudaoud, Lokman Zeddoun, OMARI Abdessalam

---

## 🚀 Résumé du Projet

Ce projet présente le développement d’un **système ALPR (Automatic License Plate Recognition)** de bout en bout, spécifiquement conçu pour les **véhicules algériens**.  
Le système est **modulaire** et s’appuie sur des techniques d’**intelligence artificielle** et de **vision par ordinateur** pour :

- La **détection et localisation** des plaques,
- L’**extraction de texte** (OCR),
- Le **suivi multi-objets**,
- Et la **classification de la couleur** des véhicules.

L’objectif principal est de créer un pipeline robuste capable de traiter des **flux vidéo** ou des **images**, de **localiser précisément** les plaques, d’**extraire les caractères**, et de **suivre les véhicules** à travers plusieurs trames.
![Exemple ALPR 1](https://github.com/DzVisionAI/alpr_dz/blob/main/example_alpr_1.png)
![Exemple ALPR 2](https://github.com/DzVisionAI/alpr_dz/blob/main/example_alpr_2.png)
🔗 **Code source et ressources :** [https://github.com/DzVisionAI](https://github.com/DzVisionAI)

---

## 🧱 Architecture et Pipeline du Système

Le système ALPR est organisé en plusieurs **modules indépendants**, intégrés dans un pipeline cohérent :

1. **Détection de véhicule**  
2. **Détection de plaque d’immatriculation**  
3. **Reconnaissance optique de caractères (OCR)**  
4. **Détection de la couleur du véhicule**  
5. **Suivi multi-objets (DeepSORT)**  
6. **Stockage et exploitation des données**

![Figure 1: Pipeline global du système ALPR](images/Figure_1_Pipeline_ALPR.png)

---

## 🔬 Modules Clés et Technologies Utilisées

### 1. 📌 Détection des Plaques d’Immatriculation

- **Modèle :** YOLOv8 (nano, medium, large)  
- **Architecture :** Backbone (extraction), Neck (fusion multi-échelle), Head (détection anchor-free)  
- **Dataset :** 10 125 images (Roboflow, 2025-04-02)

| Variante     | Paramètres (M) | Vitesse (ms/img) | mAP@0.5 | mAP@0.5-0.95 |
|-------------|---------------|------------------|--------|-------------|
| **YOLOv8n** | 3.2           | ∼3.4             | **98.7%** | 71%       |
| YOLOv8m     | 25.9          | ∼6.2             | 89.6%   | 69.7%     |
| YOLOv8l     | 43.7          | ∼8.9             | 91.2%   | **73%**   |

> 📝 **Remarque :** YOLOv8n a obtenu le meilleur score mAP@0.5, démontrant une excellente précision à faible contrainte.

![Figure 7: Exemple de détections](images/Figure_7_Detection_Example.png)

---

### 2. 📝 Reconnaissance Optique de Caractères (OCR)

- **Framework :** [EasyOCR](https://github.com/JaidedAI/EasyOCR)  
- **Pipeline :**  
  - Prétraitement (bruit, binarisation, correction de biais)  
  - Détection de texte avec **CRAFT**  
  - Reconnaissance via **CRNN + LSTM + CTC**

- **Post-traitement spécifique :**
  - Correction des caractères (ex. `O → 0`, `I → 1`, `S → 5`)  
  - Validation du format conforme aux plaques algériennes

---

### 3. 🎨 Détection de la Couleur du Véhicule

- **Dataset :** 2 004 images (Roboflow, 2022-09-22)  
- **Approches :**
  1. CNN personnalisé simple  
  2. **EfficientNetB0** avec transfert d’apprentissage

| Modèle             | Précision Validation |
|--------------------|----------------------|
| CNN personnalisé   | ≈ 50%               |
| **EfficientNetB0** | **≈ 89%**          |

![Figure 14: Aperçu du modèle EfficientNetB0](images/Figure_14_EfficientNetB0.png)

---

### 4. 🧭 Suivi des Véhicules

- **Algorithme :** [DeepSORT](https://arxiv.org/abs/1703.07402)  
- Basé sur :
  - Filtre de **Kalman**
  - Algorithme **hongrois** pour l’association de données
  - Caractéristiques visuelles pour une meilleure robustesse

---

## 💾 Stockage des Données

Les données extraites sont systématiquement stockées pour une analyse ultérieure.

- **Champs enregistrés :**
  - Numéro de plaque reconnu
  - Couleur du véhicule
  - Coordonnées des boîtes (véhicule & plaque)
  - Horodatage
  - Scores de confiance

- **Format actuel :** CSV  
- **Format futur envisagé :** Base de données relationnelle ou NoSQL

---

## 📝 Conclusion et Travaux Futurs

### ✅ Conclusion

Le système ALPR développé est une **solution complète**, adaptée aux plaques algériennes, intégrant efficacement **YOLOv8**, **EasyOCR**, et **DeepSORT** dans une architecture modulaire, flexible et extensible.

### 🚧 Améliorations Futures

1. ⚡ **Optimisation temps réel** (accélération matérielle, quantification)  
2. 🔤 **OCR plus robuste** (correction de perspective, fine-tuning local)  
3. 🗃 **Base de données intégrée** (SQL / NoSQL)  
4. 🖥 **Frontend Next.js** pour le monitoring en temps réel  
5. 🎨 **Intégration complète de la détection de couleur** dans le pipeline principal

---

© 2025 — Projet universitaire ESI SBA 🇩🇿
