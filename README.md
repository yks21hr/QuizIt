<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="Content-Security-Policy" content="default-src 'self'; script-src 'self'; style-src 'self' 'unsafe-inline';">
    <title>QuizIt!</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            margin: 0;
            padding: 0;
            background-color: #1a2a44;
            color: white;
            min-height: 100vh;
            display: flex;
            flex-direction: column;
            justify-content: space-between;
        }
        header {
            padding: 20px;
        }
        h1 {
            font-size: 4em;
            margin: 0;
            text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.5);
        }
        .buttons {
            margin: 20px 0;
        }
        .buttons button {
            background-color: #fff;
            color: #1a2a44;
            border: none;
            padding: 10px 20px;
            margin: 5px;
            border-radius: 5px;
            cursor: pointer;
            font-size: 1em;
        }
        .buttons button:hover {
            background-color: #ddd;
        }
        .cartoon-image {
            max-width: 80%;
            height: auto;
            margin-bottom: 20px;
        }
        .container {
            flex: 1 0 auto;
            display: flex;
            flex-direction: column;
            align-items: center;
        }
        .leaderboard {
            margin: 20px 0;
            max-width: 80%;
        }
        .leaderboard h2 {
            margin-bottom: 10px;
        }
        .leaderboard ul {
            list-style: none;
            padding: 0;
        }
        .leaderboard li {
            background-color: #fff;
            color: #1a2a44;
            padding: 5px 10px;
            margin: 5px 0;
            border-radius: 5px;
        }
        footer {
            padding: 20px;
            background-color: #007BFF;
            color: white;
            width: 100%;
            text-align: center;
            flex-shrink: 0;
        }
        footer a {
            color: white;
            margin: 0 10px;
            text-decoration: none;
        }
        footer a:hover {
            text-decoration: underline;
        }
        /* Quiz Modal Styles */
        .quiz-modal {
            display: none;
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background-color: rgba(0, 0, 0, 0.5);
            justify-content: center;
            align-items: center;
        }
        .quiz-content {
            background-color: #1a2a44;
            padding: 20px;
            border-radius: 5px;
            width: 80%;
            max-width: 500px;
            text-align: left;
        }
        .quiz-content h3 {
            margin-top: 0;
        }
        .quiz-content label {
            display: block;
            margin: 10px 0;
        }
        .quiz-content button {
            background-color: #007BFF;
            color: white;
            border: none;
            padding: 10px 20px;
            margin-top: 10px;
            border-radius: 5px;
            cursor: pointer;
        }
        .quiz-content button:hover {
            background-color: #0056b3;
        }
    </style>
