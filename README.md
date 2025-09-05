<!DOCTYPE html>
<html lang="fr">
<head>
  <meta charset="UTF-8">
  <title>Consignes du jour</title>
  <style>
    body { font-family: Arial, sans-serif; margin: 20px; }
    h1 { color: #333; }
    .consigne { margin: 8px 0; padding: 8px; border: 1px solid #ccc; border-radius: 5px; }
    .consigne.prioritaire { background-color: #ffcccc; } /* Rouge */
    .consigne.fait { background-color: #ccffcc; } /* Vert */
    .consigne.modifie { background-color: #ffffcc; } /* Jaune */
    .archives { margin-top: 30px; border-top: 2px solid #333; padding-top: 15px; }
    .date-archive { font-weight: bold; margin-top: 15px; }
    button { margin: 5px; padding: 5px 10px; }
  </style>
</head>
<body>
  <h1 id="titre-consignes"></h1>

  <div id="consignes"></div>

  <div>
    <button onclick="ajouterConsigne()">➕ Ajouter une consigne</button>
    <button onclick="toggleChef()">👨‍💼 Mode Chef</button>
  </div>

  <div class="archives">
    <h2>Archives</h2>
    <div id="archives"></div>
  </div>

  <script>
    let consignes = JSON.parse(localStorage.getItem("consignes")) || [];
    let archives = JSON.parse(localStorage.getItem("archives")) || {};
    let chefMode = false;

    // Titre avec la date automatique
    function majTitre() {
      const date = new Date();
      const options = { weekday: "long", year: "numeric", month: "long", day: "numeric" };
      document.getElementById("titre-consignes").innerText = 
        "Consignes du " + date.toLocaleDateString("fr-FR", options);
    }

    // Afficher les consignes
    function afficherConsignes() {
      const div = document.getElementById("consignes");
      div.innerHTML = "";
      consignes.forEach((c, i) => {
        const consDiv = document.createElement("div");
        consDiv.className = "consigne";
        if (c.prioritaire) consDiv.classList.add("prioritaire");
        if (c.fait) consDiv.classList.add("fait");
        if (c.modifie) consDiv.classList.add("modifie");

        const check = document.createElement("input");
        check.type = "checkbox";
        check.checked = c.fait;
        check.onchange = () => marquerFait(i);

        const span = document.createElement("span");
        span.innerText = c.texte;

        consDiv.appendChild(check);
        consDiv.appendChild(span);

        if (chefMode) {
          const btnP = document.createElement("button");
          btnP.innerText = "🔴 Prioritaire";
          btnP.onclick = () => basculerPrioritaire(i);
          consDiv.appendChild(btnP);

          const btnM = document.createElement("button");
          btnM.innerText = "✏️ Modifier";
          btnM.onclick = () => modifierConsigne(i);
          consDiv.appendChild(btnM);

          const btnS = document.createElement("button");
          btnS.innerText = "🗑️ Supprimer";
          btnS.onclick = () => supprimerConsigne(i);
          consDiv.appendChild(btnS);
        }

        div.appendChild(consDiv);
      });
      sauvegarder();
    }

    // Ajouter une consigne
    function ajouterConsigne() {
      const texte = prompt("Nouvelle consigne :");
      if (texte) {
        consignes.push({ texte, fait: false, prioritaire: false, modifie: false });
        afficherConsignes();
      }
    }

    // Modifier une consigne
    function modifierConsigne(i) {
      const nvTexte = prompt("Modifier la consigne :", consignes[i].texte);
      if (nvTexte && nvTexte !== consignes[i].texte) {
        consignes[i].texte = nvTexte;
        consignes[i].modifie = true;
        afficherConsignes();
      }
    }

    // Supprimer une consigne
    function supprimerConsigne(i) {
      consignes.splice(i, 1);
      afficherConsignes();
    }

    // Bascule prioritaire
    function basculerPrioritaire(i) {
      consignes[i].prioritaire = !consignes[i].prioritaire;
      afficherConsignes();
    }

    // Marquer fait -> va dans archives
    function marquerFait(i) {
      consignes[i].fait = true;
      const date = new Date().toLocaleDateString("fr-FR");
      if (!archives[date]) archives[date] = [];
      archives[date].push(consignes[i]);
      consignes.splice(i, 1);
      afficherConsignes();
      afficherArchives();
    }

    // Afficher les archives
    function afficherArchives() {
      const div = document.getElementById("archives");
      div.innerHTML = "";
      for (const date in archives) {
        const titre = document.createElement("div");
        titre.className = "date-archive";
        titre.innerText = date;
        div.appendChild(titre);

        archives[date].forEach(c => {
          const p = document.createElement("div");
          p.innerText = c.texte;
          div.appendChild(p);
        });
      }
      sauvegarder();
    }

    // Sauvegarder dans localStorage
    function sauvegarder() {
      localStorage.setItem("consignes", JSON.stringify(consignes));
      localStorage.setItem("archives", JSON.stringify(archives));
    }

    // Mode Chef
    function toggleChef() {
      chefMode = !chefMode;
      afficherConsignes();
    }

    // Mise à jour du titre
    majTitre();

    // Affichage initial
    afficherConsignes();
    afficherArchives();

    // Rafraîchissement automatique toutes les 30 sec
    setInterval(() => {
      afficherConsignes();
      afficherArchives();
    }, 30000);
  </script>
</body>
</html>
