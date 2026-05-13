[app_presenze_qr.html](https://github.com/user-attachments/files/27703166/app_presenze_qr.html)
<!DOCTYPE html>
<html lang="it">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Generatore QR Studente</title>
  <meta name="theme-color" content="#1f5eff" />
  <meta name="apple-mobile-web-app-capable" content="yes" />
  <meta name="apple-mobile-web-app-title" content="QR Studente" />
  <style>
    * { box-sizing: border-box; font-family: Arial, sans-serif; }
    body {
      margin: 0; background: #f4f6f8; color: #222;
      min-height: 100vh; display:flex; align-items:center; justify-content:center; padding:20px;
    }
    .card {
      width:100%; max-width:520px; background:white; padding:28px; border-radius:20px;
      box-shadow:0 10px 30px rgba(0,0,0,.08);
    }
    h1 { text-align:center; margin-top:0; }
    .subtitle { text-align:center; color:#666; line-height:1.4; }
    label { display:block; margin-top:15px; margin-bottom:6px; font-weight:bold; font-size:14px; }
    input, select {
      width:100%; padding:12px; border:1px solid #ccc; border-radius:10px; font-size:15px;
    }
    button {
      width:100%; padding:14px; border:none; border-radius:14px; background:#1f5eff;
      color:white; font-size:16px; font-weight:bold; cursor:pointer; margin-top:14px;
    }
    button:hover { background:#1748c7; }
    .success { background:#2c9c5c; }
    .success:hover { background:#217744; }
    .qr-area { display:none; margin-top:24px; text-align:center; }
    #qrImage {
      width:230px; height:230px; border:1px solid #ddd; border-radius:12px; padding:8px; background:white;
    }
    .preview {
      background:#f7f7f7; border-radius:10px; padding:12px; margin-top:18px; text-align:left;
      white-space:pre-line; word-break:break-word;
    }
    .error {
      display:none; margin-top:15px; padding:12px; border-radius:10px;
      background:#ffe8e8; color:#8a1f1f;
    }
  </style>
</head>
<body>
  <div class="card">
    <h1>Generatore QR personalizzato</h1>
    <p class="subtitle"><strong>COMPILA E CONSERVA IL QR CODE PERSONALIZZATO CHE VERRÀ GENERATO</strong></p>

    <label for="facolta">Facoltà / Corso di laurea</label>
    <input type="text" id="facolta" placeholder="Es. Giurisprudenza" />

    <label for="anno">Anno della facoltà</label>
    <select id="anno">
      <option value="">Seleziona anno</option>
      <option value="1° anno">1° anno</option>
      <option value="2° anno">2° anno</option>
      <option value="3° anno">3° anno</option>
      <option value="4° anno">4° anno</option>
      <option value="5° anno">5° anno</option>
      <option value="Fuori corso">Fuori corso</option>
    </select>

    <label for="nome">Nome</label>
    <input type="text" id="nome" placeholder="Inserisci il nome" />

    <label for="cognome">Cognome</label>
    <input type="text" id="cognome" placeholder="Inserisci il cognome" />

    <button type="button" id="generateButton">Genera QR Code</button>

    <div class="error" id="errorBox"></div>

    <div class="qr-area" id="qrArea">
      <img id="qrImage" alt="QR code generato" />
      <div class="preview" id="previewTesto"></div>
      <button type="button" class="success" id="downloadButton">Scarica QR Code</button>
    </div>
  </div>

  <script>
    const A_CAPO = String.fromCharCode(10);
    let ultimoUrlQR = "";
    const el = {
      facolta: document.getElementById('facolta'), anno: document.getElementById('anno'), nome: document.getElementById('nome'), cognome: document.getElementById('cognome'),
      generateButton: document.getElementById('generateButton'), downloadButton: document.getElementById('downloadButton'),
      qrImage: document.getElementById('qrImage'), qrArea: document.getElementById('qrArea'), previewTesto: document.getElementById('previewTesto'), errorBox: document.getElementById('errorBox')
    };
    function errore(msg){ el.errorBox.textContent = msg; el.errorBox.style.display='block'; }
    function noErrore(){ el.errorBox.style.display='none'; }
    function generaQR(){
      noErrore();
      const dati = {
        facolta: el.facolta.value.trim(), anno: el.anno.value.trim(), nome: el.nome.value.trim(), cognome: el.cognome.value.trim()
      };
      if (!dati.facolta || !dati.anno || !dati.nome || !dati.cognome) { errore('Compila tutti i campi prima di generare il QR code.'); return; }
      const testo = ['Facoltà: '+dati.facolta,'Anno: '+dati.anno,'Nome: '+dati.nome,'Cognome: '+dati.cognome].join(A_CAPO);
      ultimoUrlQR = 'https://api.qrserver.com/v1/create-qr-code/?'+new URLSearchParams({size:'230x230',data:testo,format:'png',margin:'10'}).toString();
      el.qrImage.src = ultimoUrlQR;
      el.previewTesto.textContent = testo;
      el.qrArea.style.display='block';
    }
    function scaricaQR(){ if (!ultimoUrlQR) { errore('Genera prima il QR code.'); return; } const a=document.createElement('a'); a.href=ultimoUrlQR; a.download='qr-code-personalizzato.png'; a.click(); }
    el.generateButton.addEventListener('click', generaQR);
    el.downloadButton.addEventListener('click', scaricaQR);
    console.assert(['Facoltà: Test','Anno: 1° anno','Nome: A','Cognome: B'].join(A_CAPO).includes('Nome: A'),'Test fallito');
  </script>
</body>
</html>
