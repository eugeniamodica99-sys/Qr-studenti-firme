<!DOCTYPE html>
<html lang="it">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>QR Code Personalizzato</title>

  <script src="https://cdn.jsdelivr.net/npm/qrcode/build/qrcode.min.js"></script>

  <style>
    body {
      font-family: Arial, sans-serif;
      background: #eaf7ea;
      padding: 20px;
      display: flex;
      justify-content: center;
    }

    .container {
      background: white;
      padding: 25px;
      border-radius: 16px;
      max-width: 420px;
      width: 100%;
      box-shadow: 0 4px 14px rgba(0, 100, 0, 0.15);
      border: 2px solid #cde8cd;
    }

    h1 {
      text-align: center;
      font-size: 1.4rem;
      font-weight: 800;
      margin-bottom: 24px;
      text-transform: uppercase;
      color: #d60000;
    }

    label {
      display: block;
      margin-top: 15px;
      font-weight: bold;
      color: #1f5f1f;
    }

    input, select, button {
      width: 100%;
      padding: 10px;
      margin-top: 6px;
      border-radius: 8px;
      border: 1px solid #9ccc9c;
      font-size: 1rem;
      box-sizing: border-box;
    }

    input:focus, select:focus {
      outline: none;
      border-color: #2e8b57;
      box-shadow: 0 0 6px rgba(46, 139, 87, 0.3);
    }

    button {
      margin-top: 20px;
      background: #2e8b57;
      color: white;
      border: none;
      cursor: pointer;
      font-weight: bold;
      transition: 0.2s;
    }

    button:hover {
      background: #246b43;
    }

    #qrcode {
      margin-top: 25px;
      text-align: center;
    }

    canvas {
      margin-top: 10px;
    }
  </style>
</head>

<body>
  <div class="container">

    <h1>Compila e conserva il QR code personalizzato che verrà generato</h1>

    <label for="facolta">Facoltà</label>
    <input type="text" id="facolta" placeholder="Inserisci la facoltà" required />

    <label for="tipoCorso">Tipo di corso</label>
    <select id="tipoCorso" required onchange="aggiornaAnni()">
      <option value="">Seleziona</option>
      <option value="Laurea a ciclo unico">Laurea a ciclo unico</option>
      <option value="Laurea triennale">Laurea triennale</option>
      <option value="Laurea magistrale">Laurea magistrale</option>
    </select>

    <label for="anno">Anno</label>
    <select id="anno" required disabled>
      <option value="">Prima seleziona il tipo di corso</option>
    </select>

    <label for="nome">Nome</label>
    <input type="text" id="nome" placeholder="Inserisci il nome" required />

    <label for="cognome">Cognome</label>
    <input type="text" id="cognome" placeholder="Inserisci il cognome" required />

    <button onclick="generaQR()">Genera QR Code</button>

    <div id="qrcode"></div>

  </div>

  <script>
    function aggiornaAnni() {
      const tipoCorso = document.getElementById("tipoCorso").value;
      const annoSelect = document.getElementById("anno");

      annoSelect.innerHTML = "";

      let opzioni = [];

      if (tipoCorso === "Laurea a ciclo unico") {
        opzioni = [
          "Primo anno",
          "Secondo anno",
          "Terzo anno",
          "Quarto anno",
          "Quinto anno"
        ];
      } else if (tipoCorso === "Laurea triennale") {
        opzioni = [
          "Primo anno",
          "Secondo anno",
          "Terzo anno"
        ];
      } else if (tipoCorso === "Laurea magistrale") {
        opzioni = [
          "Primo anno",
          "Secondo anno"
        ];
      }

      if (opzioni.length > 0) {
        annoSelect.disabled = false;
        annoSelect.innerHTML = '<option value="">Seleziona anno</option>';

        opzioni.forEach(function(anno) {
          const option = document.createElement("option");
          option.value = anno;
          option.textContent = anno;
          annoSelect.appendChild(option);
        });
      } else {
        annoSelect.disabled = true;
        annoSelect.innerHTML = '<option value="">Prima seleziona il tipo di corso</option>';
      }
    }

    function generaQR() {
      const facolta = document.getElementById("facolta").value.trim();
      const tipoCorso = document.getElementById("tipoCorso").value;
      const anno = document.getElementById("anno").value;
      const nome = document.getElementById("nome").value.trim();
      const cognome = document.getElementById("cognome").value.trim();

      if (!facolta || !tipoCorso || !anno || !nome || !cognome) {
        alert("Compila tutti i campi prima di generare il QR Code.");
        return;
      }

      const testoQR =
        "Facoltà: " + facolta + "\n" +
        "Tipo di corso: " + tipoCorso + "\n" +
        "Anno: " + anno + "\n" +
        "Nome: " + nome + "\n" +
        "Cognome: " + cognome;

      const qrcodeDiv = document.getElementById("qrcode");
      qrcodeDiv.innerHTML = "";

      QRCode.toCanvas(testoQR, { width: 250 }, function(error, canvas) {
        if (error) {
          console.error(error);
          alert("Errore nella generazione del QR Code.");
          return;
        }

        qrcodeDiv.appendChild(canvas);
      });
    }
  </script>
</body>
</html>
