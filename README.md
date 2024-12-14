<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Inah's Store</title>
    <link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/4.3.1/css/bootstrap.min.css">
    <style>
        .container {
            margin-top: 20px;
        }
        .total-label {
            font-weight: bold;
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>Inah's Store</h1>
        <div class="card">
            <div class="card-header">Item Details</div>
            <div class="card-body">
                <div class="form-group">
                    <label for="item_name">Item Name:</label>
                    <input type="text" class="form-control" id="item_name">
                </div>
                <div class="form-group">
                    <label for="item_price">Item Price:</label>
                    <input type="number" class="form-control" id="item_price">
                </div>
                <div class="form-group">
                    <label for="quantity">Quantity:</label>
                    <input type="number" class="form-control" id="quantity">
                </div>
                <button class="btn btn-primary" onclick="addItem()">Add Item</button>
            </div>
        </div>
        <div class="card mt-3">
            <div class="card-header">Shopping Cart</div>
            <div class="card-body">
                <ul class="list-group" id="cart_list"></ul>
                <div class="mt-3">
                    <span class="total-label">Total:</span>
                    <span id="total">Php 0.00</span>
                </div>
                <button class="btn btn-success mt-3" onclick="generateReceipt()">Generate Receipt</button>
            </div>
        </div>
    </div>
    <script>
        let items = [];

        function addItem() {
            const itemName = document.getElementById('item_name').value;
            const itemPrice = parseFloat(document.getElementById('item_price').value);
            const quantity = parseInt(document.getElementById('quantity').value);

            if (itemName && itemPrice > 0 && quantity > 0) {
                const item = { item_name: itemName, item_price: itemPrice, quantity: quantity };

                // Simulate backend response for now
                const total = items.reduce((sum, it) => sum + it.item_price * it.quantity, 0) + (itemPrice * quantity);
                items.push(item);
                updateCart();
                updateTotal(total);
                clearForm();
            } else {
                alert('Please fill out all fields correctly.');
            }
        }

        function removeItem(index) {
            if (index > -1 && index < items.length) {
                items.splice(index, 1);
                const total = items.reduce((sum, it) => sum + it.item_price * it.quantity, 0);
                updateCart();
                updateTotal(total);
            }
        }

        function updateCart() {
            const cartList = document.getElementById('cart_list');
            cartList.innerHTML = '';
            items.forEach((item, index) => {
                const listItem = document.createElement('li');
                listItem.className = 'list-group-item';
                listItem.innerHTML = `${item.item_name} - Php ${item.item_price.toFixed(2)} x${item.quantity} <button class="btn btn-danger btn-sm float-right" onclick="removeItem(${index})">Remove</button>`;
                cartList.appendChild(listItem);
            });
        }

        function updateTotal(total) {
            document.getElementById('total').textContent = `Php ${total.toFixed(2)}`;
        }

        function clearForm() {
            document.getElementById('item_name').value = '';
            document.getElementById('item_price').value = '';
            document.getElementById('quantity').value = '';
        }

        function generateReceipt() {
            const customerName = prompt('Enter customer name:');
            if (customerName) {
                const total = items.reduce((sum, item) => sum + item.item_price * item.quantity, 0);
                alert(`Receipt for ${customerName}\n\n${items.map(item => `${item.item_name}: Php ${item.item_price.toFixed(2)} x${item.quantity}`).join('\n')}\n\nTotal: Php ${total.toFixed(2)}`);
                items = [];
                updateCart();
                updateTotal(0);
            }
        }
    </script>
</body>
</html>
