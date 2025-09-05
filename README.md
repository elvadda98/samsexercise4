<!DOCTYPE html>
<html lang="it">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Giochi di Vocabolario Italiano</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: 'Comic Sans MS', 'Segoe UI', Tahoma, sans-serif;
        }

        body {
            background: linear-gradient(135deg, #6a89cc 0%, #4a69bd 100%);
            min-height: 100vh;
            padding: 20px;
            color: #333;
            display: flex;
            flex-direction: column;
            align-items: center;
        }

        .container {
            max-width: 1000px;
            width: 100%;
            background: white;
            border-radius: 20px;
            box-shadow: 0 15px 35px rgba(0,0,0,0.2);
            overflow: hidden;
            margin-bottom: 30px;
        }

        .header {
            background: linear-gradient(135deg, #6a89cc, #4a69bd);
            color: white;
            padding: 25px;
            text-align: center;
            position: relative;
        }

        .header::before {
            content: '';
            position: absolute;
            top: 0;
            left: 0;
            right: 0;
            bottom: 0;
            background: url('data:image/svg+xml,<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 100 100"><circle cx="20" cy="20" r="2" fill="rgba(255,255,255,0.1)"/><circle cx="80" cy="30" r="1.5" fill="rgba(255,255,255,0.1)"/><circle cx="40" cy="70" r="1" fill="rgba(255,255,255,0.1)"/><circle cx="90" cy="80" r="2.5" fill="rgba(255,255,255,0.1)"/></svg>');
        }

        .header h1 {
            font-size: 2.2em;
            margin-bottom: 10px;
            position: relative;
            z-index: 1;
            text-shadow: 2px 2px 4px rgba(0,0,0,0.2);
        }

        .header p {
            font-size: 1.1em;
            opacity: 0.9;
            position: relative;
            z-index: 1;
        }

        .game-tabs {
            display: flex;
            justify-content: center;
            gap: 5px;
            padding: 15px;
            background: #f8f9fa;
            flex-wrap: wrap;
        }

        .game-tab {
            padding: 12px 20px;
            border: none;
            border-radius: 15px;
            cursor: pointer;
            font-size: 16px;
            font-weight: bold;
            transition: all 0.3s ease;
            background: white;
            color: #666;
            box-shadow: 0 4px 10px rgba(0,0,0,0.1);
        }

        .game-tab.active {
            background: linear-gradient(135deg, #6a89cc, #4a69bd);
            color: white;
            transform: translateY(-2px);
            box-shadow: 0 6px 15px rgba(74,105,189,0.4);
        }

        .game-tab:hover:not(.active) {
            background: #e9ecef;
            transform: translateY(-1px);
        }

        .game-section {
            display: none;
            padding: 25px;
            min-height: 400px;
        }

        .game-section.active {
            display: block;
            animation: fadeIn 0.5s ease-in;
        }

        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(20px); }
            to { opacity: 1; transform: translateY(0); }
        }

        .word-bank {
            background: linear-gradient(135deg, #f5f7fa, #e8f4f2);
            padding: 15px;
            border-radius: 15px;
            margin-bottom: 20px;
            border: 2px solid #6a89cc;
            box-shadow: 0 4px 15px rgba(106,137,204,0.2);
        }

        .word-bank h3 {
            color: #2c3e50;
            margin-bottom: 15px;
            text-align: center;
            font-size: 1.3em;
        }

        .word-options {
            display: flex;
            flex-wrap: wrap;
            gap: 10px;
            justify-content: center;
        }

        .word-option {
            background: linear-gradient(135deg, #6a89cc, #4a69bd);
            color: white;
            padding: 8px 16px;
            border-radius: 20px;
            font-weight: bold;
            font-size: 15px;
            box-shadow: 0 3px 10px rgba(106,137,204,0.3);
            transition: all 0.3s ease;
            cursor: default;
        }

        .question {
            background: #f8f9fa;
            padding: 20px;
            border-radius: 15px;
            margin-bottom: 15px;
            border-left: 5px solid #6a89cc;
            box-shadow: 0 4px 15px rgba(0,0,0,0.05);
        }

        .question h3 {
            color: #2c3e50;
            margin-bottom: 12px;
            font-size: 1.2em;
        }

        .fill-blank {
            background: #fff;
            border: 2px solid #ddd;
            border-radius: 8px;
            padding: 8px 12px;
            font-size: 16px;
            margin: 0 5px;
            min-width: 120px;
            transition: border-color 0.3s ease;
            text-align: center;
        }

        .fill-blank:focus {
            outline: none;
            border-color: #6a89cc;
        }

        .fill-blank.correct {
            border-color: #4CAF50;
            background: #e8f5e8;
        }

        .fill-blank.incorrect {
            border-color: #f44336;
            background: #ffeaea;
        }

        .options {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
            gap: 12px;
            margin-top: 12px;
        }

        .option {
            padding: 12px 18px;
            border: 2px solid #ddd;
            border-radius: 10px;
            cursor: pointer;
            transition: all 0.3s ease;
            background: white;
            text-align: center;
            font-weight: 500;
        }

        .option:hover {
            border-color: #6a89cc;
            transform: translateY(-2px);
            box-shadow: 0 4px 15px rgba(106,137,204,0.2);
        }

        .option.selected {
            background: #6a89cc;
            color: white;
            border-color: #6a89cc;
        }

        .option.correct {
            background: #4CAF50;
            color: white;
            border-color: #4CAF50;
        }

        .option.incorrect {
            background: #f44336;
            color: white;
            border-color: #f44336;
        }

        .matching-container {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 15px;
            margin-top: 20px;
        }

        .match-column h4 {
            text-align: center;
            margin-bottom: 15px;
            color: #2c3e50;
            font-size: 1.2em;
        }

        .match-item {
            padding: 12px;
            margin: 8px 0;
            border: 2px solid #ddd;
            border-radius: 10px;
            cursor: pointer;
            transition: all 0.3s ease;
            background: white;
            text-align: center;
            font-weight: 500;
        }

        .match-item:hover {
            border-color: #6a89cc;
            transform: translateY(-1px);
            box-shadow: 0 4px 15px rgba(106,137,204,0.2);
        }

        .match-item.selected {
            background: #e8f4f2;
            border-color: #6a89cc;
        }

        .match-item.matched {
            background: #4CAF50;
            color: white;
            border-color: #4CAF50;
            cursor: default;
        }

        .check-btn {
            background: linear-gradient(135deg, #6a89cc, #4a69bd);
            color: white;
            border: none;
            padding: 12px 25px;
            border-radius: 25px;
            cursor: pointer;
            font-size: 16px;
            font-weight: bold;
            margin: 20px auto;
            display: block;
            transition: all 0.3s ease;
            box-shadow: 0 4px 15px rgba(106,137,204,0.3);
        }

        .check-btn:hover {
            transform: translateY(-2px);
            box-shadow: 0 6px 20px rgba(106,137,204,0.4);
        }

        .feedback {
            margin-top: 15px;
            padding: 12px;
            border-radius: 10px;
            font-weight: bold;
            text-align: center;
            animation: slideIn 0.3s ease;
        }

        @keyframes slideIn {
            from { transform: translateX(-20px); opacity: 0; }
            to { transform: translateX(0); opacity: 1; }
        }

        .feedback.correct {
            background: #d4edda;
            color: #155724;
            border: 1px solid #c3e6cb;
        }

        .feedback.incorrect {
            background: #f8d7da;
            color: #721c24;
            border: 1px solid #f5c6cb;
        }

        .score {
            background: linear-gradient(135deg, #6a89cc, #4a69bd);
            color: white;
            padding: 12px 18px;
            border-radius: 25px;
            font-weight: bold;
            box-shadow: 0 4px 15px rgba(106,137,204,0.3);
            margin-bottom: 20px;
            text-align: center;
        }

        .game-icon {
            font-size: 24px;
            margin-right: 10px;
            vertical-align: middle;
        }

        @media (max-width: 768px) {
            .matching-container {
                grid-template-columns: 1fr;
                gap: 15px;
            }
            
            .game-tabs {
                flex-direction: column;
                align-items: center;
            }
            
            .game-tab {
                width: 200px;
            }
            
            .fill-blank {
                min-width: 100px;
                margin: 5px 2px;
            }
        }
    </style>
</head>
<body>
    <div class="score">Punteggio: <span id="score">0</span>/15</div>
    
    <div class="container">
        <div class="header">
            <h1>🎮 Giochi di Vocabolario Italiano</h1>
            <p>Impara e pratica queste parole italiane: volte, cappello, correre, dappertutto, potessi, anche, aereo, pensavo, pericoloso, alcuni</p>
        </div>

        <div class="game-tabs">
            <button class="game-tab active" onclick="showGame('match')"><span class="game-icon">🔍</span>Abbina le Parole</button>
            <button class="game-tab" onclick="showGame('fill')"><span class="game-icon">📝</span>Completa la Frase</button>
            <button class="game-tab" onclick="showGame('choice')"><span class="game-icon">✅</span>Scegi la Risposta</button>
        </div>

        <!-- Matching Game -->
        <div id="game-match" class="game-section active">
            <div class="word-bank">
                <h3>🔍 Abbina le parole italiane con le loro traduzioni in inglese</h3>
            </div>

            <div class="matching-container">
                <div class="match-column">
                    <h4>Italiano</h4>
                    <div class="match-item" data-word="volte" onclick="selectMatch(this)">volte</div>
                    <div class="match-item" data-word="cappello" onclick="selectMatch(this)">cappello</div>
                    <div class="match-item" data-word="correre" onclick="selectMatch(this)">correre</div>
                    <div class="match-item" data-word="dappertutto" onclick="selectMatch(this)">dappertutto</div>
                    <div class="match-item" data-word="potessi" onclick="selectMatch(this)">potessi</div>
                </div>
                <div class="match-column">
                    <h4>English</h4>
                    <div class="match-item" data-meaning="pericoloso" onclick="selectMatch(this)">dangerous</div>
                    <div class="match-item" data-meaning="volte" onclick="selectMatch(this)">times</div>
                    <div class="match-item" data-meaning="aereo" onclick="selectMatch(this)">airplane</div>
                    <div class="match-item" data-meaning="cappello" onclick="selectMatch(this)">hat</div>
                    <div class="match-item" data-meaning="pensavo" onclick="selectMatch(this)">I thought</div>
                </div>
            </div>

            <div class="matching-container" style="margin-top: 30px;">
                <div class="match-column">
                    <h4>Italiano</h4>
                    <div class="match-item" data-word="anche" onclick="selectMatch(this)">anche</div>
                    <div class="match-item" data-word="aereo" onclick="selectMatch(this)">aereo</div>
                    <div class="match-item" data-word="pensavo" onclick="selectMatch(this)">pensavo</div>
                    <div class="match-item" data-word="pericoloso" onclick="selectMatch(this)">pericoloso</div>
                    <div class="match-item" data-word="alcuni" onclick="selectMatch(this)">alcuni</div>
                </div>
                <div class="match-column">
                    <h4>English</h4>
                    <div class="match-item" data-meaning="correre" onclick="selectMatch(this)">to run</div>
                    <div class="match-item" data-meaning="dappertutto" onclick="selectMatch(this)">everywhere</div>
                    <div class="match-item" data-meaning="alcuni" onclick="selectMatch(this)">some</div>
                    <div class="match-item" data-meaning="anche" onclick="selectMatch(this)">also</div>
                    <div class="match-item" data-meaning="potessi" onclick="selectMatch(this)">I could</div>
                </div>
            </div>

            <div class="feedback" id="match-feedback" style="display: none;"></div>
        </div>

        <!-- Fill in the Blanks Game -->
        <div id="game-fill" class="game-section">
            <div class="word-bank">
                <h3>📝 Banca Parole - Scegli tra queste parole:</h3>
                <div class="word-options">
                    <span class="word-option">volte</span>
                    <span class="word-option">cappello</span>
                    <span class="word-option">correre</span>
                    <span class="word-option">dappertutto</span>
                    <span class="word-option">potessi</span>
                    <span class="word-option">anche</span>
                    <span class="word-option">aereo</span>
                    <span class="word-option">pensavo</span>
                    <span class="word-option">pericoloso</span>
                    <span class="word-option">alcuni</span>
                </div>
            </div>

            <div class="question">
                <h3>Completa la frase:</h3>
                <p>Attraversare la strada senza guardare è <input type="text" class="fill-blank" data-answer="pericoloso" placeholder="risposta">.</p>
            </div>

            <div class="question">
                <h3>Completa la frase:</h3>
                <p>Mi piace il tuo nuovo <input type="text" class="fill-blank" data-answer="cappello" placeholder="risposta">, è molto elegante.</p>
            </div>

            <div class="question">
                <h3>Completa la frase:</h3>
                <p>Prendiamo l'<input type="text" class="fill-blank" data-answer="aereo" placeholder="risposta"> per andare a Roma.</p>
            </div>

            <div class="question">
                <h3>Completa la frase:</h3>
                <p>Ho cercato le chiavi <input type="text" class="fill-blank" data-answer="dappertutto" placeholder="risposta"> ma non le trovo.</p>
            </div>

            <div class="question">
                <h3>Completa la frase:</h3>
                <p>Voglio andare al cinema e <input type="text" class="fill-blank" data-answer="anche" placeholder="risposta"> a cena dopo.</p>
            </div>

            <div class="question">
                <h3>Completa la frase:</h3>
                <p>Devo <input type="text" class="fill-blank" data-answer="correre" placeholder="risposta"> per prendere l'autobus.</p>
            </div>

            <div class="question">
                <h3>Completa la frase:</h3>
                <p>Se <input type="text" class="fill-blank" data-answer="potessi" placeholder="risposta">, viaggerei in tutto il mondo.</p>
            </div>

            <div class="question">
                <h3>Completa la frase:</h3>
                <p>Non sapevi? <input type="text" class="fill-blank" data-answer="pensavo" placeholder="risposta"> che tu lo sapessi già.</p>
            </div>

            <div class="question">
                <h3>Completa la frase:</h3>
                <p><input type="text" class="fill-blank" data-answer="alcuni" placeholder="risposta"> studenti non hanno fatto i compiti.</p>
            </div>

            <div class="question">
                <h3>Completa la frase:</h3>
                <p>Ho provato molte <input type="text" class="fill-blank" data-answer="volte" placeholder="risposta"> a chiamarti.</p>
            </div>

            <button class="check-btn" onclick="checkFillBlanks()">Controlla le Risposte</button>
            <div class="feedback" id="fill-feedback" style="display: none;"></div>
        </div>

        <!-- Multiple Choice Game -->
        <div id="game-choice" class="game-section">
            <div class="question">
                <h3>Cosa significa "volte"?</h3>
                <div class="options">
                    <div class="option" onclick="selectOption(this, 'times')">times</div>
                    <div class="option" onclick="selectOption(this, 'places')">places</div>
                    <div class="option" onclick="selectOption(this, 'voices')">voices</div>
                    <div class="option" onclick="selectOption(this, 'volts')">volts</div>
                </div>
            </div>

            <div class="question">
                <h3>Cosa significa "cappello"?</h3>
                <div class="options">
                    <div class="option" onclick="selectOption(this, 'hat')">hat</div>
                    <div class="option" onclick="selectOption(this, 'coat')">coat</div>
                    <div class="option" onclick="selectOption(this, 'cap')">cap</div>
                    <div class="option" onclick="selectOption(this, 'head')">head</div>
                </div>
            </div>

            <div class="question">
                <h3>Cosa significa "correre"?</h3>
                <div class="options">
                    <div class="option" onclick="selectOption(this, 'to run')">to run</div>
                    <div class="option" onclick="selectOption(this, 'to correct')">to correct</div>
                    <div class="option" onclick="selectOption(this, 'to color')">to color</div>
                    <div class="option" onclick="selectOption(this, 'to carry')">to carry</div>
                </div>
            </div>

            <div class="question">
                <h3>Cosa significa "dappertutto"?</h3>
                <div class="options">
                    <div class="option" onclick="selectOption(this, 'everywhere')">everywhere</div>
                    <div class="option" onclick="selectOption(this, 'nowhere')">nowhere</div>
                    <div class="option" onclick="selectOption(this, 'somewhere')">somewhere</div>
                    <div class="option" onclick="selectOption(this, 'anywhere')">anywhere</div>
                </div>
            </div>

            <div class="question">
                <h3>Cosa significa "potessi"?</h3>
                <div class="options">
                    <div class="option" onclick="selectOption(this, 'I could')">I could</div>
                    <div class="option" onclick="selectOption(this, 'I would')">I would</div>
                    <div class="option" onclick="selectOption(this, 'I should')">I should</div>
                    <div class="option" onclick="selectOption(this, 'I can')">I can</div>
                </div>
            </div>

            <div class="question">
                <h3>Cosa significa "anche"?</h3>
                <div class="options">
                    <div class="option" onclick="selectOption(this, 'also')">also</div>
                    <div class="option" onclick="selectOption(this, 'and')">and</div>
                    <div class="option" onclick="selectOption(this, 'but')">but</div>
                    <div class="option" onclick="selectOption(this, 'or')">or</div>
                </div>
            </div>

            <div class="question">
                <h3>Cosa significa "aereo"?</h3>
                <div class="options">
                    <div class="option" onclick="selectOption(this, 'airplane')">airplane</div>
                    <div class="option" onclick="selectOption(this, 'air')">air</div>
                    <div class="option" onclick="selectOption(this, 'aerial')">aerial</div>
                    <div class="option" onclick="selectOption(this, 'airport')">airport</div>
                </div>
            </div>

            <div class="question">
                <h3>Cosa significa "pensavo"?</h3>
                <div class="options">
                    <div class="option" onclick="selectOption(this, 'I thought')">I thought</div>
                    <div class="option" onclick="selectOption(this, 'I think')">I think</div>
                    <div class="option" onclick="selectOption(this, 'I believe')">I believe</div>
                    <div class="option" onclick="selectOption(this, 'I know')">I know</div>
                </div>
            </div>

            <div class="question">
                <h3>Cosa significa "pericoloso"?</h3>
                <div class="options">
                    <div class="option" onclick="selectOption(this, 'dangerous')">dangerous</div>
                    <div class="option" onclick="selectOption(this, 'beautiful')">beautiful</div>
                    <div class="option" onclick="selectOption(this, 'powerful')">powerful</div>
                    <div class="option" onclick="selectOption(this, 'careful')">careful</div>
                </div>
            </div>

            <div class="question">
                <h3>Cosa significa "alcuni"?</h3>
                <div class="options">
                    <div class="option" onclick="selectOption(this, 'some')">some</div>
                    <div class="option" onclick="selectOption(this, 'all')">all</div>
                    <div class="option" onclick="selectOption(this, 'many')">many</div>
                    <div class="option" onclick="selectOption(this, 'few')">few</div>
                </div>
            </div>

            <button class="check-btn" onclick="checkMultipleChoice()">Controlla le Risposte</button>
            <div class="feedback" id="choice-feedback" style="display: none;"></div>
        </div>
    </div>

    <script>
        let score = 0;
        let selectedWord = null;
        let selectedMeaning = null;
        let matchedPairs = [];
        let selectedOptions = {};

        function showGame(gameId) {
            // Hide all games
            document.querySelectorAll('.game-section').forEach(game => {
                game.classList.remove('active');
            });
            
            // Remove active class from all tabs
            document.querySelectorAll('.game-tab').forEach(tab => {
                tab.classList.remove('active');
            });
            
            // Show selected game
            document.getElementById('game-' + gameId).classList.add('active');
            
            // Add active class to clicked tab
            event.target.classList.add('active');
        }

        function selectMatch(element) {
            if (element.classList.contains('matched')) return;
            
            if (element.dataset.word) {
                // Word selected
                if (selectedWord) selectedWord.classList.remove('selected');
                selectedWord = element;
                element.classList.add('selected');
            } else {
                // Meaning selected
                if (selectedMeaning) selectedMeaning.classList.remove('selected');
                selectedMeaning = element;
                element.classList.add('selected');
            }
            
            // Check if we have both word and meaning selected
            if (selectedWord && selectedMeaning) {
                checkMatch();
            }
        }

        function checkMatch() {
            const feedback = document.getElementById('match-feedback');
            
            if (selectedWord.dataset.word === selectedMeaning.dataset.meaning) {
                // Correct match
                selectedWord.classList.remove('selected');
                selectedWord.classList.add('matched');
                selectedMeaning.classList.remove('selected');
                selectedMeaning.classList.add('matched');
                
                matchedPairs.push(selectedWord.dataset.word);
                
                feedback.textContent = '✅ Abbinamento corretto!';
                feedback.className = 'feedback correct';
                
                // Check if all matches are complete
                if (matchedPairs.length === 10) {
                    feedback.textContent = '🎉 Congratulazioni! Hai abbinato tutte le parole correttamente!';
                    score += 10;
                    document.getElementById('score').textContent = score;
                }
            } else {
                // Incorrect match
                feedback.textContent = '❌ Abbinamento errato. Riprova.';
                feedback.className = 'feedback incorrect';
            }
            
            feedback.style.display = 'block';
            selectedWord = null;
            selectedMeaning = null;
        }

        function checkFillBlanks() {
            const blanks = document.querySelectorAll('.fill-blank');
            let correctCount = 0;
            let allFilled = true;
            
            blanks.forEach(blank => {
                if (blank.value.trim() === '') {
                    allFilled = false;
                }
            });
            
            if (!allFilled) {
                document.getElementById('fill-feedback').textContent = 'Per favore, riempi tutti gli spazi vuoti prima di controllare!';
                document.getElementById('fill-feedback').className = 'feedback incorrect';
                document.getElementById('fill-feedback').style.display = 'block';
                return;
            }
            
            blanks.forEach(blank => {
                const userAnswer = blank.value.toLowerCase().trim();
                const correctAnswer = blank.dataset.answer.toLowerCase();
                
                if (userAnswer === correctAnswer) {
                    blank.classList.remove('incorrect');
                    blank.classList.add('correct');
                    correctCount++;
                } else {
                    blank.classList.remove('correct');
                    blank.classList.add('incorrect');
                }
            });
            
            score += correctCount;
            document.getElementById('score').textContent = score;
            
            const feedback = document.getElementById('fill-feedback');
            if (correctCount === 10) {
                feedback.textContent = '🎉 Complimenti! Hai completato perfettamente tutte le frasi!';
                feedback.className = 'feedback correct';
            } else {
                feedback.textContent = `Hai ${correctCount} su 10 risposte corrette. Continua a provare!`;
                feedback.className = 'feedback incorrect';
            }
            feedback.style.display = 'block';
        }

        function selectOption(element, value) {
            const question = element.closest('.question');
            const questionId = Array.from(document.querySelectorAll('.question')).indexOf(question);
            
            // Remove selection from all options in this question
            const options = question.querySelectorAll('.option');
            options.forEach(opt => {
                opt.classList.remove('selected');
            });
            
            // Mark the selected option
            element.classList.add('selected');
            
            // Store the selected value
            selectedOptions[questionId] = value;
        }

        function checkMultipleChoice() {
            const questions = document.querySelectorAll('#game-choice .question');
            let correctCount = 0;
            let allAnswered = true;
            
            // Correct answers for each question
            const correctAnswers = [
                'times', 'hat', 'to run', 'everywhere', 'I could',
                'also', 'airplane', 'I thought', 'dangerous', 'some'
            ];
            
            questions.forEach((question, index) => {
                if (selectedOptions[index] === undefined) {
                    allAnswered = false;
                    return;
                }
                
                const options = question.querySelectorAll('.option');
                
                if (selectedOptions[index] === correctAnswers[index]) {
                    correctCount++;
                    options.forEach(opt => {
                        if (opt.textContent === correctAnswers[index]) {
                            opt.classList.add('correct');
                        }
                    });
                } else {
                    options.forEach(opt => {
                        if (opt.textContent === correctAnswers[index]) {
                            opt.classList.add('correct');
                        }
                        if (opt.textContent === selectedOptions[index]) {
                            opt.classList.add('incorrect');
                        }
                    });
                }
            });
            
            if (!allAnswered) {
                document.getElementById('choice-feedback').textContent = 'Per favore, rispondi a tutte le domande prima di controllare!';
                document.getElementById('choice-feedback').className = 'feedback incorrect';
                document.getElementById('choice-feedback').style.display = 'block';
                return;
            }
            
            score += correctCount;
            document.getElementById('score').textContent = score;
            
            const feedback = document.getElementById('choice-feedback');
            if (correctCount === 10) {
                feedback.textContent = '🎉 Complimenti! Hai risposto correttamente a tutte le domande!';
                feedback.className = 'feedback correct';
            } else {
                feedback.textContent = `Hai ${correctCount} su 10 risposte corrette. Continua a provare!`;
                feedback.className = 'feedback incorrect';
            }
            feedback.style.display = 'block';
        }
    </script>
</body>
</html>