</head>
<body>
    <header>
        <h1>QuizIt!</h1>
    </header>

    <div class="container">
        <div class="buttons">
            <button onclick="startQuiz('common')">Common Sense Quiz</button>
            <button onclick="startQuiz('food')">Challenge The Foodie in You</button>
            <button onclick="startQuiz('sports')">Quench Your Sports Thirst</button>
            <button onclick="startQuiz('wanderlust')">Wanderlust Quiz</button>
            <button onclick="startQuiz('movies')">Movies Quiz</button>
            <button onclick="startQuiz('science')">Science & Technology Quiz</button>
            <button onclick="startQuiz('indianHistory')">Indian Historical Places & Cities</button>
            <button onclick="startQuiz('animals')">Animal Kingdom Quiz</button>
            <button onclick="startQuiz('indianCompanies')">Indian Companies Quiz</button>
            <button onclick="startQuiz('stockMarket')">Indian Stock Market Quiz</button>
            <button onclick="startQuiz('cricket')">Cricket Quiz</button>
            <button onclick="startQuiz('football')">Football Quiz</button>
            <button onclick="startQuiz('cbse')">Class 10 CBSE Quiz</button>
        </div>

        <div class="leaderboard">
            <h2>Leaderboards</h2>
            <div id="commonLeaderboard"></div>
            <div id="foodLeaderboard"></div>
            <div id="sportsLeaderboard"></div>
            <div id="wanderlustLeaderboard"></div>
            <div id="moviesLeaderboard"></div>
            <div id="scienceLeaderboard"></div>
            <div id="indianHistoryLeaderboard"></div>
            <div id="animalsLeaderboard"></div>
            <div id="indianCompaniesLeaderboard"></div>
            <div id="stockMarketLeaderboard"></div>
            <div id="cricketLeaderboard"></div>
            <div id="footballLeaderboard"></div>
            <div id="cbseLeaderboard"></div>
        </div>

        <img src="https://via.placeholder.com/800x400?text=Cartoon+Quiz+Image" alt="Cartoon Quiz Image" class="cartoon-image">

        <!-- Quiz Modal -->
        <div id="quizModal" class="quiz-modal">
            <div class="quiz-content">
                <h3 id="quizQuestion"></h3>
                <form id="quizForm">
                    <label><input type="radio" name="option" value="0"> A) <span id="optionA"></span></label>
                    <label><input type="radio" name="option" value="1"> B) <span id="optionB"></span></label>
                    <label><input type="radio" name="option" value="2"> C) <span id="optionC"></span></label>
                    <label><input type="radio" name="option" value="3"> D) <span id="optionD"></span></label>
                    <button type="submit">Submit</button>
                </form>
            </div>
        </div>
    </div>

    <footer>
        <p>© 2025 QuizIt!</p>
        <a href="https://twitter.com">Twitter</a>
        <a href="https://facebook.com">Facebook</a>
    </footer>

    <script>
        let currentUser = '';
        let currentQuestionIndex = 0;
        let currentQuestions = [];
        let currentCategory = '';
        let score = 0;
        const scores = {
            common: {},
            food: {},
            sports: {},
            wanderlust: {},
            movies: {},
            science: {},
            indianHistory: {},
            animals: {},
            indianCompanies: {},
            stockMarket: {},
            cricket: {},
            football: {},
            cbse: {}
        };

        // Load scores from localStorage
        function loadScores() {
            const savedScores = localStorage.getItem('quizScores');
            if (savedScores) {
                Object.assign(scores, JSON.parse(savedScores));
            }
            updateLeaderboards();
        }

        // Save scores to localStorage
        function saveScores() {
            localStorage.setItem('quizScores', JSON.stringify(scores));
            updateLeaderboards();
        }

        // Update leaderboards display
        function updateLeaderboards() {
            Object.keys(scores).forEach(category => {
                const leaderboard = document.getElementById(`${category}Leaderboard`);
                leaderboard.innerHTML = `<h3>${category.charAt(0).toUpperCase() + category.slice(1).replace(/([A-Z])/g, ' $1').trim()} Quiz</h3><ul>`;
                const categoryScores = Object.entries(scores[category]).sort((a, b) => b[1] - a[1]);
                categoryScores.slice(0, 5).forEach(([name, score], index) => {
                    leaderboard.innerHTML += `<li>${index + 1}. ${name}: ${score} points</li>`;
                });
                leaderboard.innerHTML += `</ul>`;
            });
        }

        // Shuffle array function
        function shuffle(array) {
            for (let i = array.length - 1; i > 0; i--) {
                const j = Math.floor(Math.random() * (i + 1));
                [array[i], array[j]] = [array[j], array[i]];
            }
            return array;
        }

        // Quiz questions (100+ per category with placeholders)
        const quizzes = {
            common: [
                { question: "What color is the sky on a clear day?", options: ["red", "blue", "green", "yellow"], answer: "blue" },
                { question: "How many days are there in a week?", options: ["5", "7", "10", "14"], answer: "7" }
            ].concat(Array(98).fill().map(() => ({ question: "Placeholder common question", options: ["a", "b", "c", "d"], answer: "a" }))),
            food: [
                { question: "What fruit is known as the 'king of fruits'?", options: ["apple", "banana", "durian", "mango"], answer: "durian" },
                { question: "Which spice is made from saffron crocus?", options: ["cinnamon", "saffron", "turmeric", "paprika"], answer: "saffron" }
            ].concat(Array(98).fill().map(() => ({ question: "Placeholder food question", options: ["a", "b", "c", "d"], answer: "a" }))),
            sports: [
                { question: "How many players are there in a soccer team on the field?", options: ["9", "10", "11", "12"], answer: "11" },
                { question: "Which sport uses a shuttlecock?", options: ["tennis", "badminton", "squash", "table tennis"], answer: "badminton" }
            ].concat(Array(98).fill().map(() => ({ question: "Placeholder sports question", options: ["a", "b", "c", "d"], answer: "a" }))),
            wanderlust: [
                { question: "Which country is known for the Eiffel Tower?", options: ["Italy", "France", "Spain", "Germany"], answer: "france" },
                { question: "What is the capital of Brazil?", options: ["Rio", "Sao Paulo", "Brasilia", "Salvador"], answer: "brasilia" }
            ].concat(Array(98).fill().map(() => ({ question: "Placeholder wanderlust question", options: ["a", "b", "c", "d"], answer: "a" }))),
            movies: [
                { question: "Who directed 'Inception'?", options: ["Spielberg", "Nolan", "Tarantino", "Cameron"], answer: "nolan" },
                { question: "What year was 'Titanic' released?", options: ["1995", "1997", "1999", "2001"], answer: "1997" }
            ].concat(Array(98).fill().map(() => ({ question: "Placeholder movie question", options: ["a", "b", "c", "d"], answer: "a" }))),
            science: [
                { question: "What is the chemical symbol for water?", options: ["O2", "H2O", "CO2", "N2"], answer: "h2o" },
                { question: "Which planet is known as the Red Planet?", options: ["Venus", "Mars", "Jupiter", "Saturn"], answer: "mars" }
            ].concat(Array(98).fill().map(() => ({ question: "Placeholder science question", options: ["a", "b", "c", "d"], answer: "a" }))),
            indianHistory: [
                { question: "Which empire built the Taj Mahal?", options: ["Maurya", "Mughal", "Gupta", "Chola"], answer: "mughal" },
                { question: "Where is the Red Fort located?", options: ["Delhi", "Agra", "Jaipur", "Mumbai"], answer: "delhi" }
            ].concat(Array(98).fill().map(() => ({ question: "Placeholder Indian history question", options: ["a", "b", "c", "d"], answer: "a" }))),
            animals: [
                { question: "What is the largest land animal?", options: ["Elephant", "Giraffe", "Hippopotamus", "Rhinoceros"], answer: "elephant" },
                { question: "Which bird is known for its black and white stripes?", options: ["Penguin", "Zebra Finch", "Ostrich", "Eagle"], answer: "penguin" }
            ].concat(Array(98).fill().map(() => ({ question: "Placeholder animal question", options: ["a", "b", "c", "d"], answer: "a" }))),
            indianCompanies: [
                { question: "Which company is known for the brand 'Amul'?", options: ["HUL", "Amul", "Nestle", "Dabur"], answer: "amul" },
                { question: "What is the parent company of Flipkart?", options: ["Amazon", "Walmart", "Reliance", "Tata"], answer: "walmart" }
            ].concat(Array(98).fill().map(() => ({ question: "Placeholder Indian company question", options: ["a", "b", "c", "d"], answer: "a" }))),
            stockMarket: [
                { question: "Which index tracks the Indian stock market?", options: ["S&P 500", "NIFTY 50", "Dow Jones", "FTSE"], answer: "nifty 50" },
                { question: "What is the main stock exchange in India?", options: ["NYSE", "BSE", "NASDAQ", "LSE"], answer: "bse" }
            ].concat(Array(98).fill().map(() => ({ question: "Placeholder stock market question", options: ["a", "b", "c", "d"], answer: "a" }))),
            cricket: [
                { question: "Who holds the record for most runs in Test cricket?", options: ["Sachin Tendulkar", "Ricky Ponting", "Brian Lara", "Jacques Kallis"], answer: "sachin tendulkar" },
                { question: "Which country won the 2011 Cricket World Cup?", options: ["Australia", "India", "Sri Lanka", "England"], answer: "india" }
            ].concat(Array(98).fill().map(() => ({ question: "Placeholder cricket question", options: ["a", "b", "c", "d"], answer: "a" }))),
            football: [
                { question: "Which country won the FIFA World Cup in 2018?", options: ["Brazil", "Germany", "France", "Argentina"], answer: "france" },
                { question: "Who is known as the 'King of Football'?", options: ["Messi", "Pele", "Maradona", "Ronaldo"], answer: "pele" }
            ].concat(Array(98).fill().map(() => ({ question: "Placeholder football question", options: ["a", "b", "c", "d"], answer: "a" }))),
            cbse: [
                { question: "What is the chemical formula for carbon dioxide?", options: ["CO", "CO2", "CH4", "O2"], answer: "co2" },
                { question: "Who wrote the Indian National Anthem?", options: ["Tagore", "Nehru", "Gandhi", "Ambedkar"], answer: "tagore" }
            ].concat(Array(98).fill().map(() => ({ question: "Placeholder CBSE question", options: ["a", "b", "c", "d"], answer: "a" })))
        };

        function startQuiz(category) {
            let name = prompt("Please enter your name:");
            while (!name || name.trim() === "") {
                name = prompt("Please enter a valid name to start the quiz!");
            }
            currentUser = name.trim();
            currentCategory = category;
            score = 0;
            let shuffledQuestions = [...quizzes[category]];
            shuffle(shuffledQuestions);
            currentQuestions = shuffledQuestions.slice(0, 10);
            console.log('Current Questions:', currentQuestions); // Debug log
            alert(`Instructions: Each correct answer gives you 10 points. Each wrong answer deducts 3 points. Let's begin the ${category.charAt(0).toUpperCase() + category.slice(1).replace(/([A-Z])/g, ' $1').trim()} Quiz!`);
            showQuestion();
        }

        function showQuestion() {
            const modal = document.getElementById('quizModal');
            const questionElement = document.getElementById('quizQuestion');
            const optionA = document.getElementById('optionA');
            const optionB = document.getElementById('optionB');
            const optionC = document.getElementById('optionC');
            const optionD = document.getElementById('optionD');
            const form = document.getElementById('quizForm');

            if (currentQuestionIndex >= currentQuestions.length) {
                if (currentUser in scores[currentCategory]) {
                    scores[currentCategory][currentUser] = Math.max(scores[currentCategory][currentUser] || 0, score);
                } else {
                    scores[currentCategory][currentUser] = score;
                }
                saveScores();
                alert(`Quiz over, ${currentUser}! Your final score is ${score} points.`);
                displayLeaderboard(currentCategory);
                modal.style.display = 'flex';
                currentQuestionIndex = 0; // Reset for next quiz
                return;
            }

            const q = currentQuestions[currentQuestionIndex];
            if (!q) {
                console.error('No question available at index:', currentQuestionIndex);
                return;
            }
            console.log('Current Question:', q); // Debug log
            questionElement.textContent = `Q: ${q.question || 'No question loaded'}`;
            optionA.textContent = q.options[0] || 'No option';
            optionB.textContent = q.options[1] || 'No option';
            optionC.textContent = q.options[2] || 'No option';
            optionD.textContent = q.options[3] || 'No option';

            form.onsubmit = (e) => {
                e.preventDefault();
                const selectedOption = document.querySelector('input[name="option"]:checked');
                if (selectedOption) {
                    const choiceIndex = parseInt(selectedOption.value);
                    if (q.options[choiceIndex].toLowerCase() === q.answer) {
                        score += 10;
                        alert(`Correct! +10 points. Your score is now ${score}.`);
                    } else {
                        score -= 3;
                        alert(`Wrong! -3 points. The correct answer was ${q.answer}. Your score is now ${score}.`);
                    }
                    currentQuestionIndex++;
                    showQuestion();
                } else {
                    alert("Please select an option!");
                }
            };
            modal.style.display = 'flex';
            form.reset(); // Reset radio buttons
        }

        function displayLeaderboard(category) {
            let message = `Leaderboard for ${category.charAt(0).toUpperCase() + category.slice(1).replace(/([A-Z])/g, ' $1').trim()} Quiz:\n`;
            const categoryScores = Object.entries(scores[category]).sort((a, b) => b[1] - a[1]);
            categoryScores.slice(0, 5).forEach(([name, score], index) => {
                message += `${index + 1}. ${name}: ${score} points\n`;
            });
            alert(message);
        }

        // Initialize scores and leaderboards
        loadScores();
    </script>
</body>
</html>
