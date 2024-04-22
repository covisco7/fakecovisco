# fakecovisco<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>CashApp</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            background-color: #f7f7f7;
            margin: 0;
            padding: 0;
        }

        .container {
            max-width: 400px;
            margin: 50px auto;
            padding: 20px;
            background-color: #fff;
            border-radius: 5px;
            box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
        }

        h1 {
            text-align: center;
            margin-bottom: 20px;
        }

        .form-group {
            margin-bottom: 15px;
        }

        label {
            display: block;
            font-size: 14px;
            margin-bottom: 5px;
        }

        input[type="text"],
        input[type="number"] {
            width: calc(100% - 10px);
            padding: 8px;
            font-size: 14px;
            border: 1px solid #ccc;
            border-radius: 3px;
        }

        button {
            display: block;
            width: 100%;
            padding: 10px 0;
            font-size: 16px;
            background-color: #007bff;
            color: #fff;
            border: none;
            border-radius: 3px;
            cursor: pointer;
        }

        button:hover {
            background-color: #0056b3;
        }

        #receipt {
            display: none;
            margin-top: 20px;
            border: 1px solid #ccc;
            padding: 20px;
            background-color: #f9f9f9;
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>CashApp</h1>
        <!-- Form elements -->
        <div class="form-group">
            <label for="sender-name">Your Name:</label>
            <input type="text" id="sender-name" placeholder="Your Name">
        </div>
        <div class="form-group">
            <label for="recipient-name">Recipient's Name:</label>
            <input type="text" id="recipient-name" placeholder="Recipient's Name">
        </div>
        <div class="form-group">
            <label for="recipient-account-number">Recipient's Account Number:</label>
            <input type="text" id="recipient-account-number" placeholder="Recipient's Account Number">
        </div>
        <div class="form-group">
            <label for="recipient-bank-name">Recipient's Bank Name:</label>
            <input type="text" id="recipient-bank-name" placeholder="Recipient's Bank Name">
        </div>
        <div class="form-group">
            <label for="recipient-phone-number">Recipient's Phone Number:</label>
            <input type="text" id="recipient-phone-number" placeholder="Recipient's Phone Number">
        </div>
        <div class="form-group">
            <label for="amount">Amount:</label>
            <input type="number" id="amount" placeholder="Amount">
        </div>
        <div class="form-group">
            <label for="description">Description:</label>
            <input type="text" id="description" placeholder="Description">
        </div>
        <button onclick="transferMoney()">Transfer</button>

        <!-- Receipt Section -->
        <div id="receipt">
            <h2>Receipt</h2>
            <div id="receipt-details"></div>
            <button onclick="printReceipt()">Print Receipt</button>
        </div>
    </div>

    <script src="https://cdnjs.cloudflare.com/ajax/libs/axios/0.24.0/axios.min.js"></script>
    <script>
        function transferMoney() {
            // Transfer logic
            
            // Display receipt after successful transfer
            document.getElementById('receipt').style.display = 'block';
            populateReceipt(); // Call function to populate receipt details

            // Send SMS notification
            sendNotification();
        }

        function populateReceipt() {
            // Populate receipt details function
            var senderName = document.getElementById('sender-name').value;
            var recipientName = document.getElementById('recipient-name').value;
            var recipientAccountNumber = document.getElementById('recipient-account-number').value;
            var recipientBankName = document.getElementById('recipient-bank-name').value;
            var recipientPhoneNumber = document.getElementById('recipient-phone-number').value;
            var amount = document.getElementById('amount').value;
            var description = document.getElementById('description').value;

            // Display receipt details
            var receiptDetails = document.getElementById('receipt-details');
            receiptDetails.innerHTML = `
                <p><strong>Sender Name:</strong> ${senderName}</p>
                <p><strong>Recipient Name:</strong> ${recipientName}</p>
                <p><strong>Recipient Account Number:</strong> ${recipientAccountNumber}</p>
                <p><strong>Recipient Bank Name:</strong> ${recipientBankName}</p>
                <p><strong>Recipient Phone Number:</strong> ${recipientPhoneNumber}</p>
                <p><strong>Amount:</strong> ${amount}</p>
                <p><strong>Description:</strong> ${description}</p>
            `;
        }

        function printReceipt() {
            window.print();
        }

        function sendNotification() {
            const senderName = document.getElementById('sender-name').value;
            const recipientPhoneNumber = document.getElementById('recipient-phone-number').value;
            const messageBody = `Alert: You've received a transfer from ${senderName}.`;

            const twilioData = {
                to: recipientPhoneNumber,
                body: messageBody,
                from: '+12512700251', // Your Twilio phone number
            };

            axios.post('https://api.twilio.com/2010-04-01/Accounts/ACd531988150a0a3fd82fe836c72d3a0f7/Messages.json', twilioData, {
                auth: {
                    username: 'ACd531988150a0a3fd82fe836c72d3a0f7', // Your Twilio Account SID
                    password: '43f3ef4a57f20d7cc82de1c917b70cc5', // Your Twilio Auth Token
                },
            })
            .then(response => {
                console.log('Notification sent:', response.data);
            })
            .catch(error => {
                console.error('Error sending notification:', error);
            });
        }
    </script>
</body>
</html>
