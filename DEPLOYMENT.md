# 🚀 Guide de Déploiement

Ce guide vous accompagne dans le déploiement du système de gestion des demandes de documents.

## 📋 Table des matières

- [Prérequis](#prérequis)
- [Déploiement sur serveur partagé](#déploiement-sur-serveur-partagé)
- [Déploiement sur VPS](#déploiement-sur-vps)
- [Configuration de production](#configuration-de-production)
- [Optimisations](#optimisations)
- [Sécurité](#sécurité)
- [Maintenance](#maintenance)

## 🔧 Prérequis

### Serveur
- PHP >= 8.2
- MySQL >= 8.0 ou MariaDB >= 10.3
- Composer
- Node.js >= 18.x
- Extension PHP GD activée
- SSL/TLS (recommandé)

### Extensions PHP requises
```
- BCMath
- Ctype
- Fileinfo
- JSON
- Mbstring
- OpenSSL
- PDO
- Tokenizer
- XML
- GD
```

## 🌐 Déploiement sur serveur partagé

### 1. Préparer les fichiers

```bash
# Sur votre machine locale
composer install --optimize-autoloader --no-dev
npm run build
```

### 2. Upload des fichiers

Transférer tous les fichiers sauf :
- `.env` (créer un nouveau sur le serveur)
- `node_modules/`
- `storage/` (créer les dossiers sur le serveur)

### 3. Configuration

Créer `.env` sur le serveur :

```env
APP_NAME="MIAGE Documents"
APP_ENV=production
APP_KEY=base64:VOTRE_CLE_GENEREE
APP_DEBUG=false
APP_URL=https://votre-domaine.com

DB_CONNECTION=mysql
DB_HOST=localhost
DB_PORT=3306
DB_DATABASE=nom_base_de_donnees
DB_USERNAME=utilisateur_db
DB_PASSWORD=mot_de_passe_db

MAIL_MAILER=smtp
MAIL_HOST=smtp.votre-serveur.com
MAIL_PORT=587
MAIL_USERNAME=votre@email.com
MAIL_PASSWORD=votre_mot_de_passe
MAIL_ENCRYPTION=tls
MAIL_FROM_ADDRESS=noreply@votre-domaine.com
MAIL_FROM_NAME="${APP_NAME}"
```

### 4. Permissions

```bash
chmod -R 755 storage bootstrap/cache
chmod -R 775 storage/app/public
chmod -R 775 storage/framework
chmod -R 775 storage/logs
```

### 5. Migrations

```bash
php artisan migrate --force
php artisan storage:link
```

## 🖥️ Déploiement sur VPS (Ubuntu/Debian)

### 1. Installation des dépendances

```bash
# Mise à jour du système
sudo apt update && sudo apt upgrade -y

# Installation de PHP 8.2
sudo apt install -y php8.2 php8.2-fpm php8.2-mysql php8.2-xml php8.2-mbstring \
    php8.2-curl php8.2-zip php8.2-gd php8.2-bcmath php8.2-intl

# Installation de MySQL
sudo apt install -y mysql-server

# Installation de Composer
curl -sS https://getcomposer.org/installer | php
sudo mv composer.phar /usr/local/bin/composer

# Installation de Node.js
curl -fsSL https://deb.nodesource.com/setup_18.x | sudo -E bash -
sudo apt install -y nodejs

# Installation de Nginx
sudo apt install -y nginx
```

### 2. Configuration MySQL

```bash
sudo mysql_secure_installation

# Créer la base de données
sudo mysql -u root -p
```

```sql
CREATE DATABASE universite_miage CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
CREATE USER 'miage_user'@'localhost' IDENTIFIED BY 'mot_de_passe_securise';
GRANT ALL PRIVILEGES ON universite_miage.* TO 'miage_user'@'localhost';
FLUSH PRIVILEGES;
EXIT;
```

### 3. Cloner et configurer le projet

```bash
cd /var/www
sudo git clone https://github.com/Mohkone01/site-miage.git
cd site-miage

# Installation des dépendances
composer install --optimize-autoloader --no-dev
npm install
npm run build

# Configuration
cp .env.example .env
php artisan key:generate

# Éditer .env avec les bonnes valeurs
sudo nano .env

# Migrations
php artisan migrate --force
php artisan storage:link
php artisan config:cache
php artisan route:cache
php artisan view:cache
```

### 4. Configuration Nginx

Créer `/etc/nginx/sites-available/site-miage` :

```nginx
server {
    listen 80;
    listen [::]:80;
    server_name votre-domaine.com www.votre-domaine.com;
    root /var/www/site-miage/public;

    add_header X-Frame-Options "SAMEORIGIN";
    add_header X-Content-Type-Options "nosniff";

    index index.php;

    charset utf-8;

    location / {
        try_files $uri $uri/ /index.php?$query_string;
    }

    location = /favicon.ico { access_log off; log_not_found off; }
    location = /robots.txt  { access_log off; log_not_found off; }

    error_page 404 /index.php;

    location ~ \.php$ {
        fastcgi_pass unix:/var/run/php/php8.2-fpm.sock;
        fastcgi_param SCRIPT_FILENAME $realpath_root$fastcgi_script_name;
        include fastcgi_params;
    }

    location ~ /\.(?!well-known).* {
        deny all;
    }
}
```

Activer le site :

```bash
sudo ln -s /etc/nginx/sites-available/site-miage /etc/nginx/sites-enabled/
sudo nginx -t
sudo systemctl restart nginx
```

### 5. SSL avec Let's Encrypt

```bash
sudo apt install -y certbot python3-certbot-nginx
sudo certbot --nginx -d votre-domaine.com -d www.votre-domaine.com
```

### 6. Permissions

```bash
sudo chown -R www-data:www-data /var/www/site-miage
sudo chmod -R 755 /var/www/site-miage
sudo chmod -R 775 /var/www/site-miage/storage
sudo chmod -R 775 /var/www/site-miage/bootstrap/cache
```

## ⚙️ Configuration de production

### Optimisations Laravel

```bash
# Cache de configuration
php artisan config:cache

# Cache des routes
php artisan route:cache

# Cache des vues
php artisan view:cache

# Optimisation de l'autoloader
composer dump-autoload --optimize
```

### Configuration PHP (php.ini)

```ini
memory_limit = 256M
max_execution_time = 300
upload_max_filesize = 20M
post_max_size = 25M
max_input_vars = 3000

# OPcache
opcache.enable=1
opcache.memory_consumption=256
opcache.interned_strings_buffer=16
opcache.max_accelerated_files=10000
opcache.revalidate_freq=2
opcache.fast_shutdown=1
```

### Configuration MySQL

```sql
# Optimisations dans my.cnf
[mysqld]
innodb_buffer_pool_size = 1G
innodb_log_file_size = 256M
innodb_flush_log_at_trx_commit = 2
innodb_flush_method = O_DIRECT
```

## 🔒 Sécurité

### 1. Fichier .env

```bash
# Permissions strictes
chmod 600 .env
```

### 2. Pare-feu

```bash
sudo ufw allow 22/tcp
sudo ufw allow 80/tcp
sudo ufw allow 443/tcp
sudo ufw enable
```

### 3. Fail2Ban

```bash
sudo apt install -y fail2ban
sudo systemctl enable fail2ban
sudo systemctl start fail2ban
```

### 4. Headers de sécurité

Ajouter dans la configuration Nginx :

```nginx
add_header X-Frame-Options "SAMEORIGIN" always;
add_header X-Content-Type-Options "nosniff" always;
add_header X-XSS-Protection "1; mode=block" always;
add_header Referrer-Policy "no-referrer-when-downgrade" always;
add_header Content-Security-Policy "default-src 'self' http: https: data: blob: 'unsafe-inline'" always;
```

## 📊 Monitoring

### 1. Logs Laravel

```bash
# Voir les logs en temps réel
tail -f storage/logs/laravel.log

# Rotation des logs
sudo nano /etc/logrotate.d/laravel
```

### 2. Logs Nginx

```bash
# Logs d'accès
tail -f /var/log/nginx/access.log

# Logs d'erreur
tail -f /var/log/nginx/error.log
```

### 3. Supervision

Installer Supervisor pour les queues :

```bash
sudo apt install -y supervisor

# Configuration
sudo nano /etc/supervisor/conf.d/laravel-worker.conf
```

```ini
[program:laravel-worker]
process_name=%(program_name)s_%(process_num)02d
command=php /var/www/site-miage/artisan queue:work --sleep=3 --tries=3 --max-time=3600
autostart=true
autorestart=true
stopasgroup=true
killasgroup=true
user=www-data
numprocs=2
redirect_stderr=true
stdout_logfile=/var/www/site-miage/storage/logs/worker.log
stopwaitsecs=3600
```

```bash
sudo supervisorctl reread
sudo supervisorctl update
sudo supervisorctl start laravel-worker:*
```

## 🔄 Maintenance

### Sauvegardes automatiques

Créer un script de sauvegarde :

```bash
#!/bin/bash
# /usr/local/bin/backup-miage.sh

BACKUP_DIR="/var/backups/miage"
DATE=$(date +%Y%m%d_%H%M%S)

# Sauvegarde de la base de données
mysqldump -u miage_user -p'mot_de_passe' universite_miage > "$BACKUP_DIR/db_$DATE.sql"

# Sauvegarde des fichiers
tar -czf "$BACKUP_DIR/files_$DATE.tar.gz" /var/www/site-miage/storage/app

# Nettoyer les anciennes sauvegardes (garder 7 jours)
find $BACKUP_DIR -type f -mtime +7 -delete

echo "Sauvegarde terminée : $DATE"
```

Ajouter au crontab :

```bash
sudo crontab -e

# Sauvegarde quotidienne à 2h du matin
0 2 * * * /usr/local/bin/backup-miage.sh
```

### Mises à jour

```bash
cd /var/www/site-miage

# Mettre en mode maintenance
php artisan down

# Pull des dernières modifications
git pull origin main

# Mise à jour des dépendances
composer install --optimize-autoloader --no-dev
npm install
npm run build

# Migrations
php artisan migrate --force

# Clear et recache
php artisan config:clear
php artisan cache:clear
php artisan view:clear
php artisan config:cache
php artisan route:cache
php artisan view:cache

# Sortir du mode maintenance
php artisan up
```

## 📱 Vérification du responsive

### Tests sur différents appareils

1. **Chrome DevTools**
   - F12 → Toggle device toolbar
   - Tester sur : iPhone SE, iPhone 12 Pro, iPad, Desktop

2. **Tests réels**
   - iOS Safari
   - Android Chrome
   - Tablettes

3. **Points de rupture Tailwind**
   - `sm`: 640px
   - `md`: 768px
   - `lg`: 1024px
   - `xl`: 1280px
   - `2xl`: 1536px

## ✅ Checklist de déploiement

- [ ] PHP 8.2+ installé avec toutes les extensions
- [ ] MySQL configuré et sécurisé
- [ ] Extension GD activée
- [ ] Projet cloné et dépendances installées
- [ ] `.env` configuré correctement
- [ ] Migrations exécutées
- [ ] Storage link créé
- [ ] Permissions correctes (755/775)
- [ ] Nginx/Apache configuré
- [ ] SSL installé (Let's Encrypt)
- [ ] Pare-feu configuré
- [ ] Caches Laravel activés
- [ ] Supervisor configuré (si queues)
- [ ] Sauvegardes automatiques configurées
- [ ] Logs rotatifs configurés
- [ ] Tests responsive effectués
- [ ] Tests de charge effectués

## 🆘 Dépannage

### Erreur 500

```bash
# Vérifier les logs
tail -f storage/logs/laravel.log
tail -f /var/log/nginx/error.log

# Vérifier les permissions
ls -la storage/
ls -la bootstrap/cache/
```

### Problèmes de base de données

```bash
# Tester la connexion
php artisan tinker
>>> DB::connection()->getPdo();
```

### Cache problématique

```bash
php artisan cache:clear
php artisan config:clear
php artisan route:clear
php artisan view:clear
```

## 📞 Support

Pour toute question sur le déploiement :
- Ouvrir une issue sur GitHub
- Consulter la documentation Laravel

---

✅ Déploiement réussi ? N'oubliez pas de tester toutes les fonctionnalités !
