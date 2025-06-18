<!DOCTYPE html>
<html lang="he" dir="rtl">
<head>
  <meta charset="UTF-8">
  <title>חידון לינור שונאת</title>
  <style>
    body {
      margin: 0;
      padding: 0;
      font-family: 'Segoe UI', sans-serif;
      background: linear-gradient(135deg, #8b0000, #000000);
      color: white;
      text-align: center;
    }

    .container {
      padding: 50px 20px;
      max-width: 800px;
      margin: auto;
    }

    h1 {
      font-size: 48px;
      color: #ff3333;
      text-shadow: 2px 2px 5px black;
      margin-bottom: 20px;
    }

    .description {
      font-size: 20px;
      margin-bottom: 40px;
    }

    .question-box {
      background-color: #1c1c1c;
      padding: 20px;
      border-radius: 10px;
      margin-bottom: 30px;
    }

    .question {
      font-size: 22px;
      margin-bottom: 20px;
    }

    .answer {
      background-color: #333;
      border: none;
      color: white;
      padding: 10px 20px;
      margin: 10px;
      border-radius: 5px;
      cursor: pointer;
      font-size: 18px;
    }

    .answer:hover {
      background-color: #ff4444;
    }

    .results-box {
      display: none;
      background-color: #111;
      padding: 20px;
      border-radius: 10px;
    }

    .wrong {
      color: #ff6666;
      margin-top: 10px;
    }

    .link-box {
      display: none;
      margin-top: 40px;
    }

    .link-box a {
      color: #00ccff;
      font-size: 20px;
      text-decoration: none;
      border: 2px solid #00ccff;
      padding: 10px 20px;
      border-radius: 8px;
      transition: 0.3s;
    }

    .link-box a:hover {
      background-color: #00ccff;
      color: #000;
    }
  </style>
</head>
<body>
  <div class="container">
    <h1>חידון לינור שונאת</h1>
    <div class="description">ענה על כל השאלות ואז תגלה מה הציון שלך 🎯<br>ואז – תוכל להאזין לשיר הסודי 🎵</div>
    <div id="quiz"></div>

    <div class="results-box" id="results"></div>

    <div class="link-box" id="songLink">
      <a href="https://www.bandlab.com/revisions/b9128c70-c143-f011-a5f1-6045bd2f3017?sharedKey=uAnpWQNTGUqb6rTawvx6XQ" target="_blank">
        🎧 האזן לשיר המלא
      </a>
    </div>
  </div>

  <script>
    const questions = [
      { q: "מי לא סופרת את לינור?", options: ["אנה", "קורלין", "לושקה", "עוזז"], correct: 1 },
      { q: "מה לינור עושה כשאומרים לה 'בוקר טוב'?", options: ["אומרת תודה", "עוזבת", "עושה פרצוף", "מחייכת"], correct: 2 },
      { q: "מה קורה כשהיא מקבלת לבבות?", options: ["שומרת אותם", "מוחקת אחרי 5 שניות", "שולחת חזרה", "לא מגיבה"], correct: 1 },
      { q: "מה לינור חושבת על אנשים שמחייכים אליה?", options: ["חמודים", "מזויפים", "נחמדים", "מעצבנים"], correct: 1 },
      { q: "מאיפה היא שומרת רשימות של מעצבנים?", options: ["מכיתה ה'", "מהגן", "מכיתה ח'", "מהצבא"], correct: 2 },
      { q: "מה קורה אם אתה נושם?", options: ["אתה מועמד לשנאה", "היא אוהבת אותך", "שום דבר", "אתה מקבל פרס"], correct: 0 },
      { q: "מה היא עונה כשמציעים לה לחשוב חיובי?", options: ["בסדר", "לא רוצה", "שולפת רשימה", "מתעלמת"], correct: 2 },
      { q: "מה היא אומרת אם אתה שואל 'למה את אנטי?'", options: ["לא אנטי, פשוט אתם לא שווים זמן מסך", "כי אני שונאת", "לא עונה", "אין לי כוח"], correct: 0 },
      { q: "איך היא שורפת גשרים?", options: ["בלילה", "באמצע הקיץ", "בסתיו", "בחורף"], correct: 1 },
      { q: "מה כדאי לעשות אם היא צועקת עליך?", options: ["לכעוס", "לבכות", "להתעלם", "לחייך ולהגיד תודה"], correct: 3 }
    ];

    let current = 0;
    let score = 0;
    let wrongAnswers = [];

    function renderQuestion() {
      const quiz = document.getElementById("quiz");
      quiz.innerHTML = "";

      if (current >= questions.length) {
        showResults();
        return;
      }

      const qBox = document.createElement("div");
      qBox.className = "question-box";

      const q = document.createElement("div");
      q.className = "question";
      q.innerText = questions[current].q;

      qBox.appendChild(q);

      questions[current].options.forEach((option, index) => {
        const btn = document.createElement("button");
        btn.className = "answer";
        btn.innerText = option;
        btn.onclick = () => {
          if (index === questions[current].correct) {
            score++;
          } else {
            wrongAnswers.push({
              question: questions[current].q,
              correct: questions[current].options[questions[current].correct]
            });
          }
          current++;
          renderQuestion();
        };
        qBox.appendChild(btn);
      });

      quiz.appendChild(qBox);
    }

    function showResults() {
      const resBox = document.getElementById("results");
      resBox.style.display = "block";

      let html = `<h2>צברת ${score} מתוך ${questions.length} נקודות</h2>`;

      if (wrongAnswers.length > 0) {
        html += `<div class="wrong"><strong>טעויות:</strong><ul>`;
        wrongAnswers.forEach(item => {
          html += `<li>${item.question}<br><em>תשובה נכונה: ${item.correct}</em></li>`;
        });
        html += `</ul></div>`;
      } else {
        html += `<p>מצוין! לא טעית בכלל 🔥</p>`;
      }

      resBox.innerHTML = html;

      document.getElementById("songLink").style.display = "block";
    }

    renderQuestion();
  </script>
</body>
</html>
אממ
