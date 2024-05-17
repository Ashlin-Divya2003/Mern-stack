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
Task 4
index.html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Login Page</title>
    <link rel="stylesheet" href="style.css">
</head>

<body>
    <div class="container">
        <h2>Registration</h2>
        <form id="Sign-up Form">
            <div class="form-group">
                <label for="username">Username <span style="color: red;">*</span></label>
                <input type="text" id="username" name="username" required>
            </div>
            <div class="form-group">
                <label for="password">Password<span style="color: red;">*</span></label>
                <input type="password" id="password" name="password" required>
            </div>
            <div class="form-group">
                <label for="contact">Contact No: <span style="color: red;">*</span></label>
                <input type="tel" id="contact" name="contact">
            </div>
            <div class="form-group">
                <label for="age">Age:</label>
                <input type="number" id="age" name="age">
            </div>
            <div class="form-group">
                <label for="date">Date of Birth<span style="color: red;">*</span></label>
                <input type="date" id="dob" name="dob">
            </div>
            <div class="form-group">
                <label for="college">College:</label>
                <input type="text" id="college" name="college">
            </div>
            <button onclick="window.location.href='login.html'">submit</button>


        </form>
        <p id="message"></p>
    </div>
    <script src="script.js"></script>
</body>

</html>
login.html
<!DOCTYPE html>
<html>
<style>
    body {
        font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
    }

    * {
        box-sizing: border-box
    }

    input[type=text],
    input[type=password] {
        width: 100%;
        font-size: 28px;
        padding: 15px;
        margin: 5px 0 22px 0;
        display: inline-block;
        border: none;
        background: #f1f1f1;
    }

    label {
        font-size: 15px;
    }

    input[type=text]:focus,
    input[type=password]:focus {
        background-color: #ddd;
        outline: none;
    }

    hr {
        border: 1px solid #f1f1f1;
        margin-bottom: 25px;
    }

    button {
        font-size: 18px;
        font-weight: bold;
        background-color: rgb(10, 119, 13);
        color: white;
        padding: 14px 20px;
        margin: 8px 0;
        border: none;
        cursor: pointer;
        width: 100%;
        opacity: 0.9;
    }

    button:hover {
        opacity: 1;
    }

    .formContainer {
        padding: 10px;
    }

    .formContainer p {
        font-size: 28px;
    }
</style>

<body>
    <form>
        <div class="formContainer">
            <h1>Login</h1>
            <hr>
            <label for="username"><b>Username</b></label>
            <input type="text" placeholder="Enter Username" name="username" required>
            <label for="password"><b>Password</b></label>
            <input type="password" placeholder="Enter Password" name="password" required>
            <label for="repeatPassword"><b>Repeat Password</b></label>
            <input type="password" placeholder="Repeat Password" name="repeatPassword" required>
            <label>
                <input type="checkbox" checked="checked" name="remember" style:"margin-bottom:15px"> Remember me
            </label>
            <p>By creating an account you agree to our <a href="#" style="color:dodgerblue">Terms & Privacy</a>.</p>
            <div>
                <button onclick="window.location.href='website.html'">login</button>
            </div>
        </div>
    </form>
</body>

</html>
script.js
document.getElementById('loginForm').addEventListener('submit', async (e) => {
    e.preventDefault();
    const username = document.getElementById('username').value;
    const password = document.getElementById('password').value;

    const response = await fetch('/login', {
        method: 'POST',
        headers: {
            'Content-Type': 'application/x-www-form-urlencoded',
        },
        body: `username=${username}&password=${password}`,
    });

    if (response.ok) {
        const message = await response.text();
        document.getElementById('message').textContent = message;
    } else {
        const errorMessage = await response.text();
        document.getElementById('message').textContent = errorMessage;
    }
});
website.html

