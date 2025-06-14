<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Simple Calculator</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <style>
        .calculator {
            box-shadow: 0 10px 25px rgba(0, 0, 0, 0.1);
            transition: all 0.3s ease;
        }
        .calculator:hover {
            box-shadow: 0 15px 30px rgba(0, 0, 0, 0.15);
        }
        .btn {
            transition: all 0.2s ease;
        }
        .btn:hover {
            transform: translateY(-2px);
        }
        .btn:active {
            transform: translateY(0);
        }
    </style>
</head>
<body class="bg-gray-100 min-h-screen flex items-center justify-center p-4">
    <div class="calculator bg-white rounded-2xl overflow-hidden w-full max-w-xs">
        <!-- Display -->
        <div class="bg-gray-800 p-4 text-right">
            <div id="previous-operand" class="text-gray-400 text-sm h-5"></div>
            <div id="current-operand" class="text-white text-3xl font-semibold truncate">0</div>
        </div>
        
        <!-- Buttons -->
        <div class="grid grid-cols-4 gap-1 p-2 bg-gray-100">
            <!-- Row 1 -->
            <button onclick="clearAll()" class="btn bg-red-500 hover:bg-red-600 text-white col-span-2 p-4 font-medium">AC</button>
            <button onclick="deleteLastChar()" class="btn bg-gray-300 hover:bg-gray-400 p-4 font-medium">⌫</button>
            <button onclick="appendOperator('/')" class="btn bg-amber-500 hover:bg-amber-600 text-white p-4 font-medium">÷</button>
            
            <!-- Row 2 -->
            <button onclick="appendNumber('7')" class="btn bg-gray-200 hover:bg-gray-300 p-4 font-medium">7</button>
            <button onclick="appendNumber('8')" class="btn bg-gray-200 hover:bg-gray-300 p-4 font-medium">8</button>
            <button onclick="appendNumber('9')" class="btn bg-gray-200 hover:bg-gray-300 p-4 font-medium">9</button>
            <button onclick="appendOperator('*')" class="btn bg-amber-500 hover:bg-amber-600 text-white p-4 font-medium">×</button>
            
            <!-- Row 3 -->
            <button onclick="appendNumber('4')" class="btn bg-gray-200 hover:bg-gray-300 p-4 font-medium">4</button>
            <button onclick="appendNumber('5')" class="btn bg-gray-200 hover:bg-gray-300 p-4 font-medium">5</button>
            <button onclick="appendNumber('6')" class="btn bg-gray-200 hover:bg-gray-300 p-4 font-medium">6</button>
            <button onclick="appendOperator('-')" class="btn bg-amber-500 hover:bg-amber-600 text-white p-4 font-medium">-</button>
            
            <!-- Row 4 -->
            <button onclick="appendNumber('1')" class="btn bg-gray-200 hover:bg-gray-300 p-4 font-medium">1</button>
            <button onclick="appendNumber('2')" class="btn bg-gray-200 hover:bg-gray-300 p-4 font-medium">2</button>
            <button onclick="appendNumber('3')" class="btn bg-gray-200 hover:bg-gray-300 p-4 font-medium">3</button>
            <button onclick="appendOperator('+')" class="btn bg-amber-500 hover:bg-amber-600 text-white p-4 font-medium">+</button>
            
            <!-- Row 5 -->
            <button onclick="appendNumber('0')" class="btn bg-gray-200 hover:bg-gray-300 p-4 font-medium">0</button>
            <button onclick="appendNumber('.')" class="btn bg-gray-200 hover:bg-gray-300 p-4 font-medium">.</button>
            <button onclick="calculate()" class="btn bg-green-500 hover:bg-green-600 text-white col-span-2 p-4 font-medium">=</button>
        </div>
    </div>

    <script>
        let currentOperand = '0';
        let previousOperand = '';
        let operation = null;
        let resetScreen = false;

        const currentOperandElement = document.getElementById('current-operand');
        const previousOperandElement = document.getElementById('previous-operand');

        function updateDisplay() {
            currentOperandElement.innerText = currentOperand;
            previousOperandElement.innerText = previousOperand + (operation ? ` ${operation}` : '');
        }

        function appendNumber(number) {
            if (currentOperand === '0' || resetScreen) {
                currentOperand = number;
                resetScreen = false;
            } else {
                currentOperand += number;
            }
            updateDisplay();
        }

        function appendOperator(operator) {
            if (operation !== null) calculate();
            previousOperand = currentOperand;
            operation = operator;
            resetScreen = true;
            updateDisplay();
        }

        function calculate() {
            let result;
            const prev = parseFloat(previousOperand);
            const current = parseFloat(currentOperand);
            
            if (isNaN(prev) || isNaN(current)) return;
            
            switch (operation) {
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
                    result = prev / current;
                    break;
                default:
                    return;
            }
            
            currentOperand = result.toString();
            operation = null;
            previousOperand = '';
            resetScreen = true;
            updateDisplay();
        }

        function clearAll() {
            currentOperand = '0';
            previousOperand = '';
            operation = null;
            updateDisplay();
        }

        function deleteLastChar() {
            if (currentOperand.length === 1 || (currentOperand.length === 2 && currentOperand.startsWith('-'))) {
                currentOperand = '0';
            } else {
                currentOperand = currentOperand.slice(0, -1);
            }
            updateDisplay();
        }

        // Initialize display
        updateDisplay();
    </script>
</body>
</html>
