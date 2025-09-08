# 🧠 FiveM UDP Spoof Exploit POC

[logo](https://external-content.duckduckgo.com/iu/?u=https%3A%2F%2Fuser-images.githubusercontent.com%2F42814853%2F180027603-514401ba-d6bb-425a-892c-0bc50bf38310.png&f=1&nofb=1&ipt=02524c650c97aa28f1e937140cc5c83efd73532c22fca7780f521907d97a8a65)

Une simple POC montrant comment bypass toutes les protections des serveurs FiveM en envoyant des paquets UDP spoofés avec une IP source falsifiée issue d’un fichier `range.txt` contenant les blocs IP des opérateurs français, ciblant ainsi intelligemment les serveurs localisés en France.

---

## 📌 Description

Ce projet est une **preuve de concept (PoC)** visant à illustrer **une faille structurelle** dans l’architecture réseau de **FiveM**, plateforme multijoueur pour GTA V.  
Le script envoie des **paquets UDP spoofés** vers un serveur FiveM, en se faisant passer pour des joueurs français (via des plages IP d’opérateurs français). Cela a pour effet de **perturber le fonctionnement des protections anti-DDoS ou anti-spoof** mises en place côté serveur.

L’objectif est **éducatif** : démontrer comment une mauvaise conception de la stack réseau côté serveur permet ce genre d’abus, et pourquoi les patchs mis en place ne règlent **jamais** complètement le problème.

---

## ⚠️ À quoi ça sert concrètement ?

- À **illustrer** un vecteur d'attaque connu mais toujours actif sur FiveM.
- À **montrer pourquoi** certains serveurs FR continuent de subir des lags, kicks ou fausses connexions.
- À **cibler uniquement les serveurs français** pour un test plus "réaliste", grâce à un fichier `range.txt` contenant les IPs des principaux FAI français (Orange, Free, SFR, Bouygues...).

---

## 🧠 Pourquoi ça marche encore ?

Parce que **FiveM utilise UDP**, et que l'**IP spoofing est nativement possible sur UDP**, contrairement à TCP.  
Le serveur ne vérifie pas l’authenticité de l’IP source au niveau transport, ce qui permet à un attaquant d’envoyer des paquets avec une IP falsifiée (spoofée), rendant la détection côté serveur difficile, voire impossible.

---

## 🔬 Détails techniques

- Utilise un **raw socket** pour créer manuellement des paquets IP/UDP.
- Les paquets envoyés contiennent un payload `"ÿÿÿÿgetinfo.xyz"`, qui simule une requête classique de client.
- Le header IP est forgé à la main avec un checksum recalculé à chaque paquet.
- Les IP source sont tirées dynamiquement du fichier `range.txt` pour maximiser la légitimité du spoof.
- L’envoi est multithreadé et rate-limité pour éviter les détections simples par analyse du trafic.

---

## 📂 Structure du projet

