<!DOCTYPE html>
<html lang="pt-br">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Sorteio - Continental</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      background-color: #020d3f; /* Cor de fundo escuro */
      color: white;
      text-align: center;
      display: flex;
      justify-content: center;
      align-items: center;
      height: 100vh; /* Tamanho da tela */
      margin: 0; /* Remove margens */
      position: relative; /* Para permitir que os fogos fiquem no topo */
    }
    .container {
      text-align: center;
      width: 100%;
      max-width: 900px; /* Limita a largura */
    }
    h1 {
      margin-bottom: 20px;
      border-bottom: 2px solid #00aaff; /* Cor de destaque azul */
      padding-bottom: 10px;
    }
    .tabs {
      display: flex;
      justify-content: center;
      margin-bottom: 20px;
    }
    .tab {
      padding: 12px 24px;
      background-color: #333;
      margin: 0 5px;
      cursor: pointer;
      border-radius: 20px 20px 0 0; /* Arredondando a parte superior das abas */
      font-weight: bold;
    }
    .tab:hover {
      background-color: #555;
    }
    .tab-content {
      display: none;
    }
    .tab-content.active {
      display: block;
      margin-top: 20px;
    }
    #sorteio-container {
      margin-top: 20px;
      padding: 30px;
      background-color: rgba(2, 13, 63, 0.8); /* Cor de fundo com o tom #020d3f */
      border-radius: 15px; /* Bordas arredondadas */
      max-width: 600px;
      margin-left: auto;
      margin-right: auto;
      border: 2px solid #00aaff; /* Borda azul */
    }
    #input-area {
      margin-bottom: 20px;
      border-radius: 15px; /* Arredondando a área de input */
      background-color: rgba(2, 13, 63, 0.8); /* Fundo com o tom #020d3f */
    }
    #input-area textarea {
      padding: 12px;
      font-size: 14px;
      width: 100%; /* Ajusta a largura do campo de texto para ocupar toda a largura disponível */
      margin-bottom: 10px;
      border-radius: 10px; /* Arredondando a área de input */
      border: 1px solid #ccc;
      box-sizing: border-box; /* Garante que o padding e a borda sejam considerados dentro da largura total */
    }
    #input-area button {
      padding: 12px 20px;
      font-size: 14px;
      background-color: #00aaff; /* Cor azul */
      border: none;
      border-radius: 10px; /* Botões arredondados */
      color: white;
      cursor: pointer;
      margin-top: 10px;
    }
    #input-area button:hover {
      background-color: #0088cc;
    }
    #result {
      margin-top: 20px;
      font-size: 20px;
      font-weight: bold;
      padding: 15px;
      background-color: #333;
      border-radius: 10px;
      border: 2px solid #00aaff;
      white-space: nowrap; /* Impede a quebra de linha */
      overflow: hidden; /* Esconde o texto que ultrapassa a largura */
      text-overflow: ellipsis; /* Adiciona "..." ao texto que não cabe */
      width: 100%; /* A largura vai ocupar 100% do container */
      box-sizing: border-box; /* Garante que padding e borda sejam incluídos na largura */
    }
    #historico {
      margin-top: 30px;
      font-size: 16px;
      text-align: left;
      max-width: 600px;
      margin-left: auto;
      margin-right: auto;
      padding: 20px;
      background-color: rgba(2, 13, 63, 0.8); /* Fundo com o tom #020d3f */
      border-radius: 15px;
      overflow-y: auto;
      max-height: 300px; /* Ajuste o tamanho para manter fixo */
      border: 2px solid #00aaff;
    }
    #historico ul {
      list-style-type: none;
      padding: 0;
      margin: 0;
    }
    #historico li {
      margin-bottom: 10px;
    }
    #historico h3 {
      margin-top: 0;
    }
    #fireworks {
      position: absolute;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
      pointer-events: none; /* Impede que os fogos de artifício atrapalhem a interação com outros elementos */
      display: none; /* Escondido inicialmente */
    }
    .firework {
      position: absolute;
      width: 10px;
      height: 10px;
      background-color: #ffeb3b;
      border-radius: 50%;
      opacity: 0;
      animation: fireworks 1.5s forwards, spread 1.5s ease-out forwards;
    }
    .firework:nth-child(even) {
      background-color: #ff5722; /* Cor laranja */
    }
    .firework:nth-child(odd) {
      background-color: #03a9f4; /* Cor azul */
    }
    @keyframes fireworks {
      0% { opacity: 1; transform: scale(1); }
      100% { opacity: 0; transform: scale(0); }
    }
    @keyframes spread {
      0% { transform: translate(0, 0); }
      100% { transform: translate(var(--x), var(--y)); }
    }
    #sortear-button {
      padding: 12px 24px;
      background-color: #66b3ff; /* Azul claro */
      border: none;
      border-radius: 12px; /* Bordas arredondadas do botão */
      color: white;
      cursor: pointer;
      font-size: 16px;
      margin-top: 20px;
    }
    #sortear-button:hover {
      background-color: #3399cc; /* Cor do botão no hover */
    }
  </style>
