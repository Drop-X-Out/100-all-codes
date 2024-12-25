Step 6 : Create a View page for  accessing all the links added by you, in the **`pages/view/[slug].js`**using the following code :

![Untitled](https://talktohire.blr1.digitaloceanspaces.com/Untitled_2.png)

```jsx
import { URI } from "@/source";
import { useRouter } from "next/router";
import { useEffect, useState } from "react";
import { motion, sync, useCycle } from "framer-motion";

export default function View() {
  const router = useRouter();
  const [userData, setUserData] = useState([]);
  const [userName , setUsername] = useState("")
  const { slug } = router.query;

  const fetchUserData = async () => {
    const bodyObject = {
      userID: slug,
    };
    debugger;
    const response = await fetch(`${URI}/api/getlinks`, {
      method: "POST",
      headers: {
        "Content-Type": "application/json"
      },
      body: JSON.stringify(bodyObject),
    });
    const result = await response.json();
    setUserData(result.data.links);
    setUsername(result)
  };

  useEffect(() => {
    if (slug) {
      fetchUserData();
    }
  }, [slug]);

  const handlelink = (url) => {
    const newTab = window.open(url, '_blank');
    // Focus on the new tab (optional)
    if (newTab) {
      newTab.focus();
    } else {
      // If pop-up blocker prevents opening new tab, provide alternative
      console.log("Please allow pop-ups for this site to open the link.");
    }
  }
  return (
    <div className="backggroundColor flex justify-center w-full h-full">
      <div className="flex justify-center flex-col items-center w-2/6">
        <div class="circle">
          <p class="circle-inner">AY</p>
        </div>
        <div className="userName">
        @{userName?.data?.username}
        </div>
        {userData.length > 0 ? userData.map((item, index) => {
          return (
            <motion.div animate={{ y: -10 }}
              transition={{
                type: "spring", duration: 1, stiffness: 100,
                damping: 10
              }} key={index} className="cursor-pointer w-3/6 bg-white min-h-12 text-cyan-500 mb-2 border rounded-2xl mt-2 font-sans text-lg font-normal pl-12 pr-12 pt-2 pb-2">
              <div className="flex justify-center items-center bg-white font-medium font-sans" onClick={() => handlelink(item.link)}>
                {item.title}
              </div>
            </motion.div>
          )
        }) : null}
      </div>
    </div>
  );
}
```

Step 7 : Add CSS page for designing all the major component in the application, in the **`styles/global.css`**using the following code :

```css
@tailwind base;
@tailwind components;
@tailwind utilities;

@media (max-width: 1600px) {

    .deleteOptions {
        font-size: 19px;
        color: #f00;
    }

    .inActive {
        font-size: 19px;
        color: #9074d3;
    }

    .editOptions {
        font-size: 19px;
        color: #36c6d5;
    }
}

@media (max-width: 1200px) {

    .deleteOptions {
        font-size: 16px;
        color: #f00;
    }

    .inActive {
        font-size: 16px;
        color: #9074d3;
    }

    .editOptions {
        font-size: 16px;
        color: #36c6d5;
    }
}

.liveCard {
    height: 75vh;
}

.linkDiv {
    height: 9vh;
}

.headerDiv {
    height: 10vh;
}

@media (max-width: 1200px) {

    .liveCard {
        height: 80vh;
    }

    .linkDiv {
        height: 11vh;
    }

    .headerDiv {
        height: 10vh;
    }
}

@media (max-width: 1600px) {
    .liveCard {
        height: 80vh;
    }

    .linkDiv {
        height: 11vh;
    }

    .headerDiv {
        height: 10vh;
    }
}

.qrcode__container {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    width: 100%;
    height: 100vh;
}

.qrcode__container--parent {
    display: flex;
    gap: 10px;
    align-items: center;
    justify-content: center;
    width: 100%;
}

.qrcode__input {
    display: flex;
    flex-direction: column;
    align-items: center;
    width: 30%;
    margin-top: 20px;
}

.qrcode__input input {
    width: 100%;
    padding: 10px;
    font-size: 12px;
    outline: none;
    resize: none;
    border-radius: 5px;
    border: 1px solid #ccc;
    margin-bottom: 15px;
}

.qrcode__input button,
.qrcode__download button {
    display: inline-block;
    padding: 7px;
    cursor: pointer;
    border: none;
    border-radius: 5px;
    font-size: 15px;
    font-weight: 500;
    transition: background-color 0.2s;
}

.qrcode__download {
    display: flex;
    align-items: center;
    flex-direction: column;
    gap: 10px;
    margin-top: 10%;
}

.qrcode__download button {
    margin-top: 10px;
}

.backggroundColor {
    background: linear-gradient(-45deg, #ee7752, #e73c7e, #23a6d5, #23d5ab);
    background-size: 400% 400%;
    animation: gradient 15s ease infinite;
    height: 100vh;
}

@keyframes gradient {
    0% {
        background-position: 0% 50%;
    }

    50% {
        background-position: 100% 50%;
    }

    100% {
        background-position: 0% 50%;
    }
}

.circle {
    display: inline-block;
    background-color: #a0eefc;
    margin-left: 10px;
    margin-right: 10px;
    margin-top: 1px;
    margin-bottom: 10px;
    border-radius: 50%;
}

.circle-inner {
    color: rgb(0, 179, 211);
    display: table-cell;
    vertical-align: middle;
    text-align: center;
    text-decoration: none;
    height: 100px;
    width: 100px;
    font-size: 34px;
}

.userName{
    margin-bottom: 20%;
    font-size: 20px;
    font-weight: 600;
    font-family: 'Lucida Sans', 'Lucida Sans Regular', 'Lucida Grande', 'Lucida Sans Unicode', Geneva, Verdana, sans-serif;
    color: white;
}
```

Step 8 : Create Model in **`src/model/userModel.js` and add the following code  :** 

```jsx
const mongoose = require("mongoose");

const userSchema = new mongoose.Schema({
  username: { type: String, unique: true },
  password: String,
  links: [
    {
      title: { type: String },
      link: { type: String },
      status: { type: String },
    },
  ],
});
module.exports =mongoose.models.User || mongoose.model('User', userSchema);
```

Step 9 : Create Next.js API in the **`src/pages/api/getlinks.js` `src/pages/api/addlink.js` `src/pages/api/signin.js` `src/pages/api/signup.js` files :** 

```jsx
singup.js : 
import userModel from "../../model/userModel";
import connectDB from "../../lib/connectDB";

export default async function handler(req, res) {
  try {
    const { username, password } = req.body;
    console.log("Getting" , req.body.username , req.body)
    // Validate input (e.g., check if required fields are provided)
    await connectDB();
    // Check if the username already exists
    const existingUser = await userModel.findOne({ username });
    if (existingUser) {
      return res.status(400).json({ error: "Username already exists" });
    }

    // Create a new user
    const newUser = new userModel({ username, password });

    // Save the user to the database
    await newUser.save();

    res.status(201).json({ message: "User created successfully" });
  } catch (error) {
    console.error(error);
    res.status(500).json({ error: "Internal server error" });
  }
}

singin.js
import userModel from "../../model/userModel";
import connectDB from "../../lib/connectDB";
export default async function handler(req, res) {
  try {
    const { username, password } = req.body;
    console.log("data", req.body);
    await connectDB()
    // Check if the user exists
    const user = await userModel.findOne({ username });
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
}

addlinks.js
import userModel from "@/model/userModel";
import connectDB from "@/lib/connectDB";
export default async function handler(req, res) {
  try {
    await connectDB()
    const { title, link, status, userId } = req.body;
    console.log("Data getting" , title, link, status, userId)
    // Check if the user exists
    const user = await userModel.findOne({_id : userId});
    if (!user) {
      return res.status(401).json({ error: "User not found" });
    }
console.log("Data found" , user)
    // Push new link object to the links array
    user.links.push({ title, link, status });

    // Save the updated user document
    await user.save();

    res.status(200).json({ message: "Link added successfully", data: user });
  } catch (error) {
    console.error(error);
    res.status(500).json({ error: "Internal server error" });
  }
}

getlinks.js
import connectDB from "@/lib/connectDB";
import userModel from "@/model/userModel";
export default async function handler(req, res) {
  try {
    await connectDB()
    const { userID } = req.body;
    console.log("data", userID,req.body);
    // Check if the user exists
    const user = await userModel.findOne({ _id : userID });
    console.log("data" , user)
    res.status(200).json({ message: "linkData", data: user });
  } catch (error) {
    console.error(error);
    res.status(500).json({ error: "Internal server error" });
  }
}
```

Step 10 : Connect to mongodb atlas DBusing the following code in the **`src/lib/connectDB.js` :**

```jsx
import mongoose from "mongoose";

let cached = global.mongoose;

if (!cached) {
  cached = global.mongoose = { conn: null, promise: null };
}

async function connectDB() {
  if (cached.conn) {
    return cached.conn;
  }

  if (!cached.promise) {
    const opts = {
      bufferCommands: false,
    };

    cached.promise = mongoose
      .connect(
      //Add yout own connect string from the mongo db atlas
        "mongodb+srv://t@cluster0.nknrg9k.mongodb.net/?retryWrites=true&w=majority&appName=Cluster0",
        opts
      )
      .then((mongoose) => {
        console.log("Db Connected!");
        return mongoose;
      });
  }
  cached.conn = await cached.promise;
  return cached.conn;
}

export default connectDB;
```

Step 11 : Create URL link file in the **`source/index.js`** and add the following code ( add your won vercel link for deployment ) :

```jsx
const liveLink = "https://linkshortner-psi.vercel.app"
const localURI = "http://localhost:8000"
export const URI = `${localURI}`
```

Final step Run your application using the following code : 

```jsx
npm run dev -p 8000
```