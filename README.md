[file (2).html](https://github.com/user-attachments/files/34848549/file.2.html)
# Quiz-sur-l-islam
Des petits quiz sur l'islam et des moyens de révision pour les 99 noms d'Allah
<!DOCTYPE html>
<html lang="fr">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <title>Apprendre ensemble</title>

  <script src="https://cdnjs.cloudflare.com/ajax/libs/pdf.js/3.11.174/pdf.min.js"></script>

  <style>
    :root {
      --ink: #213a34;
      --green: #285c4b;
      --mint: #e5f1e9;
      --cream: #fbf8f0;
      --white: #fff;
      --muted: #68776f;
      --border: #dce5dc;
      --gold: #d7ae61;
      --red: #b42318;
      --red-bg: #fde8e7;
      --green-bg: #dff3e4;
    }

    * { box-sizing: border-box; }

    body {
      margin: 0;
      background: var(--cream);
      color: var(--ink);
      font: 16px/1.55 system-ui, sans-serif;
    }

    header {
      padding: 24px 18px;
      background: var(--green);
      color: white;
      text-align: center;
    }

    header h1 { margin: 0 0 4px; }

    main {
      width: min(900px, 100%);
      margin: 0 auto;
      padding: 24px 16px 60px;
    }

    section {
      margin: 16px 0;
      padding: 20px;
      border: 1px solid var(--border);
      border-radius: 18px;
      background: var(--white);
    }

    h2, h3 { color: var(--green); }
    h2 { margin-top: 0; }

    button {
      margin: 5px;
      padding: 11px 16px;
      border: 0;
      border-radius: 12px;
      background: var(--green);
      color: white;
      font: inherit;
      cursor: pointer;
    }

    button.secondary {
      border: 1px solid #cbd9ce;
      background: var(--mint);
      color: var(--ink);
    }

    button:disabled { cursor: not-allowed; opacity: .65; }
    button:hover:not(:disabled) { filter: brightness(.95); }

    input[type="file"] {
      display: block;
      max-width: 100%;
      margin: 12px 0;
    }

    .hidden { display: none !important; }
    .muted { color: var(--muted); }
    .row { display: flex; flex-wrap: wrap; justify-content: center; gap: 8px; }

    .category-grid {
      display: grid;
      grid-template-columns: repeat(auto-fit, minmax(230px, 1fr));
      gap: 12px;
      margin-top: 18px;
    }

    .category-button {
      display: flex;
      width: 100%;
      min-height: 78px;
      flex-direction: column;
      align-items: flex-start;
      justify-content: center;
      padding: 14px;
      text-align: left;
    }

    .category-button small { opacity: .85; }

    .answer-list {
      display: grid;
      gap: 10px;
      margin: 18px 0;
    }

    .answer {
      width: 100%;
      margin: 0;
      border: 2px solid var(--border);
      background: white;
      color: var(--ink);
      text-align: left;
    }

    .answer:hover:not(:disabled) { background: var(--mint); }
    .answer.selected {
      border-color: var(--gold);
      background: #fff5dc;
    }
    .answer.correct {
      border-color: #31945a;
      background: var(--green-bg);
      color: #174c2d;
    }
    .answer.incorrect {
      border-color: var(--red);
      background: var(--red-bg);
      color: #7d1711;
    }

    .flashcard {
      display: flex;
      min-height: 250px;
      flex-direction: column;
      justify-content: center;
      align-items: center;
      margin: 18px auto;
      padding: 28px;
      border: 2px solid #e6d7b7;
      border-radius: 24px;
      background: linear-gradient(145deg, #fff, #f4f7ef);
      text-align: center;
      cursor: pointer;
    }

    .arabic { margin: 12px; font-size: 3rem; direction: rtl; }
    .meaning { font-size: 1.3rem; font-weight: 650; }
    .progress { text-align: center; color: var(--muted); }

    .options {
      display: grid;
      grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
      gap: 8px;
      margin: 16px 0;
    }

    .options label {
      display: flex;
      gap: 8px;
      align-items: center;
      padding: 10px;
      border-radius: 10px;
      background: var(--cream);
    }

    .setting { display: block; margin: 8px 0; }

    a { color: var(--green); }
  </style>
</head>

<body>
<header>
  <h1>Apprendre ensemble</h1>
  <div>Un espace simple pour apprendre et réviser</div>
</header>

<main>
  <section id="menu">
    <h2>Bienvenue</h2>
    <p>
      Choisis un quiz ou révise les noms d’Allah. Les traductions et
      translittérations peuvent varier selon les sources.
    </p>
    <div class="row">
      <button onclick="showSection('quiz-home')">Quiz</button>
      <button class="secondary" onclick="showSection('names-setup')">
        Noms d’Allah
      </button>
    </div>
  </section>

  <section id="quiz-home" class="hidden">
    <h2>Quiz</h2>
    <p>
      Sélectionne le fichier <strong>Islam.pdf</strong> pour charger les
      questions et afficher les catégories.
    </p>
    <input id="pdf-file" type="file" accept="application/pdf,.pdf">
    <p id="pdf-status" class="muted" aria-live="polite">
      Aucun PDF chargé.
    </p>
    <div id="category-grid" class="category-grid"></div>
    <button class="secondary" onclick="showSection('menu')">Retour au menu</button>
  </section>

  <section id="quiz-play" class="hidden">
    <h2 id="quiz-category-title">Quiz</h2>
    <div id="quiz-progress" class="progress"></div>
    <p id="question-text"></p>
    <div id="answer-list" class="answer-list"></div>
    <p id="answer-feedback" aria-live="polite"></p>
    <div class="row">
      <button id="validate-button" onclick="validateAnswer()">Valider la réponse</button>
      <button id="next-button" class="hidden" onclick="nextQuestion()">Question suivante</button>
      <button class="secondary" onclick="showSection('quiz-home')">Catégories</button>
    </div>
  </section>

  <section id="quiz-finish" class="hidden">
    <h2>Quiz terminé</h2>
    <p id="quiz-summary"></p>
    <div class="row">
      <button onclick="showSection('quiz-home')">Retour aux catégories</button>
      <button class="secondary" onclick="showSection('menu')">Menu principal</button>
    </div>
  </section>

  <section id="names-setup" class="hidden">
    <h2>Choisir les cartes</h2>
    <p class="muted">Coche les noms que tu souhaites travailler.</p>
    <div class="row">
      <button class="secondary" onclick="selectAllNames(true)">Tout sélectionner</button>
      <button class="secondary" onclick="selectAllNames(false)">Tout désélectionner</button>
    </div>

    <div id="names-list" class="options"></div>

    <h3>Face affichée en premier</h3>
    <label class="setting">
      <input type="radio" name="side" value="front" checked>
      Nom et arabe
    </label>
    <label class="setting">
      <input type="radio" name="side" value="back">
      Signification
    </label>

    <h3>Ordre des cartes</h3>
    <label class="setting">
      <input type="checkbox" id="shuffle-names" checked>
      Mélanger les cartes
    </label>

    <div class="row">
      <button onclick="startNamesStudy(false)">Commencer</button>
      <button class="secondary" onclick="showSection('menu')">Retour au menu</button>
    </div>
  </section>

  <section id="names-study" class="hidden">
    <h2>Noms d’Allah</h2>
    <div id="names-progress" class="progress"></div>
    <div id="flashcard" class="flashcard" role="button" tabindex="0"
         onclick="flipNameCard()"
         onkeydown="if(event.key==='Enter'||event.key===' '){event.preventDefault();flipNameCard()}">
    </div>
    <p class="muted" style="text-align:center">Appuie sur la carte pour la retourner.</p>
    <div class="row">
      <button class="secondary" onclick="markNameCard(false)">À revoir</button>
      <button onclick="markNameCard(true)">Je connais</button>
    </div>
  </section>

  <section id="names-finish" class="hidden">
    <h2>Session terminée</h2>
    <p id="names-summary"></p>
    <div class="row">
      <button id="review-names-button" onclick="startNamesStudy(true)">
        Revoir les cartes à revoir
      </button>
      <button class="secondary" onclick="showSection('names-setup')">
        Choisir d’autres cartes
      </button>
      <button class="secondary" onclick="showSection('menu')">Menu principal</button>
    </div>
  </section>

  <section class="muted">
    <small>
      Les noms et les traductions sont proposés à titre d’étude et peuvent
      varier selon les sources. Les questions du quiz sont chargées depuis le
      PDF fourni.
    </small>
  </section>
</main>

<script>
  if (window.pdfjsLib) {
    pdfjsLib.GlobalWorkerOptions.workerSrc =
      "https://cdnjs.cloudflare.com/ajax/libs/pdf.js/3.11.174/pdf.worker.min.js";
  }

  const categories = [
    { title: "Comprendre ce qu’est l’islam", start: 1, end: 8 },
    { title: "La croyance islamique", start: 9, end: 16 },
    { title: "Le Coran", start: 17, end: 24 },
    { title: "La vie du prophète Muhammad ﷺ", start: 25, end: 34 },
    { title: "Les prophètes dans l’islam", start: 35, end: 44 },
    { title: "La pratique religieuse", start: 45, end: 54 },
    { title: "La spiritualité et le comportement", start: 55, end: 62 },
    { title: "Le halal et le haram", start: 63, end: 70 },
    { title: "La famille et les relations sociales", start: 71, end: 78 },
    { title: "L’au-delà", start: 79, end: 86 },
    { title: "Histoire de l’islam", start: 87, end: 96 },
    { title: "Les sciences islamiques", start: 97, end: 104 },
    { title: "Les différentes branches de l’islam", start: 105, end: 112 },
    { title: "Islam et monde contemporain", start: 113, end: 120 },
    { title: "Questions et idées reçues", start: 121, end: 130 },
    { title: "Les trésors méconnus de l’islam", start: 131, end: 170 }
  ];

  let allQuestions = [];
  let currentQuestions = [];
  let questionIndex = 0;
  let selectedAnswer = null;
  let score = 0;
  let checkedAnswer = false;

  function showSection(id) {
    document.querySelectorAll("main > section").forEach(section => {
      section.classList.add("hidden");
    });
    document.getElementById(id).classList.remove("hidden");
    if (id === "names-setup") renderNames();
  }

  document.getElementById("pdf-file").addEventListener("change", loadPdf);

  async function loadPdf(event) {
    const file = event.target.files[0];
    const status = document.getElementById("pdf-status");
    const grid = document.getElementById("category-grid");

    if (!file) return;

    if (!window.pdfjsLib) {
      status.textContent = "La bibliothèque de lecture PDF n’a pas pu être chargée.";
      return;
    }

    status.textContent = "Lecture du PDF en cours…";
    grid.innerHTML = "";

    try {
      const data = new Uint8Array(await file.arrayBuffer());
      const pdf = await pdfjsLib.getDocument({ data }).promise;
      let text = "";

      for (let pageNumber = 1; pageNumber <= pdf.numPages; pageNumber++) {
        const page = await pdf.getPage(pageNumber);
        const content = await page.getTextContent();
        text += content.items.map(item => item.str).join(" ") + " ";
      }

      allQuestions = parseQuestions(text);

      if (!allQuestions.length) {
        status.textContent =
          "Aucune question n’a été reconnue. Vérifie que tu as sélectionné le PDF original.";
        return;
      }

      status.textContent =
        `${allQuestions.length} questions chargées depuis « ${file.name} ».`;
      renderCategories();
    } catch (error) {
      console.error(error);
      status.textContent =
        "Impossible de lire ce PDF. Essaie de le sélectionner à nouveau.";
    }
  }

  function parseQuestions(text) {
    const normalized = text
      .replace(/\s+/g, " ")
      .replace(/(\d{1,3})\s*\.\s*/g, " $1. ")
      .replace(/Bonne\s+réponse/g, "Bonne réponse");

    const questionPattern =
      /(?:^|\s)(\d{1,3})\.\s*(.*?)\s*A\.\s*(.*?)\s*B\.\s*(.*?)\s*C\.\s*(.*?)\s*D\.\s*(.*?)(?=(?:\s+\d{1,3}\.\s)|$)/g;

    const parsed = [];
    let match;

    while ((match = questionPattern.exec(normalized)) !== null) {
      const number = Number(match[1]);
      if (number < 1 || number > 170) continue;

      const question = cleanText(match[2]);
      const rawOptions = [
        cleanText(match[3]),
        cleanText(match[4]),
        cleanText(match[5]),
        cleanText(match[6])
      ];

      const correctIndex = rawOptions.findIndex(option =>
        /bonne réponse/i.test(option)
      );

      if (!question || correctIndex === -1) continue;

      const options = rawOptions.map(option =>
        cleanText(option.replace(/\s*[—–-]?\s*Bonne réponse\s*/i, ""))
      );

      if (options.some(option => !option)) continue;

      parsed.push({
        number,
        question,
        options,
        correct: options[correctIndex]
      });
    }

    const unique = new Map();
    parsed.forEach(item => unique.set(item.number, item));
    return [...unique.values()].sort((a, b) => a.number - b.number);
  }

  function cleanText(text) {
    return text
      .replace(/\s+/g, " ")
      .replace(/^[\s:–—-]+|[\s:–—-]+$/g, "")
      .trim();
  }

  function renderCategories() {
    const grid = document.getElementById("category-grid");
    grid.innerHTML = "";

    categories.forEach((category, index) => {
      const count = allQuestions.filter(q =>
        q.number >= category.start && q.number <= category.end
      ).length;

      const button = document.createElement("button");
      button.className = "category-button";
      button.innerHTML =
        `<span>${index + 1}. ${escapeHtml(category.title)}</span>` +
        `<small>${count} question${count === 1 ? "" : "s"}</small>`;

      button.disabled = count === 0;
      button.onclick = () => startQuiz(category);
      grid.appendChild(button);
    });
  }

  function startQuiz(category) {
    currentQuestions = allQuestions.filter(q =>
      q.number >= category.start && q.number <= category.end
    );

    if (!currentQuestions.length) return;

    questionIndex = 0;
    score = 0;
    showSection("quiz-play");
    document.getElementById("quiz-category-title").textContent = category.title;
    renderQuestion();
  }

  function renderQuestion() {
    selectedAnswer = null;
    checkedAnswer = false;

    const question = currentQuestions[questionIndex];
    document.getElementById("quiz-progress").textContent =
      `Question ${questionIndex + 1} sur ${currentQuestions.length}`;
    document.getElementById("question-text").textContent = question.question;
    document.getElementById("answer-feedback").textContent = "";

    const choices = question.options.map(text => ({
      text,
      correct: text === question.correct
    }));

    shuffleArray(choices);

    const list = document.getElementById("answer-list");
    list.innerHTML = "";

    choices.forEach((choice, index) => {
      const button = document.createElement("button");
      button.className = "answer";
      button.textContent = `${String.fromCharCode(65 + index)}. ${choice.text}`;
      button.dataset.correct = String(choice.correct);
      button.onclick = () => selectAnswer(button);
      list.appendChild(button);
    });

    document.getElementById("validate-button").classList.remove("hidden");
    document.getElementById("validate-button").disabled = false;
    document.getElementById("next-button").classList.add("hidden");
  }

  function selectAnswer(button) {
    if (checkedAnswer) return;

    document.querySelectorAll(".answer").forEach(answer => {
      answer.classList.remove("selected");
    });

    button.classList.add("selected");
    selectedAnswer = button;
  }

  function validateAnswer() {
    if (checkedAnswer) return;

    if (!selectedAnswer) {
      document.getElementById("answer-feedback").textContent =
        "Sélectionne une réponse avant de valider.";
      return;
    }

    checkedAnswer = true;
    const isCorrect = selectedAnswer.dataset.correct === "true";

    document.querySelectorAll(".answer").forEach(button => {
      button.disabled = true;
      if (button.dataset.correct === "true") {
        button.classList.add("correct");
      }
    });

    if (isCorrect) {
      score++;
      document.getElementById("answer-feedback").textContent = "Bonne réponse !";
    } else {
      selectedAnswer.classList.add("incorrect");
      document.getElementById("answer-feedback").textContent =
        "Ce n’est pas la bonne réponse. La bonne réponse est en vert.";
    }

    document.getElementById("validate-button").classList.add("hidden");
    document.getElementById("next-button").classList.remove("hidden");
  }

  function nextQuestion() {
    questionIndex++;

    if (questionIndex >= currentQuestions.length) {
      document.getElementById("quiz-summary").textContent =
        `Tu as obtenu ${score} bonne(s) réponse(s) sur ${currentQuestions.length}.`;
      showSection("quiz-finish");
      return;
    }

    renderQuestion();
  }

  function shuffleArray(array) {
    for (let i = array.length - 1; i > 0; i--) {
      const j = Math.floor(Math.random() * (i + 1));
      [array[i], array[j]] = [array[j], array[i]];
    }
  }

  function escapeHtml(value) {
    return value.replace(/[&<>"']/g, character => ({
      "&": "&amp;",
      "<": "&lt;",
      ">": "&gt;",
      '"': "&quot;",
      "'": "&#039;"
    })[character]);
  }

  const rawNames = [
    ["الرَّحْمَٰنُ","Ar-Rahman","Le Tout Miséricordieux"],
    ["الرَّحِيمُ","Ar-Rahim","Le Très Miséricordieux"],
    ["الْمَلِكُ","Al-Malik","Le Roi, le Souverain"],
    ["الْقُدُّوسُ","Al-Quddus","Le Saint, le Pur"],
    ["السَّلَامُ","As-Salam","La Source de paix"],
    ["الْمُؤْمِنُ","Al-Mu’min","Celui qui accorde la sécurité"],
    ["الْمُهَيْمِنُ","Al-Muhaymin","Le Gardien, le Protecteur"],
    ["الْعَزِيزُ","Al-Aziz","Le Tout-Puissant, l’Invincible"],
    ["الْجَبَّارُ","Al-Jabbar","Le Contraignant, le Réparateur"],
    ["الْمُتَكَبِّرُ","Al-Mutakabbir","Le Suprême, le Majestueux"],
    ["الْخَالِقُ","Al-Khaliq","Le Créateur"],
    ["الْبَارِئُ","Al-Bari","L’Originateur"],
    ["الْمُصَوِّرُ","Al-Musawwir","Le Formateur"],
    ["الْغَفَّارُ","Al-Ghaffar","Le Grand Pardonneur"],
    ["الْقَهَّارُ","Al-Qahhar","Le Dominateur"],
    ["الْوَهَّابُ","Al-Wahhab","Le Donateur généreux"],
    ["الرَّزَّاقُ","Ar-Razzaq","Le Pourvoyeur"],
    ["الْفَتَّاحُ","Al-Fattah","Celui qui ouvre"],
    ["الْعَلِيمُ","Al-Alim","L’Omniscient"],
    ["الْقَابِضُ","Al-Qabid","Celui qui retient"],
    ["الْبَاسِطُ","Al-Basit","Celui qui étend"],
    ["الْخَافِضُ","Al-Khafid","Celui qui abaisse"],
    ["الرَّافِعُ","Ar-Rafi","Celui qui élève"],
    ["الْمُعِزُّ","Al-Muizz","Celui qui honore"],
    ["الْمُذِلُّ","Al-Mudhill","Celui qui humilie"],
    ["السَّمِيعُ","As-Sami","Celui qui entend tout"],
    ["الْبَصِيرُ","Al-Basir","Celui qui voit tout"],
    ["الْحَكَمُ","Al-Hakam","Le Juge"],
    ["الْعَدْلُ","Al-Adl","Le Juste"],
    ["اللَّطِيفُ","Al-Latif","Le Subtil, le Bienveillant"],
    ["الْخَبِيرُ","Al-Khabir","Le Parfaitement Informé"],
    ["الْحَلِيمُ","Al-Halim","Le Clément, le Longanime"],
    ["الْعَظِيمُ","Al-Azim","L’Immense, le Magnifique"],
    ["الْغَفُورُ","Al-Ghafur","Le Très Pardonneur"],
    ["الشَّكُورُ","Ash-Shakur","Celui qui récompense avec gratitude"],
    ["الْعَلِيُّ","Al-Ali","Le Très-Haut"],
    ["الْكَبِيرُ","Al-Kabir","Le Très-Grand"],
    ["الْحَفِيظُ","Al-Hafiz","Le Préservateur"],
    ["الْمُقِيتُ","Al-Muqit","Celui qui pourvoit et soutient"],
    ["الْحَسِيبُ","Al-Hasib","Celui qui suffit, Celui qui tient compte"],
    ["الْجَلِيلُ","Al-Jalil","Le Majestueux"],
    ["الْكَرِيمُ","Al-Karim","Le Généreux"],
    ["الرَّقِيبُ","Ar-Raqib","Le Vigilant"],
    ["الْمُجِيبُ","Al-Mujib","Celui qui répond"],
    ["الْوَاسِعُ","Al-Wasi","L’Immense, Celui dont la générosité est vaste"],
    ["الْحَكِيمُ","Al-Hakim","Le Sage"],
    ["الْوَدُودُ","Al-Wadud","Le Tout-Aimant"],
    ["الْمَجِيدُ","Al-Majid","Le Glorieux"],
    ["الْبَاعِثُ","Al-Baith","Celui qui ressuscite"],
    ["الشَّهِيدُ","Ash-Shahid","Le Témoin"],
    ["الْحَقُّ","Al-Haqq","La Vérité"],
    ["الْوَكِيلُ","Al-Wakil","Le Garant, Celui à qui l’on se remet"],
    ["الْقَوِيُّ","Al-Qawiyy","Le Très-Fort"],
    ["الْمَتِينُ","Al-Matin","L’Inébranlable"],
    ["الْوَلِيُّ","Al-Waliyy","Le Protecteur, l’Allié"],
    ["الْحَمِيدُ","Al-Hamid","Le Digne de louange"],
    ["الْمُحْصِي","Al-Muhsi","Celui qui dénombre toute chose"],
    ["الْمُبْدِئُ","Al-Mubdi","Celui qui commence la création"],
    ["الْمُعِيدُ","Al-Muid","Celui qui la fait revenir"],
    ["الْمُحْيِي","Al-Muhyi","Celui qui donne la vie"],
    ["الْمُمِيتُ","Al-Mumit","Celui qui donne la mort"],
    ["الْحَيُّ","Al-Hayy","Le Vivant"],
    ["الْقَيُّومُ","Al-Qayyum","Celui qui subsiste par Lui-même et soutient toute chose"],
    ["الْوَاجِدُ","Al-Wajid","Celui qui trouve, Celui à qui rien ne manque"],
    ["الْمَاجِدُ","Al-Majid","Le Noble, le Glorieux"],
    ["الْوَاحِدُ","Al-Wahid","L’Unique"],
    ["الْأَحَدُ","Al-Ahad","L’Un, l’Incomparable"],
    ["الصَّمَدُ","As-Samad","L’Absolu, Celui dont toute chose dépend"],
    ["الْقَادِرُ","Al-Qadir","Le Tout-Puissant"],
    ["الْمُقْتَدِرُ","Al-Muqtadir","Le Tout-Puissant, Celui qui décrète"],
    ["الْمُقَدِّمُ","Al-Muqaddim","Celui qui fait avancer"],
    ["الْمُؤَخِّرُ","Al-Mu’akhkhir","Celui qui retarde"],
    ["الْأَوَّلُ","Al-Awwal","Le Premier"],
    ["الْآخِرُ","Al-Akhir","Le Dernier"],
    ["الظَّاهِرُ","Az-Zahir","L’Apparent, le Manifeste"],
    ["الْبَاطِنُ","Al-Batin","Le Caché"],
    ["الْوَالِي","Al-Wali","Le Gouverneur"],
    ["الْمُتَعَالِي","Al-Mutaali","Le Très-Élevé"],
    ["الْبَرُّ","Al-Barr","La Source de bonté"],
    ["التَّوَّابُ","At-Tawwab","Celui qui accueille le repentir"],
    ["الْمُنْتَقِمُ","Al-Muntaqim","Celui qui rend justice"],
    ["الْعَفُوُّ","Al-Afuww","Celui qui efface les fautes"],
    ["الرَّؤُوفُ","Ar-Ra’uf","Le Très Compatissant"],
    ["مَالِكُ الْمُلْكِ","Malik-ul-Mulk","Le Maître de la souveraineté"],
    ["ذُو الْجَلَالِ وَالْإِكْرَامِ","Dhul-Jalali wal-Ikram","Le Seigneur de la Majesté et de la Générosité"],
    ["الْمُقْسِطُ","Al-Muqsit","L’Équitable"],
    ["الْجَامِعُ","Al-Jami","Celui qui rassemble"],
    ["الْغَنِيُّ","Al-Ghani","Celui qui se suffit à Lui-même"],
    ["الْمُغْنِي","Al-Mughni","Celui qui enrichit"],
    ["الْمَانِعُ","Al-Mani","Celui qui préserve et empêche"],
    ["الضَّارُّ","Ad-Darr","Celui qui peut éprouver selon Sa sagesse"],
    ["النَّافِعُ","An-Nafi","Celui qui accorde le bien"],
    ["النُّورُ","An-Nur","La Lumière"],
    ["الْهَادِي","Al-Hadi","Le Guide"],
    ["الْبَدِيعُ","Al-Badi","L’Incomparable Créateur"],
    ["الْبَاقِي","Al-Baqi","L’Éternel"],
    ["الْوَارِثُ","Al-Warith","L’Héritier"],
    ["الرَّشِيدُ","Ar-Rashid","Le Guide vers la droiture"],
    ["الصَّبُورُ","As-Sabur","Le Très-Patient"]
  ];

  const names = rawNames.map((entry, index) => ({
    id: index + 1,
    arabic: entry[0],
    transliteration: entry[1],
    meaning: entry[2],
    use: "Sens proposé pour l’étude et la méditation. Les traductions peuvent varier selon les sources."
  }));

  let namesDeck = [];
  let namesIndex = 0;
  let namesUnknown = [];
  let namesReviewing = false;
  let namesKnownCount = 0;
  let namesFirstSide = "front";
  let isBack = false;

  function renderNames() {
    document.getElementById("names-list").innerHTML = names.map(name => {
      const checked = localStorage.getItem("pick-" + name.id) !== "false";
      return `<label>
        <input type="checkbox" value="${name.id}" ${checked ? "checked" : ""}>
        ${name.id}. <span lang="ar" dir="rtl">${name.arabic}</span>
        — ${escapeHtml(name.transliteration)} — ${escapeHtml(name.meaning)}
      </label>`;
    }).join("");
  }

  function selectAllNames(value) {
    document.querySelectorAll("#names-list input").forEach(box => box.checked = value);
  }

  function startNamesStudy(onlyUnknown) {
    let selected;

    if (onlyUnknown) {
      selected = [...namesUnknown];
      namesReviewing = true;
    } else {
      selected = [...document.querySelectorAll("#names-list input:checked")]
        .map(box => names.find(name => name.id === Number(box.value)))
        .filter(Boolean);

      if (!selected.length) {
        alert("Choisis au moins une carte.");
        return;
      }

      document.querySelectorAll("#names-list input").forEach(box =>
        localStorage.setItem("pick-" + box.value, String(box.checked))
      );

      namesUnknown = [];
      namesKnownCount = 0;
      namesReviewing = false;
      namesFirstSide = document.querySelector('input[name="side"]:checked').value;

      if (document.getElementById("shuffle-names").checked) {
        shuffleArray(selected);
      }
    }

    if (!selected.length) {
      alert("Il n’y a aucune carte à revoir.");
      return;
    }

    namesDeck = selected;
    namesIndex = 0;
    isBack = namesFirstSide === "back";
    showSection("names-study");
    renderNameCard();
  }

  function renderNameCard() {
    if (namesIndex >= namesDeck.length) {
      document.getElementById("names-summary").textContent = namesReviewing
        ? `Révision terminée : ${namesUnknown.length} carte(s) restent à revoir.`
        : `Session terminée : ${namesKnownCount} carte(s) connues, ${namesUnknown.length} à revoir.`;

      document.getElementById("review-names-button").classList.toggle(
        "hidden", namesUnknown.length === 0
      );

      showSection("names-finish");
      return;
    }

    const name = namesDeck[namesIndex];
    document.getElementById("names-progress").textContent =
      `Carte ${namesIndex + 1} sur ${namesDeck.length}` +
      (namesReviewing ? " — révision des cartes à revoir" : "");

    renderNameFace(name);
  }

  function renderNameFace(name) {
    const card = document.getElementById("flashcard");

    if (!isBack) {
      card.innerHTML = `<strong>${escapeHtml(name.transliteration)}</strong>
        <div class="arabic" lang="ar">${name.arabic}</div>
        <span class="muted">Appuie pour voir le verso</span>`;
    } else {
      card.innerHTML = `<div class="meaning">${escapeHtml(name.meaning)}</div>
        <p>${escapeHtml(name.use)}</p>
        <span class="muted">Appuie pour voir le recto</span>`;
    }
  }

  function flipNameCard() {
    if (!namesDeck[namesIndex]) return;
    isBack = !isBack;
    renderNameFace(namesDeck[namesIndex]);
  }

  function markNameCard(known) {
    const current = namesDeck[namesIndex];
    if (!current) return;

    if (known) {
      namesKnownCount++;
      namesUnknown = namesUnknown.filter(name => name.id !== current.id);
    } else if (!namesUnknown.some(name => name.id === current.id)) {
      namesUnknown.push(current);
    }

    namesIndex++;
    isBack = namesFirstSide === "back";
    renderNameCard();
  }

  renderNames();
</script>
</body>
</html>