</head>
<body>
  <div class="container">
    <h1>Continental Sorteio</h1>
    
    <!-- Abas -->
    <div class="tabs">
      <div class="tab" onclick="showTab(0)">Participantes</div>
      <div class="tab" onclick="showTab(1)">Sorteio</div>
    </div>

    <!-- Aba Participantes -->
    <div class="tab-content" id="tab-participantes">
      <div id="input-area">
        <textarea id="participantes-input" rows="10" cols="50" placeholder="Cole aqui os participantes (Razão Social - CNPJ, separados por nova linha)..."></textarea><br>
        <button onclick="showTab(1)">Ir para o Sorteio</button>
      </div>
    </div>

    <!-- Aba Sorteio -->
    <div class="tab-content" id="tab-sorteio">
      <div id="sorteio-container">
        <button id="sortear-button" onclick="sortear()">Sortear</button>
        <div id="result"></div>
      </div>
      <div id="fireworks"></div> <!-- Área para os fogos de artifício -->
      
      <!-- Histórico de ganhadores -->
      <div id="historico">
        <h3>Histórico de Sorteios</h3>
        <ul id="ganhadores-list"></ul>
      </div>
    </div>
  </div>

  <script>
    let sorteados = [];
    let participantes = [];
    let ganhadores = []; // Array para armazenar o histórico dos ganhadores

    function showTab(index) {
      const tabs = document.querySelectorAll('.tab-content');
      tabs.forEach((tab, i) => {
        if (i === index) {
          tab.classList.add('active');
        } else {
          tab.classList.remove('active');
        }
      });

      if (index === 1) {
        // Carregar os participantes na aba Sorteio
        carregarParticipantes();
      }
    }

    function carregarParticipantes() {
      const inputText = document.getElementById("participantes-input").value.trim();
      if (!inputText) {
        alert("Por favor, cole os participantes primeiro.");
        return;
      }

      participantes = inputText.split("\n").map(line => {
        const [razaoSocial, cnpj] = line.split(" - ").map(item => item.trim());
        return { razaoSocial, cnpj };
      }).filter(p => p.razaoSocial && p.cnpj);
    }

    function sortear() {
      const participantesValidos = participantes.filter(p => !sorteados.includes(p.cnpj));

      if (participantesValidos.length === 0) {
        document.getElementById("result").textContent = "Todos os participantes já foram sorteados!";
        return;
      }

      // Adiciona o atraso de 2,5 segundos antes de exibir o vencedor
      setTimeout(() => {
        const vencedor = participantesValidos[Math.floor(Math.random() * participantesValidos.length)];
        sorteados.push(vencedor.cnpj);

        // Exibe o vencedor
        document.getElementById("result").textContent = `Vencedor: ${vencedor.razaoSocial} - ${vencedor.cnpj}`;

        // Adiciona o vencedor ao histórico
        ganhadores.push(vencedor);
        atualizarHistorico();

        mostrarFogos();
      }, 2500); // Atraso de 2,5 segundos (2500 ms)
    }

    function mostrarFogos() {
      const fireworksContainer = document.getElementById("fireworks");
      fireworksContainer.style.display = "block"; // Torna os fogos visíveis

      // Gerar várias explosões de fogos
      for (let i = 0; i < 50; i++) {
        const firework = document.createElement("div");
        firework.classList.add("firework");

        // Define uma posição aleatória e a distância de dispersão para cada fogos
        const angle = Math.random() * Math.PI * 2;
        const distance = Math.random() * 300 + 100; // Distância aleatória para dispersão
        const x = Math.cos(angle) * distance;
        const y = Math.sin(angle) * distance;

        firework.style.setProperty('--x', `${x}px`);
        firework.style.setProperty('--y', `${y}px`);
        fireworksContainer.appendChild(firework);

        // Remover o fogo após a animação
        setTimeout(() => {
          firework.remove();
        }, 1500); // Tempo suficiente para a animação
      }

      setTimeout(() => {
        fireworksContainer.style.display = "none"; // Esconde os fogos após a animação
      }, 2000);
    }

    function atualizarHistorico() {
      const listaGanhadores = document.getElementById("ganhadores-list");
      listaGanhadores.innerHTML = ''; // Limpa a lista antes de adicionar novos ganhadores

      // Adiciona os ganhadores com numeração
      ganhadores.forEach((vencedor, index) => {
        const li = document.createElement("li");
        li.textContent = `${index + 1}. ${vencedor.razaoSocial} - ${vencedor.cnpj}`;
        listaGanhadores.appendChild(li);
      });

      // Mantém o histórico visível
      const historico = document.getElementById("historico");
      historico.scrollTop = historico.scrollHeight; // Faz a rolagem automática para o final da lista
    }
  </script>
</body>
</html>
