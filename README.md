<!DOCTYPE html>
<html lang="id">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Belajar Seru - Game Edukasi</title>

<style>
* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
}

body {
    font-family: 'Segoe UI', sans-serif;
    min-height: 100vh;
    padding: 25px 15px;
    background: linear-gradient(135deg, #667eea, #764ba2);
    color: #252542;
}

.container {
    max-width: 850px;
    margin: auto;
}

header {
    text-align: center;
    color: white;
    padding: 30px 10px;
}

header h1 {
    font-size: 2.5rem;
    margin-bottom: 10px;
}

header p {
    opacity: .9;
}

.menu {
    display: grid;
    grid-template-columns: repeat(2, 1fr);
    gap: 18px;
}

.card {
    background: white;
    padding: 30px 15px;
    border-radius: 20px;
    text-align: center;
    cursor: pointer;
    box-shadow: 0 8px 25px #24204a30;
    transition: .25s;
}

.card:hover {
    transform: translateY(-6px);
}

.emoji {
    display: block;
    font-size: 3rem;
    margin-bottom: 12px;
}

.card p {
    color: #777;
    margin-top: 8px;
    font-size: .9rem;
}

.screen {
    display: none;
    background: white;
    padding: 25px;
    border-radius: 20px;
    margin-top: 15px;
}

.screen.active {
    display: block;
}

.score-bar {
    display: flex;
    justify-content: space-between;
    flex-wrap: wrap;
    gap: 10px;
    padding: 15px;
    border-radius: 12px;
    background: #f1f2fa;
    margin-bottom: 20px;
    font-weight: bold;
}

.question {
    padding: 30px 15px;
    background: linear-gradient(135deg, #667eea, #764ba2);
    color: white;
    border-radius: 15px;
    text-align: center;
    font-size: 1.4rem;
    font-weight: bold;
    margin-bottom: 20px;
}

.options {
    display: grid;
    grid-template-columns: repeat(2, 1fr);
    gap: 12px;
}

.option {
    padding: 17px 10px;
    background: #f4f5fc;
    border: 2px solid #e5e6f0;
    border-radius: 12px;
    cursor: pointer;
    font-size: 1rem;
    transition: .2s;
}

.option:hover:not(:disabled) {
    background: #e7eaff;
    border-color: #667eea;
}

.option:disabled {
    cursor: default;
    opacity: 1;
}

.option.correct {
    background: #d1fae5;
    border-color: #16a34a;
    color: #166534;
}

.option.wrong {
    background: #fee2e2;
    border-color: #dc2626;
    color: #991b1b;
}

.feedback {
    text-align: center;
    font-weight: bold;
    margin: 18px 0;
    min-height: 25px;
}

.btn {
    padding: 12px 25px;
    border: none;
    border-radius: 30px;
    background: linear-gradient(135deg, #667eea, #764ba2);
    color: white;
    cursor: pointer;
    font-size: 1rem;
    margin: 6px;
}

.btn:hover {
    opacity: .85;
}

.center {
    text-align: center;
    margin-top: 15px;
}

.result {
    text-align: center;
    padding: 25px 10px;
}

.result h2 {
    margin-bottom: 15px;
}

.stars {
    font-size: 2.5rem;
    margin: 15px;
}

.memory-grid {
    display: grid;
    grid-template-columns: repeat(4, 1fr);
    gap: 12px;
}

.memory-card {
    aspect-ratio: 1;
    border: none;
    border-radius: 13px;
    background: linear-gradient(135deg, #667eea, #764ba2);
    color: white;
    font-size: 2rem;
    cursor: pointer;
}

.memory-card.flipped,
.memory-card.matched {
    background: #e8eaff;
    color: #333;
}

.memory-card.matched {
    background: #d1fae5;
    cursor: default;
}

@media(max-width: 500px) {
    header h1 {
        font-size: 2rem;
    }

    .screen {
        padding: 17px;
    }

    .options {
        grid-template-columns: 1fr;
    }

    .memory-grid {
        gap: 8px;
    }

    .memory-card {
        font-size: 1.6rem;
    }
}
</style>
</head>

<body>
<div class="container">

<header>
    <h1>🎮 Belajar Seru!</h1>
    <p>Main sambil belajar, belajar jadi menyenangkan!</p>
</header>

<div id="menu" class="menu">

    <div class="card" onclick="startQuiz('math')">
        <span class="emoji">➕</span>
        <h2>Matematika</h2>
        <p>Asah kemampuan berhitung</p>
    </div>

    <div class="card" onclick="startQuiz('english')">
        <span class="emoji">🔤</span>
        <h2>Bahasa Inggris</h2>
        <p>Belajar kosakata Inggris</p>
    </div>

    <div class="card" onclick="startQuiz('science')">
        <span class="emoji">🔬</span>
        <h2>Sains</h2>
        <p>Uji pengetahuan umum</p>
    </div>

    <div class="card" onclick="startMemory()">
        <span class="emoji">🧠</span>
        <h2>Memory Card</h2>
        <p>Latihan daya ingat</p>
    </div>

</div>

<div id="quiz" class="screen">

    <div class="score-bar">
        <span>⭐ Skor: <span id="score">0</span></span>
        <span id="lives">❤️❤️❤️</span>
        <span>Soal: <span id="number">1</span>/10</span>
    </div>

    <div class="question" id="question"></div>
    <div class="options" id="options"></div>
    <div class="feedback" id="feedback"></div>

    <div class="center">
        <button class="btn" id="next" onclick="nextQuestion()" hidden>
            Soal Berikutnya →
        </button>
        <button class="btn" onclick="showMenu()">Menu</button>
    </div>

</div>

<div id="memory" class="screen">

    <div class="score-bar">
        <span>Pasangan: <span id="pairs">0</span>/6</span>
        <span>Percobaan: <span id="moves">0</span></span>
    </div>

    <div class="memory-grid" id="memoryGrid"></div>

    <div class="center">
        <button class="btn" onclick="startMemory()">Ulangi</button>
        <button class="btn" onclick="showMenu()">Menu</button>
    </div>

</div>

<div id="result" class="screen">
    <div class="result">
        <h2 id="resultTitle">Permainan Selesai!</h2>
        <div class="stars" id="stars">⭐⭐⭐</div>
        <p id="resultText"></p>
        <button class="btn" onclick="showMenu()">Kembali ke Menu</button>
    </div>
</div>

</div>

<script>
const questionData = {
math: [
["7 + 5 = ?", ["10","12","14","11"], 1],
["8 × 3 = ?", ["21","24","18","27"], 1],
["15 - 6 = ?", ["8","9","10","7"], 1],
["20 ÷ 4 = ?", ["4","5","6","3"], 1],
["9 + 8 = ?", ["16","17","18","15"], 1],
["6 × 6 = ?", ["30","36","32","42"], 1],
["25 - 10 = ?", ["10","15","20","12"], 1],
["3 × 7 = ?", ["18","21","24","20"], 1],
["12 + 13 = ?", ["24","25","26","23"], 1],
["100 ÷ 10 = ?", ["5","10","20","15"], 1]
],

english: [
["Bahasa Inggris 'Kucing'?", ["Dog","Cat","Bird","Fish"], 1],
["'Apel' dalam bahasa Inggris?", ["Banana","Apple","Orange","Grape"], 1],
["Bahasa Inggris 'Buku'?", ["Pen","Book","Pencil","Bag"], 1],
["'Selamat Pagi'?", ["Good Night","Good Morning","Hello","Goodbye"], 1],
["Arti 'School'?", ["Rumah","Sekolah","Toko","Taman"], 1],
["Bahasa Inggris 'Air'?", ["Fire","Water","Earth","Stone"], 1],
["'Matahari'?", ["Moon","Sun","Star","Cloud"], 1],
["Arti 'Friend'?", ["Musuh","Teman","Keluarga","Guru"], 1],
["Bahasa Inggris 'Rumah'?", ["Car","House","Tree","School"], 1],
["'Terima Kasih'?", ["Please","Sorry","Thank You","Hello"], 2]
],

science: [
["Planet terbesar di tata surya?", ["Mars","Jupiter","Saturnus","Bumi"], 1],
["Hewan bernapas dengan insang?", ["Burung","Ikan","Kucing","Kelinci"], 1],
["Organ untuk melihat?", ["Telinga","Mata","Hidung","Mulut"], 1],
["Proses tumbuhan membuat makanan?", ["Respirasi","Fotosintesis","Pencernaan","Penguapan"], 1],
["Jumlah planet di tata surya?", ["7","8","9","10"], 1],
["Pusat tata surya?", ["Bumi","Matahari","Bulan","Mars"], 1],
["Air membeku menjadi?", ["Uap","Es","Api","Gas"], 1],
["Hewan pemakan tumbuhan?", ["Karnivora","Herbivora","Omnivora","Predator"], 1],
["Sumber energi utama bumi?", ["Bulan","Matahari","Bintang","Angin"], 1],
["Gas yang dibutuhkan manusia untuk bernapas?", ["Karbon dioksida","Oksigen","Nitrogen","Hidrogen"], 1]
]
};

let questions = [];
let score = 0;
let lives = 3;
let current = 0;
let answered = false;

function showScreen(id) {
    document.querySelectorAll(".screen").forEach(el => {
        el.classList.remove("active");
    });

    document.getElementById("menu").style.display = "none";

    if (id === "menu") {
        document.getElementById("menu").style.display = "grid";
    } else {
        document.getElementById(id).classList.add("active");
    }
}

function showMenu() {
    showScreen("menu");
}

function startQuiz(type) {
    questions = questionData[type].map(q => [...q]);
    score = 0;
    lives = 3;
    current = 0;
    showScreen("quiz");
    renderQuestion();
}

function renderQuestion() {
    if (current >= questions.length || lives <= 0) {
        finishQuiz();
        return;
    }

    answered = false;

    const q = questions[current];

    document.getElementById("score").textContent = score;
    document.getElementById("lives").textContent = "❤️".repeat(lives);
    document.getElementById("number").textContent = current + 1;
    document.getElementById("question").textContent = q[0];
    document.getElementById("feedback").textContent = "";
    document.getElementById("next").hidden = true;

    const options = document.getElementById("options");
    options.innerHTML = "";

    q[1].forEach((answer, index) => {
        const button = document.createElement("button");
        button.className = "option";
        button.textContent = answer;

        button.onclick = () => checkAnswer(index, button);

        options.appendChild(button);
    });
}

function checkAnswer(index, button) {
    if (answered) return;

    answered = true;

    const q = questions[current];
    const buttons = document.querySelectorAll(".option");

    buttons.forEach((btn, i) => {
        btn.disabled = true;

        if (i === q[2]) {
            btn.classList.add("correct");
        }
    });

    const feedback = document.getElementById("feedback");

    if (index === q[2]) {
        score += 10;
        feedback.textContent = "🎉 Benar! Hebat!";
        feedback.style.color = "#16a34a";
    } else {
        lives--;
        button.classList.add("wrong");
        feedback.textContent = "❌ Belum tepat! Jawaban: " + q[1][q[2]];
        feedback.style.color = "#dc2626";
    }

    document.getElementById("score").textContent = score;
    document.getElementById("lives").textContent = "❤️".repeat(lives);

    document.getElementById("next").hidden = false;
}

function nextQuestion() {
    current++;
    renderQuestion();
}

function finishQuiz() {
    showScreen("result");

    const stars = score >= 80 ? "⭐⭐⭐" :
                  score >= 50 ? "⭐⭐" : "⭐";

    document.getElementById("resultTitle").textContent =
        lives <= 0 ? "Kesempatan Habis!" : "Kuis Selesai!";

    document.getElementById("stars").textContent = stars;

    document.getElementById("resultText").textContent =
        "Skor akhir kamu: " + score +
        " dari 100. Kamu menjawab " +
        (score / 10) + " soal dengan benar.";
}

// MEMORY GAME

const symbols = ["🍎","🚀","🐱","⚽","🌟","🎵"];

let firstCard = null;
let secondCard = null;
let lock = false;
let matched = 0;
let moves = 0;

function startMemory() {
    matched = 0;
    moves = 0;
    firstCard = null;
    secondCard = null;
    lock = false;

    document.getElementById("pairs").textContent = 0;
    document.getElementById("moves").textContent = 0;

    const cards = [...symbols, ...symbols]
        .sort(() => Math.random() - 0.5);

    const grid = document.getElementById("memoryGrid");
    grid.innerHTML = "";

    showScreen("memory");

    cards.forEach(symbol => {
        const card = document.createElement("button");
        card.className = "memory-card";
        card.textContent = "?";
        card.dataset.symbol = symbol;

        card.onclick = () => flipCard(card);

        grid.appendChild(card);
    });
}

function flipCard(card) {
    if (lock ||
        card.classList.contains("flipped") ||
        card.classList.contains("matched")) {
        return;
    }

    card.classList.add("flipped");
    card.textContent = card.dataset.symbol;

    if (!firstCard) {
        firstCard = card;
        return;
    }

    secondCard = card;
    moves++;

    document.getElementById("moves").textContent = moves;

    if (firstCard.dataset.symbol === secondCard.dataset.symbol) {
        firstCard.classList.add("matched");
        secondCard.classList.add("matched");

        matched++;
        document.getElementById("pairs").textContent = matched;

        firstCard = null;
        secondCard = null;

        if (matched === 6) {
            setTimeout(() => {
                showScreen("result");
                document.getElementById("resultTitle").textContent =
                    "🎉 Hebat! Kamu Menang!";
                document.getElementById("stars").textContent = "⭐⭐⭐";
                document.getElementById("resultText").textContent =
                    "Kamu berhasil menemukan semua pasangan dalam " +
                    moves + " percobaan!";
            }, 400);
        }
    } else {
        lock = true;

        setTimeout(() => {
            firstCard.classList.remove("flipped");
            secondCard.classList.remove("flipped");

            firstCard.textContent = "?";
            secondCard.textContent = "?";

            firstCard = null;
            secondCard = null;
            lock = false;
        }, 800);
    }
}

showMenu();
</script>

</body>
</html>
