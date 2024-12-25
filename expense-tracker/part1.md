# Build Expense Tracker App with MERN Stack

### Part 1: Expense Tracker Demo

### Introduction

- Overview of the Expense Tracker project.
- Features to be implemented.
- Technologies and tools to be used (Node.js, Express, MongoDB, React, Redux, Vite, Shadcn UI, etc.).

### Part 2: Backend Project Setup

### 1. Initialize Project

- Install Node.js and npm.
- Create a new directory for the project.
- Run `npm init -y` to create a `package.json` file.

### 2. Install Dependencies

- Express: `npm install express`
- Mongoose: `npm install mongoose`
- Other necessary packages: `npm install body-parser cors dotenv bcrypt jsonwebtoken`

### 3. Setup Basic Express Server

```jsx
import express, { urlencoded } from "express";
import cookieParser from "cookie-parser";
import dotenv from "dotenv";
import connectDB from "./utils/db.js";
import userRoute from "./routes/user.route.js";
import expenseRoute from "./routes/expense.route.js";
import cors from "cors";

dotenv.config({});

connectDB();

const app = express();
const PORT = process.env.PORT || 3000;

// middleware
app.use(express.json());
app.use(urlencoded({extended:true}));
app.use(cookieParser());
const corsOptions = {
    origin:"http://localhost:5173",
    credentials:true
}
app.use(cors(corsOptions));

// api's
app.use("/api/v1/user", userRoute);
app.use("/api/v1/expense", expenseRoute);

app.listen(PORT, ()=>{
    console.log(`Server listen at port ${PORT}`);
})
```

### Part 3: Database Connection + Schema Design + Login/Signup Controllers and APIs

### 1. Database Connection

- Create a `.env` file for environment variables.
- Add MongoDB connection string to `.env`.
- Setup Mongoose connection.

```jsx
import mongoose from "mongoose";

const connectDB = async () => {
    try {
        await mongoose.connect(process.env.MONGO_URI);
        console.log('mongodb connected successfully');
    } catch (error) {
        console.log(error);
    }
};
export default connectDB;
```

### 2. Schema Design

- Create User and Expense schemas.

```jsx
// Schema For User
import mongoose from "mongoose";

const userSchema = new mongoose.Schema({
    fullname:{
        type:String,
        required:true
    },
    email:{
        type:String,
        required:true
    },
    password:{
        type:String,
        required:true
    }
});

export const User  = mongoose.model("User", userSchema);

// Schema For Expense
import mongoose from "mongoose";

const expenseSchema = new mongoose.Schema({
    description: { type: String, required: true },
    amount: { type: Number, required: true },
    category: { type: String, required: true },
    done: { type: Boolean, default: false },
    userId:{type:mongoose.Schema.Types.ObjectId,ref:'User'},
}, { timestamps: true });

export const Expense = mongoose.model('Expense', expenseSchema);

```

### 3. Login/Signup Controllers and APIs

- Create controllers for user signup and login.
- Setup routes for authentication.

```jsx
import { User } from "../models/user.model.js";
import bcrypt from "bcryptjs";
import jwt from "jsonwebtoken";

export const register = async (req, res) => {
    try {
        const { fullname, email, password } = req.body;
        if (!fullname || !email || !password) {
            return res.status(400).json({
                message: "All fields are required.",
                success: false
            });
        };
        const user = await User.findOne({email});
        if (user) {
            return res.status(400).json({
                message: "User already exist with this email.",
                success: false
            });
        }
        const hashedPassword = await bcrypt.hash(password, 10);
        await User.create({
            fullname,
            email,
            password: hashedPassword
        });
        return res.status(201).json({
            message:"Account created successfully.",
            success:true
        });
    } catch (error) {
        console.log(error);
    }
}
export const login = async (req, res) => {
    try {
        const { email, password } = req.body;
        if (!email || !password) {
            return res.status(400).json({
                message: "All fields are required.",
                success: false
            });
        };
        const user = await User.findOne({email});
        if (!user) {
            return res.status(400).json({
                message: "Incorrect email or password",
                success: false
            });
        }
        const isPasswordMatch = await bcrypt.compare(password, user.password);
        if (!isPasswordMatch) {
            return res.status(400).json({
                message: "Incorrect email or password",
                success: false
            });
        };
        const tokenData = {
            userId: user._id,
        };
        const token = await jwt.sign(tokenData, process.env.SECRET_KEY, { expiresIn: '1d' });
        return res.status(200).cookie("token", token, { maxAge: 1 * 24 * 60 * 60 * 1000, httpOnly: true, sameSite: 'strict' }).json({
            message: `Welcome back ${user.fullname}`,
            user:{
                _id:user._id,
                fullname:user.fullname,
                email:user.email
            },
            success: true
        });
    } catch (error) {
        console.log(error);
    }
}
export const logout = async (req,res) => {
    try {
        return res.status(200).cookie("token", "", {maxAge:0}).json({
            message:"User logged out successfully.",
            success:true
        });   
    } catch (error) {
        console.log(error);
    }
}
```

