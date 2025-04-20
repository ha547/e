<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>IQ Test</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      background: #f0f0f0;
      padding: 20px;
      max-width: 600px;
      margin: auto;
    }
    .card {
      background: white;
      padding: 20px;
      border-radius: 10px;
      box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
    }
    .question {
      font-size: 18px;
      margin-bottom: 15px;
    }
    .button {
      display: block;
      width: 100%;
      padding: 10px;
      margin: 5px 0;
      background: #007BFF;
      color: white;
      border: none;
      border-radius: 5px;
      cursor: pointer;
    }
    .button:hover {
      background: #0056b3;
    }
    input[type="number"], input[type="text"] {
      width: 100%;
      padding: 10px;
      margin: 10px 0;
      border-radius: 5px;
      border: 1px solid #ccc;
    }
    .certificate {
      text-align: center;
      padding: 20px;
      border: 2px solid #007BFF;
      border-radius: 10px;
      margin-top: 20px;
      background-color: #f9f9f9;
    }
    .certificate h2 {
      margin-bottom: 10px;
      color: #007BFF;
    }
    .certificate p {
      font-size: 18px;
      margin: 10px 0;
    }
    .certificate p span {
      font-weight: bold;
    }
    .certificate .emoji {
      font-size: 36px;
    }
  </style>
</head>
<body>
  <div class="card" id="app">
    <h2>IQ Test</h2>
    <div id="content"></div>
  </div>

  <script>
    const questionBank = [
      { q: "Which number comes next? 2, 4, 8, 16, ?", options: ["18", "24", "32", "30"], correct: 2, difficulty: "easy" },
      { q: "If ALL SQUARES are RECTANGLES, and some RECTANGLES are CIRCLES, then which of the following must be true?", options: ["All squares are circles", "Some squares are circles", "Some circles are rectangles", "All rectangles are squares"], correct: 2, difficulty: "medium" },
      { q: "Find the odd one out: Apple, Banana, Carrot, Mango", options: ["Apple", "Banana", "Carrot", "Mango"], correct: 2, difficulty: "easy" },
      { q: "What is to 3 as 81 is to 9?", options: ["Square", "Cube", "Root", "None"], correct: 0, difficulty: "medium" },
      { q: "What comes next: J, F, M, A, M, J, ?", options: ["K", "J", "A", "S"], correct: 3, difficulty: "medium" },
      { q: "Select the related word: Hand is to Glove as Foot is to?", options: ["Shoe", "Sock", "Boot", "Slipper"], correct: 1, difficulty: "easy" },
      { q: "Which shape has the most sides?", options: ["Triangle", "Pentagon", "Hexagon", "Octagon"], correct: 3, difficulty: "easy" },
      { q: "Which one does not belong? Dog, Cat, Lion, Car", options: ["Dog", "Cat", "Lion", "Car"], correct: 3, difficulty: "easy" },
      { q: "Complete the pattern: 1, 1, 2, 3, 5, 8, ?", options: ["11", "12", "13", "15"], correct: 2, difficulty: "medium" },
      { q: "Mirror image of 'pqbd' is:", options: ["dbqp", "pqbd", "bdpq", "qbpd"], correct: 0, difficulty: "hard" },
      { q: "What is 15% of 200?", options: ["25", "30", "35", "40"], correct: 1, difficulty: "easy" },
      { q: "A clock shows 3:15. What angle is between the hands?", options: ["0°", "7.5°", "15°", "30°"], correct: 1, difficulty: "hard" },
      { q: "Which is the next term? 121, 144, 169, ?", options: ["185", "196", "200", "210"], correct: 1, difficulty: "medium" },
      { q: "Which letter replaces the question mark? A, C, F, J, O, ?", options: ["S", "T", "U", "V"], correct: 2, difficulty: "hard" },
      { q: "Which is heavier: 1kg of iron or 1kg of cotton?", options: ["Iron", "Cotton", "Same", "None"], correct: 2, difficulty: "easy" },
    ];

    const getPoints = (difficulty) => difficulty === "easy" ? 1 : difficulty === "medium" ? 2 : 3;

    let selectedQuestions = [];
    let current = 0;
    let score = 0;
    let name = '';
    let age = 0;
    let maxScore = 0;

    const content = document.getElementById("content");

    function startNameAgePrompt() {
      content.innerHTML = `
        <label for="name">Enter your name:</label>
        <input type="text" id="name" />
        <label for="age">Enter your age:</label>
        <input type="number" id="age" min="5" max="100" />
        <button class="button" onclick="submitNameAge()">Start Test</button>
      `;
    }

    function submitNameAge() {
      const nameInput = document.getElementById("name").value;
      const ageInput = parseInt(document.getElementById("age").value);
      
      if (nameInput && !isNaN(ageInput) && ageInput > 0) {
        name = nameInput;
        age = ageInput;
        startTest();
      } else {
        alert("Please enter both your name and a valid age.");
      }
    }

    function startTest() {
      selectedQuestions = questionBank.sort(() => 0.5 - Math.random()).slice(0, 25);
      maxScore = selectedQuestions.reduce((sum, q) => sum + getPoints(q.difficulty), 0);
      current = 0;
      score = 0;
      showQuestion();
    }

    function showQuestion() {
      const q = selectedQuestions[current];
      content.innerHTML = `
        <div class="question">${q.q}</div>
        ${q.options.map((opt, idx) => `<button class="button" onclick="selectAnswer(${idx})">${opt}</button>`).join("")}
        <p style="text-align: right; font-size: 12px;">Question ${current + 1} of 25</p>
      `;
    }

    function selectAnswer(index) {
      const q = selectedQuestions[current];
      if (index === q.correct) {
        score += getPoints(q.difficulty);
      }
      if (current + 1 < selectedQuestions.length) {
        current++;
        showQuestion();
      } else {
        showResult();
      }
    }

    function showResult() {
      const baseIQ = Math.round((score / maxScore) * 100);
      const ageFactor = Math.max(0, 18 - age) * 1.5;
      let finalIQ = baseIQ + ageFactor;
      finalIQ = Math.max(70, Math.min(finalIQ, 145));

      content.innerHTML = `
        <h3>Your Estimated IQ: <span class="emoji">🧠</span></h3>
        <p style="font-size: 36px; color: #007BFF;">${finalIQ}</p>
        <p>(Score: ${score} out of ${maxScore}, Age: ${age})</p>
        <div class="certificate">
          <h2>Standard IQ Test Certificate 🎓</h2>
          <p>Name: <span>${name}</span></p>
          <p>Age: <span>${age}</span></p>
          <p>IQ Score: <span>${finalIQ}</span></p>
          <p>Marks: <span>${score} / ${maxScore}</span></p>
          <p>👏 Well done! Keep up the good work! 💡</p>
        </div>
        <p>Thank you for completing the test!</p>
      `;
    }

    startNameAgePrompt();
  </script>
</body>
</html>
