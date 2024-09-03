---
layout: post  
title: "Pourquoi et Comment Migrer Votre WordPress d'OVH vers AWS S3 Statique"  
date: 2024-08-20  
tags: [WordPress, OVH, AWS S3, CloudFront, Staatic, Laragon]  
categories: french  
---

## Pourquoi Migrer Votre WordPress en Statique sur AWS ?

Migrer un site WordPress vers une version statique hébergée sur AWS S3 a de nombreux avantages.

Cela peut sembler un peu complexe au premier abord, mais les bénéfices en termes de performance, sécurité et économies sont indéniables. Dans cet article, je vais vous expliquer comment faire cette migration et pourquoi c’est une excellente idée.

### Les Avantages

- **Vitesse et Performance** : Les sites statiques, c’est rapide. Très rapide. Contrairement à un site WordPress classique, un site statique se charge en un éclair car le contenu est déjà prêt et n’a pas besoin de faire appel à une base de données. Si vous utilisez AWS CloudFront pour distribuer votre contenu, c’est encore plus rapide.

- **Sécurité et Maintenance** : En passant à un site statique, vous éliminez beaucoup de failles de sécurité typiques de WordPress, comme les attaques par injection SQL ou les vulnérabilités des plugins. Et le meilleur ? Moins de maintenance ! Plus besoin de gérer les mises à jour fréquentes de WordPress ou de ses plugins.

- **Réduction des Coûts** : L’hébergement d’un site statique sur AWS S3 coûte souvent beaucoup moins cher qu’un site WordPress dynamique sur un serveur. Vous payez simplement pour le stockage et la bande passante utilisée, sans les soucis de gestion de serveur.

## Guide de Migration : Étape par Étape

### Contexte

J'avais un site WordPress hébergé sur OVH avec plusieurs noms de domaine, et je voulais tout regrouper sur AWS. J'ai donc exporté mon site WordPress en local, puis je l'ai converti en site statique avec Staatic. Ensuite, j'ai téléchargé les fichiers sur Amazon S3, activé le mode site web, et utilisé CloudFront pour distribuer mon contenu.

### Outils Utilisés

- [LocalWP](https://localwp.com/) : Un outil gratuit pour créer un environnement de développement WordPress sur votre ordinateur.
- [Staatic](https://staatic.com/) : Un plugin WordPress qui transforme votre site dynamique en site statique, parfait pour AWS S3.
- [Laragon](https://laragon.org/) : Un environnement de développement portable, rapide et puissant pour PHP, Node.js, Python, Java, Go, Ruby, etc.

### Problèmes Rencontrés

#### Exportation en Local

1. **Évitez d'exporter Staatic directement depuis OVH** : Le processus consomme beaucoup de ressources (RAM, CPU, stockage), ce qui peut causer des problèmes. Il est donc préférable de faire cela sur une version locale de votre site.

#### Blocage par Wordfence sur la Version en Ligne

Lorsque j'ai essayé d'exporter le site directement depuis OVH, Wordfence, un plugin de sécurité, a bloqué mon adresse IP, pensant qu'il s'agissait d'une activité suspecte.

**Détails de l'erreur** :
- **Adresse IP bloquée** : XXXXXX
- **Raison** : "Trop de requêtes par minute pour les robots ou les humains."
- **Durée du blocage** : 1 mois

Pour éviter ce genre de problème, il est donc crucial d’effectuer l’exportation Staatic sur une version locale.

### Liste des Actions

#### Étape 1 : Exportation en Local

La première étape consiste à obtenir une version hors ligne de votre site WordPress. J'ai utilisé LocalWP par Flywheel, mais comme j’ai eu des problèmes pour importer le site, j'ai finalement opté pour Laragon.

##### Procédure avec Laragon

1. **Création rapide d'un environnement WordPress** :
   - Quick create -> WordPress
   - Accédez à l'URL locale générée
   - Choisissez la langue lors de l'installation de WordPress

2. **Migration des fichiers et de la base de données** :
   - Avant de configurer l'identifiant et le mot de passe WordPress, copiez le dossier "wp-content" original (du site en ligne) et collez-le dans le nouveau dossier local WordPress.
   - Importez le fichier SQL original via phpMyAdmin pour remplacer la base de données locale.

3. **Résolution des erreurs** :
   - **Activation de SSL** : Pour corriger les erreurs 404, activez SSL.
   - **Modification du fichier `php.ini`** : Augmentez la mémoire allouée en modifiant `memory_limit = 128M` en `memory_limit = 512M`.
   - **Installation de l'extension `php_imagemagick`** : Pour gérer correctement les images. Modifiez le fichier `php.ini` et ajoutez `extension=php_imagemagick.dll`, puis redémarrez Apache.

4. **Désactivation des extensions non nécessaires** :
   - **Désactivation de Wordfence** : Wordfence n'est pas nécessaire en mode hors ligne. 
   - **Désactivation d'Autoptimize** : Autoptimize injectait du JavaScript et du CSS, ce qui causait des problèmes.

#### Étape 2 : Exportation Statique avec Staatic

Une fois les modifications locales effectuées, j'ai utilisé Staatic pour exporter le site en version statique. J'ai aussi créé un autre dossier dans Laragon, par exemple `wordpress-static`, pour tester cette version statique avant de la déployer sur AWS.

#### Étape 3 : Déploiement sur AWS S3 et CloudFront

Après l'exportation statique, j'ai téléchargé les fichiers sur Amazon S3, activé le mode site web, et configuré CloudFront pour distribuer le contenu.

Staatic permet de configurer S3 directement depuis l'interface, ce qui rend le déploiement automatique de votre site encore plus facile.

## Et Après ?

Migrer votre site WordPress vers une version statique sur AWS demande un peu de préparation, mais les avantages en termes de performance, sécurité, et coûts sont indéniables. 

Une fois la migration effectuée, vous pourrez vous concentrer sur l'amélioration de votre site sans vous soucier des mises à jour de WordPress ou des menaces de sécurité. Explorez aussi les autres possibilités offertes par AWS, comme l'automatisation des déploiements.

Pour moi, l'avantage principal est de ne plus payer l'hébergement OVH, ce qui représente une grosse économie. Au final, je gagne en argent, en rapidité, en sécurité et en stabilité. Je peux me concentrer sur l'essentiel : la création de mon site.

## Liens Utiles

- [Import/Export d'un site WordPress - Local (localwp.com)](https://localwp.com/help-docs/getting-started/how-to-import-a-wordpress-site-into-local/?utm_source=local-app&utm_medium=local-internal&utm_content=local-import-help-doc&utm_campaign=local)
- [Documentation Laragon](https://laragon.org/docs/quick-add)
- [PHP Imagick Setup](https://www.php.net/manual/en/imagick.setup.php)
- [Laragon Quick Add](https://laragon.org/docs/quick-add)
- [Staatic Documentation](https://staatic.com/documentation)
- [AMPPS](http://ampps.com/download) - Note : AMPPS est désormais payant et nécessite une version premium pour gérer WordPress, ce qui n'est pas idéal.

---

Vous pouvez également me suivre sur les réseaux sociaux pour rester informé des dernières publications.

[LinkedIn](https://www.linkedin.com/in/gatienboquet/) | [GitHub](https://github.com/gatienboquet)
