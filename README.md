# Expense--tracker-
Help in gaining better financial life. Create a simple expense tracker using HTML, CSS, and JavaScript. By the end, helping you gain better control over your spending.
# Expense Tracker

A simple, responsive web-based expense tracker application built with HTML, CSS (Tailwind CSS), and Vanilla JavaScript. This project allows users to add and delete income and expense transactions and see their balance, income, and total expenses updated in real-time.

---

### Features

- **Real-time Balance Update:** The total balance is automatically calculated and displayed.
- **Income/Expense Tracking:** Separates income and expense totals for easy financial overview.
- **Transaction History:** A list of all transactions is maintained and displayed.
- **Add/Delete Transactions:** Users can add new transactions and remove existing ones from the history.
- **Simple UI:** A clean and modern user interface styled with Tailwind CSS.

---

### Technologies Used

- **HTML5:** For the basic structure of the application.
- **Tailwind CSS:** For all styling and a responsive design.
- **JavaScript (ES6):** For all the application's logic and DOM manipulation.

---

### Project Setup

There are no complex build tools or dependencies required to run this project. You can get it running in a few simple steps.

#### **Prerequisites**

You only need a modern web browser (like Chrome, Firefox, or Edge) to run this application.

#### **Installation**

1.  **Clone the repository:**
    ```bash
    git clone [https://github.com/your-username/expense-tracker.git](https://github.com/your-username/expense-tracker.git)
    ```
    Replace `your-username` with your actual GitHub username.

2.  **Navigate to the project directory:**
    ```bash
    cd expense-tracker
    ```

3.  **Open the `index.html` file:**
    Simply double-click the `index.html` file in your file explorer, and it will open in your default web browser.

---

### Usage

- **Adding a Transaction:**
    - Enter a description in the **"Description"** field.
    - Enter the amount in the **"Amount"** field.
    - For an **expense**, enter a **negative** number (e.g., `-50`).
    - For an **income**, enter a **positive** number (e.g., `300`).
    - Click the **"Add Transaction"** button.

- **Deleting a Transaction:**
    - Hover over a transaction in the history list.
    - Click the **`x`** button that appears on the right side.
    - <!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Expense Tracker</title>
    <!-- Tailwind CSS CDN for styling -->
    <script src="https://cdn.tailwindcss.com"></script>
    <!-- Use Inter font for a modern look -->
    <style>
        body {
            font-family: 'Inter', sans-serif;
            background-color: #f7fafc;
            display: flex;
            justify-content: center;
            align-items: flex-start;
            min-height: 100vh;
            padding: 2rem;
        }
        .container {
            max-width: 480px;
            width: 100%;
            background: white;
            border-radius: 1.5rem;
            box-shadow: 0 10px 15px -3px rgba(0, 0, 0, 0.1), 0 4px 6px -2px rgba(0, 0, 0, 0.05);
            padding: 2.5rem;
        }
        .btn {
            @apply w-full bg-blue-500 text-white font-bold py-3 px-4 rounded-lg transition-transform transform active:scale-95 hover:bg-blue-600;
        }
        .list-item {
            @apply flex justify-between items-center bg-white p-4 my-2 rounded-lg shadow-sm border-l-4;
            transition: all 0.3s ease;
        }
        .list-item-plus {
            border-left-color: #10B981; /* Tailwind's green-500 */
        }
        .list-item-minus {
            border-left-color: #EF4444; /* Tailwind's red-500 */
        }
        .delete-btn {
            @apply bg-red-500 text-white w-6 h-6 flex items-center justify-center rounded-full text-xs font-bold transition-all opacity-0 group-hover:opacity-100;
        }
        .balance-box, .inc-exp-box {
            @apply bg-gray-100 p-6 rounded-lg;
        }
    </style>
