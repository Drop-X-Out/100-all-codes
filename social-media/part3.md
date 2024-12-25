
1. Inside the **`src/Data/TrendData.jsx`** add the following code :

```jsx
export const TrendData= [
    {
      name: "Minions",
      shares: 97,
    },
    {
      name: "Avangers",
      shares: 80.5,
    },
    {
      name: "Zainkeepscode",
      shares: 75.5,
    },
    {
      name: "Reactjs",
      shares: 72,
    },
    {
      name: "Elon Musk",
      shares: 71.9,
    },
    {
      name: "Need for Speed",
      shares: 20,
    },
  ];
 
```

This data will be used in the trends right card.

## Step 2 : Create Backend using Node.js and Express

### **Setting Up the Node.js Project**

Let's start by setting up a new Node.js project. Create a new folder named server inside the root folder ( i.e. : outside the src folder ) Open your terminal and inside the server folder execute the following commands:

```jsx
npm init -y
```

### **Installing Dependencies**

```jsx
npm install express
npm install cors
npm install body-parser mongodb mongoose multer fs-extra
```

1. Create a folder structure like this in the server folder :

```jsx
.
├── models
│   ├── postModels.js
│   └── userModel.js 
├── package-lock.json
├── package.json     
├── routes
│   ├── post.js      
│   ├── profile.js   
│   ├── uploads
│   │   ├── Tzuyu-163559.png  //Random Data
│   │   └── video
│   │       ├── Tzuyu-100208.mp4//Random Data
│   │       └── Tzuyu-385501.mp4//Random Data
│   └── user.js
├── server.js
└── uploads
```

1. Inside the **`server/models/postModels.js`** file add the following code : 

```jsx
const mongoose = require('mongoose');

const postSchema = new mongoose.Schema({
  imgPath: {
    type: String,
    required: true
  },
  name: {
    type: String,
    required: true
  },
  desc: {
    type: String,
    required: true
  },
  format: {
    type: String,
    required: true
  },
  likes: {
    type: Number,
    default: 0
  },
  liked: {
    type: Boolean,
    default: false
  },
  userId: {
    type: mongoose.Schema.Types.ObjectId,
    ref: 'User',
    required: true
  },
  likedUser : [{type : String , default : 0}]
});

const postModel = mongoose.model('Post', postSchema);

module.exports = postModel;
```

This file will create a model for post schema.

1.  Inside the **`server/models/userModel.js`** file add the following code : 

```jsx
const mongoose = require('mongoose');

const userSchema = new mongoose.Schema({
  firstName: String,
  lastName: String,
  username: { type: String, unique: true },
  password: String,
  followings: { type: Number, default: 0 },
  followers: { type: Number, default: 0 },
  followersList: [{ type: String, default: 0 }]
});

module.exports = mongoose.model('User', userSchema);
```

This module will create user schema.

1. Inside the **`server/routes/post.js`** file add the following code : 

