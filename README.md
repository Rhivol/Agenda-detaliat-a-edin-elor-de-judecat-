<!DOCTYPE html>
<html lang="ro">
<head>
  <meta charset="UTF-8" />
  <title>Asistent Juridic Moldinsolv</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      background: #f4f4f4;
      margin: 0 auto;
      max-width: 600px;
      padding: 30px;
    }
    h2 {
      text-align: center;
      color: #2a4d69;
    }
    #chatbox {
      border: 1px solid #ccc;
      border-radius: 8px;
      background: white;
      padding: 10px;
      height: 350px;
      overflow-y: scroll;
      margin-bottom: 10px;
    }
    .message {
      margin: 10px 0;
    }
    .user {
      text-align: right;
      color: #4b3832;
    }
    .bot {
      text-align: left;
      color: #2a4d69;
    }
    input, button {
      padding: 10px;
      font-size: 16px;
    }
    #userInput {
      width: 75%;
      border: 1px solid #ccc;
      border-radius: 5px;
    }
    button {
      border: none;
      background: #2a4d69;
      color: white;
      border-radius: 5px;
      cursor: pointer;
    }
    button:hover {
      background: #1f3552;
    }
  </style>
</head>
<body>
  <h2>Asistent Juridic Moldinsolv</h2>
  <div id="chatbox"></div>
  <input type="text" id="userInput" placeholder="Scrie o întrebare juridică...">
  <button onclick="sendMessage()">Trimite</button>

  <script>
    const chatbox = document.getElementById('chatbox');

    function addMessage(text, sender = 'bot') {
      const msg = document.createElement('div');
      msg.className = 'message ' + sender;
      msg.innerHTML = text;
      chatbox.appendChild(msg);
      chatbox.scrollTop = chatbox.scrollHeight;
    }

    async function sendMessage() {
      const input = document.getElementById('userInput');
      const message = input.value.trim();
      if (!message) return;
      addMessage(message, 'user');
      input.value = '';

      try {
        const resp = await fetch(`https://your-api-endpoint.com/cauta?q=${encodeURIComponent(message)}`);
        const data = await resp.json();

        if (data.rezultate) {
          let rasp = "<strong>Rezultate de pe moldinsolv.md:</strong><ul>";
          for (const r of data.rezultate) {
            rasp += `<li><a href="${r.url}" target="_blank">${r.titlu}</a></li>`;
          }
          rasp += "</ul>";
          addMessage(rasp, 'bot');
        } else if (data.mesaj) {
          addMessage(data.mesaj, 'bot');
        } else {
          addMessage("Nu am găsit informații relevante.", 'bot');
        }
      } catch (e) {
        addMessage("Eroare la conectarea cu serverul.", 'bot');
        console.error(e);
      }
    }
  </script>
</body>
</html>
