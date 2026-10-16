
<!DOCTYPE html>
<html lang="pt-BR">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Missão Matemática</title>
<style>
* { box-sizing: border-box; }

body {
  margin: 0;
  font-family: Arial, sans-serif;
  background: #dceeff;
  color: #17345b;
}

header {
  text-align: center;
  padding: 25px 15px;
  background: #2878d0;
  color: white;
}

main {
  max-width: 700px;
  margin: 30px auto;
  padding: 20px;
}

.card {
  background: white;
  border: 1px solid #b8d8f5;
  border-radius: 20px;
  padding: 25px;
  margin-bottom: 20px;
  box-shadow: 0 5px 15px #17345b12;
}

h1 { margin: 0; }
h2 { color: #2878d0; }

button {
  display: block;
  width: 100%;
  padding: 14px;
  margin: 10px 0;
  border: 1px solid #b8d8f5;
  border-radius: 12px;
  background: white;
  color: #17345b;
  font-size: 16px;
  cursor: pointer;
}

button:hover {
  background: #dceeff;
  border-color: #2878d0;
}

.primary {
  background: #2878d0;
  color: white;
  border: none;
  font-weight: bold;
}

.primary:hover {
  background: #175ca8;
  color: white;
}

#feedback { min-height: 45px; line-height: 1.5; }
#progress { color: #2878d0; font-weight: bold; }
#result { text-align: center; }
.hidden { display: none; }
</style>
</head>
<body>

<header>
  <h1>🚀 Missão Matemática</h1>
  <p>JOGO MATEMÁTICO · 10 FASES</p>
</header>

<main>
  <section class="card" id="intro">
    <h2>Prepare-se para a missão!</h2>
    <p>
      Viaje pelo universo da Matemática! Responda aos desafios,
      vença as fases e ganhe estrelas.
    </p>
    <p>⭐ Cada resposta correta vale 10 pontos.</p>
    <p>🎯 Sua missão: conseguir até 100 pontos!</p>
    <button class="primary" onclick="startGame()">
      Começar a jogar 🚀
    </button>
  </section>

  <section class="card hidden" id="game">
    <p id="progress">FASE 1 DE 10</p>
    <p>⭐ Pontos: <strong id="score">0</strong></p>
    <h2 id="question"></h2>
    <div id="answers"></div>
    <p id="feedback"></p>
    <button class="primary" id="next" onclick="nextQuestion()" disabled>
      Próximo desafio →
    </button>
  </section>

  <section class="card hidden" id="result">
    <h2>🏆 Missão concluída!</h2>
    <h1 id="finalScore"></h1>
    <p id="message"></p>
    <button class="primary" onclick="startGame()">
      Jogar novamente ↻
    </button>
  </section>
</main>

<script>
const questions = [
  {q:"Quanto é 8 × 7?", a:["54","56","64"], c:1},
  {q:"Quantos lados tem um triângulo?", a:["3","4","5"], c:0},
  {q:"Quanto é 100 ÷ 4?", a:["20","25","40"], c:1},
  {q:"Quanto é 50% de 80?", a:["20","30","40"], c:2},
  {q:"Quantas faces tem um cubo?", a:["4","6","8"], c:1},
  {q:"Quanto é 15 + 27?", a:["32","42","52"], c:1},
  {q:"Um produto custa R$ 60 e tem 10% de desconto. Quanto custa agora?", a:["R$ 50","R$ 54","R$ 56"], c:1},
  {q:"Quanto é 9 × 6?", a:["45","54","63"], c:1},
  {q:"Qual é o perímetro de um quadrado de lado 5 cm?", a:["10 cm","20 cm","25 cm"], c:1},
  {q:"Uma dúzia corresponde a quantas unidades?", a:["10","12","24"], c:1}
];

let current = 0;
let points = 0;
let answered = false;

function startGame() {
  current = 0;
  points = 0;
  document.getElementById("intro").classList.add("hidden");
  document.getElementById("result").classList.add("hidden");
  document.getElementById("game").classList.remove("hidden");
  showQuestion();
}

function showQuestion() {
  answered = false;
  const q = questions[current];

  document.getElementById("progress").textContent =
    `FASE ${current + 1} DE ${questions.length}`;
  document.getElementById("score").textContent = points;
  document.getElementById("question").textContent = q.q;
  document.getElementById("feedback").textContent = "";
  document.getElementById("next").disabled = true;
  document.getElementById("next").textContent =
    current === questions.length - 1
      ? "Ver resultado 🏆"
      : "Próximo desafio →";

  const area = document.getElementById("answers");
  area.innerHTML = "";

  q.a.forEach((answer, index) => {
    const button = document.createElement("button");
    button.textContent =
      String.fromCharCode(65 + index) + ". " + answer;
    button.onclick = () => checkAnswer(index);
    area.appendChild(button);
  });
}

function checkAnswer(index) {
  if (answered) return;
  answered = true;

  const q = questions[current];
  const buttons = document.querySelectorAll("#answers button");

  buttons.forEach((button, i) => {
    button.disabled = true;
    if (i === q.c) {
      button.style.background = "#d7f5e5";
      button.style.borderColor = "#18794e";
    }
  });

  if (index === q.c) {
    points += 10;
    document.getElementById("feedback").textContent =
      "🎉 Resposta correta! Você ganhou 10 pontos!";
  } else {
    document.getElementById("feedback").textContent =
      "Quase! A resposta correta é " + q.a[q.c] + ".";
  }

  document.getElementById("score").textContent = points;
  document.getElementById("next").disabled = false;
}

function nextQuestion() {
  if (!answered) return;

  current++;

  if (current < questions.length) {
    showQuestion();
  } else {
    document.getElementById("game").classList.add("hidden");
    document.getElementById("result").classList.remove("hidden");
    document.getElementById("finalScore").textContent =
      points + " / 100 pontos";

    document.getElementById("message").textContent =
      points === 100
        ? "Perfeito! Você venceu todas as fases! 🚀"
        : points >= 60
        ? "Muito bem! Você está quase dominando o universo da Matemática!"
        : "Boa tentativa! Jogue novamente para melhorar sua pontuação.";
  }
}
</script>

</body>
</html>

