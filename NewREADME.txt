Voici le fichier `README.md` complet et professionnel pour **Bên-School**.

Ce document est la "Bible Technique" du projet. Il est conçu pour être lu par n'importe quel développeur (vous ou un futur collaborateur) pour comprendre, installer et déployer l'application sans aucune ambiguïté.

Copiez le contenu ci-dessous et sauvegardez-le dans un fichier nommé **`README.md`** à la racine de votre dossier projet.

---

```markdown
# 🏫 Bên-School - Plateforme LMS Hybride & Communautaire

> **Version :** 1.0.0  
> **Propriétaire :** Pasteur Galvani Mvondo  
> **Stack :** Next.js 14, Supabase, Google Gemini AI, Tailwind CSS.

---

## 📖 Présentation

**Bên-School** est une plateforme d'e-learning de nouvelle génération conçue pour l'excellence éducative et spirituelle. Contrairement aux LMS classiques, elle fusionne l'apprentissage vidéo avec une dimension communautaire forte et une gamification poussée.

### ✨ Fonctionnalités Clés
* **LMS Hybride :** Lecteur vidéo immersif, gestion de chapitres/leçons, ressources PDF.
* **Intelligence Artificielle :** Génération automatique de Quiz (QCM) via Google Gemini après chaque leçon.
* **Gamification :** Système de points XP, niveaux, badges (Trophées), et classements (Leaderboard).
* **Communauté ("Tribus") :** Espaces de discussion, flux d'actualité, likes et commentaires.
* **Paiements Universels :** Support des paiements par Carte (Stripe) et Mobile Money (Orange/MTN) via liens externes.
* **Sécurité RBAC :** Gestion stricte des rôles (Étudiant, Instructeur, Admin).

---

## 🛠️ Stack Technique

* **Frontend :** [Next.js 14](https://nextjs.org/) (App Router), React, TypeScript.
* **Styling :** [Tailwind CSS](https://tailwindcss.com/) + [Shadcn UI](https://ui.shadcn.com/) + Lucide Icons.
* **Backend (Serverless) :** Next.js Server Actions & API Routes.
* **Base de Données :** [Supabase](https://supabase.com/) (PostgreSQL).
* **Authentification :** Supabase Auth (Email/Password, Social Login).
* **IA :** Google Generative AI SDK (`gemini-pro`).

---

## 🚀 Installation & Démarrage Local

Suivez ces étapes pour lancer le projet sur votre machine.

### 1. Prérequis
* Node.js 18.17 ou supérieur.
* Un compte Supabase.
* Une clé API Google AI Studio (Gemini).

### 2. Cloner le projet
```bash
git clone [https://github.com/votre-username/ben-school.git](https://github.com/votre-username/ben-school.git)
cd ben-school

```

### 3. Installer les dépendances

```bash
npm install
# ou
yarn install

```

### 4. Configuration des Variables d'Environnement

Créez un fichier `.env.local` à la racine du projet. Copiez-y les clés suivantes (remplacez par vos vraies valeurs Supabase) :

```env
# --- SUPABASE CONFIGURATION ---
# URL publique de votre projet Supabase
NEXT_PUBLIC_SUPABASE_URL=[https://votre-projet.supabase.co](https://votre-projet.supabase.co)

# Clé Publique (Anon) - Peut être exposée côté client
NEXT_PUBLIC_SUPABASE_ANON_KEY=votre_cle_publique_ici

# Clé Secrète (Service Role) - NE JAMAIS EXPOSER COTÉ CLIENT
# Utilisée pour les tâches d'administration (bypasser le RLS)
SUPABASE_SERVICE_ROLE_KEY=votre_cle_secrete_service_role_ici

# --- APP CONFIGURATION ---
NEXT_PUBLIC_APP_URL=http://localhost:3000

# --- GOOGLE AI (Gemini) ---
GOOGLE_API_KEY=votre_cle_api_google_gemini

```

### 5. Lancer le serveur de développement

```bash
npm run dev

```

Ouvrez [http://localhost:3000](https://www.google.com/search?q=http://localhost:3000) dans votre navigateur.

---

## 🗄️ Base de Données (Setup)

Le projet repose sur une structure SQL précise (18 tables).
Le schéma inclut des **Triggers** pour la création automatique de profils et des politiques **RLS (Row Level Security)** pour la sécurité.

1. Allez sur votre Dashboard Supabase > **SQL Editor**.
2. Exécutez le script SQL complet fourni dans la documentation d'architecture (création tables `users`, `courses`, `payments`, etc.).
3. Vérifiez que les tables sont créées dans l'onglet **Table Editor**.

---

## 📂 Structure du Projet

L'architecture suit les standards Next.js 14 App Router.

```text
/src
 ├── /actions          # Server Actions (Logique Backend: Paiements, IA, Posts)
 ├── /app              # Pages & Routes (App Router)
 │   ├── (auth)        # Routes Login/Register
 │   ├── (dashboard)   # Espace connecté (Student/Instructor)
 │   ├── (marketing)   # Marketplace & Landing Page
 │   └── api           # Webhooks & Endpoints JSON
 ├── /components
 │   ├── /ui           # Composants atomiques (Boutons, Inputs...)
 │   ├── /lms          # Lecteur vidéo, Sidebar de cours
 │   ├── /gamification # Badges, XP Bar, Leaderboard
 │   └── /checkout     # Modals & Boutons de paiement
 ├── /lib
 │   ├── /supabase     # Clients Supabase (Client, Server, Admin)
 │   └── /ai           # Logique de prompt engineering Gemini
 └── /types            # Définitions TypeScript (Interfaces DB)

```

---

## 🔑 Rôles & Permissions

Le système gère 4 niveaux de permission via la colonne `role` dans la table `public.users` :

1. **Student (Défaut) :** Accès aux cours achetés et à la communauté.
2. **Instructor :** Peut créer des cours et voir ses élèves.
3. **Admin :** Contrôle total (via Dashboard Admin).

**Comment devenir Admin ?**
Après votre inscription, allez dans la table `users` sur Supabase et modifiez manuellement votre rôle :

```sql
UPDATE public.users SET role = 'admin' WHERE email = 'votre@email.com';

```

---

## 💳 Gestion des Paiements

Le système utilise une approche de **Redirection (Payment Links)** pour simplifier la gestion internationale et locale.

1. **Produit :** Chaque entrée dans la table `products` possède un champ `external_checkout_url`.
2. **Flux :**
* L'utilisateur clique sur "Acheter".
* Il est redirigé vers le lien (Stripe Payment Link ou Lien Campay/WhatsApp).
* Le paiement se fait sur la plateforme externe sécurisée.
* (Optionnel) Un webhook peut être configuré pour débloquer l'accès automatiquement, sinon l'accès est donné manuellement ou via validation.



---

## 🚢 Déploiement (Production)

La plateforme est optimisée pour **Vercel**.

1. Poussez votre code sur un dépôt GitHub (privé recommandé).
2. Créez un nouveau projet sur [Vercel](https://vercel.com).
3. Importez votre dépôt GitHub.
4. **Important :** Dans les paramètres Vercel ("Environment Variables"), ajoutez toutes les variables définies dans votre `.env.local`.
5. Cliquez sur **Deploy**.

---

## 🎨 Charte Graphique

* **Primaire (Violet) :** `#7C3AED` (Spiritualité & Technologie)
* **Secondaire (Bordeaux) :** `#800020` (Premium & Ancrage)
* **Fond :** Blanc ou `#F9FAFB` (Clarté)

---

*© 2024-2025 Bên-School - Tous droits réservés.*

```

```