<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <script src="https://kit.fontawesome.com/7101c27beb.js" crossorigin="anonymous"></script>
    <title>Document</title>
</head>
<body>
    


    <div class="game-container">
        <h1>Trouve le Numéro  </h1>
        <div class="input-container">
          <label for="userGuess">Entrez un nombre entre 1 et 100 : </label>
          <input type="number" id="userGuess" min="1" max="100">
          <button onclick="checkGuess()">Vérifier</button>
          <button class="restart-button hidden" onclick="restartGame()">Recommencer</button>
        </div>
        <div class="message" id="message"></div>
        <div class="attempts hidden" id="attempts"></div>
    
        <div class="scores-container">
          <h2>Anciens scores </h2>
          <ul id="scores-list" > </ul>
        </div>
    
      </div>
    


    <script>
        const randomNumber = Math.floor(Math.random() * 100) + 1;
        let attempts = 0;
        const maxAttempts = 7;
    
        window.addEventListener('load', () => {
          const scores = getScoresFromCookies();
          displayScores(scores);
        });
    
        function checkGuess() {
          const userGuessInput = document.getElementById('userGuess');
          const userGuess = parseInt(userGuessInput.value);
    
          if (isNaN(userGuess) || userGuess < 1 || userGuess > 100) {
            showMessage('Veuillez entrer un nombre valide entre 1 et 100.');
            return;
          }
    
          attempts++;
    
          if (userGuess === randomNumber) {
            showSuccessMessage(`Bravo ! Vous avez trouvé le nombre ${randomNumber} en ${attempts} essais.`);
            disableInput();
            showRestartButton();
            saveScoreToCookies(attempts);
            displayScores(getScoresFromCookies());
          } else if (attempts === maxAttempts) {
            showFailureMessage(`Désolé, vous avez utilisé toutes vos chances. Le nombre était ${randomNumber}.`);
            disableInput();
            showRestartButton();
          } else {
            if (userGuess < randomNumber) {
              showMessage(`Le nombre est plus grand que ${userGuess}.`);
            } else {
              showMessage(`Le nombre est plus petit que ${userGuess}.`);
            }
            userGuessInput.value = '';
            userGuessInput.focus();
            showAttempts();
          }
        }
    
        function showMessage(message) {
          const messageDiv = document.getElementById('message');
          messageDiv.textContent = message;
        }
    
        function showSuccessMessage(message) {
          const messageDiv = document.getElementById('message');
          messageDiv.textContent = message;
          messageDiv.style.color = 'green';
        }
    
        function showFailureMessage(message) {
          const messageDiv = document.getElementById('message');
          messageDiv.textContent = message;
          messageDiv.style.color = 'red';
        }
    
        function showAttempts() {
          const attemptsDiv = document.getElementById('attempts');
          const remainingAttempts = maxAttempts - attempts;
          attemptsDiv.textContent = `Essai ${attempts}/${maxAttempts} (${remainingAttempts} restant${remainingAttempts > 1 ? 's' : ''})`;
          attemptsDiv.classList.remove('hidden');
        }
    
        function disableInput() {
          const userGuessInput = document.getElementById('userGuess');
          const checkButton = document.querySelector('button');
          userGuessInput.disabled = true;
          checkButton.disabled = true;
        }
    
        function showRestartButton() {
          const restartButton = document.querySelector('.restart-button');
          restartButton.classList.remove('hidden');
        }
    
        function restartGame() {
          const userGuessInput = document.getElementById('userGuess');
          const messageDiv = document.getElementById('message');
          const attemptsDiv = document.getElementById('attempts');
          const restartButton = document.querySelector('.restart-button');
    
          userGuessInput.value = '';
          userGuessInput.disabled = false;
          userGuessInput.focus();
    
          messageDiv.textContent = '';
          attemptsDiv.textContent = '';
          attemptsDiv.classList.add('hidden');
    
          restartButton.classList.add('hidden');
    
          attempts = 0;
          const randomNumber = Math.floor(Math.random() * 100) + 1;
        }
    
        function saveScoreToCookies(score) {
          const scores = getScoresFromCookies();
          scores.push(score);
          const serializedScores = JSON.stringify(scores);
          document.cookie = `scores=${serializedScores}`;
        }
    
        function getScoresFromCookies() {
          const cookies = document.cookie.split(';');
          const scoresCookie = cookies.find(cookie => cookie.trim().startsWith('scores='));
          if (scoresCookie) {
            const serializedScores = scoresCookie.split('=')[1];
            return JSON.parse(serializedScores);
          }
          return [];
        }
    
        function displayScores(scores) {
          const scoresList = document.getElementById('scores-list');
          scoresList.innerHTML = '';
          scores.forEach(score => {
            const li = document.createElement('li');
            li.textContent = `Score: ${score}`;
            scoresList.appendChild(li);
          });
        }


        function displayScores(scores) {
  const scoresList = document.getElementById('scores-list');
  scoresList.innerHTML = '';

  const bestScore = scores.length > 0 ? Math.min(...scores) : null;

  if (bestScore !== null) {
    const bestScoreLi = document.createElement('li');
    bestScoreLi.textContent = `Meilleur score:  ${bestScore}`;
    bestScoreLi.style.fontWeight = 'bold';
    scoresList.appendChild(bestScoreLi);
  }

  scores.forEach(score => {
    if (score !== bestScore) {
      const li = document.createElement('li');
      li.textContent = `Score: ${score}`;
      scoresList.appendChild(li);
    }
  });
}

      </script>
</body>
</html>


<style>
    body {
      font-family: Arial, sans-serif;
      background-color: #f3f3f3;
      margin: 0;
      padding: 0;
    }
  
    .game-container {
      margin: 50px auto;
      max-width: 400px;
      background-color: #fff;
      padding: 20px;
      box-shadow: 0px 0px 5px rgba(0, 0, 0, 0.2);
      border-radius: 5px;
    }
  
    h1 {
      margin: 0 0 20px;
      color: #333;
    }
  
    .input-container {
      margin-bottom: 20px;
    }
  
    label {
      display: block;
      margin-bottom: 10px;
      font-weight: bold;
      color: #333;
    }
  
    input[type="number"] {
      padding: 5px;
      width: 100px;
      font-size: 16px;
      border: 1px solid #ccc;
      border-radius: 5px;
    }
  
    button {
      padding: 8px 16px;
      background-color: #050505;
      color: #fff;
      border: none;
      border-radius: 5px;
      font-size: 16px;
      cursor: pointer;
    }
  
    button:disabled {
      background-color: #ccc;
      cursor: not-allowed;
    }
  
    .message {
      margin-bottom: 20px;
      font-size: 18px;
      color: #333;
    }
  
    .success-message {
      color: rgb(4, 4, 4);
    }
  
    .failure-message {
      color: red;
    }
  
    .attempts {
      font-size: 16px;
      color: #333;
    }
  
    .scores-container {
      margin-top: 40px;
    }
  
    .scores-container h2 {
      margin: 0 0 10px;
      color: #333;
    }
  
    .scores-container ul {
      padding: 0;
      list-style: none;
    }
  
    .scores-container li {
      margin-bottom: 5px;
      color: #333;
    }
  </style>
  
