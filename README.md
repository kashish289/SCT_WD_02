

<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8"/>
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>Modern Stopwatch</title>
  <style>
    * {
      box-sizing: border-box;
      padding: 0;
      margin: 0;
    }

    body {
      font-family: 'Poppins', sans-serif;
      background: linear-gradient(135deg, #74ebd5, #ACB6E5);
      display: flex;
      justify-content: center;
      align-items: center;
      height: 100vh;
      color: #333;
    }

    .stopwatch {
      background: #fff;
      padding: 40px 30px;
      border-radius: 20px;
      box-shadow: 0 15px 35px rgba(0, 0, 0, 0.15);
      text-align: center;
      width: 350px;
      transition: 0.3s ease;
    }

    .time {
      font-size: 48px;
      font-weight: 600;
      margin-bottom: 25px;
      letter-spacing: 1px;
    }

    .buttons {
      display: flex;
      flex-wrap: wrap;
      justify-content: center;
      gap: 10px;
    }

    .buttons button {
      padding: 12px 20px;
      border: none;
      border-radius: 12px;
      background: #007bff;
      color: #fff;
      font-size: 16px;
      cursor: pointer;
      transition: background 0.3s ease, transform 0.2s;
      box-shadow: 0 5px 15px rgba(0, 123, 255, 0.3);
    }

    .buttons button:hover {
      background: #0056b3;
      transform: translateY(-2px);
    }

    .laps {
      margin-top: 25px;
      max-height: 150px;
      overflow-y: auto;
      text-align: left;
    }

    .laps ul {
      list-style: none;
      padding-left: 0;
    }

    .laps li {
      background: #f1f1f1;
      margin: 6px 0;
      padding: 10px 15px;
      border-radius: 10px;
      font-size: 14px;
      font-weight: 500;
      color: #444;
    }

    /* Scrollbar style */
    .laps ul::-webkit-scrollbar {
      width: 6px;
    }

    .laps ul::-webkit-scrollbar-thumb {
      background: #ccc;
      border-radius: 6px;
    }

    @media (max-width: 400px) {
      .stopwatch {
        width: 90%;
        padding: 30px 20px;
      }

      .time {
        font-size: 40px;
      }

      .buttons button {
        padding: 10px 14px;
        font-size: 14px;
      }
    }
  </style>
</head>
<body>

  <div class="stopwatch">
    <div class="time" id="display">00:00:00.000</div>
    <div class="buttons">
      <button onclick="startStopwatch()">Start</button>
      <button onclick="pauseStopwatch()">Pause</button>
      <button onclick="resetStopwatch()">Reset</button>
      <button onclick="lapTime()">Lap</button>
    </div>
    <div class="laps">
      <ul id="lapList"></ul>
    </div>
  </div>

  <script>
    let startTime, updatedTime, difference, timerInterval;
    let running = false;
    let lapCounter = 1;

    const display = document.getElementById("display");
    const lapList = document.getElementById("lapList");

    function updateDisplay(time) {
      let milliseconds = time % 1000;
      let seconds = Math.floor((time / 1000) % 60);
      let minutes = Math.floor((time / (1000 * 60)) % 60);
      let hours = Math.floor((time / (1000 * 60 * 60)));

      display.textContent =
        `${String(hours).padStart(2, '0')}:` +
        `${String(minutes).padStart(2, '0')}:` +
        `${String(seconds).padStart(2, '0')}.` +
        `${String(milliseconds).padStart(3, '0')}`;
    }

    function startStopwatch() {
      if (!running) {
        startTime = new Date().getTime() - (difference || 0);
        timerInterval = setInterval(() => {
          updatedTime = new Date().getTime();
          difference = updatedTime - startTime;
          updateDisplay(difference);
        }, 10);
        running = true;
      }
    }

    function pauseStopwatch() {
      if (running) {
        clearInterval(timerInterval);
        running = false;
      }
    }

    function resetStopwatch() {
      clearInterval(timerInterval);
      running = false;
      startTime = 0;
      difference = 0;
      updateDisplay(0);
      lapList.innerHTML = "";
      lapCounter = 1;
    }

    function lapTime() {
      if (running) {
        const lapItem = document.createElement("li");
        lapItem.textContent = `Lap ${lapCounter++}: ${display.textContent}`;
        lapList.appendChild(lapItem);
      }
    }
  </script>
</body>
</html>
