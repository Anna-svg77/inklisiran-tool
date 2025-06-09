<!DOCTYPE html>
<html lang="ru">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <title>Назначение Инклисирана</title>
  <style>
    body { font-family: sans-serif; padding: 20px; background: #f6f6f6; }
    .container { max-width: 400px; margin: auto; background: #fff; padding: 20px; border-radius: 12px; box-shadow: 0 2px 8px rgba(0,0,0,0.1); }
    label { display: block; margin-top: 10px; }
    input[type="checkbox"] { margin-right: 10px; }
    select, input[type="number"] { width: 100%; padding: 8px; font-size: 16px; margin-top: 5px; }
    button { margin-top: 20px; width: 100%; padding: 10px; font-size: 16px; background: #2a7bff; color: white; border: none; border-radius: 8px; cursor: pointer; }
    button:hover { background: #1d5edc; }
    .result { margin-top: 20px; padding: 15px; border-radius: 8px; font-weight: bold; }
    .ok { background: #e6fbe6; color: #1a7b1a; }
    .warn { background: #fff8e1; color: #a67800; }
    .danger { background: #fdecea; color: #b00020; }
  </style>
</head>
<body>
  <div class="container">
    <h2>Назначение Инклисирана</h2>
    <label><input type="checkbox" id="hasCVD"> ССЗ (инфаркт, ИБС и т.п.)</label>
    <label><input type="checkbox" id="hasDM"> СД2 с поражением органов-мишеней</label>
    <label><input type="checkbox" id="hasFH"> Подозрение на ФГХ</label>

    <label for="statinDose">Доза статинов:</label>
    <select id="statinDose">
      <option value="none">Нет</option>
      <option value="low">Низкая</option>
      <option value="medium">Средняя</option>
      <option value="high">Высокая</option>
    </select>

    <label><input type="checkbox" id="ezetimibe"> Приём эзетимиба</label>

    <label for="ldl">ЛПНП (ммоль/л):</label>
    <input type="number" id="ldl" step="0.1" min="0">

    <button onclick="evaluatePatient()">ОЦЕНИТЬ</button>

    <div id="result" class="result"></div>
  </div>

  <script>
    function evaluatePatient() {
      const hasCVD = document.getElementById('hasCVD').checked;
      const hasDM = document.getElementById('hasDM').checked;
      const hasFH = document.getElementById('hasFH').checked;
      const statinDose = document.getElementById('statinDose').value;
      const ezetimibe = document.getElementById('ezetimibe').checked;
      const ldl = parseFloat(document.getElementById('ldl').value);
      const resultBox = document.getElementById('result');

      if (isNaN(ldl)) {
        resultBox.innerHTML = "Введите уровень ЛПНП.";
        resultBox.className = "result danger";
        return;
      }

      // Определяем, высока ли доза статинов (high) или есть эзетимиб
      const highStatinOrEzet = (statinDose === 'high') || ezetimibe;

      let message = "";
      let style = "result warn";

      if (hasFH && ldl >= 4.9) {
        message = "Показан инклисиран: подозрение на ФГХ при ЛПНП ≥ 4.9 ммоль/л.";
        style = "result ok";
      } else if ((hasCVD || hasDM) && ldl >= 1.4 && highStatinOrEzet) {
        message = "Показан инклисиран: высокий риск по КР 2025, ЛПНП ≥ 1.4 ммоль/л на высокой дозе статинов и/или приёме эзетимиба.";
        style = "result ok";
      } else {
        message = "Инклисиран не показан. Рассмотрите стандартную терапию.";
        style = "result warn";
      }

      resultBox.innerHTML = message;
      resultBox.className = style;
    }
  </script>
</body>
</html>