<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Simple HTML HomePage</title>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/5.15.3/css/all.min.css">
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Sriracha&display=swap');

        body {
            margin: 0;
            box-sizing: border-box;
        }

        /* CSS for header */
        .header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            background-color: #f5f5f5;
        }

        .header .logo {
            font-size: 25px;
            font-family: 'Sriracha', cursive;
            color: #000;
            text-decoration: none;
            margin-left: 30px;
        }

        .nav-items {
            display: flex;
            justify-content: space-around;
            align-items: center;
            background-color: #f5f5f5;
            margin-right: 20px;
        }

        .nav-items a {
            text-decoration: none;
            color: #000;
            padding: 35px 20px;
        }

        /* CSS for main element */
        .intro {
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            width: 100%;
            height: 520px;
            background: linear-gradient(to bottom, rgba(0, 0, 0, 0.5) 0%, rgba(0, 0, 0, 0.5) 100%), url("https://images.unsplash.com/photo-1587620962725-abab7fe55159?ixlib=rb-1.2.1&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=1031&q=80");
            background-size: cover;
            background-position: center;
            background-repeat: no-repeat;
        }

        .intro h1 {
            font-family: sans-serif;
            font-size: 60px;
            color: #fff;
            font-weight: bold;
            text-transform: uppercase;
            margin: 0;
        }

        .intro p {
            font-size: 20px;
            color: #d1d1d1;
            text-transform: uppercase;
            margin: 20px 0;
        }

        .intro button {
            background-color: #5edaf0;
            color: #000;
            padding: 10px 25px;
            border: none;
            border-radius: 5px;
            font-size: 20px;
            font-weight: bold;
            cursor: pointer;
            box-shadow: 0px 0px 20px rgba(255, 255, 255, 0.4)
        }

        .achievements {
            display: flex;
            justify-content: space-around;
            align-items: center;
            padding: 40px 80px;
        }

        .achievements .work {
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            padding: 0 40px;
        }

        .achievements .work i {
            width: fit-content;
            font-size: 50px;
            color: #333333;
            border-radius: 50%;
            border: 2px solid #333333;
            padding: 12px;
        }

        .achievements .work .work-heading {
            font-size: 20px;
            color: #333333;
            text-transform: uppercase;
            margin: 10px 0;
        }

        .achievements .work .work-text {
            font-size: 15px;
            color: #585858;
            margin: 10px 0;
        }

        .about-me {
            display: flex;
            justify-content: center;
            align-items: center;
            padding: 40px 80px;
            border-top: 2px solid #eeeeee;
        }

        .about-me img {
            width: 500px;
            max-width: 100%;
            height: auto;
            border-radius: 10px;
        }

        .about-me-text h2 {
            font-size: 30px;
            color: #333333;
            text-transform: uppercase;
            margin: 0;
        }

        .about-me-text p {
            font-size: 15px;
            color: #585858;
            margin: 10px 0;
        }

        /* CSS for footer */
        .footer {
            display: flex;
            justify-content: space-between;
            align-items: center;
            background-color: #302f49;
            padding: 40px 80px;
        }

        .footer .copy {
            color: #fff;
        }

        .bottom-links {
            display: flex;
            justify-content: space-around;
            align-items: center;
            padding: 40px 0;
        }

        .bottom-links .links {
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            padding: 0 40px;
        }

        .bottom-links .links span {
            font-size: 20px;
            color: #fff;
            text-transform: uppercase;
            margin: 10px 0;
        }

        .bottom-links .links a {
            text-decoration: none;
            color: #a1a1a1;
            padding: 10px 20px;
        }
    </style>
</head>

