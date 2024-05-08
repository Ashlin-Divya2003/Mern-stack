index.html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Website Data Management</title>
  <link rel="stylesheet" href="styles.css">
</head>
<body>
  <header>
    <h1>Website Data Management</h1>
  </header>
  
  <main>
    <section id="add-section">
      <h2>Add Data</h2>
      <form id="add-form">
        <label for="title">Title:</label>
        <input type="text" id="title" name="title" required>
        
        <label for="description">Description:</label>
        <textarea id="description" name="description" rows="4" required></textarea>
        
        <label for="category">Category:</label>
        <input type="text" id="category" name="category" required>
        
        <button type="submit">Add Data</button>
      </form>
    </section>
    
    <section id="data-section">
      <h2>Data List</h2>
      <ul id="data-list"></ul>
    </section>
  </main>

  <script src="https://unpkg.com/axios/dist/axios.min.js"></script>
  <script src="scripts.js"></script>
</body>
</html>
styles.css
body {
  font-family: Arial, sans-serif;
  margin: 0;
  padding: 0;
  background-color: #f9f9f9;
}

header {
  background-color: #333;
  color: #fff;
  padding: 20px 0;
  text-align: center;
}

main {
  max-width: 800px;
  margin: 0 auto;
  padding: 20px;
}

section {
  margin-bottom: 30px;
}

h2 {
  margin-bottom: 10px;
}

form label {
  display: block;
  margin-bottom: 5px;
}

form input[type="text"],
form textarea {
  width: calc(100% - 10px);
  padding: 10px;
  margin-bottom: 15px;
  border: 1px solid #ccc;
  border-radius: 5px;
}

form button {
  padding: 10px 20px;
  background-color: #007bff;
  color: #fff;
  border: none;
  border-radius: 5px;
  cursor: pointer;
}

form button:hover {
  background-color: #0056b3;
}

#data-list {
  list-style: none;
  padding: 0;
}

#data-list li {
  background-color: #fff;
  padding: 15px;
  margin-bottom: 10px;
  border-radius: 5px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

#data-list li:last-child {
  margin-bottom: 0;
}
appp.js
// Import required packages
const express = require('express');
const bodyParser = require('body-parser');
const mongoose = require('mongoose');

// Create an instance of Express
const app = express();

// Middleware for parsing JSON and URL-encoded request bodies
app.use(bodyParser.json());
app.use(bodyParser.urlencoded({ extended: true }));

// Connect to MongoDB database
mongoose.connect('mongodb://localhost/mydatabase', { useNewUrlParser: true, useUnifiedTopology: true });
const db = mongoose.connection;
db.on('error', console.error.bind(console, 'MongoDB connection error:'));
db.once('open', () => {
  console.log('Connected to MongoDB');
});

// Define a schema for the data
const DataSchema = new mongoose.Schema({
  title: String,
  description: String,
  category: String,
});

// Create a model based on the schema
const DataModel = mongoose.model('Data', DataSchema);

// Endpoint to add new data
app.post('/add', (req, res) => {
  const { title, description, category } = req.body;
  
  // Create a new document based on the submitted data
  const newData = new DataModel({
    title,
    description,
    category,
  });

  // Save the document to the database
  newData.save((err) => {
    if (err) {
      console.error('Error saving data:', err);
      res.status(500).send('Error saving data');
    } else {
      console.log('Data saved successfully:', newData);
      res.send('Data added successfully!');
    }
  });
});

// Endpoint to retrieve all data
app.get('/data', (req, res) => {
  // Find all documents in the collection
  DataModel.find({}, (err, data) => {
    if (err) {
      console.error('Error fetching data:', err);
      res.status(500).send('Error fetching data');
    } else {
      res.json(data);
    }
  });
});

// Endpoint to remove data by ID
app.delete('/remove/:id', (req, res) => {
  const { id } = req.params;
  
  // Find the document by ID and delete it
  DataModel.findByIdAndRemove(id, (err) => {
    if (err) {
      console.error('Error removing data:', err);
      res.status(500).send('Error removing data');
    } else {
      res.send('Data removed successfully!');
    }
  });
});

// Start the server
const PORT = process.env.PORT || 3000;
app.listen(PORT, () => {
  console.log(`Server is running on http://localhost:${PORT}`);
});
