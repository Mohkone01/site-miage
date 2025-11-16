# 🤝 Guide de Contribution

Merci de votre intérêt pour contribuer au système de gestion des demandes de documents MIAGE !

## 📋 Code de conduite

En participant à ce projet, vous acceptez de respecter notre code de conduite :
- Soyez respectueux et professionnel
- Acceptez les critiques constructives
- Concentrez-vous sur ce qui est le mieux pour la communauté
- Faites preuve d'empathie envers les autres membres

## 🚀 Comment contribuer

### Signaler un bug

Si vous trouvez un bug, veuillez créer une issue avec :
- Une description claire du problème
- Les étapes pour reproduire le bug
- Le comportement attendu vs le comportement actuel
- Des captures d'écran si pertinent
- Votre environnement (OS, PHP version, etc.)

### Proposer une fonctionnalité

Pour proposer une nouvelle fonctionnalité :
1. Vérifiez qu'elle n'existe pas déjà dans les issues
2. Créez une issue détaillant :
   - Le problème que cela résout
   - La solution proposée
   - Des alternatives envisagées
   - Des mockups/wireframes si applicable

### Soumettre une Pull Request

1. **Fork le projet**
   ```bash
   git clone https://github.com/Mohkone01/site-miage.git
   cd site-miage
   ```

2. **Créer une branche**
   ```bash
   git checkout -b feature/ma-nouvelle-fonctionnalite
   ```

3. **Faire vos modifications**
   - Suivez les conventions de code Laravel
   - Ajoutez des tests si nécessaire
   - Mettez à jour la documentation

4. **Commit vos changements**
   ```bash
   git commit -m "feat: ajout de la fonctionnalité X
   
   - Description détaillée
   - Impact sur le système
   - Tests ajoutés"
   ```

5. **Push vers votre fork**
   ```bash
   git push origin feature/ma-nouvelle-fonctionnalite
   ```

6. **Créer une Pull Request**
   - Décrivez clairement vos changements
   - Référencez les issues liées
   - Ajoutez des captures d'écran si pertinent

## 📝 Conventions de code

### PHP (PSR-12)

```php
<?php

namespace App\Services;

use App\Models\Document;
use Illuminate\Support\Collection;

class DocumentService
{
    /**
     * Récupérer les documents d'un utilisateur
     *
     * @param int $userId
     * @return Collection
     */
    public function getUserDocuments(int $userId): Collection
    {
        return Document::where('user_id', $userId)
            ->orderBy('created_at', 'desc')
            ->get();
    }
}
```

### Blade Templates

```blade
<div class="container mx-auto px-4">
    @if($documents->isNotEmpty())
        <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-4">
            @foreach($documents as $document)
                <x-document-card :document="$document" />
            @endforeach
        </div>
    @else
        <p class="text-gray-500">Aucun document trouvé.</p>
    @endif
</div>
```

### JavaScript

```javascript
// Utiliser Alpine.js pour l'interactivité
document.addEventListener('alpine:init', () => {
    Alpine.data('documentManager', () => ({
        documents: [],
        loading: false,
        
        async fetchDocuments() {
            this.loading = true;
            try {
                const response = await fetch('/api/documents');
                this.documents = await response.json();
            } catch (error) {
                console.error('Erreur:', error);
            } finally {
                this.loading = false;
            }
        }
    }));
});
```

## 🧪 Tests

Avant de soumettre une PR, assurez-vous que tous les tests passent :

```bash
# Tests unitaires
php artisan test

# Tests avec couverture
php artisan test --coverage

# Tests spécifiques
php artisan test --filter=DocumentRequestTest
```

## 📚 Documentation

Si vous ajoutez une nouvelle fonctionnalité :
- Mettez à jour le README.md
- Ajoutez des commentaires dans le code
- Créez/mettez à jour la documentation technique

## 🔍 Revue de code

Toutes les Pull Requests seront revues selon ces critères :
- ✅ Qualité du code
- ✅ Tests appropriés
- ✅ Documentation à jour
- ✅ Pas de régression
- ✅ Performance
- ✅ Sécurité

## 💡 Bonnes pratiques

### Commits

Utilisez des messages de commit clairs :
- `feat:` nouvelle fonctionnalité
- `fix:` correction de bug
- `docs:` documentation
- `style:` formatage
- `refactor:` refactoring
- `test:` ajout de tests
- `chore:` tâches de maintenance

Exemple :
```
feat: ajout de la génération de certificats de scolarité

- Nouveau template PDF pour les certificats
- Validation des données étudiantes
- Tests unitaires ajoutés
- Documentation mise à jour

Closes #123
```

### Code Review

Lors de la revue de code :
- Soyez constructif et respectueux
- Expliquez le "pourquoi" de vos suggestions
- Proposez des alternatives
- Approuvez si tout est bon

## 🎯 Priorités actuelles

Domaines où nous recherchons des contributions :
- 🐛 Correction de bugs
- 📱 Amélioration du responsive
- ♿ Accessibilité (WCAG 2.1)
- 🌍 Internationalisation
- ⚡ Optimisations de performance
- 📝 Documentation
- 🧪 Tests

## 📞 Questions ?

Si vous avez des questions :
- Ouvrez une issue avec le tag `question`
- Consultez la documentation existante
- Regardez les issues fermées

## 🙏 Remerciements

Merci à tous les contributeurs qui aident à améliorer ce projet !

---

Votre contribution, quelle qu'elle soit, est précieuse. Merci ! ⭐