```jsx
const express = require('express')
const multer = require('multer')
const path = require("path")
const fs = require('fs');
const postModel = require("../models/postModels");

const app = express()

var upload = multer({ dest: "./uploads" });

function generateRandomNumber() {
  return Math.floor(100000 + Math.random() * 900000);
}

app.get("/fetchAllPosts", async (req, res) => {
  try {
    console.log("Hit")
    const data = await postModel.find();
    const modifiedData = await Promise.all(data.map((post) => {
      // Read the video file content
      const newObj = {...post};
      const videoContent = fs.readFileSync(post.imgPath);
      console.log("Path", post.imgPath)
      // Convert the video content to base64
      const base64Video = videoContent.toString('base64');
      newObj.postBase64 = base64Video;
      return newObj;
    }));
    res.status(200).send(modifiedData)
  }
  catch (err) {
    console.log("Error ", err)
    res.status(500).send({ message: err })
  }
})

app.post('/upload', upload.array('images', 5), (req, res) => {
  try {
    const imageBlob = req.body.images;
    const imageName = `${req.body.name}`;
    const userId  = req.body.userId

    // Create directory if it doesn't exist
    const uploadDir = path.join(__dirname, 'uploads');
    if (!fs.existsSync(uploadDir)) {
      fs.mkdirSync(uploadDir);
    }

    // Construct image path
    const randomSixDigitNumber = generateRandomNumber();

    const imagePath = path.join(uploadDir, `${imageName}-${randomSixDigitNumber}.png`);
    let base64Image = imageBlob.split(';base64,').pop();

    // Write image data to file
    fs.writeFile(imagePath, base64Image, { encoding: 'base64' }, async (err) => {
      if (err) {
        console.error('Error saving image:', err);
        return res.status(500).send('Failed to save image.');
      }
      const postdata = new postModel({
        imgPath: imagePath,
        name: imageName,
        format: "image",
        userId : userId,
        desc: req.body.desc,
        likes: req.body.likes,
        liked: req.body.liked
      });
      await postdata.save();

      console.log('Image saved successfully:', imagePath);
      res.status(200).send('Image uploaded successfully.');
    })
  }
  catch (err) {
    console.log("Error ", err)
    res.status(500).send({ message: err })
  }
});

app.post('/upload/video', upload.array('video', 5), (req, res) => {
  try {
    console.log("Video")
    const imageBlob = req.body.images;
    const imageName = `${req.body.name}`;

    // Create directory if it doesn't exist
    const uploadDir = path.join(__dirname, 'uploads/video');
    if (!fs.existsSync(uploadDir)) {
      fs.mkdirSync(uploadDir);
    }

    // Construct image path
    const randomSixDigitNumber = generateRandomNumber();

    const imagePath = path.join(uploadDir, `${imageName}-${randomSixDigitNumber}.mp4`);
    let base64Image = imageBlob.split(';base64,').pop();

    // Write image data to file
    fs.writeFile(imagePath, base64Image, { encoding: 'base64' }, async (err) => {
      if (err) {
        console.error('Error saving image:', err);
        return res.status(500).send('Failed to save image.');
      }
      const postdata = new postModel({
        imgPath: imagePath,
        name: imageName,
        format: "video",
        desc: req.body.desc,
        likes: req.body.likes,
        liked: req.body.liked,
        userId : req.body.userId
      });
      await postdata.save();

      console.log('Image saved successfully:', imagePath);
      res.status(200).send('Image uploaded successfully.');
    })
  }
  catch (err) {
    console.log("Error ", err)
    res.status(500).send({ message: err })
  }
});

module.exports = app;
```

This module will create three API routes which will handle all the post upload operations.

1. Inside the **`server/routes/profile.js`** file add the following code : 

```jsx
const User = require('./../models/userModel');
const Post = require("../models/postModels");
const express = require("express");
const bodyParser = require("body-parser");
const app = express();

app.use(bodyParser.urlencoded({ extended: true }));
app.use(bodyParser.json());

app.post("/data", async (req, res) => {
    try {
        const { userId } = req.body;
        const user = await Post.countDocuments({ userId });
        const userData = await User.findOne({ _id: userId })
        console.log("USerData", userData)
        const profileData = {
            posts: user,
            followers: userData.followers,
            followings: userData.followings
        }
        res.status(200).json(profileData);
    } catch (error) {
        console.error(error);
        res.status(500).json({ error: "Internal server error" });
    }
});
app.post("/follow", async (req, res) => {
    try {
        const { userId, followerId } = req.body;
        console.log("Getting", req.body)
        const updatedUser = await User.findOneAndUpdate(
            { _id: userId },
            { $push: { followersList: followerId }, $inc: { followings: 1 } },
            { new: true } // This option ensures that the updated document is returned
        );

        console.log("updatedUser", updatedUser)
        res.status(200).json(updatedUser);
    } catch (error) {
        console.error(error);
        res.status(500).json({ error: "Internal server error" });
    }
});
app.post("/like", async (req, res) => {
    try {
        const { _id, userId } = req.body;
        console.log("Getting", req.body);

        // Check if the user has already liked the post
        const userLiked = await Post.findOne({ _id: _id, likedUser: userId });

        if (userLiked) {
            const updatedUser = await Post.findOneAndUpdate(
                { _id: _id, likedUser: userId },
                { $pull: { likedUser: userId }, $inc: { likes: -1 } }, 
                { new: true }
            );

            console.log("updatedUser dec", updatedUser);
            res.status(200).json(updatedUser);
        } else {
            // If the user has not already liked the post, increase the likes count
            const updatedUser = await Post.findOneAndUpdate(
                { _id: _id },
                { $push: { likedUser: userId }, $inc: { likes: 1 } },
                { new: true }
            );

            console.log("updatedUser inc", updatedUser);
            res.status(200).json(updatedUser);
        }
    } catch (error) {
        console.error(error);
        res.status(500).json({ error: "Internal server error" });
    }
});

module.exports = app
```