<body>
    <header class="header">
        <a href="#" class="logo">Developer</a>
        <nav class="nav-items">
            <a href="#">Home</a>
            <a href="#">About</a>
            <a href="#">Contact</a>
        </nav>
    </header>
    <main>
        <div class="intro">
            <h1>A Web Developer</h1>
            <p>I am a web developer and I love to create websites.</p>
            <button>Learn More</button>
        </div>
        <div class="achievements">
            <div class="work">
                <i class="fas fa-atom"></i>
                <p class="work-heading">Projects</p>
                <p class="work-text">I have worked on many projects and I am very proud of them. I am a very good
                    developer and I am always looking for new projects.</p>
            </div>
            <div class="work">
                <i class="fas fa-skiing"></i>
                <p class="work-heading">Skills</p>
                <p class="work-text">I have a lot of skills and I am very good at them. I am very good at programming
                    and I am always looking for new skills.</p>
            </div>
            <div class="work">
                <i class="fas fa-ethernet"></i>
                <p class="work-heading">Network</p>
                <p class="work-text">I have a lot of network skills and I am very good at them. I am very good at
                    networking and I am always looking for new network skills.</p>
            </div>
        </div>
        <div class="about-me">
            <div class="about-me-text">
                <h2>About Me</h2>
                <p>I am a web developer and I love to create websites. I am a very good developer and I am always
                    looking for new projects. I am a very good developer and I am always looking for new projects.</p>
            </div>
            <img src="https://images.unsplash.com/photo-1596495578065-6e0763fa1178?ixlib=rb-1.2.1&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=871&q=80"
                alt="me">
        </div>
    </main>
    <footer class="footer">
        <div class="copy">&copy; 2024 Developer</div>
        <div class="bottom-links">
            <div class="links">
                <span>More Info</span>
                <a href="#">Home</a>
                <a href="#">About</a>
                <a href="#">Contact</a>
            </div>
            <div class="links">
                <span>Social Links</span>
                <a href="#"><i class="fab fa-facebook"></i></a>
                <a href="#"><i class="fab fa-twitter"></i></a>
                <a href="#"><i class="fab fa-instagram"></i></a>
            </div>
        </div>
    </footer>
</body>

</html>

<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Simple HTML HomePage</title>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/5.15.3/css/all.min.css">
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Sriracha&display=swap');

        body {
            margin: 0;
            box-sizing: border-box;
        }

        /* CSS for header */
        .header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            background-color: #f5f5f5;
        }

        .header .logo {
            font-size: 25px;
            font-family: 'Sriracha', cursive;
            color: #000;
            text-decoration: none;
            margin-left: 30px;
        }

        .nav-items {
            display: flex;
            justify-content: space-around;
            align-items: center;
            background-color: #f5f5f5;
            margin-right: 20px;
        }

        .nav-items a {
            text-decoration: none;
            color: #000;
            padding: 35px 20px;
        }

        /* CSS for main element */
        .intro {
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            width: 100%;
            height: 520px;
            background: linear-gradient(to bottom, rgba(0, 0, 0, 0.5) 0%, rgba(0, 0, 0, 0.5) 100%), url("https://images.unsplash.com/photo-1587620962725-abab7fe55159?ixlib=rb-1.2.1&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=1031&q=80");
            background-size: cover;
            background-position: center;
            background-repeat: no-repeat;
        }

        .intro h1 {
            font-family: sans-serif;
            font-size: 60px;
            color: #fff;
            font-weight: bold;
            text-transform: uppercase;
            margin: 0;
        }

        .intro p {
            font-size: 20px;
            color: #d1d1d1;
            text-transform: uppercase;
            margin: 20px 0;
        }

        .intro button {
            background-color: #5edaf0;
            color: #000;
            padding: 10px 25px;
            border: none;
            border-radius: 5px;
            font-size: 20px;
            font-weight: bold;
            cursor: pointer;
            box-shadow: 0px 0px 20px rgba(255, 255, 255, 0.4)
        }

        .achievements {
            display: flex;
            justify-content: space-around;
            align-items: center;
            padding: 40px 80px;
        }

        .achievements .work {
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            padding: 0 40px;
        }

        .achievements .work i {
            width: fit-content;
            font-size: 50px;
            color: #333333;
            border-radius: 50%;
            border: 2px solid #333333;
            padding: 12px;
        }

        .achievements .work .work-heading {
            font-size: 20px;
            color: #333333;
            text-transform: uppercase;
            margin: 10px 0;
        }

        .achievements .work .work-text {
            font-size: 15px;
            color: #585858;
            margin: 10px 0;
        }

        .about-me {
            display: flex;
            justify-content: center;
            align-items: center;
            padding: 40px 80px;
            border-top: 2px solid #eeeeee;
        }

        .about-me img {
            width: 500px;
            max-width: 100%;
            height: auto;
            border-radius: 10px;
        }

        .about-me-text h2 {
            font-size: 30px;
            color: #333333;
            text-transform: uppercase;
            margin: 0;
        }

        .about-me-text p {
            font-size: 15px;
            color: #585858;
            margin: 10px 0;
        }

        /* CSS for footer */
        .footer {
            display: flex;
            justify-content: space-between;
            align-items: center;
            background-color: #302f49;
            padding: 40px 80px;
        }

        .footer .copy {
            color: #fff;
        }

        .bottom-links {
            display: flex;
            justify-content: space-around;
            align-items: center;
            padding: 40px 0;
        }

        .bottom-links .links {
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            padding: 0 40px;
        }

        .bottom-links .links span {
            font-size: 20px;
            color: #fff;
            text-transform: uppercase;
            margin: 10px 0;
        }

        .bottom-links .links a {
            text-decoration: none;
            color: #a1a1a1;
            padding: 10px 20px;
        }
    </style>
