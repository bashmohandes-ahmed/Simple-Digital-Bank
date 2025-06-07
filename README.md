<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>NextGen Digital Bank</title>
  <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/css/bootstrap.min.css" rel="stylesheet">
  <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;600&display=swap" rel="stylesheet">
  <style>
    body {
      font-family: 'Inter', sans-serif;
      background-image: url('https://images.unsplash.com/photo-1501785888041-af3ef285b470?ixlib=rb-4.0.3&auto=format&fit=crop&w=1920&q=80');
      background-size: cover;
      background-position: center;
      background-repeat: no-repeat;
      backdrop-filter: blur(2px);
    }
    .header {
      background-color: rgba(28, 28, 28, 0.8);
      color: white;
      padding: 1rem;
      text-align: center;
      font-size: 1.8rem;
    }
    .bank-box {
      max-width: 600px;
      margin: 2rem auto;
      padding: 2rem;
      background-color: rgba(255, 255, 255, 0.95);
      border-radius: 16px;
      box-shadow: 0 8px 16px rgba(0, 0, 0, 0.2);
    }
    .bank-box h2 {
      margin-top: 1.5rem;
    }
    .form-control, .btn {
      margin-top: 1rem;
    }
    footer {
      text-align: center;
      margin-top: 3rem;
      font-size: 0.9rem;
      color: white;
      background-color: rgba(0, 0, 0, 0.5);
      padding: 1rem;
    }
  </style>
</head>
<body>
  <div class="header">
    NextGen Digital Bank
  </div>

  <div class="bank-box">
    <input id="username" class="form-control" placeholder="Enter your name">
    <button class="btn btn-primary w-100" onclick="createAccount()">Create Account</button>

    <h2>Your Current Balance</h2>
    <div id="balance" class="mb-3 fw-bold fs-4">$0.00 USD</div>

    <select id="currency" class="form-select">
      <option value="USD">USD</option>
      <option value="EUR">EUR</option>
      <option value="EGP">EGP</option>
      <option value="CNY">CNY</option>
      <option value="RUB">RUB</option>
      <option value="ARS">ARS</option>
    </select>

    <input type="number" id="depositAmount" class="form-control" placeholder="Deposit amount">
    <button class="btn btn-success w-100" onclick="depositMoney()">Deposit</button>

    <input type="number" id="sendAmount" class="form-control" placeholder="Send amount">
    <button class="btn btn-danger w-100" onclick="sendMoney()">Send Money</button>

    <textarea id="messageBox" class="form-control" placeholder="Write a message to send"></textarea>
    <button class="btn btn-info w-100" onclick="sendMessage()">Send Message</button>

    <button class="btn btn-secondary w-100" onclick="viewTransactions()">Transactions</button>
    <button class="btn btn-dark w-100" onclick="logout()">Logout</button>
  </div>

  <footer>
    Powered by NextGenBank | Multilingual: EN, AR, FR, DE, ES, RU, CN, IT
  </footer>

  <script>
    let balance = 0.0;
    let currency = 'USD';
    let transactions = [];
    let user = localStorage.getItem('simple_user') || '';

    if (user) document.getElementById('username').value = user;

    function createAccount() {
      user = document.getElementById('username').value.trim();
      if (!user) return alert("Please enter your name.");
      localStorage.setItem('simple_user', user);
      alert("Welcome, " + user);
    }

    function updateBalanceDisplay() {
      document.getElementById('balance').innerText = `${balance.toFixed(2)} ${currency}`;
    }

    function depositMoney() {
      let amount = parseFloat(document.getElementById('depositAmount').value);
      if (isNaN(amount) || amount <= 0) return alert("Enter a valid deposit amount.");
      balance += amount;
      transactions.push(`Deposited ${amount} ${currency}`);
      updateBalanceDisplay();
    }

    function sendMoney() {
      let amount = parseFloat(document.getElementById('sendAmount').value);
      if (isNaN(amount) || amount <= 0) return alert("Enter a valid send amount.");
      if (amount > balance) return alert("Insufficient funds.");
      balance -= amount;
      transactions.push(`Sent ${amount} ${currency}`);
      updateBalanceDisplay();
    }

    function sendMessage() {
      const message = document.getElementById('messageBox').value.trim();
      if (!message) return alert("Enter a message to send.");
      transactions.push(`Message sent: "${message}"`);
      alert("Message sent!");
      document.getElementById('messageBox').value = '';
    }

    function viewTransactions() {
      if (transactions.length === 0) return alert("No transactions yet.");
      alert(transactions.join("\n"));
    }

    function logout() {
      localStorage.removeItem('simple_user');
      user = '';
      document.getElementById('username').value = '';
      alert("Logged out.");
    }

    document.getElementById('currency').addEventListener('change', function (e) {
      currency = e.target.value;
      updateBalanceDisplay();
    });
  </script>
</body>
</html>
