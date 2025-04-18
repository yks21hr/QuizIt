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
        </div>

        <div class="leaderboard">
            <h2>Leaderboards</h2>
            <div id="commonLeaderboard"></div>
            <div id="foodLeaderboard"></div>
            <div id="sportsLeaderboard"></div>
        </div>

        <img src="https://via.placeholder.com/800x400?text=Cartoon+Quiz+Image" alt="Cartoon Quiz Image" class="cartoon-image">
    </div>

    <footer>
        <p>© 2025 QuizIt!</p>
        <a href="https://twitter.com">Twitter</a>
        <a href="https://facebook.com">Facebook</a>
    </footer>

    <script>
        let currentUser = '';
        const scores = {
            common: {},
            food: {},
            sports: {}
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
            ['common', 'food', 'sports'].forEach(category => {
                const leaderboard = document.getElementById(`${category}Leaderboard`);
                leaderboard.innerHTML = `<h3>${category.charAt(0).toUpperCase() + category.slice(1)} Quiz</h3><ul>`;
                const categoryScores = Object.entries(scores[category]).sort((a, b) => b[1] - a[1]);
                categoryScores.slice(0, 5).forEach(([name, score], index) => {
                    leaderboard.innerHTML += `<li>${index + 1}. ${name}: ${score} points</li>`;
                });
                leaderboard.innerHTML += `</ul>`;
            });
        }

        // Quiz questions
        const quizzes = {
            common: [
                { question: "What color is the sky on a clear day?", options: ["red", "blue", "green", "yellow"], answer: "blue" },
                { question: "How many days are there in a week?", options: ["5", "7", "10", "14"], answer: "7" },
                { question: "What do you use to write on a chalkboard?", options: ["pen", "chalk", "pencil", "marker"], answer: "chalk" },
                { question: "What is the primary source of energy for Earth?", options: ["moon", "sun", "stars", "wind"], answer: "sun" },
                { question: "How many legs does a spider have?", options: ["6", "8", "10", "12"], answer: "8" },
                { question: "What do plants need to grow besides water?", options: ["stone", "sunlight", "sand", "ice"], answer: "sunlight" },
                { question: "What do you call a baby cat?", options: ["puppy", "kitten", "cub", "foal"], answer: "kitten" },
                { question: "What is the capital city of France?", options: ["London", "Berlin", "Paris", "Madrid"], answer: "paris" },
                { question: "What do you use to brush your teeth?", options: ["comb", "toothbrush", "spoon", "fork"], answer: "toothbrush" },
                { question: "How many colors are there in a rainbow?", options: ["5", "6", "7", "8"], answer: "7" }
            ],
            food: [
                { question: "What fruit is known as the 'king of fruits'?", options: ["apple", "banana", "durian", "mango"], answer: "durian" },
                { question: "Which spice is made from saffron crocus?", options: ["cinnamon", "saffron", "turmeric", "paprika"], answer: "saffron" },
                { question: "What is the main ingredient in guacamole?", options: ["tomato", "avocado", "onion", "pepper"], answer: "avocado" },
                { question: "Which country is famous for sushi?", options: ["China", "Japan", "Thailand", "Korea"], answer: "japan" },
                { question: "What is the primary ingredient in bread?", options: ["rice", "flour", "corn", "potato"], answer: "flour" },
                { question: "Which drink is made from fermented grapes?", options: ["beer", "wine", "whiskey", "vodka"], answer: "wine" },
                { question: "What cheese is used in traditional lasagna?", options: ["cheddar", "mozzarella", "parmesan", "brie"], answer: "mozzarella" },
                { question: "Which fruit is known for its spiky exterior?", options: ["pineapple", "orange", "grape", "pear"], answer: "pineapple" },
                { question: "What is the main ingredient in hummus?", options: ["lentils", "chickpeas", "beans", "peas"], answer: "chickpeas" },
                { question: "Which country is famous for pizza?", options: ["Spain", "Italy", "France", "Greece"], answer: "italy" }
            ],
            sports: [
                { question: "How many players are there in a soccer team on the field?", options: ["9", "10", "11", "12"], answer: "11" },
                { question: "Which sport uses a shuttlecock?", options: ["tennis", "badminton", "squash", "table tennis"], answer: "badminton" },
                { question: "How many holes are there in a standard golf course?", options: ["9", "12", "18", "20"], answer: "18" },
                { question: "In which sport is a 'home run' scored?", options: ["cricket", "baseball", "rugby", "hockey"], answer: "baseball" },
                { question: "How many players are on a basketball team on the court?", options: ["4", "5", "6", "7"], answer: "5" },
                { question: "Which country hosted the 2016 Summer Olympics?", options: ["China", "Brazil", "USA", "UK"], answer: "brazil" },
                { question: "What is the maximum score in a single frame of bowling?", options: ["100", "200", "250", "300"], answer: "300" },
                { question: "Which sport is played at Wimbledon?", options: ["cricket", "tennis", "rugby", "golf"], answer: "tennis" },
                { question: "How long is an Olympic swimming pool (in meters)?", options: ["25", "50", "75", "100"], answer: "50" },
                { question: "In which sport do you perform a 'slam dunk'?", options: ["volleyball", "basketball", "netball", "handball"], answer: "basketball" }
            ]
        };

        function startQuiz(category) {
            currentUser = prompt("Please enter your name:");
            if (!currentUser) {
                alert("Please enter a name to start the quiz!");
                return;
            }
            alert(`Instructions: Each correct answer gives you 10 points. Each wrong answer deducts 3 points. Let's begin the ${category.charAt(0).toUpperCase() + category.slice(1)} Quiz!`);
            askQuestion(category, 0);
        }

        function askQuestion(category, index) {
            if (index >= quizzes[category].length) {
                if (currentUser in scores[category]) {
                    scores[category][currentUser] = Math.max(scores[category][currentUser] || 0, score);
                } else {
                    scores[category][currentUser] = score;
                }
                saveScores();
                alert(`Quiz over, ${currentUser}! Your final score is ${score} points.`);
                displayLeaderboard(category);
                return;
            }

            const q = quizzes[category][index];
            let options = q.options.join(", ");
            let userChoice = prompt(`${q.question}\nOptions: ${options}`);
            if (userChoice) {
                userChoice = userChoice.toLowerCase().trim();
                if (userChoice === q.answer) {
                    score += 10;
                    alert(`Correct! +10 points. Your score is now ${score}.`);
                } else {
                    score -= 3;
                    alert(`Wrong! -3 points. The correct answer was ${q.answer}. Your score is now ${score}.`);
                }
            } else {
                alert("Please select an option!");
                askQuestion(category, index);
                return;
            }
            askQuestion(category, index + 1);
        }

        function displayLeaderboard(category) {
            let message = `Leaderboard for ${category.charAt(0).toUpperCase() + category.slice(1)} Quiz:\n`;
            const categoryScores = Object.entries(scores[category]).sort((a, b) => b[1] - a[1]);
            categoryScores.slice(0, 5).forEach(([name, score], index) => {
                message += `${index + 1}. ${name}: ${score} points\n`;
            });
            alert(message);
        }

        // Initialize scores and leaderboards
        let score = 0;
        loadScores();
    </script>
</body>
</html>
