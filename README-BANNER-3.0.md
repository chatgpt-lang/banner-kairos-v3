# Banner 3.0 - version experimentale

Cette version est separee de `Banner-2.0-modifie` pour tester les nouvelles idees sans casser la version stable.

## Ce qui est ajoute

- Deux nouveaux templates 9:16 :
  - `social-fr.html`
  - `social-en.html`
- Deux nouveaux onglets dans le simulateur :
  - Story FR 9:16
  - Story EN 9:16
- Les liens animes incluent maintenant les 6 formats :
  - Desktop FR/EN
  - Mobile FR/EN
  - Story FR/EN
- La generation HD reste volontairement limitee aux 4 anciens formats tant que Make/Doppio n'a pas les deux nouveaux modules Story. Comme ca, le bouton HD existant ne casse pas.
- Ajout d'un export/import projet en JSON :
  - textes
  - couleurs
  - URLs Cloudinary
  - positions/taille des elements

## Make a dupliquer, pas modifier directement

Pour tester sans risque :

1. Dupliquer le scenario Make actuel.
2. Brancher le nouveau formulaire uniquement sur le scenario duplique.
3. Garder l'ancien scenario actif pour la version stable.
4. Ajouter deux modules Doppio quand le format Story est valide :
   - `social-fr.html`, viewport `1080 x 1920`
   - `social-en.html`, viewport `1080 x 1920`
5. Ajouter dans la reponse/polling HD :
   - `social_fr`
   - `social_en`

## Photoroom

Le probleme Photoroom doit etre diagnostique dans Make. Les points a verifier :

- La cle API est toujours valide.
- L'URL envoyee a Photoroom est publique et accessible.
- Le module Photoroom renvoie bien une URL finale, pas seulement un job async.
- Les champs renvoyes sont bien mappes vers le formulaire.

Le formulaire cherche deja ces aliases :

- `booster_processed_url`, `booster_cutout_url`, `booster_no_bg_url`
- `display_processed_url`, `display_cutout_url`, `display_no_bg_url`
- `card1_processed_url`, `card1_cutout_url`, `card1_no_bg_url`
- `card2_processed_url`, `card2_cutout_url`, `card2_no_bg_url`

Si Make renvoie un autre nom de champ, il faudra soit changer le mapping Make, soit ajouter l'alias dans `applyProcessedAssets()`.

## Site public avec une utilisation gratuite

Une simple page statique ne suffit pas pour limiter serieusement l'usage. Pour une vraie limitation, il faut :

- un front heberge, par exemple Vercel
- une base de donnees, par exemple Supabase
- une table utilisateurs/projets/usages
- un statut admin pour Sarah

Limitation conseillee :

- par compte/email ou magic link
- plus fiable qu'une IP
- IP utilisable seulement comme signal anti-abus, pas comme limite principale

Regle simple pour le lancement :

- utilisateur normal : 1 generation gratuite
- admin : illimite

## Sauvegarde de projets

La version actuelle permet deja un export/import JSON local.

Version future avec compte client :

- enregistrer les projets dans Supabase
- un projet contient les assets, les textes, les couleurs et les overrides
- l'utilisateur peut rouvrir un projet, changer un produit et regenerator

## Fonds generes par IA

Ne pas mettre de cle API image directement dans le HTML public.

Architecture conseillee :

1. Le formulaire envoie l'image produit et le choix :
   - upload fond manuel
   - generer fond IA
2. Make ou un backend serveur appelle l'API image.
3. Le fond genere est stocke sur Cloudinary.
4. L'URL Cloudinary est renvoyee au formulaire.

Prompts de depart :

- Vertical/story : `peux tu me generer un fond vertical inspire du fond de cet objet ? pas de texte, pas de personnage juste le fond`
- Horizontal/web/mobile : `peux tu me generer un fond horizontal inspire du fond de cet objet ? pas de texte, pas de personnage juste le fond`

## Video

Pour le moment, garder les liens HTML animes.

L'export video propre demande un serveur ou un service de rendu capable de :

- ouvrir l'HTML anime
- enregistrer quelques secondes
- exporter MP4/WebM
- stocker le fichier

A faire quand l'entreprise a un serveur ou une architecture backend plus stable.
