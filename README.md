# Projeto-
Trabalho de escola
<!DOCTYPE html>
<html lang="pt-BR">
<head>
  <meta charset="UTF-8">
  <title>Laboratório Interativo da Maria</title>
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <!-- Estilo geral -->
  <style>
    body {
      font-family: Arial, sans-serif;
      background-color: #f0f4f8;
      color: #222;
      padding: 20px;
    }
    h1, h2 {
      text-align: center;
      color: #2c3e50;
    }
    .section {
      margin: 40px auto;
      max-width: 800px;
      padding: 20px;
      background-color: #ffffff;
      border-radius: 10px;
      box-shadow: 0 0 10px rgba(0,0,0,0.1);
    }
    #congrats {
      display: none;
      text-align: center;
      font-size: 24px;
      color: green;
      animation: pulse 1s infinite;
    }
    @keyframes pulse {
      0% { transform: scale(1); }
      50% { transform: scale(1.1); }
      100% { transform: scale(1); }
    }
  </style>
</head>
<body>

<h1>🧬 Laboratório Interativo da Maria</h1>

<!-- 🧲 Quebra-Cabeça Faraday -->
<div class="section">
  <h2>🧲 Quebra-Cabeça: Faraday em Ação</h2>
  <p>Monte a imagem que representa o experimento de indução eletromagnética. Complete antes do tempo acabar!</p>

  <style>
    #puzzle {
      display: grid;
      grid-template-columns: repeat(3, 100px);
      grid-template-rows: repeat(3, 100px);
      gap: 5px;
      justify-content: center;
      margin: 20px auto;
    }
    .piece {
      width: 100px;
      height: 100px;
      background-size: 300px 300px;
      border: 1px solid #ccc;
      cursor: move;
    }
    #timer {
      font-size: 20px;
      text-align: center;
      margin-top: 10px;
    }
  </style>

  <div id="puzzle"></div>
  <div id="timer">⏱️ Tempo restante: <span id="time">60</span> segundos</div>
  <div id="congrats">🎉 Parabéns! Você completou o experimento! ⚡</div>
  <audio id="sound" src="https://www.soundjay.com/button/beep-07.wav"></audio>

  <script>
    const puzzle = document.getElementById('puzzle');
    const positions = [
      '0px 0px', '100px 0px', '200px 0px',
      '0px 100px', '100px 100px', '200px 100px',
      '0px 200px', '100px 200px', '200px 200px'
    ];
    const shuffled = [...positions].sort(() => Math.random() - 0.5);
    const correctOrder = [...positions];
    let dragged;

    shuffled.forEach(pos => {
      const piece = document.createElement('div');
      piece.className = 'piece';
      piece.style.backgroundImage = "url('https://bing.com/th/id/BCO.377da4ff-cfbe-4b7a-a21f-073e91977d9a.png')";
      piece.style.backgroundPosition = pos;
      piece.draggable = true;
      puzzle.appendChild(piece);
    });

    puzzle.addEventListener('dragstart', e => {
      dragged = e.target;
    });

    puzzle.addEventListener('dragover', e => {
      e.preventDefault();
    });

    puzzle.addEventListener('drop', e => {
      if (e.target.className === 'piece') {
        const temp = dragged.style.backgroundPosition;
        dragged.style.backgroundPosition = e.target.style.backgroundPosition;
        e.target.style.backgroundPosition = temp;
        checkCompletion();
      }
    });

    function checkCompletion() {
      const pieces = document.querySelectorAll('.piece');
      const current = Array.from(pieces).map(p => p.style.backgroundPosition);
      if (JSON.stringify(current) === JSON.stringify(correctOrder)) {
        document.getElementById('congrats').style.display = 'block';
        document.getElementById('sound').play();
        clearInterval(timerInterval);
      }
    }

    let timeLeft = 60;
    const timerDisplay = document.getElementById('time');
    const timerInterval = setInterval(() => {
      timeLeft--;
      timerDisplay.textContent = timeLeft;
      if (timeLeft <= 0) {
        clearInterval(timerInterval);
        alert("⏳ O tempo acabou! Tente novamente.");
        location.reload();
      }
    }, 1000);
  </script>
</div>

<!-- 🔠 Jogo da Forca Inclusivo -->
<div class="section">
  <h2>🔠 Jogo da Forca Inclusivo – Tema: Ciências</h2>
  <p id="description" aria-label="Instruções do jogo">Adivinhe a palavra relacionada à ciência. Clique nas letras abaixo. Você tem 6 tentativas!</p>

  <style>
    #word { font-size: 32px; letter-spacing: 10px; text-align: center; margin: 20px; }
    #letters { display: flex; flex-wrap: wrap; justify-content: center; gap: 5px; }
    .letter-btn {
      font-size: 20px; padding: 10px; width: 40px;
      background-color: #eee; border: 1px solid #ccc; cursor: pointer;
    }
    .letter-btn:disabled { background-color: #ccc; cursor: not-allowed; }
    #status { text-align: center; font-size: 24px; margin-top: 20px; }
    #hangman { text-align: center; font-size: 100px; }
  </style>

  <div id="word" role="text" aria-label="Palavra oculta"></div>
  <div id="letters" role="group" aria-label="Botões de letras"></div>
  <div id="hangman" aria-label="Tentativas restantes">🙂</div>
  <div id="status" role="status" aria-live="polite"></div>

  <script>
    const palavras = ["energia", "átomo", "molécula", "gravidade", "planeta"];
    const palavra = palavras[Math.floor(Math.random() * palavras.length)].toUpperCase();
    const wordDisplay = document.getElementById("word");
    const lettersDiv = document.getElementById("letters");
    const statusDiv = document.getElementById("status");
    const hangmanDiv = document.getElementById("hangman");

    let tentativas = 6;
    let acertos = [];

    function atualizarPalavra() {
      wordDisplay.textContent = palavra.split("").map(l => acertos.includes(l) ? l : "_").join(" ");
    }

    function verificarFim() {
      if (!wordDisplay.textContent.includes("_")) {
        statusDiv.textContent = "🎉 Parabéns! Você acertou!";
        hangmanDiv.textContent = "😄";
        desativarLetras();
      } else if (tentativas === 0) {
        statusDiv.textContent = `😢 Você perdeu! A palavra era: ${palavra}`;
        hangmanDiv.textContent = "💀";
        desativarLetras();
      }
    }

    function desativarLetras() {
      document.querySelectorAll(".letter-btn").forEach(btn => btn.disabled = true);
    }

    function criarBotoes() {
      "ABCDEFGHIJKLMNOPQRSTUVWXYZ".split("").forEach(letra => {
        const btn = document.createElement("button");
        btn.textContent = letra;
        btn.className = "letter-btn";
        btn.setAttribute("aria-label", `Letra ${letra}`);
        btn.onclick = () => {
          btn.disabled = true;
          if (palavra.includes(letra)) {
            acertos.push(letra);
          } else {
            tentativas--;
            hangmanDiv.textContent = ["🙂","😐","😕","😟","😢","😭","💀"][6 - tentativas];
          }
          atualizarPalavra();
          verificarFim();
        };
        lettersDiv.appendChild(btn);
      });
    }

    atualizarPalavra();
    criarBotoes();
  </script>
</div>

<!-- 🤟 VLibras Avatar Tradutor -->
<div class="section">
  <h2>🤟 Tradução em Libras</h2>
  <p>Este site