This api routes will handle all the profile operations.

1. Inside the **`server/routes/user.js`** file add the following code : 

```jsx
const User = require('./../models/userModel');
const express = require("express");
const bodyParser = require("body-parser");

const app = express();

app.use(bodyParser.urlencoded({ extended: true }));
app.use(bodyParser.json());

// Define the route to handle form data
app.post("/signup", async (req, res) => {
    try {
        const { firstName, lastName, username, password } = req.body;
        // Validate input (e.g., check if required fields are provided)

        // Check if the username already exists
        const existingUser = await User.findOne({ username });
        if (existingUser) {
            return res.status(400).json({ error: 'Username already exists' });
        }

        // Create a new user
        const newUser = new User({ firstName, lastName, username, password });

        // Save the user to the database
        await newUser.save();

        res.status(201).json({ message: 'User created successfully' });
    } catch (error) {
        console.error(error);
        res.status(500).json({ error: 'Internal server error' });
    }
});

// Login endpoint
app.post("/login", async (req, res) => {
    try {
        const { username, password } = req.body;
        console.log("data", req.body)
        // Check if the user exists
        const user = await User.findOne({ username });
        if (!user) {
            return res.status(401).json({ error: "Invalid username or password" });
        }

        // Check if the password matches
        if (user.password !== password) {
            return res.status(401).json({ error: "Invalid username or password" });
        }
        res.status(200).json({ message: "Login successful", data: user });
    } catch (error) {
        console.error(error);
        res.status(500).json({ error: "Internal server error" });
    }
});

app.get("/fetchUsers", async (req, res) => {
    try {
        // Check if the user exists
        const user = await User.find();

        res.status(200).json(user);
    } catch (error) {
        console.error(error);
        res.status(500).json({ error: "Internal server error" });
    }
});

module.exports = app
```

These api routes will handle all the api routes for user operations.

1. Inside the **`server/server.js`** file add the following code : 

```jsx
const express = require("express");
const { MongoClient } = require("mongodb");
const mongoose = require("mongoose")
const cors = require("cors");
const postApi = require("./routes/post");
const userApi = require("./routes/user");
const profileApi = require("./routes/profile");
const bodyparser  = require("body-parser")

const app = express();
const PORT = process.env.PORT || 8080;
app.use(cors({
  origin: ['http://localhost:3000']
}));
app.use(bodyparser.urlencoded({ extended: true }))
app.use(bodyparser.json())
app.use(express.json());
app.use("/api/posts", postApi);
app.use("/api/users", userApi);
app.use("/api/profile",profileApi);

app.get("/", (req, res) => {
  res.send("Hello from Express!");
});

mongoose.connect( //Your own mongodb Atlas Connection string ( FREE )
  "mongodb+srv://root:t@cluster0.0mlseyi.mongodb.net/?retryWrites=true&w=majority&appName=Cluster0",
  {
    bufferCommands: false,
    useNewUrlParser: true,
    useUnifiedTopology: true

  }
)
  .then((client) => {
    console.log("Connected to MongoDB Atlas");

    app.listen(PORT, () => {
      console.log(`Server is running on http://localhost:${PORT}`);
    });
  })
  .catch((err) => {
    console.error("Error connecting to MongoDB Atlas", err);
  });
```

This will run server on 8080. when you run the command **node server** in the terminal.

## Step 3  : Test the application

To test the application run the command **npm start** in the project root and **node server** inside the server folder terminal. After creating 

You will be able to perform the following operations : 

- Like photo and  Unlike photo.
- Follow and unfollow other users.
- Upload photo in the application.
- Upload video in the application.

[socialrun1.mp4](https://drive.google.com/file/d/1oJShYQYacxEFLIGHKy00TTrMS0ymWUK-/view?usp=sharing)