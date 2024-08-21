require('dotenv').config(); // Load environment variables from .env file

const express = require('express');
const bodyParser = require('body-parser');
const { MongoClient } = require('mongodb');

const app = express();
const port = process.env.PORT || 3000; // Use the port from the .env file

// Middleware to parse incoming requests
app.use(bodyParser.urlencoded({ extended: true }));
app.use(bodyParser.json());

// MongoDB connection URL from .env file
const url = process.env.MONGODB_URI;
const dbName = 'groceryDeliveryDB';

let db;

// Connect to MongoDB
MongoClient.connect(url, { useUnifiedTopology: true })
    .then(client => {
        console.log('Connected to MongoDB');
        db = client.db(dbName);
    })
    .catch(error => console.error(error));

// Handle POST request for form submission
app.post('/submit-order', (req, res) => {
    const orderData = {
        name: req.body.name,
        address: req.body.address,
        email: req.body.email,
        phone: req.body.phone,
        quantity: req.body.quantity,
        items: req.body.items.split(',').map(item => item.trim()),
        deliveryDate: req.body.deliveryDate,
        timeSlot: req.body.timeSlot,
        additionalServices: req.body.service,
        paymentMethod: req.body.paymentMethod,
    };

    const collection = db.collection('orders');
    collection.insertOne(orderData)
        .then(result => {
            console.log('Order saved:', result);
            res.status(200).send('Order submitted successfully!');
        })
        .catch(error => {
            console.error('Error saving order:', error);
            res.status(500).send('Failed to submit order');
        });
});

// Start the server
app.listen(port, () => {
    console.log(`Server running on http://localhost:${port}`);
});