</head>

<body>
    <header class="header">
        <a href="#" class="logo">Developer</a>
        <nav class="nav-items">
            <a href="#">Home</a>
            <a href="#">About</a>
            <a href="#">Contact</a>
        </nav>
    </header>
    <main>
        <div class="intro">
            <h1>A Web Developer</h1>
            <p>I am a web developer and I love to create websites.</p>
            <button>Learn More</button>
        </div>
        <div class="achievements">
            <div class="work">
                <i class="fas fa-atom"></i>
                <p class="work-heading">Projects</p>
                <p class="work-text">I have worked on many projects and I am very proud of them. I am a very good
                    developer and I am always looking for new projects.</p>
            </div>
            <div class="work">
                <i class="fas fa-skiing"></i>
                <p class="work-heading">Skills</p>
                <p class="work-text">I have a lot of skills and I am very good at them. I am very good at programming
                    and I am always looking for new skills.</p>
            </div>
            <div class="work">
                <i class="fas fa-ethernet"></i>
                <p class="work-heading">Network</p>
                <p class="work-text">I have a lot of network skills and I am very good at them. I am very good at
                    networking and I am always looking for new network skills.</p>
            </div>
        </div>
        <div class="about-me">
            <div class="about-me-text">
                <h2>About Me</h2>
                <p>I am a web developer and I love to create websites. I am a very good developer and I am always
                    looking for new projects. I am a very good developer and I am always looking for new projects.</p>
            </div>
            <img src="https://images.unsplash.com/photo-1596495578065-6e0763fa1178?ixlib=rb-1.2.1&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=871&q=80"
                alt="me">
        </div>
    </main>
    <footer class="footer">
        <div class="copy">&copy; 2024 Developer</div>
        <div class="bottom-links">
            <div class="links">
                <span>More Info</span>
                <a href="#">Home</a>
                <a href="#">About</a>
                <a href="#">Contact</a>
            </div>
            <div class="links">
                <span>Social Links</span>
                <a href="#"><i class="fab fa-facebook"></i></a>
                <a href="#"><i class="fab fa-twitter"></i></a>
                <a href="#"><i class="fab fa-instagram"></i></a>
            </div>
        </div>
    </footer>
</body>
server.js

const express = require('express');
const app = express();
const PORT = 4000;

// Middleware
app.use(express.urlencoded({ extended: true }));
app.use(express.static('public'));

// Routes
app.get('/', (req, res) => {
    res.sendFile(__dirname + '/index.html');
});

app.post('/login', (req, res) => {
    const { username, password } = req.body;
    // Here you would check the username and password against your database
    // For demonstration, let's assume username is 'admin' and password is 'password'
    if (username === 'admin' && password === 'password') {
        res.send('Login successful!');
    } else {
        res.status(401).send('Invalid username or password');
    }
});

// Start server
app.listen(PORT, () => {
    console.log(`Server is running on http://localhost:${PORT}`);
});

</html>
