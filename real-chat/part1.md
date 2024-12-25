# How to create a real-time Chat Application with MERN Stack?

A chat app is a tool for chatting in real-time, letting you send and receive messages instantly. When you build a chat app with the MERN stack, the key feature is the Socket architecture, which includes the Socket API and the Socket Server.

The Socket Server is the part that handles incoming connections from users, processes what they need, and sends back responses. The Socket API, on the other hand, provides the necessary tools and protocols to set up and manage these connections from the user's side.

Here is overview of the Architecture of the Chat Application

![Untitled-2024-03-01-1305.png](https://talktohire.blr1.digitaloceanspaces.com/Untitled-2024-03-01-1305.png)

## Setting up the Environment

Let’s start setting up the environment of both the client and server of the application. Open your terminal and execute the following command to make a directory for the app.

```bash
mkdir chatapp
cd chatapp
```

### Building the Server Back-end of the App

Execute the given commands by following these instructions:

- Enter “server” as input for package name after running `npm init`
- Click return for all other options asked to maintain the default state, this will create a server directory inside your root directory

```bash
npm init
package name: (user) server
```

Now that the directory has been initialized we can start building the server of the Chat-App

1. Step 1: Create the below folder structure inside the server directory

```bash
my-app/
├─ node_modules/
├─ middleware/
│  ├─ protect.js
├─ models/
│  ├─ avatars.js
│  ├─ messageModel.js
│  ├─ userModel.js
│  ├─ tokenModel.js
├─ routes/
│  ├─ avatarRoute.js
│  ├─ userRoute.js
├─ utils/
│  ├─ sendEmail.js
├─ controllers/
│  ├─ registerController.js
│  ├─ profileController.js
│  ├─ peopleController.js
│  ├─ loginController.js
│  ├─ emailVerifyController.js
│  ├─ avatarController.js
│  ├─ messageController.js
├─ db/
│  ├─ db.js
├─ .gitignore
├─ package.json
├─ package-lock.json
├─ index.js
├─ wsServer.js

```

1. Step 2: Install server dependencies

```bash
npm install bcrypt cookie-parser cors dotenv express joi joi-password-complexity
jsonwebtoken mongoose nodemailer nodemon path ws

```

1. Create Schema Models by copy pasting the following code inside models directory:
- `userModel.js`

```jsx
const mongoose = require("mongoose");
const jwt = require("jsonwebtoken");
const Joi = require("joi");
const passwordComplexity = require("joi-password-complexity");

const userSchema = new mongoose.Schema(
  {
    firstName: { type: String, required: true },
    lastName: { type: String, required: true },
    email: { type: String, required: true },
    password: { type: String, required: true },
    verified: { type: Boolean, default: false },
    verificationLinkSent: { type: Boolean, default: false },
    avatarLink: { type: String },
  },
  { timestamps: true }
);

userSchema.methods.generateAuthToken = function () {
  const token = jwt.sign(
    {
      _id: this._id,
      firstName: this.firstName,
      lastName: this.lastName,
      email: this.email,
    },
    process.env.JWTPRIVATEKEY,
    { expiresIn: "7d" }
  );
  return token;
};

const User = mongoose.model("user", userSchema);

const validateRegister = (data) => {
  const schema = Joi.object({
    firstName: Joi.string().required().label("First Name"),
    lastName: Joi.string().required().label("Last Name"),
    email: Joi.string().email().required().label("Email"),
    password: passwordComplexity().required().label("Password"),
  });
  return schema.validate(data);
};

const validateLogin = (data) => {
  const schema = Joi.object({
    email: Joi.string().email().required().label("Email"),
    password: passwordComplexity().required().label("Password"),
  });
  return schema.validate(data);
};

module.exports = { User, validateRegister, validateLogin };
```

- `messageModel.js` - This is the schema structure for the message a user will send to another user. It consists of a sender, receiver and the message text.

```jsx
const mongoose = require("mongoose");

const MessageSchema = new mongoose.Schema(
  {
    sender: { type: mongoose.Schema.Types.ObjectId, ref: "User" },
    recipient: { type: mongoose.Schema.Types.ObjectId, ref: "User" },
    text: { type: String, required: true },
  },
  { timestamps: true }
);

const Message = mongoose.model("Message", MessageSchema);

module.exports = Message;
```

- `tokenModel.js`

```jsx
const mongoose = require("mongoose");
const Schema = mongoose.Schema;

const tokenSchema = new Schema({
  userId: {
    type: Schema.Types.ObjectId,
    required: true,
    ref: "user",
    unique: true,
  },
  token: { type: String, required: true },
  createdAt: { type: Date, default: Date.now },
  expiresAt: { type: Date, default: Date.now + 3600000 },
});

const Token = mongoose.model("token", tokenSchema);

module.exports = { Token };

```

- `avatars.js` - these are to store avatar images in the DB

```jsx
const mongoose = require("mongoose");

const AvatarSchema = new mongoose.Schema(
  {
    link: {
      type: String,
      required: true,
      default: "https://i.imgur.com/qGsYvAK.png",
    },
  },
  { timestamps: true }
);

const Avatar = mongoose.model("Avatar", AvatarSchema);

module.exports = Avatar;
```

1. Write the following code in `protect.js` file inside the `middleware` directory. This will act as a barrier for all unauthenticated request by checking out the cookies and verify the token inside the request headers. We will be using `jwt` along with `cookies` to implement this

```jsx
const jwt = require("jsonwebtoken");

async function protect(req) {
  return new Promise((resolve, reject) => {
    const token = req.cookies?.authToken;
    if (token) {
      jwt.verify(token, process.env.JWTPRIVATEKEY, {}, (err, userData) => {
        if (err) {
          reject(err);
        } else {
          resolve(userData);
        }
      });
    } else {
      reject("no token");
    }
  });
}

module.exports = protect;
```

1. Next, create the `index.js` server file. Copy and paste the provided code snippet to set up your server's index file. This will establish the `DB` connection, configure `cors`, initiate the `socket` server call, and set up the routes.
- `index.js`

```jsx
require("dotenv").config();
const express = require("express");
const cors = require("cors");
const app = express();
const connection = require("./db/db.js");
const userRoute = require("./routes/userRoute.js");
const avatarRoute = require("./routes/avatarRoute.js");
const cookieParser = require('cookie-parser')
const createWebSocketServer = require("./wsServer.js");
const path = require("path");

//database connection
connection();
app.use(express.json())
app.use(cookieParser())

//middlewares
app.use(express.json());
const allowedOrigins = [
  "http://localhost:5173",
  "http://localhost:4000",
    "https://swifty-chatty-appy.onrender.com"
];

const corsOptions = {
    origin: (origin, callback) => {
        if (allowedOrigins.includes(origin) || !origin) {
            callback(null, true);
        } else {
            callback(new Error("Not allowed by CORS"));
        }
    },
    methods: "GET,HEAD,PUT,PATCH,POST,DELETE",
    optionsSuccessStatus: 204,
    credentials: true, // Allow credentials like cookies
};
app.use(cors(corsOptions));

app.use("/api/user", userRoute);
app.use("/api/avatar", avatarRoute);
const port = process.env.PORT || 8000;
const server = app.listen(port, () => console.log(`Application Running on Port ${port}`));

createWebSocketServer(server); 
app.use(express.static(path.join(__dirname, "..", "frontend", "dist")));

app.get('/*', (req, res) => {
    res.sendFile(path.join(__dirname, '../frontend/dist/index.html'), (err) => {
        if (err) {
            console.error('Error sending file:', err);
        }
    });
});
```

1. To setup DB connection , setup the following file inside `db.js`file inside the db directory which is imported in the **`index.js`** server

```jsx
const mongoose = require('mongoose')

module.exports = async () => {
  try {
    await mongoose.connect(process.env.DB);
    console.log("DB CONNECTED SUCCESSFULLY")
  } catch (error) {
    console.log(error)
    console.log("COULD NOT CONNECT TO DB")
  }
}
```

1. Now that we've set up the routes in the index, we need to establish the API routes for controllers in the `routes` directory. Open this directory and insert the following code:
- `avatarRoute.js`

```jsx
//avatarRoute.js
const express = require('express');
const avatarController = require('../controllers/avatarController');
const router = express.Router();

router.post("/", avatarController.avatarController);
router.get("/all", avatarController.getAllAvatars);

module.exports = router;
```

- `userRoute.js`

```jsx
//userRoute.js
const express = require('express');
const registerController = require('../controllers/registerController');
const loginController = require('../controllers/loginController');
const verifyEmail = require('../controllers/emailVerifyController');
const profileController = require("../controllers/profileController");
const messageController = require("../controllers/messageController");
const peopleController = require("../controllers/peopleController");
const router = express.Router();

router.post("/register", registerController);
router.post("/login", loginController);
router.get("/:id/verify/:token", verifyEmail);
router.get("/profile", profileController.profileController);
router.get("/messages/:userId", messageController);
router.get("/people", peopleController);
router.put("/profile/update", profileController.profileUpdate);

module.exports = router;
```

1. **Setting up the Authentication -** To setup the authentication we need to configure the login, register and emailVerification controller files inside the **`controllers`** directory. Open up the directory and paste the following code
- `registerController.js`

```jsx
//registerController.js

const bcrypt = require("bcrypt");
const { User, validateRegister } = require("../models/userModel.js");
const { Token } = require("../models/tokenModel.js");
const sendEmail = require("../utils/sendEmail.js");
const crypto = require("crypto");

const registerController = async (req, res) => {
  try {
    const { error } = validateRegister(req.body);

    if (error) {
      return res.status(400).send({ message: error.details[0].message });
    }

    // Check if user with the given email already exists
    let user = await User.findOne({ email: req.body.email });

    if (user && user.verified) {
      return res
        .status(409)
        .send({ message: "User with given email already exists" });
    }
    if (user && user.verificationLinkSent) {
      return res
        .status(400)
        .send({
          message: "A verification link has been already sent to this Email",
        });
    }

    const salt = await bcrypt.genSalt(Number(process.env.SALT));
    const hashPassword = await bcrypt.hash(req.body.password, salt);
    // Save the user with hashed password
    user = await new User({ ...req.body, password: hashPassword }).save();

    // Generate a verification token and send an email
    const token = await new Token({
      userId: user._id,
      token: crypto.randomBytes(16).toString("hex"),
      createdAt: Date.now(),
      expiresAt: Date.now() + 3600000,
    }).save();

    const url = `${process.env.BASE_URL}/users/${user._id}/verify/${token.token}`;
    await sendEmail(user.email, "Verify Email", url);

    user.verificationLinkSent = true;
    await user.save();
    res
      .status(201)
      .send({ message: `Verification Email Sent to ${user.email}` });
  } catch (error) {
    console.error("Error in registerController:", error);
    res.status(500).send({ message: "Internal Server Error" });
  }
};

module.exports = registerController;
```

- `loginController.js`

```jsx
//loginController.js
const bcrypt = require("bcrypt");
const { User, validateLogin } = require("../models/userModel.js");

const loginController = async (req, res) => {
  try {
    const { error } = validateLogin(req.body);

    if (error) {
      return res.status(400).send({ message: error.details[0].message });
    }

    // Find the user by email
    const user = await User.findOne({ email: req.body.email });

    if (!user) {
      return res.status(401).send({ message: "Invalid Email" });
    }

    // Check password validity using bcrypt
    const validPassword = await bcrypt.compare(
      req.body.password,
      user.password
    );
    if (!validPassword) {
      return res.status(401).send({ message: "Invalid Password" });
    }

    // Check if the user's email is verified
    if (!user.verified) {
      return res.status(400).send({ message: "User doesn't exist" });
    }

    // Generate authentication token and send successful login response
    const token = user.generateAuthToken();
    res
      .status(200)
      .cookie("authToken", token, {
        httpOnly: false,
        sameSite: "none",
        secure: true,
        expires: new Date(Date.now() + 7 * 24 * 60 * 60 * 1000),
      })
      .send({ message: "Login successful", status: 200 });
    return;
  } catch (error) {
    console.error("Error in loginController:", error);
    res.status(500).send({ message: "Internal Server Error" });
  }
};

module.exports = loginController;
```

- `emailVerificationController.js`

```jsx
//emailVerificationController.js
const { User } = require("../models/userModel.js");
const { Token } = require("../models/tokenModel.js");
const verifyEmail = async (req, res) => {
  try {
    const user = await User.findById(req.params.id);

    if (!user) {
      return res.status(400).send({ message: "User doesn't exist" });
    }

    if (user.verified) {
      return res.status(400).send({ message: "Email already verified" });
    }

    // Find the token for the user
    const token = await Token.findOne({
      userId: user._id,
      token: req.params.token,
    });

    if (!token) {
      return res.status(400).send({ message: "Invalid Link" });
    }

    if (token.expiresAt < Date.now()) {
      user.verificationLinkSent = false;
      await user.save();
      return res.status(400).send({ message: "Verification link has expired" });
    }

    user.verified = true;
    await user.save();

    res.status(200).send({ message: "Email Verified Successfully" });
  } catch (error) {
    console.error("Error in verifyEmail:", error);
    res.status(500).send({ message: "Internal Server Error" });
  }
};

module.exports = verifyEmail;
```

1. Create an Email Verification Transporter inside the `utils` directory. Open the `sendEmail.js` file and paste the following code into it. This will enable to send the verification mail using nodemailer. You can get your SMTP userId and pass via any SMTP provider.

```jsx
//sendEmail.js

const nodemailer = require("nodemailer");

module.exports = async (email, subject, text) => {
  try {
    const transporter = nodemailer.createTransport({
      host: process.env.HOST,
      service: process.env.SERVICE,
      port: 587,
      // secure: Boolean(process.env.SECURE),
      auth: {
        user: process.env.SMTP_USER,
        pass: process.env.SMTP_PASS,
      },
    });

    await transporter.sendMail({
      from: process.env.USER,
      to: email,
      subject: subject,
      text: text,
    });

    console.log(`Email sent to ${email}`);
  } catch (error) {
    console.error(error);
    console.error(`Error sending mail to ${email}`);
    throw error; // Re-throw the error for the calling code to handle
  }
};
```

1. Now, let's create a WebSocket server that was utilized in the `index.js` server for real-time information exchange. Copy and paste the complete code into the `wsServer.js` file located in the root of the server directory. This will take care of the live connection between users, information exchange and reconnection logic.

```jsx
const ws = require("ws");
const jwt = require("jsonwebtoken");
const fs = require("fs");
const Message = require("./models/messageModel");
const { clear } = require("console");
const { User } = require("./models/userModel");

const createWebSocketServer = (server) => {
  const wss = new ws.WebSocketServer({ server });

  wss.on("connection", (connection, req) => {
    const notifyAboutOnlinePeople = async () => {
      const onlineUsers = await Promise.all(
        Array.from(wss.clients).map(async (client) => {
          const { userId, username } = client;
          const user = await User.findById(userId);
          const avatarLink = user ? user.avatarLink : null;

          return {
            userId,
            username,
            avatarLink,
          };
        })
      );

      [...wss.clients].forEach((client) => {
        client.send(
          JSON.stringify({
            online: onlineUsers,
          })
        );
      });
    };

    connection.isAlive = true;

    connection.timer = setInterval(() => {
      connection.ping();
      connection.deathTimer = setTimeout(() => {
        connection.isAlive = false;
        clearInterval(connection.timer);
        connection.terminate();
        notifyAboutOnlinePeople();
        console.log("dead");
      }, 1000);
    }, 5000);

    connection.on("pong", () => {
      clearTimeout(connection.deathTimer);
    });

    const cookies = req.headers.cookie;

    if (cookies) {
      const tokenString = cookies
        .split(";")
        .find((str) => str.startsWith("authToken="));

      if (tokenString) {
        const token = tokenString.split("=")[1];
        jwt.verify(token, process.env.JWTPRIVATEKEY, {}, (err, userData) => {
          if (err) console.log(err);

          const { _id, firstName, lastName } = userData;
          connection.userId = _id;
          connection.username = `${firstName} ${lastName}`;
        });
      }
    }

    connection.on("message", async (message) => {
      const messageData = JSON.parse(message.toString());
      const { recipient, text } = messageData;
      const msgDoc = await Message.create({
        sender: connection.userId,
        recipient,
        text,
      });

      if (recipient && text) {
        [...wss.clients].forEach((client) => {
          if (client.userId === recipient) {
            client.send(
              JSON.stringify({
                sender: connection.username,
                text,
                id: msgDoc._id,
              })
            );
          }
        });
      }
    });
    notifyAboutOnlinePeople();
    // Sending online user list to all clients

    // Log online users to the console
    console.log("Online Users:", onlineUsers);
  });
};

module.exports = createWebSocketServer;
```

1. To setup the `profileController.js` open the file and paste the following code. This controller will be used for CRUD operations on user profile.

```jsx
const jwt = require("jsonwebtoken");
const { User } = require("../models/userModel");

const profileController = async (req, res) => {
  const token = req.cookies?.authToken;
  if (token) {
    jwt.verify(token, process.env.JWTPRIVATEKEY, {}, async (err, userData) => {
      console.log(userData);
      if (err) throw err;
      const user = await User.findOne({ _id: userData._id });
      res.json(user);
    });
  } else {
    res.status(401).json("no token");
  }
};

const profileUpdate = async (req, res) => {
  const token = req.cookies?.authToken;
  if (token) {
    jwt.verify(token, process.env.JWTPRIVATEKEY, {}, (err, userData) => {
      if (err) throw err;
    });
  } else {
    res.status(401).json("no token");
  }

  const { firstName, lastName, email, avatarLink } = req.body;
  const user = await User.findOne({ email });

  if (user) {
    user.firstName = firstName;
    user.lastName = lastName;
    user.email = email;
    user.avatarLink = avatarLink;
    await user.save();
  }
  res.json(user);
};

module.exports = { profileController, profileUpdate };
```

1. Let's proceed with setting up `peopleController.js` and `avatarController.js` files. We'll use `peopleController` to verify users, while `avatarController` will handle avatar updates.

```jsx
//peopleController.js

const { User } = require("../models/userModel");

const peopleController = async (req, res) => {
  const users = await User.find({ verified: true });
  res.json(users);
  // console.log(users);
};

module.exports = peopleController;
```

```jsx
//avatarController.js
const Avatar = require("../models/avatars");

async function avatarController(req, res) {
  const { link } = req.body;

  // Check if the link is provided
  if (!link) {
    return res.status(400).json({ error: "Link is required" });
  }

  try {
    // Create a new avatar entry in the database
    const newAvatar = new Avatar({ link });
    await newAvatar.save();

    // Return success response
    return res
      .status(201)
      .json({ success: true, message: "Avatar link added successfully" });
  } catch (error) {
    // Handle any errors that occur during the process
    console.error(error);
    return res.status(500).json({ error: "Internal Server Error" });
  }
}

async function getAllAvatars(req, res) {
  try {
    // Fetch all avatars from the database
    const avatars = await Avatar.find();

    // Return the list of avatars
    return res.status(200).json({ success: true, avatars });
  } catch (error) {
    // Handle any errors that occur during the process
    console.error(error);
    return res.status(500).json({ error: "Internal Server Error" });
  }
}

module.exports = { avatarController, getAllAvatars };
```

1. The final step is to set up `messageController.js`. This file will match messages for the requested user and send them as a response. To create a functional `messageController`, copy and paste the given code into the file.

```jsx
const protect = require("../middleware/protect");
const Message = require("../models/messageModel");

const messageController = async (req, res) => {
  const { userId } = req.params;
  const userData = await protect(req);
  console.log("userData", userData);

  const ourUserId = userData._id;
  console.log("ourUserId", ourUserId);
  console.log("userId", userId);

  const messages = await Message.find({
    sender: { $in: [userId, ourUserId] },
    recipient: { $in: [userId, ourUserId] },
  }).sort({ createdAt: 1 });

  res.json(messages);
  console.log("messages", messages);
};

module.exports = messageController;
```

1. At last create a `.env` file inside the server with following variables

```bash
#mongodb DB URI
DB=""
#any private key you want
JWTPRIVATEKEY =
#integer value for salt 
SALT = 
PORT = 4000
# frontend url
BASE_URL = "http://localhost:5173"

#SMTP Detials, example gmail.com, mailtrap etc
HOST=
SERVICE=
EMAIL_PORT=587
SECURE= true
SMTP_USER=""
SMTP_PASS=""
NODE_ENV=development
```

1. Your server code is now ready and you can now execute it using `nodemon` by pasting the following code in your `package.json`

```bash
"scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "start" : "node index.js", //production
    "dev": "nodemon index.js"  //development
  },

```

Start the development server by running the following command, this will run the server on your `.env` PORT number in `localhost`

```jsx
npm run dev
```

## Setting up the Frontend of the App

Execute the command, open a new terminal and follow these instructions:

- Enter "frontend" when prompted for the Project name
- Select React as the framework and JavaScript as the variant

```bash
npm create vite@latest

✔ Project name: … frontend
✔ Select a framework: › React
✔ Select a variant: › JavaScript
```

This will create the frontend directory in your root `chatapp` folder. Run the following commands after it.

```bash
cd frontend
npm install
npm run dev
```

This will install the initial dependencies and start the development server of Vite running on `localhost:5173`. Open your browser and write the `localhost` URL to see the development server.

Execute the following command to install both dependencies and development dependencies.

```jsx
npm install axios js-cookie lodash react react-dom react-hot-toast react-router-dom \
&& npm install --save-dev @types/react @types/react-dom @vitejs/plugin-react autoprefixer eslint eslint-plugin-react eslint-plugin-react-hooks eslint-plugin-react-refresh postcss tailwindcss vite
```

<aside>
💡 Note : You need to install `TailwindCSS` with Vite before starting this project. Here is the steps to install Tailwind in your app.

</aside>

```bash
npm install -D tailwindcss postcss autoprefixer
npx tailwindcss init -p
```

In your `index.css` file copy paste these 3 lines of code

```css
@tailwind base;
@tailwind components;
@tailwind utilities;
```

Now we will setup the `tailwind.config.js` file, open the file and paste the code to add color themes

```jsx
/** @type {import('tailwindcss').Config} */
export default {
  content: ["./index.html", "./src/**/*.{js,ts,jsx,tsx}"],
  theme: {
    extend: {
      colors: {
        dark : "#131313",
        background: "#202329",
        primary: "#3f4654",
        primarySecond: "#6B8AFD",
        secondary: "#30E0A1",
        tertiary: "#EB7050",
      },
    },
  },
  plugins: [],
};
```

## **Setting Up the Folder Tree Structure**

Set up your frontend directory according to the structure shown below:

```bash
frontend/
├── node_modules/
├── public/
│   └── index.html
├── src/
│   ├── assets/
│   │   ├── react.svg
│   │   ├── hero.png
│   ├── components/
│   │   ├── Chat/
│   │   │   ├── Avatar.jsx
│   │   │   ├── ChatMessages.jsx
│   │   │   ├── Contact.jsx
│   │   │   ├── MessageInputForm.jsx
│   │   │   ├── Nav.jsx
│   │   │   ├── OnlineUserList.jsx
│   │   │   └── TopBar.jsx
│   │   ├── CustomerLogos.jsx
│   │   ├── Features.jsx
│   │   ├── Footer.jsx
│   │   ├── Hero.jsx
│   │   ├── LandingNav.jsx
│   │   ├── Payments.jsx
│   │   ├── Profile.jsx
│   │   └── SelectAvatar.jsx
│   ├── context/
│   │   ├── authContext.jsx
│   │   └── profileContext.jsx
│   ├── pages/
│   │   ├── ChatHome.jsx
│   │   ├── Home.jsx
│   │   ├── Login.jsx
│   │   ├── Register.jsx
│   │   └── VerifyEmail.jsx
│   ├── App.jsx
│   └── main.jsx
├── .gitignore
├── apiConfig.js
├── index.html
├── package-lock.json
├── package.json
├── postcss.config.js
├── README.md
├── tailwind.config.js
└── vite.config.js

```

*You can add any* `hero.png` *section for you Hero section*

1. To set up the `authContext.jsx` file, copy and paste the entire code into the file. This `authContext` file will be used to verify if the user is authenticated.

```jsx
import Cookies from "js-cookie";
import React, { createContext, useContext, useState } from "react";

const AuthContext = createContext();

export const AuthProvider = ({ children }) => {
  const [isAuthenticated, setIsAuthenticated] = useState(false);
  const setAuthenticated = (value) => {
    setIsAuthenticated(value);
  };
  const checkAuth = () => {
    const token = Cookies.get("authToken");
    console.log("Checking authentication...");
    if (token) {
      console.log("Token exists. Setting authenticated to true.");
      setAuthenticated(true);
      console.log(isAuthenticated);
    } else {
      console.log("Token does not exist. Setting authenticated to false.");
      setAuthenticated(false);
    }
  };

  const logout = () => {
    Cookies.remove("authToken");
    setAuthenticated(false);
  };
  return (
    <AuthContext.Provider
      value={{ isAuthenticated, setAuthenticated, checkAuth, logout }}
    >
      {children}
    </AuthContext.Provider>
  );
};

export const useAuth = () => {
  return useContext(AuthContext);
};
```