### Part 4: Auth Middleware + Expense Controllers and APIs

### 1. Auth Middleware

- Create middleware to verify JWT tokens.

```jsx
import jwt from "jsonwebtoken";
const isAuthenticated = async (req,res, next)=>{
    try {
        const token = req.cookies.token;
        if(!token){
            return res.status(401).json({
                message:"User not authenticated",
                success:false
            })
        };
        const decode = await jwt.verify(token, process.env.SECRET_KEY);
        if(!decode){
            return res.status(401).json({
                message:"Invalid token",
                success:false
            });
        }
        req.id = decode.userId;
        next();
    } catch (error) {
        console.log(error);
    }
}
export default isAuthenticated;

```

### 2. Expense Controllers

- Create controllers to handle adding, getting, updating, and removing expenses.

```jsx
import { Expense } from "../models/expense.model.js";

export const addExpense = async (req, res) => {
    try {
        const { description, amount, category } = req.body;
        const userId = req.id; // current logged in user
        if (!description || !amount || !category) {
            return res.status(400).json({
                message: "Something is missing!",
                success: false
            });
        }
        const expense = await Expense.create({
            description,
            amount: Number(amount),
            category,
            userId
        });
        return res.status(201).json({
            message: "New Expense Added.",
            expense,
            success: true
        });
    } catch (error) {
        console.log(error);
    }
};
export const getAllExpenses = async (req, res) => {
    try {
        const userId = req.id;
        let category = req.query.category || ""; // Default to empty string if category is not provided
        const done = req.query.done || ""; // Default to empty string if done is not provided

        // Build the query object
        const query = {
            userId, // Filter by userId
        };

        // Handle special case for category "all"
        if (category.toLowerCase() === "all") {
            // No need to filter by category
        } else {
            // Regular category filter with case-insensitive regex match
            query.category = { $regex: category, $options: 'i' };
        }

        // Handle done filter interpretation
        if (done.toLowerCase() === "done") {
            query.done = true; // Filter for expenses marked as done (true)
        } else if (done.toLowerCase() === "undone") {
            query.done = false; // Filter for expenses marked as pending (false)
        }
        // Fetch expenses based on the constructed query
        const expenses = await Expense.find(query);

        // Check if expenses were found
        if (!expenses || expenses.length === 0) {
            return res.status(404).json({
                message: "No expenses found.",
                success: false
            });
        }

        // Return the found expenses
        return res.status(200).json({
            expenses,
            success: true
        });
    } catch (error) {
        console.log(error);
    }
};
export const markAsDoneOrUndone = async (req, res) => {
    try {
        const expenseId = req.params.id;
        const done = req.body;
        const expense = await Expense.findByIdAndUpdate(expenseId, done, { new: true });
        if (!expense) {
            return res.status(404).json({
                message: "Expense not found.",
                success: false
            })
        };
        return res.status(200).json({
            message: `Expense marked as ${expense.done ? 'done' : 'undone'}.`,
            success: true
        });
    } catch (error) {
        console.log(error);
    }
}
export const removeExpense = async (req, res) => {
    try {
        const expenseId = req.params.id;
        await Expense.findByIdAndDelete(expenseId);
        return res.status(200).json({
            message: "Expense removed.",
            success: true
        });
    } catch (error) {
        console.log(error);
    }
}
export const updateExpense = async (req, res) => {
    try {
        const { description, amount, category } = req.body;
        
        const expenseId = req.params.id;
        const updateData = { description, amount, category };
        
        const expense = await Expense.findByIdAndUpdate(expenseId, updateData, { new: true });
        return res.status(200).json({
            message: "Expense Updated.",
            expense,
            success: true
        });

    } catch (error) {
        console.log(error);
    }
}
```

### Part 5: API Testing + Setup React Vite with Shadcn UI + Integrate Login & Signup API

### 1. API Testing

- Use Postman or Insomnia to test the APIs created in the previous parts.

### 2. Setup React Vite with Shadcn UI

- Create a new React project using Vite: `npm create vite@latest`
- Install Shadcn UI components.

# **Vite**

Install and configure Vite.

### **Create project**

Start by creating a new React project using `vite`:

**`npm** create vite@latest`

### **Add Tailwind and its configuration**

Install `tailwindcss` and its peer dependencies, then generate your `tailwind.config.js` and `postcss.config.js` files:

**`npm** install -D tailwindcss postcss autoprefixer`

**`npx** tailwindcss init -p`

### **Edit tsconfig.json file**

The current version of Vite splits TypeScript configuration into three files, two of which need to be edited. Add the `baseUrl` and `paths` properties to the `compilerOptions` section of the `tsconfig.json` and `tsconfig.app.json` files:

```jsx
{
  "compilerOptions": {
    // ...
    "baseUrl": ".",
    "paths": {
      "@/*": [
        "./src/*"
      ]
    }
    // ...
  }
}
```