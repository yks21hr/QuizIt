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
            min-height: 100vh; /* Ensure body takes full viewport height */
            display: flex;
            flex-direction: column;
            justify-content: space-between; /* Push footer to bottom */
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
            flex: 1 0 auto; /* Allow container to grow but not shrink beyond content */
            display: flex;
            flex-direction: column;
            align-items: center;
        }
        footer {
            padding: 20px;
            background-color: #007BFF;
            color: white;
            width: 100%;
            text-align: center;
            flex-shrink: 0; /* Prevent footer from shrinking */
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
            <button onclick="startCommonSenseQuiz()">Common Sense Quiz</button>
            <button>Challenge The Foodie in You</button>
            <button>Quench Your Sports Thirst</button>
        </div>

        <img src="https://via.placeholder.com/800x400?text=Cartoon+Quiz+Image" alt="Cartoon Quiz Image" class="cartoon-image">
    </div>

    <footer>
        <p>© 2025 QuizIt!</p>
        <a href="https://twitter.com">Twitter</a>
        <a href="https://facebook.com">Facebook</a>
    </footer>

    <script>
        let score = 0;
        const questions = [
            {
                question: "What color is the sky on a clear day?",
                answer: "blue"
            },
            {
                question: "How many days are there in a week?",
                answer: "7"
            },
            {
                question: "What do you use to write on a chalkboard?",
                answer: "chalk"
            },
            {
                question: "What is the primary source of energy for Earth?",
                answer: "sun"
            },
            {
                question: "How many legs does a spider have?",
                answer: "8"
            },
            {
                question: "What do plants need to grow besides water?",
                answer: "sunlight"
            },
            {
                question: "What do you call a baby cat?",
                answer: "kitten"
            },
            {
                question: "What is the capital city of France?",
                answer: "paris"
            },
            {
                question: "What do you use to brush your teeth?",
                answer: "toothbrush"
            },
            {
                question: "How many colors are there in a rainbow?",
                answer: "7"
            }
        ];

        function startCommonSenseQuiz() {
            alert("Instructions: Each correct answer gives you 10 points. Each wrong answer deducts 3 points. Let's begin!");
            score = 0;
            askQuestion(0);
        }

        function askQuestion(index) {
            if (index >= questions.length) {
                alert(`Quiz over! Your final score is ${score} points.`);
                return;
            }

            const userAnswer = prompt(questions[index].question).toLowerCase().trim();
            if (userAnswer === questions[index].answer) {
                score += 10;
                alert(`Correct! +10 points. Your score is now ${score}.`);
            } else {
                score -= 3;
                alert(`Wrong! -3 points. The correct answer was ${questions[index].answer}. Your score is now ${score}.`);
            }
            askQuestion(index + 1);
        }
    </script>
</body>
</html>
