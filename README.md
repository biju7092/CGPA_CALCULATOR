# CGPA_CALCULATOR
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>BE CGPA Calculator</title>
  <style>
    /* --- General Styles --- */
    body {
      font-family: "Segoe UI", Arial, sans-serif;
      background: #f4f7fb;
      margin: 0;
      padding: 0;
      display: flex;
      justify-content: center;
      align-items: flex-start;
      min-height: 100vh;
    }

    .container {
      background: #fff;
      margin: 20px;
      padding: 20px;
      border-radius: 15px;
      box-shadow: 0 4px 10px rgba(0,0,0,0.1);
      max-width: 400px;
      width: 100%;
    }

    h1 {
      text-align: center;
      color: #333;
      font-size: 22px;
      margin-bottom: 15px;
    }

    label {
      font-weight: bold;
      display: block;
      margin-bottom: 6px;
    }

    input {
      padding: 8px;
      width: 100%;
      font-size: 14px;
      border: 1px solid #ccc;
      border-radius: 6px;
      margin-bottom: 12px;
    }

    button {
      background: #0078d7;
      color: white;
      border: none;
      padding: 10px 15px;
      font-size: 14px;
      border-radius: 8px;
      cursor: pointer;
      margin: 6px;
    }

    button:disabled {
      background: #aaa;
      cursor: not-allowed;
    }

    .grade-buttons {
      display: flex;
      flex-wrap: wrap;
      justify-content: center;
      gap: 10px;
      margin: 15px 0;
    }

    .grade-buttons button {
      width: 90px;
      height: 50px;
      font-size: 16px;
    }

    .summary {
      margin-top: 15px;
      border: 1px solid #ddd;
      border-radius: 8px;
      padding: 10px;
      background: #f9f9f9;
      max-height: 150px;
      overflow-y: auto;
    }

    .result {
      font-weight: bold;
      text-align: center;
      margin: 15px 0;
      font-size: 16px;
      color: #444;
    }

    .reset-btn {
      display: block;
      margin: 0 auto;
      background: #e63946;
    }
  </style>
</head>
<body>
  <div class="container">
    <h1>BE CGPA Calculator</h1>

    <label for="subjects">Number of Subjects:</label>
    <input type="number" id="subjects" min="1" placeholder="Enter number of subjects">
    <button onclick="startCalc()">Start</button>

    <p id="prompt">Enter subjects and press Start</p>

    <div class="grade-buttons">
      <button onclick="choose('O')" disabled>O</button>
      <button onclick="choose('A+')" disabled>A+</button>
      <button onclick="choose('A')" disabled>A</button>
      <button onclick="choose('B+')" disabled>B+</button>
      <button onclick="choose('B')" disabled>B</button>
      <button onclick="choose('RA')" disabled>RA</button>
    </div>

    <div class="summary" id="summary"></div>

    <div class="result" id="result"></div>

    <button class="reset-btn" onclick="resetAll()">Reset</button>
  </div>

  <script>
    // --- Grade mapping ---
    const GRADE_POINTS = { "O": 10, "A+": 9, "A": 8, "B+": 7, "B": 6, "RA": 0 };

    let numSubjects = 0;
    let subjectIndex = 0;
    let points = [];

    function letterFromCGPA(cgpa) {
      if (cgpa >= 9.1) return "O (Outstanding)";
      if (cgpa >= 8.1) return "A+ (Excellent)";
      if (cgpa >= 7.1) return "A (Very Good)";
      if (cgpa >= 6.1) return "B+ (Good)";
      if (cgpa >= 5.0) return "B (Average)";
      return "RA (Reappear)";
    }

    function startCalc() {
      const input = document.getElementById("subjects");
      numSubjects = parseInt(input.value);

      if (isNaN(numSubjects) || numSubjects <= 0) {
        alert("Enter a valid positive number of subjects.");
        return;
      }

      subjectIndex = 0;
      points = [];
      document.getElementById("summary").innerHTML = "";
      document.getElementById("result").innerHTML = "";
      document.getElementById("prompt").innerText = `Subject 1 of ${numSubjects}: tap a grade`;

      // Enable grade buttons
      document.querySelectorAll(".grade-buttons button").forEach(btn => btn.disabled = false);

      input.disabled = true;
    }

    function choose(gradeKey) {
      const p = GRADE_POINTS[gradeKey];
      points.push(p);
      subjectIndex++;

      const summaryDiv = document.getElementById("summary");
      summaryDiv.innerHTML += `<p>Subject ${subjectIndex}: ${gradeKey} → ${p}</p>`;

      if (subjectIndex >= numSubjects) {
        document.querySelectorAll(".grade-buttons button").forEach(btn => btn.disabled = true);

        const cgpa = points.reduce((a, b) => a + b, 0) / numSubjects;
        const final = letterFromCGPA(cgpa);
        document.getElementById("result").innerText = `CGPA: ${cgpa.toFixed(2)} | Final Grade: ${final}`;

        document.getElementById("subjects").disabled = false;
      } else {
        document.getElementById("prompt").innerText = `Subject ${subjectIndex + 1} of ${numSubjects}: tap a grade`;
      }
    }

    function resetAll() {
      document.getElementById("subjects").value = "";
      document.getElementById("subjects").disabled = false;
      document.getElementById("summary").innerHTML = "";
      document.getElementById("result").innerHTML = "";
      document.getElementById("prompt").innerText = "Enter subjects and press Start";
      document.querySelectorAll(".grade-buttons button").forEach(btn => btn.disabled = true);
      points = [];
      subjectIndex = 0;
      numSubjects = 0;
    }
  </script>
</body>
</html>
<link rel="stylesheet" href="style.css">
<script src="script.js"></script>
