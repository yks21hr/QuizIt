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
            <button onclick="startQuiz('genZ')">The GenZ Quiz</button>
            <button onclick="startQuiz('beautyBeast')">Beauty and the Beast Quiz</button>
            <button onclick="startQuiz('historicalMonuments')">Historical Monuments in India</button>
            <button onclick="startQuiz('indianCities')">Indian Cities Quiz</button>
            <button onclick="startQuiz('solveRiddle')">Solve The Riddle</button>
            <button onclick="startQuiz('birdLocations')">Where Do You Find These Birds</button>
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
            <div id="genZLeaderboard"></div>
            <div id="beautyBeastLeaderboard"></div>
            <div id="historicalMonumentsLeaderboard"></div>
            <div id="indianCitiesLeaderboard"></div>
            <div id="solveRiddleLeaderboard"></div>
            <div id="birdLocationsLeaderboard"></div>
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
        let shuffledQuestions = []; // Array to store shuffled questions for the current quiz

        const scores = {
            common: {},
            food: {},
            sports: {},
            animalKingdom: {},
            indianCinema: {},
            foodTravel: {},
            indianCompanies: {},
            stockMarket: {},
            scienceTech: {},
            genZ: {},
            beautyBeast: {},
            historicalMonuments: {},
            indianCities: {},
            solveRiddle: {},
            birdLocations: {}
        };

        // Function to shuffle an array (Fisher-Yates algorithm)
        function shuffleArray(array) {
            const shuffled = [...array]; // Create a copy to avoid modifying the original
            for (let i = shuffled.length - 1; i > 0; i--) {
                const j = Math.floor(Math.random() * (i + 1));
                [shuffled[i], shuffled[j]] = [shuffled[j], shuffled[i]];
            }
            return shuffled;
        }

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
            ['common', 'food', 'sports', 'animalKingdom', 'indianCinema', 'foodTravel', 'indianCompanies', 'stockMarket', 'scienceTech', 'genZ', 'beautyBeast', 'historicalMonuments', 'indianCities', 'solveRiddle', 'birdLocations'].forEach(category => {
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
                { question: "How many colors are there in a rainbow?", options: ["5", "6", "7", "8"], answer: "7" },
                { question: "What is the shape of a stop sign?", options: ["Circle", "Triangle", "Octagon", "Square"], answer: "octagon" },
                { question: "Which season comes after summer?", options: ["Spring", "Winter", "Autumn", "Monsoon"], answer: "autumn" },
                { question: "What do bees produce?", options: ["Milk", "Honey", "Butter", "Wax"], answer: "honey" },
                { question: "What is the largest continent?", options: ["Africa", "Asia", "Australia", "Europe"], answer: "asia" },
                { question: "What do you call a group of fish?", options: ["Flock", "School", "Herd", "Pack"], answer: "school" },
                { question: "What is the opposite of 'big'?", options: ["Large", "Small", "Tall", "Wide"], answer: "small" },
                { question: "What do you use to tell time?", options: ["Ruler", "Clock", "Compass", "Thermometer"], answer: "clock" },
                { question: "What is the smell of rain like?", options: ["Sweet", "Sour", "Bitter", "Fresh"], answer: "fresh" },
                { question: "How many fingers are on one hand?", options: ["3", "4", "5", "6"], answer: "5" },
                { question: "What do you call the place where you sleep?", options: ["Kitchen", "Bedroom", "Bathroom", "Living Room"], answer: "bedroom" }
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
                { question: "Which country is famous for pizza?", options: ["Spain", "Italy", "France", "Greece"], answer: "italy" },
                { question: "What is the main ingredient in a traditional Caesar salad?", options: ["Kale", "Romaine lettuce", "Spinach", "Arugula"], answer: "romaine lettuce" },
                { question: "Which nut is used to make pesto sauce?", options: ["Almond", "Pine nut", "Walnut", "Cashew"], answer: "pine nut" },
                { question: "What is the primary ingredient in a falafel?", options: ["Chickpeas", "Lentils", "Rice", "Potato"], answer: "chickpeas" },
                { question: "Which fruit is used to make a traditional key lime pie?", options: ["Lemon", "Lime", "Orange", "Grapefruit"], answer: "lime" },
                { question: "What type of grain is used in risotto?", options: ["Barley", "Quinoa", "Arborio rice", "Wheat"], answer: "arborio rice" },
                { question: "Which country is known for its dish 'pad thai'?", options: ["Vietnam", "Thailand", "Malaysia", "Indonesia"], answer: "thailand" },
                { question: "What is the main ingredient in a traditional borscht soup?", options: ["Potato", "Beetroot", "Cabbage", "Carrot"], answer: "beetroot" },
                { question: "Which spice gives curry its yellow color?", options: ["Cumin", "Turmeric", "Coriander", "Paprika"], answer: "turmeric" },
                { question: "What is the main ingredient in a traditional tiramisu?", options: ["Mascarpone cheese", "Ricotta cheese", "Cream cheese", "Cottage cheese"], answer: "mascarpone cheese" },
                { question: "Which country is famous for its dish 'croissant'?", options: ["Italy", "France", "Spain", "Germany"], answer: "france" }
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
                { question: "In which sport do you perform a 'slam dunk'?", options: ["volleyball", "basketball", "netball", "handball"], answer: "basketball" },
                { question: "Which sport is known as the 'king of sports'?", options: ["Football", "Cricket", "Hockey", "Athletics"], answer: "football" },
                { question: "How many sets are typically played in a professional tennis match for men?", options: ["3", "5", "7", "9"], answer: "5" },
                { question: "Which country is famous for the sport of sumo wrestling?", options: ["China", "Japan", "Korea", "Thailand"], answer: "japan" },
                { question: "What is the name of the trophy awarded in the FIFA World Cup?", options: ["Jules Rimet Trophy", "Vince Lombardi Trophy", "Stanley Cup", "Larry O'Brien Trophy"], answer: "jules rimet trophy" },
                { question: "In which sport is the term 'bogey' used?", options: ["Golf", "Tennis", "Cricket", "Rugby"], answer: "golf" },
                { question: "How many players are on a rugby union team on the field?", options: ["11", "13", "15", "17"], answer: "15" },
                { question: "Which sport involves a 'face-off'?", options: ["Ice hockey", "Basketball", "Volleyball", "Soccer"], answer: "ice hockey" },
                { question: "What is the distance of a marathon race in kilometers?", options: ["21", "42", "50", "100"], answer: "42" },
                { question: "Which sport uses a piece of equipment called a 'puck'?", options: ["Ice hockey", "Field hockey", "Lacrosse", "Curling"], answer: "ice hockey" },
                { question: "Which country won the most medals in the 2020 Tokyo Olympics?", options: ["USA", "China", "Japan", "Russia"], answer: "usa" }
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
                { question: "What is the only mammal that can fly?", options: ["bat", "bird", "flying squirrel", "pterodactyl"], answer: "bat" },
                { question: "Which animal is known for its pouch to carry its young?", options: ["Kangaroo", "Koala", "Wombat", "Tasmanian Devil"], answer: "kangaroo" },
                { question: "What is the largest living bird?", options: ["Ostrich", "Emu", "Albatross", "Condor"], answer: "ostrich" },
                { question: "Which animal is known for its ability to change color?", options: ["Chameleon", "Octopus", "Cuttlefish", "Squid"], answer: "chameleon" },
                { question: "What is the slowest animal in the world?", options: ["Sloth", "Tortoise", "Snail", "Starfish"], answer: "sloth" },
                { question: "Which animal has the longest lifespan?", options: ["Tortoise", "Whale", "Parrot", "Elephant"], answer: "tortoise" },
                { question: "Which animal is known for its distinctive black and white stripes?", options: ["Zebra", "Tiger", "Leopard", "Cheetah"], answer: "zebra" },
                { question: "What is the primary diet of a koala?", options: ["Bamboo", "Eucalyptus leaves", "Grass", "Insects"], answer: "eucalyptus leaves" },
                { question: "Which animal is the largest member of the cat family?", options: ["Lion", "Tiger", "Leopard", "Cheetah"], answer: "tiger" },
                { question: "Which marine animal is known for its eight arms?", options: ["Squid", "Octopus", "Cuttlefish", "Nautilus"], answer: "octopus" },
                { question: "Which animal is known for building dams?", options: ["Beaver", "Otter", "Muskrat", "Badger"], answer: "beaver" }
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
                { question: "Who composed the music for 'Roja'?", options: ["A.R. Rahman", "Ilaiyaraaja", "Shankar-Ehsaan-Loy", "Vishal-Shekhar"], answer: "a.r. rahman" },
                { question: "Which film is known as the longest-running film in Indian cinema?", options: ["Sholay", "Dilwale Dulhania Le Jayenge", "Mughal-e-Azam", "Mother India"], answer: "dilwale dulhania le jayenge" },
                { question: "Who directed the film 'Lagaan'?", options: ["Ashutosh Gowariker", "Sanjay Leela Bhansali", "Karan Johar", "Rajkumar Hirani"], answer: "ashutosh gowariker" },
                { question: "Which actor is known as 'Mr. Perfectionist' in Bollywood?", options: ["Aamir Khan", "Salman Khan", "Shah Rukh Khan", "Akshay Kumar"], answer: "aamir khan" },
                { question: "Which film won the National Film Award for Best Feature Film in 2020?", options: ["Chhichhore", "Gully Boy", "Article 15", "Uri: The Surgical Strike"], answer: "gully boy" },
                { question: "Which actress is known for her role in 'Queen'?", options: ["Kangana Ranaut", "Vidya Balan", "Anushka Sharma", "Priyanka Chopra"], answer: "kangana ranaut" },
                { question: "Which film is set during the partition of India?", options: ["Partition: 1947", "Veer-Zaara", "Bajirao Mastani", "Padmaavat"], answer: "partition: 1947" },
                { question: "Who directed the film '3 Idiots'?", options: ["Rajkumar Hirani", "Anurag Kashyap", "Zoya Akhtar", "Rakesh Roshan"], answer: "rajkumar hirani" },
                { question: "Which film features the song 'Tum Hi Ho'?", options: ["Aashiqui 2", "Yeh Jawaani Hai Deewani", "Ram-Leela", "Bajrangi Bhaijaan"], answer: "aashiqui 2" },
                { question: "Which actor debuted in the film 'Dhadak'?", options: ["Ishaan Khatter", "Varun Dhawan", "Sidharth Malhotra", "Arjun Kapoor"], answer: "ishaan khatter" },
                { question: "Which film is based on the life of wrestler Mahavir Singh Phogat?", options: ["Sultan", "Dangal", "Mary Kom", "Bhaag Milkha Bhaag"], answer: "dangal" }
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
                { question: "What Italian city is famous for its canals?", options: ["Rome", "Venice", "Florence", "Milan"], answer: "venice" },
                { question: "Which country is known for its dish 'tacos'?", options: ["Spain", "Mexico", "Argentina", "Brazil"], answer: "mexico" },
                { question: "What is the capital city of Brazil?", options: ["Rio de Janeiro", "Sao Paulo", "Brasilia", "Salvador"], answer: "brasilia" },
                { question: "Which dish is a staple in Indian cuisine?", options: ["Sushi", "Biryani", "Pasta", "Falafel"], answer: "biryani" },
                { question: "Which country is famous for its Oktoberfest?", options: ["Germany", "Austria", "Switzerland", "Belgium"], answer: "germany" },
                { question: "What is the main ingredient in a traditional Greek souvlaki?", options: ["Chicken", "Pork", "Lamb", "Beef"], answer: "pork" },
                { question: "Which city is known for its Machu Picchu ruins?", options: ["Lima", "Cusco", "Arequipa", "Trujillo"], answer: "cusco" },
                { question: "Which country is famous for its dish 'pho'?", options: ["Vietnam", "Thailand", "Cambodia", "Laos"], answer: "vietnam" },
                { question: "What is the capital city of Morocco?", options: ["Casablanca", "Marrakesh", "Rabat", "Fes"], answer: "rabat" },
                { question: "Which fruit is a key ingredient in a traditional Cuban mojito?", options: ["Lemon", "Lime", "Orange", "Grapefruit"], answer: "lime" },
                { question: "Which country is known for its dish 'feijoada'?", options: ["Portugal", "Brazil", "Mexico", "Argentina"], answer: "brazil" }
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
                { question: "What company is known for the 'Dettol' brand?", options: ["Reckitt Benckiser", "HUL", "P&G", "Colgate"], answer: "reckitt benckiser" },
                { question: "Which company is known for its 'Patanjali' brand?", options: ["Dabur", "Patanjali Ayurved", "Himalaya", "Godrej"], answer: "patanjali ayurved" },
                { question: "Which Indian company manufactures 'Maruti' cars?", options: ["Tata Motors", "Maruti Suzuki", "Mahindra", "Hyundai"], answer: "maruti suzuki" },
                { question: "Which company is known for its 'Airtel' telecom services?", options: ["Reliance Jio", "Vodafone Idea", "Bharti Airtel", "BSNL"], answer: "bharti airtel" },
                { question: "Which company produces the 'Haldiram's' snack brand?", options: ["ITC", "Haldiram's", "Bikanervala", "Nestle"], answer: "haldiram's" },
                { question: "Which Indian company is a major player in the cement industry?", options: ["Ambuja Cement", "Tata Chemicals", "Reliance Cement", "JSW Cement"], answer: "ambuja cement" },
                { question: "Which company is known for its 'Asian Paints' brand?", options: ["Berger Paints", "Asian Paints", "Nerolac", "Shalimar Paints"], answer: "asian paints" },
                { question: "Which company is a leading pharmaceutical company in India?", options: ["Sun Pharma", "Cipla", "Dr. Reddy's", "Lupin"], answer: "sun pharma" },
                { question: "Which company owns the 'Big Bazaar' retail chain?", options: ["Reliance Retail", "Future Group", "Aditya Birla Retail", "Tata Trent"], answer: "future group" },
                { question: "Which Indian company is known for its 'Lakmé' cosmetics brand?", options: ["HUL", "L'Oréal", "Godrej", "Emami"], answer: "hul" },
                { question: "Which company is a major player in the Indian aviation industry?", options: ["IndiGo", "SpiceJet", "Air India", "Vistara"], answer: "indigo" }
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
                { question: "Which Indian company has the highest market capitalization as of 2025?", options: ["Reliance Industries", "Tata Group", "HDFC Bank", "Infosys"], answer: "reliance industries" },
                { question: "What is the term for a market where prices are rising?", options: ["Bear market", "Bull market", "Stag market", "Flat market"], answer: "bull market" },
                { question: "Which index includes 30 major companies on the BSE?", options: ["NIFTY 50", "SENSEX", "MIDCAP", "SMALLCAP"], answer: "sensex" },
                { question: "What is a 'dividend' in the stock market?", options: ["A loan", "A share price increase", "A profit share", "A tax"], answer: "a profit share" },
                { question: "Which company is known for its banking services and listed on BSE?", options: ["HDFC Bank", "Infosys", "TCS", "Reliance"], answer: "hdfc bank" },
                { question: "What is the term for a sudden drop in stock prices?", options: ["Crash", "Rally", "Surge", "Spike"], answer: "crash" },
                { question: "Which type of stock represents ownership in a company?", options: ["Bond", "Equity", "Derivative", "Mutual Fund"], answer: "equity" },
                { question: "What is the purpose of a stock exchange?", options: ["To regulate banks", "To facilitate trading", "To issue loans", "To manage taxes"], answer: "to facilitate trading" },
                { question: "Which company is a major player in the Indian telecom sector and listed on NSE?", options: ["Bharti Airtel", "Tata Steel", "Maruti Suzuki", "Sun Pharma"], answer: "bharti airtel" },
                { question: "What is the term for a portfolio of multiple stocks?", options: ["Mutual Fund", "Bond", "ETF", "Warrant"], answer: "mutual fund" },
                { question: "Which financial instrument represents a loan to a company?", options: ["Stock", "Bond", "Option", "Future"], answer: "bond" }
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
                { question: "What is the most abundant element in the universe?", options: ["oxygen", "hydrogen", "helium", "carbon"], answer: "hydrogen" },
                { question: "What is the chemical symbol for gold?", options: ["Au", "Ag", "Fe", "Cu"], answer: "au" },
                { question: "Which scientist developed the theory of relativity?", options: ["Isaac Newton", "Albert Einstein", "Galileo Galilei", "Stephen Hawking"], answer: "albert einstein" },
                { question: "What is the unit of force in the metric system?", options: ["Newton", "Joule", "Watt", "Pascal"], answer: "newton" },
                { question: "Which planet is known for its rings?", options: ["Jupiter", "Saturn", "Uranus", "Neptune"], answer: "saturn" },
                { question: "What is the primary source of energy for Earth's climate system?", options: ["Wind", "Sun", "Geothermal", "Tides"], answer: "sun" },
                { question: "Which technology is used for wireless communication?", options: ["Bluetooth", "Ethernet", "USB", "HDMI"], answer: "bluetooth" },
                { question: "What is the chemical formula for water?", options: ["H2O", "CO2", "O2", "N2"], answer: "h2o" },
                { question: "Which scientist discovered penicillin?", options: ["Alexander Fleming", "Marie Curie", "Thomas Edison", "Nikola Tesla"], answer: "alexander fleming" },
                { question: "What is the largest organ in the human body?", options: ["Liver", "Skin", "Heart", "Lungs"], answer: "skin" },
                { question: "Which programming language was created first?", options: ["Python", "C", "Java", "Fortran"], answer: "fortran" }
            ],
            genZ: [
                { question: "What does 'slay' mean in GenZ slang?", options: ["to fail", "to succeed impressively", "to sleep", "to argue"], answer: "to succeed impressively" },
                { question: "What is 'yeet' used to express?", options: ["excitement or throwing something", "sadness", "confusion", "agreement"], answer: "excitement or throwing something" },
                { question: "What does 'sus' mean?", options: ["suspicious", "superb", "sunny", "surprised"], answer: "suspicious" },
                { question: "What is a 'stan'?", options: ["a casual fan", "an obsessive fan", "a critic", "a friend"], answer: "an obsessive fan" },
                { question: "What does 'bet' mean in GenZ slang?", options: ["to bet money", "okay or sure", "to disagree", "to leave"], answer: "okay or sure" },
                { question: "What does 'no cap' mean?", options: ["no limit", "honestly", "no problem", "no chance"], answer: "honestly" },
                { question: "What is 'vibing'?", options: ["arguing", "relaxing and enjoying", "working hard", "traveling"], answer: "relaxing and enjoying" },
                { question: "What does 'lit' describe?", options: ["something boring", "something exciting", "something old", "something quiet"], answer: "something exciting" },
                { question: "What does 'salty' mean in GenZ slang?", options: ["happy", "upset or bitter", "hungry", "tired"], answer: "upset or bitter" },
                { question: "What does 'GOAT' stand for?", options: ["Greatest Of All Time", "Good At Talking", "Gathering Of Teens", "Goal Of Action"], answer: "greatest of all time" },
                { question: "What does 'finna' mean?", options: ["Finished", "Planning to", "Fighting", "Finding"], answer: "planning to" },
                { question: "What is a 'glow-up'?", options: ["A party", "A transformation", "A failure", "A trend"], answer: "a transformation" },
                { question: "What does 'tea' refer to?", options: ["A drink", "Gossip", "A plan", "A challenge"], answer: "gossip" },
                { question: "What does 'bussin' mean?", options: ["Broken", "Amazing", "Boring", "Busy"], answer: "amazing" },
                { question: "What is a 'simp'?", options: ["Someone who is smart", "Someone who is overly attentive", "Someone who is shy", "Someone who is strong"], answer: "someone who is overly attentive" },
                { question: "What does 'cap' mean?", options: ["A hat", "A lie", "A compliment", "A goal"], answer: "a lie" },
                { question: "What does 'extra' describe?", options: ["Something minimal", "Something excessive", "Something calm", "Something ordinary"], answer: "something excessive" },
                { question: "What is 'clout'?", options: ["Influence or fame", "Clothing", "A game", "A mistake"], answer: "influence or fame" },
                { question: "What does 'periodt' emphasize?", options: ["A question", "A statement", "A request", "A suggestion"], answer: "a statement" },
                { question: "What does 'sksksk' express?", options: ["Anger", "Excitement", "Sadness", "Confusion"], answer: "excitement" }
            ],
            beautyBeast: [
                { question: "What is the main benefit of hyaluronic acid in skincare?", options: ["exfoliation", "hydration", "oil control", "brightening"], answer: "hydration" },
                { question: "How many minutes should you wait after applying sunscreen before going outside?", options: ["5", "15", "30", "60"], answer: "15" },
                { question: "What type of exercise is best for building muscle?", options: ["cardio", "yoga", "strength training", "pilates"], answer: "strength training" },
                { question: "Which vitamin is known for promoting hair growth?", options: ["Vitamin A", "Vitamin B7 (Biotin)", "Vitamin C", "Vitamin D"], answer: "vitamin b7 (biotin)" },
                { question: "What is a common ingredient in acne treatments?", options: ["coconut oil", "salicylic acid", "sugar syrup", "lemon extract"], answer: "salicylic acid" },
                { question: "How often should you typically exfoliate your skin?", options: ["daily", "2-3 times a week", "once a month", "never"], answer: "2-3 times a week" },
                { question: "What is the recommended daily water intake for adults (in liters)?", options: ["1", "2", "3", "4"], answer: "2" },
                { question: "Which muscle group is targeted by a plank exercise?", options: ["biceps", "core", "quadriceps", "calves"], answer: "core" },
                { question: "What does SPF stand for in sunscreen?", options: ["Skin Protection Factor", "Sun Protection Factor", "Solar Power Factor", "Skin Pigment Factor"], answer: "sun protection factor" },
                { question: "What is a key benefit of retinol in skincare?", options: ["moisturizing", "anti-aging", "soothing", "tanning"], answer: "anti-aging" },
                { question: "Which ingredient is known for brightening skin?", options: ["Vitamin C", "Aloe Vera", "Coconut Oil", "Glycerin"], answer: "vitamin c" },
                { question: "What is the primary benefit of a face mask?", options: ["Exfoliation", "Deep cleansing", "Tanning", "Hair growth"], answer: "deep cleansing" },
                { question: "Which exercise improves cardiovascular health?", options: ["Weightlifting", "Yoga", "Running", "Stretching"], answer: "running" },
                { question: "What is a common ingredient in lip balms?", options: ["Sugar", "Beeswax", "Salt", "Vinegar"], answer: "beeswax" },
                { question: "How many hours of sleep are recommended for adults nightly?", options: ["4-5", "6-7", "7-9", "10-12"], answer: "7-9" },
                { question: "Which muscle group is targeted by squats?", options: ["Chest", "Glutes", "Biceps", "Abs"], answer: "glutes" },
                { question: "What is the purpose of a toner in skincare?", options: ["Moisturizing", "Balancing skin pH", "Exfoliating", "Tanning"], answer: "balancing skin ph" },
                { question: "Which nutrient is essential for strong nails?", options: ["Calcium", "Protein", "Vitamin E", "Iron"], answer: "protein" },
                { question: "What is the benefit of using a serum in skincare?", options: ["Sun protection", "Targeted treatment", "Exfoliation", "Cleansing"], answer: "targeted treatment" },
                { question: "Which exercise is best for improving flexibility?", options: ["Weightlifting", "Yoga", "Sprinting", "Cycling"], answer: "yoga" }
            ],
            historicalMonuments: [
                { question: "In which city is the Taj Mahal located?", options: ["Delhi", "Agra", "Jaipur", "Mumbai"], answer: "agra" },
                { question: "Which monument is known as the 'Iron Pillar' and located in Delhi?", options: ["Qutub Minar", "Iron Pillar of Delhi", "Red Fort", "India Gate"], answer: "iron pillar of delhi" },
                { question: "Which South Indian temple is famous for its Dravidian architecture?", options: ["Meenakshi Temple", "Konark Sun Temple", "Ajanta Caves", "Hampi"], answer: "meenakshi temple" },
                { question: "Which monument was built by Emperor Ashoka in Sarnath?", options: ["Sanchi Stupa", "Dhamek Stupa", "Amaravati Stupa", "Nalanda"], answer: "dhamek stupa" },
                { question: "What is the primary material used in the construction of the Red Fort?", options: ["White Marble", "Red Sandstone", "Granite", "Limestone"], answer: "red sandstone" },
                { question: "Which monument in Hampi is known for its musical pillars?", options: ["Vittala Temple", "Virupaksha Temple", "Lotus Mahal", "Hazara Rama Temple"], answer: "vittala temple" },
                { question: "Which Mughal emperor commissioned the Humayun's Tomb?", options: ["Akbar", "Babur", "Humayun", "Jahangir"], answer: "humayun" },
                { question: "Which monument is a rock-cut cave complex in Maharashtra?", options: ["Ellora Caves", "Elephanta Caves", "Ajanta Caves", "Badami Caves"], answer: "ajanta caves" },
                { question: "Which monument in Jaipur is known as the 'Palace of Winds'?", options: ["Jal Mahal", "Hawa Mahal", "City Palace", "Amber Fort"], answer: "hawa mahal" },
                { question: "Which ancient university in Bihar is a UNESCO World Heritage Site?", options: ["Taxila", "Nalanda", "Vikramashila", "Odantapuri"], answer: "nalanda" },
                { question: "Which monument is known as the 'Sun Temple' in Odisha?", options: ["Konark Sun Temple", "Lingaraja Temple", "Jagannath Temple", "Mukteshvara Temple"], answer: "konark sun temple" },
                { question: "Which fort in Rajasthan is known for its mirror work?", options: ["Mehrangarh Fort", "Amber Fort", "Chittorgarh Fort", "Kumbhalgarh Fort"], answer: "amber fort" },
                { question: "Which monument in Delhi is a war memorial?", options: ["India Gate", "Red Fort", "Qutub Minar", "Lotus Temple"], answer: "india gate" },
                { question: "Which South Indian temple is known for its gopuram?", options: ["Brihadeeswara Temple", "Srirangam Temple", "Ramanathaswamy Temple", "Virupaksha Temple"], answer: "srirangam temple" },
                { question: "Which monument in Karnataka is a group of monolithic structures?", options: ["Hampi", "Badami Caves", "Pattadakal", "Aihole"], answer: "hampi" },
                { question: "Which Mughal monument is known for its symmetrical design?", options: ["Safdarjung Tomb", "Bibi Ka Maqbara", "Taj Mahal", "Jama Masjid"], answer: "taj mahal" },
                { question: "Which fort in Hyderabad is known for its acoustics?", options: ["Golconda Fort", "Charminar", "Qutb Shahi Tombs", "Falaknuma Palace"], answer: "golconda fort" },
                { question: "Which monument in Gujarat is a stepwell?", options: ["Rani ki Vav", "Sabarmati Ashram", "Sidi Saiyyed Mosque", "Dwarakadhish Temple"], answer: "rani ki vav" },
                { question: "Which monument in Kolkata was built during British rule?", options: ["Victoria Memorial", "Howrah Bridge", "Marble Palace", "Belur Math"], answer: "victoria memorial" },
                { question: "Which monument in Maharashtra is a sea fort?", options: ["Murud-Janjira Fort", "Shivneri Fort", "Raigad Fort", "Sindhudurg Fort"], answer: "murud-janjira fort" }
            ],
            indianCities: [
                { question: "Which city is known as the 'Silicon Valley of India'?", options: ["Mumbai", "Bengaluru", "Hyderabad", "Pune"], answer: "bengaluru" },
                { question: "Which city is the capital of West Bengal?", options: ["Kolkata", "Siliguri", "Durgapur", "Asansol"], answer: "kolkata" },
                { question: "Which city is famous for its Pink City nickname?", options: ["Jaipur", "Jodhpur", "Udaipur", "Bikaner"], answer: "jaipur" },
                { question: "Which city is known as the 'City of Joy'?", options: ["Mumbai", "Kolkata", "Chennai", "Delhi"], answer: "kolkata" },
                { question: "Which city is the financial capital of India?", options: ["Delhi", "Mumbai", "Bengaluru", "Hyderabad"], answer: "mumbai" },
                { question: "Which city is known for its IT industry and called 'Cyberabad'?", options: ["Chennai", "Hyderabad", "Pune", "Ahmedabad"], answer: "hyderabad" },
                { question: "Which city is famous for its Marina Beach?", options: ["Mumbai", "Chennai", "Goa", "Kochi"], answer: "chennai" },
                { question: "Which city is known as the 'City of Pearls'?", options: ["Surat", "Hyderabad", "Jaipur", "Lucknow"], answer: "hyderabad" },
                { question: "Which city is the capital of Uttar Pradesh?", options: ["Kanpur", "Lucknow", "Varanasi", "Agra"], answer: "lucknow" },
                { question: "Which city is known for its textile industry and called 'Manchester of India'?", options: ["Ahmedabad", "Surat", "Coimbatore", "Ludhiana"], answer: "ahmedabad" },
                { question: "Which city is known as the 'City of Lakes'?", options: ["Udaipur", "Bhopal", "Jodhpur", "Pune"], answer: "udaipur" },
                { question: "Which city is the capital of Kerala?", options: ["Kochi", "Thiruvananthapuram", "Kozhikode", "Thrissur"], answer: "thiruvananthapuram" },
                { question: "Which city is famous for its Golden Temple?", options: ["Amritsar", "Chandigarh", "Ludhiana", "Jalandhar"], answer: "amritsar" },
                { question: "Which city is known as the 'Gateway of India'?", options: ["Delhi", "Mumbai", "Kolkata", "Chennai"], answer: "mumbai" },
                { question: "Which city is the capital of Gujarat?", options: ["Ahmedabad", "Gandhinagar", "Surat", "Vadodara"], answer: "gandhinagar" },
                { question: "Which city is known for its tea gardens?", options: ["Darjeeling", "Shimla", "Ooty", "Munnar"], answer: "darjeeling" },
                { question: "Which city is the capital of Tamil Nadu?", options: ["Chennai", "Coimbatore", "Madurai", "Salem"], answer: "chennai" },
                { question: "Which city is known as the 'City of Palaces'?", options: ["Mysore", "Jaipur", "Kolkata", "Lucknow"], answer: "mysore" },
                { question: "Which city is the capital of Punjab?", options: ["Amritsar", "Ludhiana", "Chandigarh", "Patiala"], answer: "chandigarh" },
                { question: "Which city is known for its shipbuilding industry?", options: ["Visakhapatnam", "Mumbai", "Chennai", "Kochi"], answer: "visakhapatnam" }
            ],
            solveRiddle: [
                { question: "I speak without a mouth and hear without ears. I have no body, but I come alive with the wind. What am I?", options: ["a ghost", "an echo", "a dream", "a shadow"], answer: "an echo" },
                { question: "The more you take, the more you leave behind. What am I?", options: ["money", "footprints", "time", "friends"], answer: "footprints" },
                { question: "I’m tall when I’m young, and I’m short when I’m old. What am I?", options: ["[=]a candle", "a pencil", "a tree", "a person"], answer: "a candle" },
                { question: "What has keys but can’t open locks?", options: ["a piano", "a map", "a book", "a computer"], answer: "a piano" },
                { question: "I have cities but no houses, forests but no trees, and rivers but no water. What am I?", options: ["a painting", "a map", "a dream", "a story"], answer: "a map" },
                { question: "What can travel around the world while staying in a corner?", options: ["a letter", "a stamp", "an email", "a dream"], answer: "a stamp" },
                { question: "What gets wetter the more it dries?", options: ["a towel", "a sponge", "a cloud", "a river"], answer: "a towel" },
                { question: "I have branches, but no fruit, trunk, or leaves. What am I?", options: ["a bank", "a library", "a river", "a company"], answer: "a bank" },
                { question: "What can you catch but not throw?", options: ["a ball", "a cold", "a fish", "a wave"], answer: "a cold" },
                { question: "What has a neck but no head, a body but no legs, and arms but no hands?", options: ["a shirt", "a snake", "a bottle", "a statue"], answer: "a shirt" },
                { question: "What has a heart that doesn’t beat?", options: ["A stone", "An artichoke", "A clock", "A tree"], answer: "an artichoke" },
                { question: "The more you have of me, the less you see. What am I?", options: ["Light", "Darkness", "Time", "Money"], answer: "darkness" },
                { question: "I’m light as a feather, yet the strongest man can’t hold me for long. What am I?", options: ["Breath", "A feather", "A cloud", "A dream"], answer: "breath" },
                { question: "What has one eye but cannot see?", options: ["A needle", "A storm", "A potato", "A clock"], answer: "a needle" },
                { question: "What comes once in a minute, twice in a moment, but never in a thousand years?", options: ["The letter M", "A second", "A day", "A year"], answer: "the letter m" },
                { question: "I’m always running but never move. What am I?", options: ["A river", "A clock", "A treadmill", "A road"], answer: "a clock" },
                { question: "What has a thumb and four fingers but is not alive?", options: ["A glove", "A hand", "A statue", "A puppet"], answer: "a glove" },
                { question: "What can you break, even if you never pick it up or touch it?", options: ["A glass", "A promise", "A stone", "A stick"], answer: "a promise" },
                { question: "I have keys but no locks, space but no room, and you can enter, but not go outside. What am I?", options: ["A keyboard", "A house", "A car", "A book"], answer: "a keyboard" },
                { question: "What has a head, a tail, is brown, and has no legs?", options: ["A snake", "A coin", "A worm", "A tadpole"], answer: "a coin" }
            ],
            birdLocations: [
                { question: "Where is the Indian Peafowl primarily found?", options: ["South America", "India", "Australia", "Africa"], answer: "india" },
                { question: "In which region is the Himalayan Monal commonly found?", options: ["Himalayas", "Sundarbans", "Western Ghats", "Thar Desert"], answer: "himalayas" },
                { question: "Where can you find the Great Indian Bustard?", options: ["Andaman Islands", "Rajasthan", "Kerala", "Arunachal Pradesh"], answer: "rajasthan" },
                { question: "Which region is home to the Sarus Crane?", options: ["Northern India", "Eastern India", "Southern India", "Western India"], answer: "northern india" },
                { question: "Where is the Black-Necked Crane primarily found in India?", options: ["Ladakh", "Goa", "Tamil Nadu", "Odisha"], answer: "ladakh" },
                { question: "In which wetland is the Siberian Crane a winter visitor?", options: ["Chilika Lake", "Keoladeo National Park", "Loktak Lake", "Vembanad Lake"], answer: "keoladeo national park" },
                { question: "Where is the Nicobar Pigeon found?", options: ["Nicobar Islands", "Lakshadweep", "Gujarat", "Punjab"], answer: "nicobar islands" },
                { question: "Which region hosts the Indian Skimmer?", options: ["Chambal River", "Brahmaputra River", "Godavari River", "Narmada River"], answer: "chambal river" },
                { question: "Where can the White-bellied Heron be found in India?", options: ["Assam", "Karnataka", "Rajasthan", "Jharkhand"], answer: "assam" },
                { question: "Which national park is a key habitat for the Bengal Florican?", options: ["Kaziranga National Park", "Jim Corbett National Park", "Sundarbans", "Ranthambore"], answer: "kaziranga national park" },
                { question: "Where is the Malabar Trogon primarily found?", options: ["Western Ghats", "Eastern Ghats", "Himalayas", "Sundarbans"], answer: "western ghats" },
                { question: "Which region is a habitat for the Indian Roller?", options: ["Deccan Plateau", "Andaman Islands", "Ladakh", "Kerala"], answer: "deccan plateau" },
                { question: "Where can the Painted Stork be commonly found?", options: ["Bharatpur", "Goa", "Sikkim", "Manipur"], answer: "bharatpur" },
                { question: "Which wetland is a key habitat for the Bar-headed Goose?", options: ["Chilika Lake", "Wular Lake", "Vembanad Lake", "Sambhar Lake"], answer: "chilika lake" },
                { question: "Where is the Nilgiri Wood Pigeon found?", options: ["Nilgiri Hills", "Aravalli Hills", "Satpura Hills", "Vindhya Hills"], answer: "nilgiri hills" },
                { question: "Which national park is home to the Red-headed Vulture?", options: ["Bandhavgarh National Park", "Kanha National Park", "Pench National Park", "Ranthambore"], answer: "ranthambore" },
                { question: "Where is the Indian Cormorant commonly found?", options: ["Coastal regions", "Himalayas", "Thar Desert", "Northeast India"], answer: "coastal regions" },
                { question: "Which region is a breeding ground for the Greater Flamingo?", options: ["Rann of Kutch", "Sundarbans", "Western Ghats", "Ladakh"], answer: "rann of kutch" },
                { question: "Where can the Indian Pitta be found?", options: ["Central India", "Andaman Islands", "Lakshadweep", "Sikkim"], answer: "central india" },
                { question: "Which national park is a habitat for the White-rumped Vulture?", options: ["Gir National Park", "Sundarbans", "Corbett National Park", "Kaziranga"], answer: "corbett national park" }
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
            // Shuffle questions for this quiz session
            shuffledQuestions = shuffleArray(quizzes[currentCategory]);
            console.log('Starting quiz for category:', category, 'with shuffled questions:', shuffledQuestions); // Debug log
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

            if (currentQuestionIndex >= shuffledQuestions.length) {
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

            const q = shuffledQuestions[currentQuestionIndex];
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
