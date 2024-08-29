# Task2
Creating a quiz using HTML, CSS, and JavaScript
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Interactive Quiz</title>
    <style>
        body {
            font-family: 'Arial', sans-serif;
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
            margin: 0;
            background: linear-gradient(to right, #e0c3fc, #8ec5fc);
        }

        #quiz-container {
            background: #ffffff;
            padding: 20px;
            border-radius: 15px;
            box-shadow: 0 8px 16px rgba(0, 0, 0, 0.2);
            text-align: center;
            max-width: 600px;
            width: 100%;
        }

        h2 {
            color: #333;
            margin-bottom: 20px;
        }

        .hidden {
            display: none;
        }

        button {
            background-color: #5cb85c;
            color: white;
            border: none;
            padding: 12px 25px;
            border-radius: 5px;
            cursor: pointer;
            font-size: 16px;
            transition: background-color 0.3s ease;
        }

        button:hover {
            background-color: #4cae4c;
        }

        button:disabled {
            background-color: #aaa;
            cursor: not-allowed;
        }

        #answers label {
            display: block;
            margin: 10px 0;
            padding: 10px;
            background-color: #f7f7f7;
            border-radius: 5px;
            cursor: pointer;
            transition: background-color 0.3s ease;
            border: 1px solid #ddd;
        }

        #answers label:hover {
            background-color: #e0e0e0;
        }

        #answers input {
            margin-right: 10px;
        }

        #result-container h2 {
            font-size: 24px;
            color: #5cb85c;
        }
    </style>
</head>
<body>
    <div id="quiz-container">
        <div id="question-container">
            <h2 id="question"></h2>
            <div id="answers"></div>
        </div>
        <button id="next-button" disabled>Next</button>
        <div id="result-container" class="hidden">
            <h2>Your Score: <span id="score"></span></h2>
        </div>
    </div>
    <script>
        const questions = [
            {
                question: "What is the capital of France?",
                answers: ["Paris", "London", "Berlin", "Madrid"],
                correct: "Paris"
            },
            {
                question: "What is 2 + 2?",
                answers: ["3", "4", "5", "6"],
                correct: "4"
            },
            {
                question: "Which planet is known as the Red Planet?",
                answers: ["Earth", "Mars", "Jupiter", "Saturn"],
                correct: "Mars"
            }
        ];

        let currentQuestionIndex = 0;
        let score = 0;

        const questionElement = document.getElementById('question');
        const answersElement = document.getElementById('answers');
        const nextButton = document.getElementById('next-button');
        const resultContainer = document.getElementById('result-container');
        const scoreElement = document.getElementById('score');

        function loadQuestion() {
            const currentQuestion = questions[currentQuestionIndex];
            questionElement.textContent = currentQuestion.question;
            answersElement.innerHTML = '';

            currentQuestion.answers.forEach(answer => {
                const answerId = `answer-${answer}`;
                answersElement.innerHTML += `
                    <label>
                        <input type="radio" name="answer" value="${answer}" id="${answerId}">
                        ${answer}
                    </label>
                `;
            });

            nextButton.disabled = true;
            const inputs = document.querySelectorAll('input[name="answer"]');
            inputs.forEach(input => {
                input.addEventListener('change', () => {
                    nextButton.disabled = false;
                });
            });
        }

        function checkAnswer() {
            const selectedAnswer = document.querySelector('input[name="answer"]:checked');
            if (selectedAnswer) {
                const answerValue = selectedAnswer.value;
                if (answerValue === questions[currentQuestionIndex].correct) {
                    score++;
                }
            }
        }

        function showResult() {
            questionElement.textContent = '';
            answersElement.innerHTML = '';
            nextButton.classList.add('hidden');
            resultContainer.classList.remove('hidden');
            scoreElement.textContent = `${score} out of ${questions.length}`;
        }

        nextButton.addEventListener('click', () => {
            checkAnswer();
            currentQuestionIndex++;

            if (currentQuestionIndex < questions.length) {
                loadQuestion();
            } else {
                showResult();
            }
        });

        loadQuestion();
    </script>
</body>
</html>
