# Diagramme de l'application
mon-appli/
├── index.html       # Prototype HTML/CSS/JS
├── README.md        # Documentation et diagramme
└── docs/            # Dossier optionnel pour images ou diagrammes
    └── diagramme.png

# 📌 Application de Consignes Interactives

Cette application permet aux **chefs** de diffuser des consignes aux **intervenants**, avec gestion des priorités, notifications, surlignage des modifications, et un historique automatique après chaque journée.

---

## 👥 Rôles des utilisateurs

- **Chefs** :
  - Créent et modifient les consignes
  - Marquent certaines consignes comme **prioritaires**
  - Modifient l’encadré des **informations importantes**
  - Consultent l’historique

- **Intervenants** :
  - Lisent les consignes du jour
  - Peuvent cocher « ✅ fait » sur une consigne
  - Peuvent ajouter un commentaire si la consigne n’est pas réalisée
  - Ont accès à l’historique en **lecture seule**

---

## ⚙️ Fonctionnalités principales

- 📢 **Diffusion avec notification** dès qu’une consigne est créée ou modifiée  
- 🔴 **Consignes prioritaires** affichées en rouge  
- 🟨 **Surlignage des modifications** (même une lettre changée)  
- ✅ **Checklist** pour marquer une consigne comme faite  
- 💬 **Commentaires** si non fait    
- 📚 **Archivage automatique** : les consignes passées sont déplacées dans un bandeau « Historique », **en lecture seule** pour garder une trace

---

## 📊 Diagramme général

```mermaid
flowchart TD
  subgraph Utilisateurs
    Chef
    Intervenant
  end

  Chef -->|Crée/Modifie| Base[(Base de données)]
  Intervenant -->|Lit/Exécute| Base

  Base -->|🔔 Notifications temps réel| Intervenant
  Base -->|🔔 Notifications temps réel| Ch
