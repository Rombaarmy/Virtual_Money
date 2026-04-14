      color: #00ff00;
    }
    .contact {
      margin: 10px;
      font-size: 1.1em;
    }
    select, input, button {
      padding: 12px;
      font-size: 1em;
      margin: 10px;
      border-radius: 8px;
      border: none;
    }
    select, input {
      width: 80%;
      max-width: 400px;
    }
    button {
      background-color: #00cc00;
      color: white;
      cursor: pointer;
      transition: all 0.2s ease;
    }
    button:hover {
      background-color: #00e600;
      transform: scale(1.05);
    }
    .tarifs, .formulaire, .resume {
      margin: 20px auto;
      font-size: 1.1em;
      background: #1a1a1a;
      padding: 20px;
      border-radius: 8px;
      box-shadow: 0 0 10px rgba(0,255,0,0.2);
      max-width: 400px;
      display: none;
    }
    .tarifs button {
      display: block;
      width: 100%;
      margin-bottom: 10px;
    }
    .confirmation {
      margin-top: 15px;
      font-size: 0.9em;
      color: #aaa;
    }
    .highlight {
      color: #00ffcc;
      font-weight: bold;
    }
  </style>
</head>
<body>
  <header>
    <h1>VIRTUAL MONEY</h1>
    <div class="contact"><i class="fab fa-whatsapp"></i> WhatsApp : +226 56631696</div>
  </header>

  <select id="produitSelect" onchange="afficherTarifs()">
    <option value="">Choisir un produit</option>
    <option value="freefire">💎 Diamants Free Fire</option>
    <option value="pubg">🎮 UC PUBG</option>
  </select>

  <div id="tarifs" class="tarifs"></div>

  <div id="formulaire" class="formulaire">
    <div>
      <label for="identifiant">Ton identifiant de jeu :</label><br>
      <input type="text" id="identifiant" placeholder="Entrer l'ID du joueur">
    </div>
    <button onclick="afficherResume()">Continuer</button>
  </div>

  <div id="resume" class="resume">
    <p>📝 <strong>Résumé de ta commande :</strong></p>
    <p id="resumeDetails"></p>
    <div class="confirmation">
      Ensuite, envoie une <strong>capture du dépôt</strong> pour confirmer le paiement.
    </div>
    <button onclick="envoyerWhatsApp()">Commander</button>
  </div>

  <script>
    const tarifs = {
      freefire: [
        ["150", "1600 F"],
        ["310", "2500 F"],
        ["625", "5000 F"],
        ["925", "7500 F"],
        ["1235", "10 000 F"],
        ["1550", "12 000 F"],
        ["2475", "19 500 F"],
        ["3150", "20 000 F"],
        ["4700", "32 000 F"],
        ["6350", "40 000 F"]
      ],
      pubg: [
        ["60 UC", "900 F"],
        ["325 UC", "3 500 F"],
        ["660 UC", "6 500 F"],
        ["985 UC", "10 000 F"],
        ["1320 UC", "13 000 F"],
        ["1800 UC", "16 000 F"],
        ["2460 UC", "22 500 F"],
        ["3850 UC", "31 000 F"],
        ["5650 UC", "47 000 F"],
        ["8100 UC", "60 000 F"]
      ]
    };

    let produitChoisi = "";
    let packChoisi = "";
    let prixChoisi = "";

    function afficherTarifs() {
      const choix = document.getElementById("produitSelect").value;
      produitChoisi = choix;
      const conteneur = document.getElementById("tarifs");
      conteneur.innerHTML = "";
      document.getElementById("formulaire").style.display = "none";
      document.getElementById("resume").style.display = "none";
      if (tarifs[choix]) {
        tarifs[choix].forEach(([pack, prix]) => {
          const bouton = document.createElement("button");
          bouton.textContent = `${pack} → ${prix}`;
          bouton.onclick = () => {
            packChoisi = pack;
            prixChoisi = prix;
            document.getElementById("formulaire").style.display = "block";
            document.getElementById("resume").style.display = "none";
          };
          conteneur.appendChild(bouton);
        });
        conteneur.style.display = "block";
      } else {
        conteneur.style.display = "none";
      }
    }

    function afficherResume() {
      const id = document.getElementById("identifiant").value.trim();
      if (!id) {
        alert("Merci de remplir ton identifiant de jeu.");
        return;
      }
      const resume = `Tu as choisi le jeu <span class='highlight'>${produitChoisi.toUpperCase()}</span><br>
      Le pack : <span class='highlight'>${packChoisi} pour ${prixChoisi}</span><br>
      Ton identifiant : <span class='highlight'>${id}</span>`;
      document.getElementById("resumeDetails").innerHTML = resume;
      document.getElementById("resume").style.display = "block";
    }

    function envoyerWhatsApp() {
      const id = document.getElementById("identifiant").value.trim();
      const message = `Bonjour, je veux commander :\nProduit : ${produitChoisi}\nPack : ${packChoisi} → ${prixChoisi}\nID du joueur : ${id}`;
      const numero = "22656631696";
      const lien = `https://wa.me/${numero}?text=${encodeURIComponent(message)}`;
      window.open(lien, '_blank');
    }
  </script>
</body>
</html>