</head>
<body class="bg-gray-100 p-8">

    <!-- Main Container -->
    <div class="container">
        <!-- Title -->
        <h1 class="text-3xl font-bold text-center mb-6 text-gray-800">Expense Tracker</h1>

        <!-- Balance Display -->
        <div class="balance-box mb-6">
            <h2 class="text-xl font-semibold text-gray-600 mb-2">Your Balance</h2>
            <h1 id="balance" class="text-4xl font-extrabold text-gray-900">$0.00</h1>
        </div>
        
        <!-- Income and Expense Summary -->
        <div class="inc-exp-box flex justify-between mb-6">
            <div class="flex-1 text-center border-r border-gray-300 pr-4">
                <h3 class="text-md font-medium text-gray-500">Income</h3>
                <p id="money-plus" class="text-2xl font-bold text-green-500 mt-1">$0.00</p>
            </div>
            <div class="flex-1 text-center pl-4">
                <h3 class="text-md font-medium text-gray-500">Expense</h3>
                <p id="money-minus" class="text-2xl font-bold text-red-500 mt-1">$0.00</p>
            </div>
        </div>

        <!-- Transaction History -->
        <h3 class="text-xl font-semibold border-b pb-2 mb-4 text-gray-700">History</h3>
        <ul id="list" class="list-none p-0 mb-6">
            <!-- Transactions will be added here dynamically -->
        </ul>

        <!-- Add New Transaction Form -->
        <h3 class="text-xl font-semibold border-b pb-2 mb-4 text-gray-700">Add New Transaction</h3>
        <form id="form" class="space-y-4">
            <div>
                <label for="text" class="block text-sm font-medium text-gray-700">Description</label>
                <input type="text" id="text" placeholder="Enter description..."
                    class="mt-1 block w-full rounded-md border-gray-300 shadow-sm focus:border-blue-500 focus:ring-blue-500 p-3 border-2" />
            </div>
            <div>
                <label for="amount" class="block text-sm font-medium text-gray-700">Amount
                    <br />
                    (negative - expense, positive - income)</label>
                <input type="number" id="amount" placeholder="Enter amount..."
                    class="mt-1 block w-full rounded-md border-gray-300 shadow-sm focus:border-blue-500 focus:ring-blue-500 p-3 border-2" />
            </div>
            <button class="btn">Add Transaction</button>
        </form>
    </div>

    <!-- JavaScript for functionality -->
    <script>
        // Get DOM elements
        const balance = document.getElementById('balance');
        const money_plus = document.getElementById('money-plus');
        const money_minus = document.getElementById('money-minus');
        const list = document.getElementById('list');
        const form = document.getElementById('form');
        const text = document.getElementById('text');
        const amount = document.getElementById('amount');

        // Sample transactions
        const dummyTransactions = [
            { id: 1, text: 'Groceries', amount: -50 },
            { id: 2, text: 'Salary', amount: 3000 },
            { id: 3, text: 'Book', amount: -20 },
            { id: 4, text: 'Freelance work', amount: 500 }
        ];

        let transactions = dummyTransactions;

        // Function to generate a random ID
        function generateID() {
            return Math.floor(Math.random() * 100000000);
        }

        // Function to add a transaction to the DOM
        function addTransactionDOM(transaction) {
            // Determine sign for styling
            const sign = transaction.amount < 0 ? '-' : '+';
            const item = document.createElement('li');

            // Add class based on sign
            item.classList.add('list-item', 'group', transaction.amount < 0 ? 'list-item-minus' : 'list-item-plus');

            item.innerHTML = `
                ${transaction.text} <span>${sign}$${Math.abs(transaction.amount).toFixed(2)}</span>
                <button class="delete-btn" onclick="removeTransaction(${transaction.id})">x</button>
            `;

            list.appendChild(item);
        }

        // Function to update the balance, income, and expense
        function updateValues() {
            const amounts = transactions.map(transaction => transaction.amount);

            // Calculate total balance
            const total = amounts.reduce((acc, item) => (acc += item), 0).toFixed(2);

            // Calculate income
            const income = amounts
                .filter(item => item > 0)
                .reduce((acc, item) => (acc += item), 0)
                .toFixed(2);

            // Calculate expenses
            const expense = (
                amounts.filter(item => item < 0).reduce((acc, item) => (acc += item), 0) * -1
            ).toFixed(2);

            balance.innerText = `$${total}`;
            money_plus.innerText = `$${income}`;
            money_minus.innerText = `$${expense}`;
        }

        // Function to remove a transaction by ID
        function removeTransaction(id) {
            transactions = transactions.filter(transaction => transaction.id !== id);
            init();
        }

        // Event listener for form submission
        form.addEventListener('submit', (e) => {
            e.preventDefault(); // Prevent default form submission

            if (text.value.trim() === '' || amount.value.trim() === '') {
                alert('Please add a description and amount'); // Simple alert for validation
            } else {
                const transaction = {
                    id: generateID(),
                    text: text.value,
                    amount: +amount.value // Convert string to number
                };

                transactions.push(transaction);
                addTransactionDOM(transaction);
                updateValues();

                // Clear input fields
                text.value = '';
                amount.value = '';
            }
        });

        // Initialize the app
        function init() {
            list.innerHTML = ''; // Clear the list
            transactions.forEach(addTransactionDOM);
            updateValues();
        }

        // Initial call to start the app
        init();
    </script>

</body>
</html>

