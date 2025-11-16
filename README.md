# 📚 Système de Gestion des Demandes de Documents - MIAGE

> Module de demande et génération automatique de documents administratifs pour les étudiants

[![Laravel](https://img.shields.io/badge/Laravel-12.x-red.svg)](https://laravel.com)
[![PHP](https://img.shields.io/badge/PHP-8.2+-blue.svg)](https://php.net)
[![License](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)

## 📋 À propos du projet

Ce projet représente **le module de gestion des demandes de documents** développé dans le cadre d'un système universitaire plus large. Il permet aux étudiants de soumettre des demandes de documents administratifs (attestations, certificats, fiches d'inscription, etc.) et de suivre leur traitement à travers un workflow d'approbation multi-niveaux.

> **⚠️ Note importante** : Ce repository contient **la documentation complète** du module de demande de documents avec captures d'écran et quelques extraits de code d'exemple pour illustrer l'architecture. Le code source complet n'est pas public. Le site universitaire complet comprend également d'autres modules (gestion des cours, notes, emplois du temps, etc.) qui ne sont pas inclus ici.

## ✨ Fonctionnalités principales

### Pour les étudiants
- 📝 Soumission de demandes de documents en ligne
- 📎 Upload de pièces justificatives
- 🔔 Notifications en temps réel sur l'avancement
- 📄 Consultation et téléchargement des documents générés
- 📊 Historique complet des demandes

### Pour l'administration
- ✅ Workflow d'approbation multi-niveaux
- 🔄 Génération automatique de PDF signés électroniquement
- 👥 Gestion des rôles (Vérificateur, Responsable financier, Directeur MIAGE, Directeur UFR)
- 📈 Tableau de bord avec statistiques
- 🔍 Système de recherche et filtrage avancé

## 🖼️ Captures d'écran

### Interface étudiant

#### Tableau de bord étudiant
![Tableau de bord](png/CAP1.png)
*Vue d'ensemble des demandes et statistiques personnelles*

#### Formulaire de demande
![Formulaire de demande](png/CAP3.png)
*Interface intuitive pour soumettre une nouvelle demande*

#### Suivi des demandes
![Suivi des demandes](png/CAP4.png)
*Visualisation en temps réel du statut de chaque demande*

#### Documents générés
![Documents générés](png/CAP5.png)
*Consultation et téléchargement des documents finalisés*

### Interface administration

#### Tableau de bord administrateur
![Dashboard admin](png/CAP7.png)
*Vue globale avec statistiques et métriques*

#### Gestion des demandes
![Gestion demandes](png/CAP8.png)
*Interface de traitement et validation des demandes*

#### Workflow d'approbation
![Workflow](png/CAP9.png)
*Suivi du processus d'approbation multi-niveaux*

#### Génération de documents
![Documents admin](png/CAP10.png)
*Prévisualisation et gestion des documents générés*

#### Notifications
![Notifications](png/CAP11.png)
*Système de notifications en temps réel*

#### Paramètres
![Paramètres](png/CAP13.png)
*Configuration du système et gestion des utilisateurs*

#### Profil utilisateur
![Profil](png/CAP14.png)
*Gestion du profil et des préférences*

## 🛠️ Technologies utilisées

### Backend
- **Laravel 12.x** - Framework PHP moderne
- **PHP 8.2+** - Langage de programmation
- **MySQL** - Base de données relationnelle
- **DomPDF** - Génération de documents PDF
- **Laravel Notifications** - Système de notifications

### Frontend
- **Blade Templates** - Moteur de templates Laravel
- **Tailwind CSS** - Framework CSS utility-first
- **Alpine.js** - Framework JavaScript léger
- **Lucide Icons** - Bibliothèque d'icônes moderne

### Outils de développement
- **Composer** - Gestionnaire de dépendances PHP
- **NPM** - Gestionnaire de paquets JavaScript
- **Vite** - Build tool moderne

## 📦 Installation

### Prérequis

- PHP >= 8.2
- Composer
- MySQL >= 8.0
- Node.js >= 18.x
- Extension PHP GD (pour la génération de PDF avec images)

### Étapes d'installation

1. **Cloner le repository**
```bash
git clone https://github.com/Mohkone01/site-miage.git
cd site-miage
```

2. **Installer les dépendances PHP**
```bash
composer install
```

3. **Installer les dépendances JavaScript**
```bash
npm install
```

4. **Configurer l'environnement**
```bash
cp .env.example .env
php artisan key:generate
```

5. **Configurer la base de données**

Éditer le fichier `.env` :
```env
DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=universite_miage
DB_USERNAME=votre_utilisateur
DB_PASSWORD=votre_mot_de_passe
```

6. **Exécuter les migrations**
```bash
php artisan migrate
```

7. **Générer les données de test (optionnel)**
```bash
php artisan db:seed
```

8. **Compiler les assets**
```bash
npm run build
```

9. **Démarrer le serveur**
```bash
php artisan serve
```

Le site sera accessible sur `http://localhost:8000`

## 🔧 Configuration

### Extension PHP GD

L'extension GD est requise pour la génération de PDF avec images (logos, photos).

**Windows :**
1. Ouvrir `php.ini`
2. Décommenter : `extension=gd`
3. Redémarrer le serveur web

**Linux :**
```bash
sudo apt-get install php-gd
sudo systemctl restart apache2
```

### Permissions des dossiers

```bash
chmod -R 775 storage bootstrap/cache
chown -R www-data:www-data storage bootstrap/cache
```

## 📱 Responsive Design

Le système est entièrement responsive et optimisé pour :
- 📱 Smartphones (iOS & Android)
- 📱 Tablettes
- 💻 Ordinateurs de bureau
- 🖥️ Grands écrans

Tous les composants s'adaptent automatiquement à la taille de l'écran pour une expérience utilisateur optimale.

## 🔐 Rôles et permissions

Le système implémente un contrôle d'accès basé sur les rôles (RBAC) :

| Rôle | Permissions |
|------|------------|
| **Étudiant** | Soumettre des demandes, consulter ses documents |
| **Vérificateur** | Vérifier les demandes initiales |
| **Responsable Financier** | Approuver les aspects financiers |
| **Responsable Niveau** | Valider les demandes de son niveau |
| **Directeur MIAGE** | Traiter les demandes MIAGE |
| **Directeur UFR** | Approbation finale |

## 📊 Workflow des demandes

```
Étudiant soumet demande
    ↓
Vérificateur valide
    ↓
Responsable financier approuve
    ↓
Responsable niveau valide
    ↓
Directeur MIAGE traite
    ↓
Directeur UFR approuve
    ↓
Document généré automatiquement
    ↓
Étudiant notifié et peut télécharger
```

## 🎨 Architecture du code

```
app/
├── Http/
│   ├── Controllers/
│   │   ├── DocumentRequestController.php    # Gestion des demandes
│   │   └── DocumentGenereController.php     # Gestion des documents générés
│   └── Middleware/
│       └── CheckRole.php                     # Vérification des rôles
├── Models/
│   ├── DocumentRequest.php                   # Modèle de demande
│   ├── DocumentGenere.php                    # Modèle de document généré
│   └── User.php                              # Modèle utilisateur
├── Services/
│   ├── DocumentPdfService.php                # Génération de PDF
│   ├── DocumentRequestAdapterService.php     # Adaptation des données
│   └── SignatureElectroniqueService.php      # Signature électronique
└── Notifications/
    └── DocumentRequestNotification.php       # Notifications

resources/
├── views/
│   ├── documents/
│   │   ├── index.blade.php                   # Liste des demandes
│   │   ├── create.blade.php                  # Formulaire de demande
│   │   ├── show.blade.php                    # Détails d'une demande
│   │   └── mes_documents.blade.php           # Documents de l'étudiant
│   └── layouts/
│       └── app.blade.php                     # Layout principal
└── js/
    └── app.js                                # JavaScript principal
```

## 🧪 Tests

```bash
# Exécuter tous les tests
php artisan test

# Tests avec couverture
php artisan test --coverage
```

## 📝 Types de documents supportés

- 📄 Attestation de fréquentation
- 📄 Certificat de scolarité
- 📄 Relevé de notes
- 📄 Attestation de réussite
- 📄 Fiche d'inscription
- 📄 Attestation de stage
- 📄 Certificat de fin d'études

## 🤝 Contribution

Les contributions sont les bienvenues ! N'hésitez pas à :

1. Fork le projet
2. Créer une branche (`git checkout -b feature/AmazingFeature`)
3. Commit vos changements (`git commit -m 'Add some AmazingFeature'`)
4. Push vers la branche (`git push origin feature/AmazingFeature`)
5. Ouvrir une Pull Request

## 📦 Contenu de ce repository

### ✅ Ce qui est inclus

- **Documentation complète** : README détaillé, guides de déploiement, documentation technique
- **Captures d'écran** : 11 images illustrant toutes les fonctionnalités
- **Architecture** : Description détaillée du système et du workflow
- **Exemples de code** : Quelques extraits pour illustrer la structure
- **Configuration** : Fichiers de configuration de base (composer.json, package.json)

### ❌ Ce qui n'est PAS inclus

- **Code source complet** : Le code backend et frontend complet n'est pas public
- **Autres modules** : Gestion des cours, notes, emplois du temps, etc.
- **Données sensibles** : Aucune donnée de production ou information confidentielle
- **Logique métier complète** : Les services et la logique métier restent privés

> **💡 Pourquoi ?** Ce repository sert de **portfolio et documentation** pour présenter le projet. Le code source complet est maintenu en privé pour des raisons de sécurité et de propriété intellectuelle.

## 📄 Licence

Ce projet est sous licence MIT. Voir le fichier `LICENSE` pour plus de détails.

## 👨‍💻 Auteur

**Mohkone01**

- GitHub: [@Mohkone01](https://github.com/Mohkone01)

## 🙏 Remerciements

- Laravel Framework
- Tailwind CSS
- DomPDF
- Tous les contributeurs open source

## 📞 Support

Pour toute question ou problème :
- Ouvrir une issue sur GitHub
- Consulter la documentation Laravel

---

⭐ Si ce projet vous a été utile, n'hésitez pas à lui donner une étoile !
