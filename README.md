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
        let shuffledQuestions = [];

        const scores = {
            common: {}, food: {}, sports: {}, animalKingdom: {}, indianCinema: {}, foodTravel: {},
            indianCompanies: {}, stockMarket: {}, scienceTech: {}, genZ: {}, beautyBeast: {},
            historicalMonuments: {}, indianCities: {}, solveRiddle: {}, birdLocations: {}
        };

        function shuffleArray(array) {
            const shuffled = [...array];
            for (let i = shuffled.length - 1; i > 0; i--) {
                const j = Math.floor(Math.random() * (i + 1));
                [shuffled[i], shuffled[j]] = [shuffled[j], shuffled[i]];
            }
            return shuffled;
        }

        function loadScores() {
            const savedScores = localStorage.getItem('quizScores');
            if (savedScores) {
                Object.assign(scores, JSON.parse(savedScores));
            }
            updateLeaderboards();
        }

        function saveScores() {
            localStorage.setItem('quizScores', JSON.stringify(scores));
            updateLeaderboards();
        }

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

        const quizzes = {
            common: [
                { question: "What color is the sky on a clear day?", options: ["Red", "Blue", "Green", "Yellow"], answer: "blue" },
                { question: "How many days are there in a week?", options: ["5", "7", "10", "14"], answer: "7" },
                { question: "What do you use to write on a chalkboard?", options: ["Pen", "Chalk", "Pencil", "Marker"], answer: "chalk" },
                { question: "What is the primary source of energy for Earth?", options: ["Moon", "Sun", "Stars", "Wind"], answer: "sun" },
                { question: "How many legs does a spider have?", options: ["6", "8", "10", "12"], answer: "8" },
                { question: "What do plants need to grow besides water?", options: ["Stone", "Sunlight", "Sand", "Ice"], answer: "sunlight" },
                { question: "What do you call a baby cat?", options: ["Puppy", "Kitten", "Cub", "Foal"], answer: "kitten" },
                { question: "What is the capital city of France?", options: ["London", "Berlin", "Paris", "Madrid"], answer: "paris" },
                { question: "What do you use to brush your teeth?", options: ["Comb", "Toothbrush", "Spoon", "Fork"], answer: "toothbrush" },
                { question: "How many colors are there in a rainbow?", options: ["5", "6", "7", "8"], answer: "7" },
                { question: "What is the smell of rain like?", options: ["Petrichor", "Chlorine", "Sulfur", "Ammonia"], answer: "petrichor" },
                { question: "Which planet is closest to the sun?", options: ["Venus", "Earth", "Mercury", "Mars"], answer: "mercury" },
                { question: "What do bees produce?", options: ["Milk", "Honey", "Silk", "Wax"], answer: "honey" },
                { question: "What is the largest ocean on Earth?", options: ["Atlantic", "Indian", "Arctic", "Pacific"], answer: "pacific" },
                { question: "What do you call a group of fish?", options: ["Flock", "School", "Pack", "Herd"], answer: "school" },
                { question: "Which season comes after winter?", options: ["Spring", "Summer", "Autumn", "Monsoon"], answer: "spring" },
                { question: "What is the boiling point of water in Celsius?", options: ["50", "75", "100", "150"], answer: "100" },
                { question: "What do you call a young dog?", options: ["Kitten", "Puppy", "Calf", "Foal"], answer: "puppy" },
                { question: "Which fruit is known for keeping the doctor away?", options: ["Banana", "Orange", "Apple", "Grape"], answer: "apple" },
                { question: "What is the tallest mountain in the world?", options: ["K2", "Kangchenjunga", "Everest", "Lhotse"], answer: "everest" }
            ],
            food: [
                { question: "What fruit is known as the 'king of fruits'?", options: ["Apple", "Banana", "Durian", "Mango"], answer: "durian" },
                { question: "Which spice is made from saffron crocus?", options: ["Cinnamon", "Saffron", "Turmeric", "Paprika"], answer: "saffron" },
                { question: "What is the main ingredient in guacamole?", options: ["Tomato", "Avocado", "Onion", "Pepper"], answer: "avocado" },
                { question: "Which country is famous for sushi?", options: ["China", "Japan", "Thailand", "Korea"], answer: "japan" },
                { question: "What is the primary ingredient in bread?", options: ["Rice", "Flour", "Corn", "Potato"], answer: "flour" },
                { question: "Which drink is made from fermented grapes?", options: ["Beer", "Wine", "Whiskey", "Vodka"], answer: "wine" },
                { question: "What cheese is used in traditional lasagna?", options: ["Cheddar", "Mozzarella", "Parmesan", "Brie"], answer: "mozzarella" },
                { question: "Which fruit is known for its spiky exterior?", options: ["Pineapple", "Orange", "Grape", "Pear"], answer: "pineapple" },
                { question: "What is the main ingredient in hummus?", options: ["Lentils", "Chickpeas", "Beans", "Peas"], answer: "chickpeas" },
                { question: "Which country is famous for pizza?", options: ["Spain", "Italy", "France", "Greece"], answer: "italy" },
                { question: "Which nut is used to make marzipan?", options: ["Almond", "Peanut", "Cashew", "Walnut"], answer: "almond" },
                { question: "What is the primary ingredient in soy sauce?", options: ["Soybeans", "Wheat", "Rice", "Corn"], answer: "soybeans" },
                { question: "Which country is known for its dish 'Poutine'?", options: ["France", "Canada", "Belgium", "Switzerland"], answer: "canada" },
                { question: "What type of fish is commonly used in fish and chips?", options: ["Salmon", "Cod", "Tuna", "Mackerel"], answer: "cod" },
                { question: "Which fruit is used to make a traditional key lime pie?", options: ["Lemon", "Lime", "Orange", "Grapefruit"], answer: "lime" },
                { question: "Which country is famous for its dish 'Pho'?", options: ["Thailand", "Vietnam", "China", "Japan"], answer: "vietnam" },
                { question: "What type of pasta is shaped like small tubes?", options: ["Spaghetti", "Rigatoni", "Fusilli", "Farfalle"], answer: "rigatoni" },
                { question: "Which spice gives curry its yellow color?", options: ["Cumin", "Turmeric", "Coriander", "Paprika"], answer: "turmeric" },
                { question: "What is the main ingredient in a traditional pesto sauce?", options: ["Basil", "Parsley", "Cilantro", "Mint"], answer: "basil" },
                { question: "Which country is known for its dish 'Tacos'?", options: ["Spain", "Mexico", "Brazil", "Argentina"], answer: "mexico" }
            ],
            sports: [
                { question: "How many players are there in a soccer team on the field?", options: ["9", "10", "11", "12"], answer: "11" },
                { question: "Which sport uses a shuttlecock?", options: ["Tennis", "Badminton", "Squash", "Table Tennis"], answer: "badminton" },
                { question: "How many holes are there in a standard golf course?", options: ["9", "12", "18", "20"], answer: "18" },
                { question: "In which sport is a 'home run' scored?", options: ["Cricket", "Baseball", "Rugby", "Hockey"], answer: "baseball" },
                { question: "How many players are on a basketball team on the court?", options: ["4", "5", "6", "7"], answer: "5" },
                { question: "Which country hosted the 2016 Summer Olympics?", options: ["China", "Brazil", "USA", "UK"], answer: "brazil" },
                { question: "What is the maximum score in a single frame of bowling?", options: ["100", "200", "250", "300"], answer: "300" },
                { question: "Which sport is played at Wimbledon?", options: ["Cricket", "Tennis", "Rugby", "Golf"], answer: "tennis" },
                { question: "How long is an Olympic swimming pool (in meters)?", options: ["25", "50", "75", "100"], answer: "50" },
                { question: "In which sport do you perform a 'slam dunk'?", options: ["Volleyball", "Basketball", "Netball", "Handball"], answer: "basketball" },
                { question: "Which country won the FIFA World Cup in 2018?", options: ["Germany", "France", "Brazil", "Argentina"], answer: "france" },
                { question: "In which sport is the term 'bogey' used?", options: ["Tennis", "Golf", "Cricket", "Rugby"], answer: "golf" },
                { question: "How many points is a touchdown worth in American football?", options: ["3", "6", "7", "10"], answer: "6" },
                { question: "Which sport is known as 'the gentleman’s game'?", options: ["Cricket", "Tennis", "Golf", "Polo"], answer: "cricket" },
                { question: "In which sport do players compete for the 'Ryder Cup'?", options: ["Tennis", "Golf", "Rugby", "Cricket"], answer: "golf" },
                { question: "How many players are on a volleyball team on the court?", options: ["5", "6", "7", "8"], answer: "6" },
                { question: "Which country is famous for the martial art 'Judo'?", options: ["China", "Japan", "Korea", "Thailand"], answer: "japan" },
                { question: "What is the distance of a marathon race (in kilometers)?", options: ["21", "42", "50", "100"], answer: "42" },
                { question: "In which sport is the term 'checkmate' used?", options: ["Chess", "Boxing", "Fencing", "Wrestling"], answer: "chess" },
                { question: "Which country hosted the 2020 Summer Olympics?", options: ["China", "Japan", "Brazil", "USA"], answer: "japan" }
            ],
            animalKingdom: [
                { question: "Which animal is known as the 'king of the jungle'?", options: ["Lion", "Tiger", "Elephant", "Bear"], answer: "lion" },
                { question: "What is the largest land animal?", options: ["Elephant", "Giraffe", "Hippopotamus", "Rhinoceros"], answer: "elephant" },
                { question: "Which bird is known for its black and white stripes?", options: ["Penguin", "Zebra Finch", "Ostrich", "Eagle"], answer: "penguin" },
                { question: "What is the fastest land animal?", options: ["Cheetah", "Lion", "Horse", "Deer"], answer: "cheetah" },
                { question: "Which animal has a body covered with scales?", options: ["Snake", "Dog", "Cat", "Rabbit"], answer: "snake" },
                { question: "What is the largest species of shark?", options: ["Great White", "Whale Shark", "Tiger Shark", "Hammerhead"], answer: "whale shark" },
                { question: "Which animal is known for its black and white fur and lives in China?", options: ["Panda", "Zebra", "Skunk", "Lemur"], answer: "panda" },
                { question: "What type of animal is a jellyfish?", options: ["Cnidarian", "Mammal", "Reptile", "Amphibian"], answer: "cnidarian" },
                { question: "Which animal is famous for its long neck?", options: ["Giraffe", "Camel", "Llama", "Ostrich"], answer: "giraffe" },
                { question: "What is the only mammal that can fly?", options: ["Bat", "Bird", "Flying Squirrel", "Pterodactyl"], answer: "bat" },
                { question: "Which animal is known for its pouch?", options: ["Kangaroo", "Koala", "Wombat", "Tasmanian Devil"], answer: "kangaroo" },
                { question: "What is the largest mammal in the ocean?", options: ["Blue Whale", "Orca", "Sperm Whale", "Humpback Whale"], answer: "blue whale" },
                { question: "Which bird is known for its colorful feathers and ability to mimic sounds?", options: ["Parrot", "Peacock", "Flamingo", "Toucan"], answer: "parrot" },
                { question: "Which animal is known for its humps used to store fat?", options: ["Camel", "Llama", "Alpaca", "Guanaco"], answer: "camel" },
                { question: "What is the smallest mammal in the world?", options: ["Pygmy Shrew", "Bumblebee Bat", "Mouse Lemur", "Etruscan Shrew"], answer: "bumblebee bat" },
                { question: "Which animal is known for its black and white stripes?", options: ["Zebra", "Tiger", "Leopard", "Cheetah"], answer: "zebra" },
                { question: "What type of animal is a crocodile?", options: ["Mammal", "Reptile", "Amphibian", "Bird"], answer: "reptile" },
                { question: "Which animal is known for its ability to change color?", options: ["Chameleon", "Frog", "Snake", "Lizard"], answer: "chameleon" },
                { question: "What is the tallest bird in the world?", options: ["Ostrich", "Emu", "Cassowary", "Rhea"], answer: "ostrich" },
                { question: "Which animal is known for its strong sense of smell and long trunk?", options: ["Elephant", "Rhinoceros", "Hippopotamus", "Tapir"], answer: "elephant" }
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
                { question: "Which film is known as the first Indian talkie?", options: ["Alam Ara", "Raja Harishchandra", "Pundalik", "Sant Tukaram"], answer: "alam ara" },
                { question: "Who directed the film 'Lagaan'?", options: ["Ashutosh Gowariker", "Sanjay Leela Bhansali", "Karan Johar", "Anurag Kashyap"], answer: "ashutosh gowariker" },
                { question: "Which actor is known as 'Mr. Perfectionist' in Bollywood?", options: ["Aamir Khan", "Salman Khan", "Shah Rukh Khan", "Akshay Kumar"], answer: "aamir khan" },
                { question: "Which film won the National Award for Best Feature Film in 2020?", options: ["Gully Boy", "Chhichhore", "Super 30", "Article 15"], answer: "gully boy" },
                { question: "Who played the role of 'Bajirao' in 'Bajirao Mastani'?", options: ["Ranveer Singh", "Hrithik Roshan", "Ajay Devgn", "Saif Ali Khan"], answer: "ranveer singh" },
                { question: "Which film is based on the life of Milkha Singh?", options: ["Mary Kom", "Bhaag Milkha Bhaag", "Dangal", "Sultan"], answer: "bhaag milkha bhaag" },
                { question: "Which actress is known for her role in 'Queen'?", options: ["Kangana Ranaut", "Vidya Balan", "Anushka Sharma", "Parineeti Chopra"], answer: "kangana ranaut" },
                { question: "Which film features the song 'Tum Hi Ho'?", options: ["Aashiqui 2", "Yeh Jawaani Hai Deewani", "Ram-Leela", "Raanjhanaa"], answer: "aashiqui 2" },
                { question: "Who directed the film 'Pather Panchali'?", options: ["Satyajit Ray", "Guru Dutt", "Bimal Roy", "Raj Kapoor"], answer: "satyajit ray" },
                { question: "Which film is known for the dialogue 'Mogambo Khush Hua'?", options: ["Mr. India", "Sholay", "Deewar", "Don"], answer: "mr. india" }
            ],
            foodTravel: [
                { question: "Which country is famous for paella?", options: ["Italy", "Spain", "France", "Greece"], answer: "spain" },
                { question: "What is the capital city of Thailand?", options: ["Bangkok", "Phuket", "Chiang Mai", "Pattaya"], answer: "bangkok" },
                { question: "Which spice is a key ingredient in curry?", options: ["Saffron", "Turmeric", "Oregano", "Basil"], answer: "turmeric" },
                { question: "What is the famous landmark in Paris?", options: ["Eiffel Tower", "Big Ben", "Colosseum", "Statue of Liberty"], answer: "eiffel tower" },
                { question: "Which cuisine is known for sushi?", options: ["Chinese", "Japanese", "Korean", "Thai"], answer: "japanese" },
                { question: "What desert country is known for its pyramids?", options: ["Mexico", "Egypt", "Peru", "India"], answer: "egypt" },
                { question: "Which fruit is a staple in Hawaiian poke?", options: ["Pineapple", "Mango", "Banana", "Avocado"], answer: "avocado" },
                { question: "What is the largest city in Australia?", options: ["Sydney", "Melbourne", "Perth", "Brisbane"], answer: "sydney" },
                { question: "Which dish is made from fermented cabbage?", options: ["Sauerkraut", "Kimchi", "Pickles", "Relish"], answer: "kimchi" },
                { question: "What Italian city is famous for its canals?", options: ["Rome", "Venice", "Florence", "Milan"], answer: "venice" },
                { question: "Which country is famous for its dish 'Feijoada'?", options: ["Brazil", "Argentina", "Chile", "Peru"], answer: "brazil" },
                { question: "What is the capital city of Mexico?", options: ["Cancun", "Guadalajara", "Mexico City", "Tijuana"], answer: "mexico city" },
                { question: "Which spice is known as the 'queen of spices'?", options: ["Cardamom", "Cinnamon", "Clove", "Nutmeg"], answer: "cardamom" },
                { question: "What is the famous landmark in New York City?", options: ["Statue of Liberty", "Golden Gate Bridge", "Big Ben", "Eiffel Tower"], answer: "statue of liberty" },
                { question: "Which cuisine is known for dim sum?", options: ["Japanese", "Chinese", "Thai", "Vietnamese"], answer: "chinese" },
                { question: "Which country is known for Machu Picchu?", options: ["Brazil", "Peru", "Bolivia", "Chile"], answer: "peru" },
                { question: "Which fruit is commonly used in a traditional pavlova?", options: ["Kiwi", "Strawberry", "Mango", "Pineapple"], answer: "kiwi" },
                { question: "What is the capital city of Brazil?", options: ["Rio de Janeiro", "Sao Paulo", "Brasilia", "Salvador"], answer: "brasilia" },
                { question: "Which dish is a staple in Ethiopian cuisine?", options: ["Injera", "Couscous", "Fufu", "Jollof Rice"], answer: "injera" },
                { question: "What Spanish city is famous for its running of the bulls?", options: ["Madrid", "Barcelona", "Pamplona", "Seville"], answer: "pamplona" }
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
                { question: "Which company owns the 'Zomato' food delivery platform?", options: ["Info Edge", "Zomato", "Swiggy", "Tata"], answer: "zomato" },
                { question: "What is the largest private sector bank in India?", options: ["HDFC Bank", "ICICI Bank", "Axis Bank", "Kotak Mahindra"], answer: "hdfc bank" },
                { question: "Which company is known for the 'Bournvita' brand?", options: ["Cadbury", "Nestle", "Amul", "HUL"], answer: "cadbury" },
                { question: "What company manufactures the 'Maruti' cars?", options: ["Tata Motors", "Maruti Suzuki", "Mahindra", "Hyundai"], answer: "maruti suzuki" },
                { question: "Which Indian company is a major player in pharmaceuticals?", options: ["Sun Pharma", "TCS", "Reliance", "Wipro"], answer: "sun pharma" },
                { question: "What company owns the 'Big Bazaar' retail chain?", options: ["Reliance", "Future Group", "Tata", "Aditya Birla"], answer: "future group" },
                { question: "Which company is known for the 'Patanjali' brand?", options: ["Dabur", "Patanjali Ayurved", "HUL", "Godrej"], answer: "patanjali ayurved" },
                { question: "What company is a leader in Indian cement production?", options: ["UltraTech Cement", "Ambuja Cement", "ACC", "Shree Cement"], answer: "ultratech cement" },
                { question: "Which company owns the 'Airtel' telecom brand?", options: ["Vodafone", "Jio", "Bharti Airtel", "BSNL"], answer: "bharti airtel" },
                { question: "What company is known for the 'Lakmé' cosmetics brand?", options: ["HUL", "L'Oréal", "P&G", "Godrej"], answer: "hul" }
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
                { question: "What does NSE stand for?", options: ["National Stock Exchange", "New Stock Exchange", "National Securities Exchange", "Nifty Stock Exchange"], answer: "national stock exchange" },
                { question: "Which type of market is also known as the 'bear market'?", options: ["Bull Market", "Stagnant Market", "Declining Market", "Volatile Market"], answer: "declining market" },
                { question: "What is the term for a company's total market value?", options: ["Market Capitalization", "Net Worth", "Equity Value", "Asset Value"], answer: "market capitalization" },
                { question: "Which financial instrument represents ownership in a company?", options: ["Bond", "Share", "Mutual Fund", "Debenture"], answer: "share" },
                { question: "What is the term for a sudden drop in stock prices?", options: ["Crash", "Dip", "Surge", "Rally"], answer: "crash" },
                { question: "Which company is known for its Sensex index?", options: ["NSE", "BSE", "SEBI", "RBI"], answer: "bse" },
                { question: "What does DEMAT stand for?", options: ["Dematerialized Account", "Digital Equity Market", "Deposit Management", "Direct Equity Market"], answer: "dematerialized account" },
                { question: "Which type of trading involves high risk and high reward?", options: ["Equity Trading", "Derivatives Trading", "Bond Trading", "Mutual Fund Trading"], answer: "derivatives trading" },
                { question: "What is the term for a portfolio of stocks and other assets?", options: ["Mutual Fund", "ETF", "Index Fund", "Hedge Fund"], answer: "mutual fund" },
                { question: "Which Indian company is a major player in the IT sector on the stock market?", options: ["Infosys", "Reliance", "HDFC", "Adani"], answer: "infosys" }
            ],
            scienceTech: [
                { question: "What gas makes up most of Earth's atmosphere?", options: ["Oxygen", "Nitrogen", "Carbon Dioxide", "Hydrogen"], answer: "nitrogen" },
                { question: "What is the primary source of energy for the solar system?", options: ["Moon", "Sun", "Earth Core", "Stars"], answer: "sun" },
                { question: "Which element has the atomic number 1?", options: ["Helium", "Hydrogen", "Oxygen", "Carbon"], answer: "hydrogen" },
                { question: "What technology is used in touchscreens?", options: ["LCD", "Capacitive", "LED", "Plasma"], answer: "capacitive" },
                { question: "Which planet is known as the Red Planet?", options: ["Venus", "Mars", "Jupiter", "Saturn"], answer: "mars" },
                { question: "What is the main component of a CPU?", options: ["RAM", "Motherboard", "Processor", "Hard Drive"], answer: "processor" },
                { question: "Which gas is most responsible for the greenhouse effect?", options: ["Oxygen", "Carbon Dioxide", "Nitrogen", "Argon"], answer: "carbon dioxide" },
                { question: "What is the unit of electrical resistance?", options: ["Volt", "Ohm", "Ampere", "Watt"], answer: "ohm" },
                { question: "Which invention is credited to Alexander Graham Bell?", options: ["Telephone", "Light Bulb", "Radio", "Television"], answer: "telephone" },
                { question: "What is the most abundant element in the universe?", options: ["Oxygen", "Hydrogen", "Helium", "Carbon"], answer: "hydrogen" },
                { question: "Which scientist proposed the theory of relativity?", options: ["Isaac Newton", "Albert Einstein", "Galileo Galilei", "Stephen Hawking"], answer: "albert einstein" },
                { question: "What is the speed of light in a vacuum (in meters per second)?", options: ["300,000", "3,000,000", "300,000,000", "30,000,000"], answer: "300,000,000" },
                { question: "Which planet is known for its rings?", options: ["Jupiter", "Saturn", "Uranus", "Neptune"], answer: "saturn" },
                { question: "What does DNA stand for?", options: ["Deoxyribonucleic Acid", "Dioxyribonucleic Acid", "Dynamic Nucleic Acid", "Deoxyribonucleic Antigen"], answer: "deoxyribonucleic acid" },
                { question: "Which device converts electrical energy into mechanical energy?", options: ["Generator", "Motor", "Transformer", "Battery"], answer: "motor" },
                { question: "What is the unit of frequency?", options: ["Hertz", "Watt", "Joule", "Newton"], answer: "hertz" },
                { question: "Which scientist discovered penicillin?", options: ["Marie Curie", "Alexander Fleming", "Louis Pasteur", "Jonas Salk"], answer: "alexander fleming" },
                { question: "What is the largest planet in our solar system?", options: ["Earth", "Jupiter", "Saturn", "Mars"], answer: "jupiter" },
                { question: "Which technology is used for wireless communication?", options: ["Bluetooth", "Ethernet", "HDMI", "USB"], answer: "bluetooth" },
                { question: "What is the chemical symbol for gold?", options: ["Au", "Ag", "Fe", "Cu"], answer: "au" }
            ],
            genZ: [
                { question: "What does 'slay' mean in GenZ slang?", options: ["To fail", "To succeed impressively", "To sleep", "To argue"], answer: "to succeed impressively" },
                { question: "What is 'yeet' used to express?", options: ["Excitement or throwing something", "Sadness", "Confusion", "Agreement"], answer: "excitement or throwing something" },
                { question: "What does 'sus' mean?", options: ["Suspicious", "Superb", "Sunny", "Surprised"], answer: "suspicious" },
                { question: "What is a 'stan'?", options: ["A casual fan", "An obsessive fan", "A critic", "A friend"], answer: "an obsessive fan" },
                { question: "What does 'bet' mean in GenZ slang?", options: ["To bet money", "Okay or sure", "To disagree", "To leave"], answer: "okay or sure" },
                { question: "What does 'no cap' mean?", options: ["No limit", "Honestly", "No problem", "No chance"], answer: "honestly" },
                { question: "What is 'vibing'?", options: ["Arguing", "Relaxing and enjoying", "Working hard", "Traveling"], answer: "relaxing and enjoying" },
                { question: "What does 'lit' describe?", options: ["Something boring", "Something exciting", "Something old", "Something quiet"], answer: "something exciting" },
                { question: "What does 'salty' mean in GenZ slang?", options: ["Happy", "Upset or bitter", "Hungry", "Tired"], answer: "upset or bitter" },
                { question: "What does 'GOAT' stand for?", options: ["Greatest Of All Time", "Good At Talking", "Gathering Of Teens", "Goal Of Action"], answer: "greatest of all time" },
                { question: "What does 'finna' mean?", options: ["Finished", "Going to", "Fine", "Finding"], answer: "going to" },
                { question: "What does 'tea' refer to?", options: ["A drink", "Gossip", "Truth", "Tired"], answer: "gossip" },
                { question: "What does 'lowkey' mean?", options: ["Secretly", "Loudly", "Quickly", "Sadly"], answer: "secretly" },
                { question: "What does 'extra' describe?", options: ["Minimal", "Over the top", "Normal", "Quiet"], answer: "over the top" },
                { question: "What does 'fam' refer to?", options: ["Family", "Friends", "Famous", "Fans"], answer: "friends" },
                { question: "What does 'shade' mean?", options: ["Light", "Insult", "Support", "Shade"], answer: "insult" },
                { question: "What does 'woke' mean?", options: ["Asleep", "Aware", "Angry", "Funny"], answer: "aware" },
                { question: "What does 'clapback' refer to?", options: ["A dance move", "A quick comeback", "A compliment", "A mistake"], answer: "a quick comeback" },
                { question: "What does 'sksksk' express?", options: ["Laughter", "Sadness", "Confusion", "Agreement"], answer: "laughter" },
                { question: "What does 'drip' refer to?", options: ["Rain", "Style", "Food", "Sleep"], answer: "style" }
            ],
            beautyBeast: [
                { question: "What is the main benefit of hyaluronic acid in skincare?", options: ["Exfoliation", "Hydration", "Oil control", "Brightening"], answer: "hydration" },
                { question: "How many minutes should you wait after applying sunscreen before going outside?", options: ["5", "15", "30", "60"], answer: "15" },
                { question: "What type of exercise is best for building muscle?", options: ["Cardio", "Yoga", "Strength training", "Pilates"], answer: "strength training" },
                { question: "Which vitamin is known for promoting hair growth?", options: ["Vitamin A", "Vitamin B7 (Biotin)", "Vitamin C", "Vitamin D"], answer: "vitamin b7 (biotin)" },
                { question: "What is a common ingredient in acne treatments?", options: ["Coconut oil", "Salicylic acid", "Sugar syrup", "Lemon extract"], answer: "salicylic acid" },
                { question: "How often should you typically exfoliate your skin?", options: ["Daily", "2-3 times a week", "Once a month", "Never"], answer: "2-3 times a week" },
                { question: "What is the recommended daily water intake for adults (in liters)?", options: ["1", "2", "3", "4"], answer: "2" },
                { question: "Which muscle group is targeted by a plank exercise?", options: ["Biceps", "Core", "Quadriceps", "Calves"], answer: "core" },
                { question: "What does SPF stand for in sunscreen?", options: ["Skin Protection Factor", "Sun Protection Factor", "Solar Power Factor", "Skin Pigment Factor"], answer: "sun protection factor" },
                { question: "What is a key benefit of retinol in skincare?", options: ["Moisturizing", "Anti-aging", "Soothing", "Tanning"], answer: "anti-aging" },
                { question: "Which ingredient is known for brightening skin?", options: ["Vitamin C", "Aloe Vera", "Shea Butter", "Jojoba Oil"], answer: "vitamin c" },
                { question: "What type of exercise improves cardiovascular health?", options: ["Strength Training", "Yoga", "Cardio", "Stretching"], answer: "cardio" },
                { question: "Which nutrient is essential for strong nails?", options: ["Calcium", "Biotin", "Iron", "Zinc"], answer: "biotin" },
                { question: "What is the primary benefit of a clay mask?", options: ["Hydration", "Oil Absorption", "Brightening", "Anti-Aging"], answer: "oil absorption" },
                { question: "How many hours of sleep are recommended for adults per night?", options: ["4-5", "6-7", "7-9", "10-12"], answer: "7-9" },
                { question: "Which muscle group is targeted by squats?", options: ["Biceps", "Glutes", "Chest", "Shoulders"], answer: "glutes" },
                { question: "What is the main benefit of aloe vera in skincare?", options: ["Exfoliation", "Soothing", "Oil Control", "Brightening"], answer: "soothing" },
                { question: "Which type of workout combines strength and cardio?", options: ["Pilates", "HIIT", "Yoga", "Stretching"], answer: "hiit" },
                { question: "What is the primary source of collagen in the body?", options: ["Protein", "Carbohydrates", "Fats", "Vitamins"], answer: "protein" },
                { question: "Which ingredient is used to treat hyperpigmentation?", options: ["Niacinamide", "Coconut Oil", "Glycerin", "Mineral Oil"], answer: "niacinamide" }
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
                { question: "Which monument is known as the 'Sun Temple' in Odisha?", options: ["Konark Sun Temple", "Brihadeeswara Temple", "Lingaraja Temple", "Jagannath Temple"], answer: "konark sun temple" },
                { question: "Which fort in Rajasthan is famous for its mirror work?", options: ["Mehrangarh Fort", "Amber Fort", "Chittorgarh Fort", "Jaisalmer Fort"], answer: "amber fort" },
                { question: "Which monument in Delhi is a war memorial?", options: ["India Gate", "Red Fort", "Qutub Minar", "Lotus Temple"], answer: "india gate" },
                { question: "Which South Indian temple is known for its 1000-pillar hall?", options: ["Brihadeeswara Temple", "Ramanathaswamy Temple", "Madurai Meenakshi Temple", "Chidambaram Temple"], answer: "ramanathaswamy temple" },
                { question: "Which monument is a stepwell in Gujarat?", options: ["Rani ki Vav", "Adalaj Stepwell", "Chand Baori", "Agrasen ki Baoli"], answer: "rani ki vav" },
                { question: "Which fort in Agra was the main residence of Mughal emperors?", options: ["Agra Fort", "Red Fort", "Gwalior Fort", "Golconda Fort"], answer: "agra fort" },
                { question: "Which monument in Karnataka is known for its monolithic statue of Gommateshwara?", options: ["Hampi", "Shravanabelagola", "Badami Caves", "Pattadakal"], answer: "shravanabelagola" },
                { question: "Which temple in Tamil Nadu is a UNESCO World Heritage Site?", options: ["Brihadeeswara Temple", "Shore Temple", "Meenakshi Temple", "Ramanathaswamy Temple"], answer: "brihadeeswara temple" },
                { question: "Which monument in Hyderabad is known for its four minarets?", options: ["Charminar", "Golconda Fort", "Qutb Shahi Tombs", "Mecca Masjid"], answer: "charminar" },
                { question: "Which monument in Delhi is known for its minaret, the tallest brick minaret in the world?", options: ["Qutub Minar", "India Gate", "Red Fort", "Humayun's Tomb"], answer: "qutub minar" }
            ],
            indianCities: [
                { question: "Which city is known as the 'Pink City'?", options: ["Jaipur", "Jodhpur", "Udaipur", "Bikaner"], answer: "jaipur" },
                { question: "What is the capital of India?", options: ["Mumbai", "Delhi", "Kolkata", "Chennai"], answer: "delhi" },
                { question: "Which city is famous for its IT industry and called the 'Silicon Valley of India'?", options: ["Hyderabad", "Bengaluru", "Pune", "Chennai"], answer: "bengaluru" },
                { question: "Which city is known as the 'City of Joy'?", options: ["Kolkata", "Mumbai", "Delhi", "Chennai"], answer: "kolkata" },
                { question: "Which city is home to the Bollywood film industry?", options: ["Delhi", "Mumbai", "Chennai", "Kolkata"], answer: "mumbai" },
                { question: "Which city is famous for its Charminar?", options: ["Hyderabad", "Lucknow", "Ahmedabad", "Bhopal"], answer: "hyderabad" },
                { question: "Which city is known as the 'Venice of the East'?", options: ["Kochi", "Alappuzha", "Varanasi", "Udaipur"], answer: "alappuzha" },
                { question: "Which city is famous for its Golden Temple?", options: ["Amritsar", "Patna", "Chandigarh", "Ludhiana"], answer: "amritsar" },
                { question: "Which city is known as the 'Manchester of India' for its textile industry?", options: ["Surat", "Ahmedabad", "Coimbatore", "Kanpur"], answer: "ahmedabad" },
                { question: "Which city is famous for its Meenakshi Temple?", options: ["Madurai", "Tiruchirappalli", "Thanjavur", "Kanchipuram"], answer: "madurai" },
                { question: "Which city is known as the 'City of Lakes'?", options: ["Bhopal", "Udaipur", "Srinagar", "Guwahati"], answer: "udaipur" },
                { question: "Which city is the capital of Uttar Pradesh?", options: ["Kanpur", "Lucknow", "Varanasi", "Agra"], answer: "lucknow" },
                { question: "Which city is famous for its tea gardens and toy train?", options: ["Darjeeling", "Shimla", "Ooty", "Munnar"], answer: "darjeeling" },
                { question: "Which city is known as the 'Gateway of South India'?", options: ["Chennai", "Bengaluru", "Hyderabad", "Kochi"], answer: "chennai" },
                { question: "Which city is famous for its ancient ghats along the Ganges?", options: ["Varanasi", "Haridwar", "Rishikesh", "Allahabad"], answer: "varanasi" },
                { question: "Which city is the capital of Gujarat?", options: ["Ahmedabad", "Gandhinagar", "Surat", "Vadodara"], answer: "gandhinagar" },
                { question: "Which city is known for its spice markets and Kochi-Muziris Biennale?", options: ["Kochi", "Thiruvananthapuram", "Kozhikode", "Kollam"], answer: "kochi" },
                { question: "Which city is famous for its Mysore Palace?", options: ["Mysuru", "Bengaluru", "Mangalore", "Hubli"], answer: "mysuru" },
                { question: "Which city is known as the 'City of Pearls' for its pearl trade?", options: ["Hyderabad", "Surat", "Jaipur", "Kolkata"], answer: "hyderabad" },
                { question: "Which city is the capital of Punjab?", options: ["Amritsar", "Ludhiana", "Chandigarh", "Jalandhar"], answer: "chandigarh" }
            ],
            solveRiddle: [
                { question: "What has keys but can't open locks?", options: ["A piano", "A book", "A car", "A clock"], answer: "a piano" },
                { question: "What gets wetter the more it dries?", options: ["A towel", "A sponge", "A bucket", "A mop"], answer: "a towel" },
                { question: "What has a neck but no head?", options: ["A bottle", "A shirt", "A snake", "A rope"], answer: "a shirt" },
                { question: "What can travel around the world while staying in a corner?", options: ["A stamp", "A map", "A globe", "A postcard"], answer: "a stamp" },
                { question: "What has one eye but cannot see?", options: ["A needle", "A potato", "A storm", "A camera"], answer: "a needle" },
                { question: "What has cities but no houses, forests but no trees, and rivers but no water?", options: ["A map", "A painting", "A book", "A dream"], answer: "a map" },
                { question: "What can run but never walks, has a mouth but never talks?", options: ["A river", "A dog", "A wind", "A clock"], answer: "a river" },
                { question: "What has hands but cannot clap?", options: ["A clock", "A statue", "A puppet", "A robot"], answer: "a clock" },
                { question: "What has a heart that doesn’t beat?", options: ["An artichoke", "A stone", "A tree", "A cloud"], answer: "an artichoke" },
                { question: "What is full of holes but still holds water?", options: ["A sponge", "A net", "A bucket", "A sieve"], answer: "a sponge" },
                { question: "What begins with T, ends with T, and has T in it?", options: ["A teapot", "A table", "A tent", "A tree"], answer: "a teapot" },
                { question: "What has a thumb and four fingers but is not alive?", options: ["A glove", "A hand", "A puppet", "A statue"], answer: "a glove" },
                { question: "What can you catch but not throw?", options: ["A cold", "A ball", "A fish", "A wave"], answer: "a cold" },
                { question: "What has a head, a tail, but no body?", options: ["A coin", "A snake", "A tadpole", "A rope"], answer: "a coin" },
                { question: "What goes up but never comes down?", options: ["Your age", "A balloon", "A rocket", "A ladder"], answer: "your age" },
                { question: "What has wings but cannot fly?", options: ["A building", "A butterfly", "A plane", "A bat"], answer: "a building" },
                { question: "What is always in front of you but cannot be seen?", options: ["The future", "The past", "The present", "The horizon"], answer: "the future" },
                { question: "What has a bottom at the top?", options: ["A leg", "A bottle", "A hill", "A ladder"], answer: "a leg" },
                { question: "What can you break without touching it?", options: ["A promise", "A glass", "A bone", "A stick"], answer: "a promise" },
                { question: "What has many teeth but cannot bite?", options: ["A comb", "A shark", "A saw", "A zipper"], answer: "a comb" }
            ],
            birdLocations: [
                { question: "Where is the Indian Peafowl commonly found?", options: ["Himalayas", "Sundarbans", "Indian Subcontinent", "Andaman Islands"], answer: "indian subcontinent" },
                { question: "Which bird, known for its long migratory journey, is found in the Arctic and winters in India?", options: ["Siberian Crane", "Flamingo", "Sarus Crane", "Hornbill"], answer: "siberian crane" },
                { question: "Where is the Great Indian Bustard primarily found?", options: ["Thar Desert", "Western Ghats", "Eastern Himalayas", "Gangetic Plains"], answer: "thar desert" },
                { question: "Which bird is commonly seen in the wetlands of Bharatpur, Rajasthan?", options: ["Painted Stork", "Kingfisher", "Parakeet", "Myna"], answer: "painted stork" },
                { question: "Where is the Malabar Grey Hornbill found?", options: ["Western Ghats", "Deccan Plateau", "Sundarbans", "Himalayas"], answer: "western ghats" },
                { question: "Which bird is a common sight in the mangroves of the Sundarbans?", options: ["Kingfisher", "Heron", "Eagle", "Vulture"], answer: "kingfisher" },
                { question: "Where is the Himalayan Monal, a colorful pheasant, primarily found?", options: ["Himalayas", "Central India", "Coastal Regions", "Deserts"], answer: "himalayas" },
                { question: "Which bird is found in the saline lakes of Kutch, Gujarat?", options: ["Flamingo", "Pelican", "Stork", "Crane"], answer: "flamingo" },
                { question: "Where is the Sarus Crane, the tallest flying bird, commonly found?", options: ["Gangetic Plains", "Western Ghats", "Andaman Islands", "Thar Desert"], answer: "gangetic plains" },
                { question: "Which bird is a resident of the Andaman and Nicobar Islands?", options: ["Nicobar Pigeon", "Parakeet", "Myna", "Owl"], answer: "nicobar pigeon" },
                { question: "Where is the Indian Roller, known for its blue wings, commonly seen?", options: ["Open Grasslands", "Dense Forests", "High Mountains", "Coastal Areas"], answer: "open grasslands" },
                { question: "Which bird is found in the dense forests of the Northeast?", options: ["Great Hornbill", "Flamingo", "Stork", "Crane"], answer: "great hornbill" },
                { question: "Where is the Black-necked Crane found during winter?", options: ["Ladakh", "Kerala", "Rajasthan", "Tamil Nadu"], answer: "ladakh" },
                { question: "Which bird is commonly found in urban areas across India?", options: ["House Sparrow", "Eagle", "Vulture", "Hornbill"], answer: "house sparrow" },
                { question: "Where is the White-throated Kingfisher widely distributed?", options: ["Across India", "Himalayas Only", "Deserts Only", "Coastal Areas Only"], answer: "across india" },
                { question: "Which bird is a winter visitor to the wetlands of Chilika Lake?", options: ["Bar-headed Goose", "Parakeet", "Myna", "Owl"], answer: "bar-headed goose" },
                { question: "Where is the Indian Pitta, known for its vibrant colors, commonly found?", options: ["Central India", "Arctic", "Sundarbans", "Ladakh"], answer: "central india" },
                { question: "Which bird is found in the high-altitude regions of Sikkim?", options: ["Blood Pheasant", "Flamingo", "Stork", "Crane"], answer: "blood pheasant" },
                { question: "Where is the Red-wattled Lapwing commonly seen?", options: ["Farmlands", "Dense Forests", "High Mountains", "Deep Oceans"], answer: "farmlands" },
                { question: "Which bird is a resident of the mangroves in Bhitarkanika, Odisha?", options: ["White-bellied Sea Eagle", "Parakeet", "Myna", "Owl"], answer: "white-bellied sea eagle" }
            ]
        };

        function startQuiz(category) {
            currentCategory = category;
            currentQuestionIndex = 0;
            score = 0;
            shuffledQuestions = shuffleArray(quizzes[category]);
            currentUser = prompt("Enter your name:");
            if (!currentUser) currentUser = "Anonymous";
            showQuestion();
            document.getElementById('quizModal').style.display = 'flex';
        }

        function showQuestion() {
            if (currentQuestionIndex < 10) {
                const question = shuffledQuestions[currentQuestionIndex];
                document.getElementById('quizQuestion').textContent = question.question;
                document.getElementById('optionA').textContent = question.options[0];
                document.getElementById('optionB').textContent = question.options[1];
                document.getElementById('optionC').textContent = question.options[2];
                document.getElementById('optionD').textContent = question.options[3];
                document.getElementById('quizForm').reset();
            } else {
                endQuiz();
            }
        }

        function endQuiz() {
            document.getElementById('quizModal').style.display = 'none';
            scores[currentCategory][currentUser] = score;
            saveScores();
            alert(`Quiz finished! Your score: ${score}/10`);
        }

        document.getElementById('quizForm').addEventListener('submit', function(e) {
            e.preventDefault();
            const selectedOption = document.querySelector('input[name="option"]:checked');
            if (selectedOption) {
                const answerIndex = parseInt(selectedOption.value);
                const correctAnswer = shuffledQuestions[currentQuestionIndex].answer.toLowerCase();
                const selectedAnswer = shuffledQuestions[currentQuestionIndex].options[answerIndex].toLowerCase();
                if (selectedAnswer === correctAnswer) {
                    score++;
                }
                currentQuestionIndex++;
                showQuestion();
            } else {
                alert("Please select an option!");
            }
        });

        loadScores();
    </script>
</body>
</html>
