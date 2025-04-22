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
            <button onclick="startQuiz('animalKingdom')">Animal Kingdom Quiz</button>
            <button onclick="startQuiz('indianCinema')">Indian Cinema Quiz</button>
            <button onclick="startQuiz('foodTravel')">Food and Travel Quiz</button>
            <button onclick="startQuiz('indianCompanies')">Indian Companies Quiz</button>
            <button onclick="startQuiz('stockMarket')">Stock Market Quiz</button>
            <button onclick="startQuiz('scienceTech')">Science and Technology Quiz</button>
        </div>

        <div class="leaderboard">
            <h2>Leaderboards</h2>
            <div id="commonLeaderboard"></div>
            <div id="foodLeaderboard"></div>
            <div id="sportsLeaderboard"></div>
            <div id="animalKingdomLeaderboard"></div>
            <div id="indianCinemaLeaderboard"></div>
            <div id="foodTravelLeaderboard"></div>
            <div id="indianCompaniesLeaderboard"></div>
            <div id="stockMarketLeaderboard"></div>
            <div id="scienceTechLeaderboard"></div>
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
        let currentCategory = '';
        let score = 0;
        const scores = {
            common: {},
            food: {},
            sports: {},
            animalKingdom: {},
            indianCinema: {},
            foodTravel: {},
            indianCompanies: {},
            stockMarket: {},
            scienceTech: {}
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
            ['common', 'food', 'sports', 'animalKingdom', 'indianCinema', 'foodTravel', 'indianCompanies', 'stockMarket', 'scienceTech'].forEach(category => {
                const leaderboard = document.getElementById(`${category}Leaderboard`);
                leaderboard.innerHTML = `<h3>${category.charAt(0).toUpperCase() + category.slice(1).replace(/([A-Z])/g, ' $1').trim()} Quiz</h3><ul>`;
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
            ],
            animalKingdom: [
                { question: "Which animal is known as the 'king of the jungle'?", options: ["lion", "tiger", "elephant", "bear"], answer: "lion" },
                { question: "What is the largest land animal?", options: ["elephant", "giraffe", "hippopotamus", "rhinoceros"], answer: "elephant" },
                { question: "Which bird is known for its black and white stripes?", options: ["penguin", "zebra finch", "ostrich", "eagle"], answer: "penguin" },
                { question: "What is the fastest land animal?", options: ["cheetah", "lion", "horse", "deer"], answer: "cheetah" },
                { question: "Which animal has a body covered with scales?", options: ["snake", "dog", "cat", "rabbit"], answer: "snake" },
                { question: "What is the largest species of shark?", options: ["great white", "whale shark", "tiger shark", "hammerhead"], answer: "whale shark" },
                { question: "Which animal is known for its black and white fur and lives in China?", options: ["panda", "zebra", "skunk", "lemur"], answer: "panda" },
                { question: "What type of animal is a jellyfish?", options: ["cnidarian", "mammal", "reptile", "amphibian"], answer: "cnidarian" },
                { question: "Which animal is famous for its long neck?", options: ["giraffe", "camel", "llama", "ostrich"], answer: "giraffe" },
                { question: "What is the only mammal that can fly?", options: ["bat", "bird", "flying squirrel", "pterodactyl"], answer: "bat" }
            ],
            indianCinema: [
                { question: "Which year was the first Indian feature film released?", options: ["1913", "1920", "1901", "1930"], answer: "1913" },
                { question: "Who directed the film 'Sholay'?", options: ["Raj Kapoor", "Ramesh Sippy", "Yash Chopra", "Satyajit Ray"], answer: "ramesh sippy" },
                { question: "Which actor is known as the 'Shahenshah of Bollywood'?", options: ["Amitabh Bachchan", "Shah Rukh Khan", "Aamir Khan", "Salman Khan"], answer: "amitabh bachchan" },
                { question: "What was the first Indian film to win an Oscar?", options: ["Mother India", "Salaam Bombay!", "Lagaan", "Slumdog Millionaire"], answer: "slumdog millionaire" },
                { question: "Which Indian filmmaker is famous for the 'Apu Trilogy'?", options: ["Satyajit Ray", "Mrinal Sen", "Ritwik Ghatak", "Guru Dutt"], answer: "satyajit ray" },
                { question: "Which film introduced the song 'Chaiyya Chaiyya'?", options: ["Dil Se", "Dilwale Dulhania Le Jayenge", "Kuch Kuch Hota Hai", "Kabhi Khushi Kabhie Gham"], answer: "dil se" },
                { question: "Who played the lead role in 'Mughal-e-Azam'?", options: ["Dilip Kumar", "Rajesh Khanna", "Dev Anand", "Shammi Kapoor"], answer: "dilip kumar" },
                { question: "Which actress debuted in Bollywood with 'Om Shanti Om'?", options: ["Priyanka Chopra", "Deepika Padukone", "Katrina Kaif", "Aishwarya Rai"], answer: "deepika padukone" },
                { question: "Which film features the character 'Devdas'?", options: ["Devdas", "Parineeta", "Umrao Jaan", "Pakeezah"], answer: "devdas" },
                { question: "Who composed the music for 'Roja'?", options: ["A.R. Rahman", "Ilaiyaraaja", "Shankar-Ehsaan-Loy", "Vishal-Shekhar"], answer: "a.r. rahman" }
            ],
            foodTravel: [
                { question: "Which country is famous for paella?", options: ["Italy", "Spain", "France", "Greece"], answer: "spain" },
                { question: "What is the capital city of Thailand?", options: ["Bangkok", "Phuket", "Chiang Mai", "Pattaya"], answer: "bangkok" },
                { question: "Which spice is a key ingredient in curry?", options: ["saffron", "turmeric", "oregano", "basil"], answer: "turmeric" },
                { question: "What is the famous landmark in Paris?", options: ["Eiffel Tower", "Big Ben", "Colosseum", "Statue of Liberty"], answer: "eiffel tower" },
                { question: "Which cuisine is known for sushi?", options: ["Chinese", "Japanese", "Korean", "Thai"], answer: "japanese" },
                { question: "What desert country is known for its pyramids?", options: ["Mexico", "Egypt", "Peru", "India"], answer: "egypt" },
                { question: "Which fruit is a staple in Hawaiian poke?", options: ["pineapple", "mango", "banana", "avocado"], answer: "avocado" },
                { question: "What is the largest city in Australia?", options: ["Sydney", "Melbourne", "Perth", "Brisbane"], answer: "sydney" },
                { question: "Which dish is made from fermented cabbage?", options: ["sauerkraut", "kimchi", "pickles", "relish"], answer: "kimchi" },
                { question: "What Italian city is famous for its canals?", options: ["Rome", "Venice", "Florence", "Milan"], answer: "venice" }
            ],
            indianCompanies: [
                { question: "Which company is known for the brand 'Amul'?", options: ["HUL", "Amul", "Nestle", "Dabur"], answer: "amul" },
                { question: "What is the parent company of Flipkart?", options: ["Amazon", "Walmart", "Reliance", "Tata"], answer: "walmart" },
                { question: "Which Indian company is a major IT service provider?", options: ["TCS", "HDFC", "Reliance", "Infosys"], answer: "tcs" },
                { question: "What company produces the 'Fair & Lovely' brand?", options: ["HUL", "P&G", "L'Oréal", "Godrej"], answer: "hul" },
                { question: "Which company is the largest steel producer in India?", options: ["Tata Steel", "JSW Steel", "SAIL", "Jindal Steel"], answer: "tata steel" },
                { question: "What company owns the 'Parle-G' biscuit brand?", options: ["Britannia", "Parle", "ITC", "Nestle"], answer: "parle" },
                { question: "Which Indian company is known for its luxury hotels?", options: ["Oberoi", "Taj", "Leela", "ITC"], answer: "taj" },
                { question: "What company manufactures the 'Hero' bicycles?", options: ["Hero Cycles", "Bajaj", "TVS", "Honda"], answer: "hero cycles" },
                { question: "Which company is a leader in Indian two-wheeler manufacturing?", options: ["Hero MotoCorp", "Bajaj Auto", "TVS", "Royal Enfield"], answer: "hero motocorp" },
                { question: "What company is known for the 'Dettol' brand?", options: ["Reckitt Benckiser", "HUL", "P&G", "Colgate"], answer: "reckitt benckiser" }
            ],
            stockMarket: [
                { question: "Which index tracks the Indian stock market?", options: ["S&P 500", "NIFTY 50", "Dow Jones", "FTSE"], answer: "nifty 50" },
                { question: "What is the main stock exchange in India?", options: ["NYSE", "BSE", "NASDAQ", "LSE"], answer: "bse" },
                { question: "What does IPO stand for?", options: ["Initial Public Offering", "International Portfolio Organization", "Investment Profit Option", "Internal Private Order"], answer: "initial public offering" },
                { question: "Which company was the first to list on NSE?", options: ["Reliance", "TCS", "Infosys", "Larsen & Toubro"], answer: "tcs" },
                { question: "What is the minimum age to trade in the stock market in India?", options: ["16", "18", "21", "25"], answer: "18" },
                { question: "Which regulator oversees the Indian stock market?", options: ["RBI", "SEBI", "IRDA", "NSE"], answer: "sebi" },
                { question: "What is a stock split intended to do?", options: ["Increase share value", "Reduce share price", "Eliminate dividends", "Raise capital"], answer: "reduce share price" },
                { question: "Which city hosts the Bombay Stock Exchange?", options: ["Delhi", "Mumbai", "Chennai", "Kolkata"], answer: "mumbai" },
                { question: "What is the term for buying and selling stocks within the same day?", options: ["Long-term trading", "Day trading", "Swing trading", "Position trading"], answer: "day trading" },
                { question: "Which Indian company has the highest market capitalization as of 2025?", options: ["Reliance Industries", "Tata Group", "HDFC Bank", "Infosys"], answer: "reliance industries" }
            ],
            scienceTech: [
                { question: "What gas makes up most of Earth's atmosphere?", options: ["oxygen", "nitrogen", "carbon dioxide", "hydrogen"], answer: "nitrogen" },
                { question: "What is the primary source of energy for the solar system?", options: ["moon", "sun", "earth core", "stars"], answer: "sun" },
                { question: "Which element has the atomic number 1?", options: ["helium", "hydrogen", "oxygen", "carbon"], answer: "hydrogen" },
                { question: "What technology is used in touchscreens?", options: ["LCD", "Capacitive", "LED", "Plasma"], answer: "capacitive" },
                { question: "Which planet is known as the Red Planet?", options: ["Venus", "Mars", "Jupiter", "Saturn"], answer: "mars" },
                { question: "What is the main component of a CPU?", options: ["RAM", "Motherboard", "Processor", "Hard Drive"], answer: "processor" },
                { question: "Which gas is most responsible for the greenhouse effect?", options: ["oxygen", "carbon dioxide", "nitrogen", "argon"], answer: "carbon dioxide" },
                { question: "What is the unit of electrical resistance?", options: ["volt", "ohm", "ampere", "watt"], answer: "ohm" },
                { question: "Which invention is credited to Alexander Graham Bell?", options: ["telephone", "light bulb", "radio", "television"], answer: "telephone" },
                { question: "What is the most abundant element in the universe?", options: ["oxygen", "hydrogen", "helium", "carbon"], answer: "hydrogen" }
            ]
        };

        function startQuiz(category) {
            currentUser = prompt("Please enter your name:");
            if (!currentUser || currentUser.trim() === "") {
                alert("Quiz cancelled. Please enter a valid name to start.");
                return;
            }
            currentCategory = category;
            score = 0;
            currentQuestionIndex = 0;
            console.log('Starting quiz for category:', category); // Debug log
            alert(`Instructions: Each correct answer gives you 10 points. Each wrong answer deducts 3 points. Let's begin the ${category.charAt(0).toUpperCase() + category.slice(1).replace(/([A-Z])/g, ' $1').trim()} Quiz! Click Cancel to stop the quiz.`);
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

            if (currentQuestionIndex >= quizzes[currentCategory].length) {
                if (currentUser in scores[currentCategory]) {
                    scores[currentCategory][currentUser] = Math.max(scores[currentCategory][currentUser] || 0, score);
                } else {
                    scores[currentCategory][currentUser] = score;
                }
                saveScores();
                alert(`Quiz over, ${currentUser}! Your final score is ${score} points.`);
                displayLeaderboard(currentCategory);
                modal.style.display = 'none';
                return;
            }

            const q = quizzes[currentCategory][currentQuestionIndex];
            if (!q) {
                console.error('No question available at index:', currentQuestionIndex);
                modal.style.display = 'none';
                return;
            }
            console.log('Current Question:', q); // Debug log
            questionElement.textContent = `Q: ${q.question}`;
            optionA.textContent = q.options[0];
            optionB.textContent = q.options[1];
            optionC.textContent = q.options[2];
            optionD.textContent = q.options[3];

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
