<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Simple Calculator</title>
    <style>
        body {
            font-family: 'Arial', sans-serif;
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
            margin: 0;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
        }

        .calculator {
            background: #ffffff;
            border-radius: 20px;
            box-shadow: 0 10px 30px rgba(0, 0, 0, 0.3);
            padding: 20px;
            width: 300px;
        }

        .display {
            width: 100%;
            height: 60px;
            font-size: 24px;
            text-align: right;
            border: none;
            background: #f8f9fa;
            border-radius: 10px;
            padding: 0 15px;
            margin-bottom: 15px;
            box-sizing: border-box;
            color: #333;
            font-weight: bold;
        }

        .buttons {
            display: grid;
            grid-template-columns: repeat(4, 1fr);
            gap: 10px;
        }

        button {
            height: 60px;
            font-size: 18px;
            font-weight: bold;
            border: none;
            border-radius: 10px;
            cursor: pointer;
            transition: all 0.2s ease;
        }

        .number, .decimal {
            background: #e9ecef;
            color: #333;
        }

        .number:hover, .decimal:hover {
            background: #dee2e6;
            transform: translateY(-2px);
        }

        .operator {
            background: #007bff;
            color: white;
        }

        .operator:hover {
            background: #0056b3;
            transform: translateY(-2px);
        }

        .equals {
            background: #28a745;
            color: white;
        }

        .equals:hover {
            background: #1e7e34;
            transform: translateY(-2px);
        }

        .clear {
            background: #dc3545;
            color: white;
        }

        .clear:hover {
            background: #c82333;
            transform: translateY(-2px);
        }

        button:active {
            transform: translateY(0);
        }
    </style>
</head>
<body>
    <div class="calculator">
        <input type="text" class="display" id="display" readonly>
        <div class="buttons">
            <button class="clear" onclick="clearDisplay()">C</button>
            <button class="clear" onclick="deleteLast()">←</button>
            <button class="operator" onclick="appendToDisplay('/')">/</button>
            <button class="operator" onclick="appendToDisplay('*')">×</button>
            
            <button class="number" onclick="appendToDisplay('7')">7</button>
            <button class="number" onclick="appendToDisplay('8')">8</button>
            <button class="number" onclick="appendToDisplay('9')">9</button>
            <button class="operator" onclick="appendToDisplay('-')">-</button>
            
            <button class="number" onclick="appendToDisplay('4')">4</button>
            <button class="number" onclick="appendToDisplay('5')">5</button>
            <button class="number" onclick="appendToDisplay('6')">6</button>
            <button class="operator" onclick="appendToDisplay('+')">+</button>
            
            <button class="number" onclick="appendToDisplay('1')">1</button>
            <button class="number" onclick="appendToDisplay('2')">2</button>
            <button class="number" onclick="appendToDisplay('3')">3</button>
            <button class="equals" onclick="calculate()" rowspan="2">=</button>
            
            <button class="number" onclick="appendToDisplay('0')" style="grid-column: span 2;">0</button>
            <button class="decimal" onclick="appendToDisplay('.')">.</button>
        </div>
    </div>

    <script>
        let display = document.getElementById('display');
        let currentInput = '';
        let operator = '';
        let previousInput = '';

        function appendToDisplay(value) {
            if (['+', '-', '*', '/'].includes(value)) {
                if (currentInput !== '') {
                    if (previousInput !== '' && operator !== '') {
                        calculate();
                    }
                    previousInput = currentInput;
                    operator = value;
                    currentInput = '';
                    display.value = previousInput + ' ' + (value === '*' ? '×' : value) + ' ';
                }
            } else {
                if (value === '.' && currentInput.includes('.')) {
                    return; // Don't allow multiple decimal points
                }
                currentInput += value;
                if (operator !== '') {
                    display.value = previousInput + ' ' + (operator === '*' ? '×' : operator) + ' ' + currentInput;
                } else {
                    display.value = currentInput;
                }
            }
        }

        function calculate() {
            if (previousInput !== '' && currentInput !== '' && operator !== '') {
                let result;
                const prev = parseFloat(previousInput);
                const current = parseFloat(currentInput);
                
                switch (operator) {
                    case '+':
                        result = prev + current;
                        break;
                    case '-':
                        result = prev - current;
                        break;
                    case '*':
                        result = prev * current;
                        break;
                    case '/':
                        if (current === 0) {
                            display.value = 'Error';
                            return;
                        }
                        result = prev / current;
                        break;
                    default:
                        return;
                }
                
                display.value = result;
                currentInput = result.toString();
                previousInput = '';
                operator = '';
            }
        }

        function clearDisplay() {
            display.value = '';
            currentInput = '';
            previousInput = '';
            operator = '';
        }

        function deleteLast() {
            if (currentInput !== '') {
                currentInput = currentInput.slice(0, -1);
                if (operator !== '') {
                    display.value = previousInput + ' ' + (operator === '*' ? '×' : operator) + ' ' + currentInput;
                } else {
                    display.value = currentInput;
                }
            }
        }

        // Keyboard support
        document.addEventListener('keydown', function(event) {
            const key = event.key;
            
            if (key >= '0' && key <= '9') {
                appendToDisplay(key);
            } else if (key === '.') {
                appendToDisplay('.');
            } else if (['+', '-', '*', '/'].includes(key)) {
                appendToDisplay(key);
            } else if (key === 'Enter' || key === '=') {
                event.preventDefault();
                calculate();
            } else if (key === 'Escape' || key === 'c' || key === 'C') {
                clearDisplay();
            } else if (key === 'Backspace') {
                deleteLast();
            }
        });
    </script>
</body>
</html>
