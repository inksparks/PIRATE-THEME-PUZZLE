<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Pirate Treasure Puzzle</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            text-align: center;
            background-color: #0a0f1e;
            color: white;
            position: relative;
            height: 100vh;
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
        }
        .puzzle-container {
            display: grid;
            grid-template-columns: repeat(3, 100px);
            grid-template-rows: repeat(3, 100px);
            gap: 2px;
            width: 306px;
            background: black;
            position: relative;
            left: 75px; /* Adjusted for alignment */
        }
        .puzzle-piece {
            width: 100px;
            height: 100px;
            background-size: 300px 300px;
            cursor: pointer;
            border: 1px solid white;
        }
        .hidden {
            background-color: black;
            border: none;
        }
        .message {
            display: none;
            font-size: 22px;
            font-weight: bold;
            color: white;
            margin-top: 20px;
        }
        .full-image {
            display: none;
            width: 300px;
            height: 300px;
            margin-top: 20px;
        }
        .congrats-message {
            display: none;
            font-size: 18px;
            color: #FFD700;
            margin-top: 10px;
        }
    </style>
</head>
<body>
    <h2>Solve the Puzzle to Reveal the Treasure!</h2>
    <div class="puzzle-container" id="puzzle-container"></div>
    <div class="message" id="message">Congratulations!</div>
    <div class="congrats-message" id="congrats-message">Congratulations! You have unlocked the treasure: <a href="#" style="color: #FFD700; text-decoration: underline;">Sample Link</a></div>
    <img src="pirate.png.webp" alt="Complete Puzzle" class="full-image" id="fullImage">
    <script>
        const puzzleContainer = document.getElementById('puzzle-container');
        const message = document.getElementById('message');
        const fullImage = document.getElementById('fullImage');
        const congratsMessage = document.getElementById('congrats-message');
        
        let positions = [
            [0, 0], [100, 0], [200, 0],
            [0, 100], [100, 100], [200, 100],
            [0, 200], [100, 200], [null, null]
        ];
        
        let shuffledPositions = [...positions];
        
        function shufflePuzzle() {
            do {
                shuffledPositions = [...positions].sort(() => Math.random() - 0.5);
            } while (!isSolvable(shuffledPositions));
        }

        function isSolvable(puzzle) {
            let flatArray = puzzle.flat().filter(num => num !== null);
            let inversions = 0;
            for (let i = 0; i < flatArray.length; i++) {
                for (let j = i + 1; j < flatArray.length; j++) {
                    if (flatArray[i] > flatArray[j]) inversions++;
                }
            }
            return inversions % 2 === 0;
        }
        
        function createPuzzle() {
            puzzleContainer.innerHTML = '';
            shuffledPositions.forEach((pos, index) => {
                let piece = document.createElement('div');
                piece.classList.add('puzzle-piece');
                if (pos[0] !== null) {
                    piece.style.backgroundImage = "url('pirate.png.webp')";
                    piece.style.backgroundPosition = `-${pos[0]}px -${pos[1]}px`;
                } else {
                    piece.classList.add('hidden');
                }
                piece.dataset.index = index;
                piece.addEventListener('click', () => movePiece(index));
                puzzleContainer.appendChild(piece);
            });
        }

        function movePiece(index) {
            let emptyIndex = shuffledPositions.findIndex(pos => pos[0] === null);
            let validMoves = [emptyIndex - 1, emptyIndex + 1, emptyIndex - 3, emptyIndex + 3];
            if (validMoves.includes(index)) {
                [shuffledPositions[index], shuffledPositions[emptyIndex]] = [shuffledPositions[emptyIndex], shuffledPositions[index]];
                createPuzzle();
                checkWin();
            }
        }
        
        function checkWin() {
            if (JSON.stringify(shuffledPositions) === JSON.stringify(positions)) {
                message.style.display = 'block';
                puzzleContainer.style.display = 'none';
                fullImage.style.display = 'block';
                congratsMessage.style.display = 'block';
            }
        }
        
        shufflePuzzle();
        createPuzzle();
    </script>
</body>
</html>

