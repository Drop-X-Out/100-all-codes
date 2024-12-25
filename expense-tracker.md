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

### **Update vite.config.ts**

Add the following code to the vite.config.ts so your app can resolve paths without error

```jsx
import path from "path"
import react from "@vitejs/plugin-react"
import { defineConfig } from "vite"
 
export default defineConfig({
  plugins: [react()],
  resolve: {
    alias: {
      "@": path.resolve(__dirname, "./src"),
    },
  },
})
```

### **Run the CLI**

Run the `shadcn-ui` init command to setup your project:

**`npx** shadcn-ui@latest init`

### **Configure components.json**

You will be asked a few questions to configure `components.json`:

```jsx
Would you like to use TypeScript (recommended)? no / yes
Which style would you like to use? › Default
Which color would you like to use as base color? › Slate
Where is your global CSS file? › › src/index.css
Do you want to use CSS variables for colors? › no / yes
Where is your tailwind.config.js located? › tailwind.config.js
Configure the import alias for components: › @/components
Configure the import alias for utils: › @/lib/utils
Are you using React Server Components? › no / yes (no)
```

### **That's it**

You can now start adding components to your project.

**`npx** shadcn-ui@latest add button`

The command above will add the `Button` component to your project. You can then import it like this:

```jsx
import { Button } from "@/components/ui/button"
 
export default function Home() {
  return (
    <div>
      <Button>Click me</Button>
    </div>
  )
}
```

### 3. Integrate Login & Signup API

- Create a login and signup form in React.
- Use Axios to make API calls to the backend.

Signup Page API Integration

```jsx
import React, { useState } from 'react'
import { Label } from './ui/label'
import { Input } from './ui/input'
import { Button } from './ui/button'
import Logo from './shared/Logo'
import { Link, useNavigate } from 'react-router-dom'
import { toast } from 'sonner'
import axios from 'axios'
import { USER_API_END_POINT } from '@/utils/endpoints'
import { Loader2 } from 'lucide-react'
import { useDispatch, useSelector } from 'react-redux'
import { setLoading } from '@/redux/authSlice'

const Signup = () => {
    const [input, setInput] = useState({
        fullname: "",
        email: "",
        password: ""
    });
    const {loading} = useSelector(store=>store.auth);
    const dispatch = useDispatch();
    const navigate = useNavigate();
    const changeEventHandler = (e) => {
        setInput({ ...input, [e.target.name]: e.target.value });
    }
    // api integration
    const submitHandler = async (e) => {
        e.preventDefault();
        try {
           dispatch(setLoading(true)); 
            const res = await axios.post(`${USER_API_END_POINT}/register`, input, {
                headers: {
                    'Content-Type': "application/json"
                },
                withCredentials: true
            });
            console.log(res);
            if (res.data.success) {
                navigate("/login");
                toast.success(res.data.message);
            }
        } catch (error) {
            console.log(error);
            toast.error(error.response.data.message);
        } finally {
            dispatch(setLoading(false)); 
        }
    }
    return (
        <div className='flex items-center justify-center w-screen h-screen'>
            <form onSubmit={submitHandler} className='w-96 p-8 shadow-lg'>
                <div className='w-full flex justify-center mb-5'>
                    <Logo />
                </div>
                <div>
                    <Label>Full Name</Label>
                    <Input
                        type="text"
                        name="fullname"
                        value={input.fullname}
                        onChange={changeEventHandler}
                    />
                </div>
                <div>
                    <Label>Email</Label>
                    <Input
                        type="email"
                        name="email"
                        value={input.email}
                        onChange={changeEventHandler}
                    />
                </div>
                <div>
                    <Label>Password</Label>
                    <Input
                        type="password"
                        name="password"
                        value={input.password}
                        onChange={changeEventHandler}
                    />
                </div>
                {
                    loading ? (
                        <Button className='w-full my-4'>
                            <Loader2 className='mr-2 h-4 w-4 animate-spin' />
                            Please wait
                        </Button>
                    ) : (
                        <Button type="submit" className="w-full my-5">Signup</Button>
                    )
                }

                <p className='text-center text-sm'>Already have an account? <Link to="/login" className="text-blue-600">Login</Link></p>
            </form>
        </div>
    )
}

export default Signup
```

Login Page API Integration

```jsx
import React, { useState } from 'react'
import { Label } from './ui/label'
import { Input } from './ui/input'
import { Button } from './ui/button'
import Logo from './shared/Logo'
import { Link, useNavigate } from 'react-router-dom'
import axios from 'axios'
import { USER_API_END_POINT } from '@/utils/endpoints'
import { toast } from 'sonner'
import { useDispatch, useSelector } from 'react-redux'
import { setAuthUser, setLoading } from '@/redux/authSlice'
import { Loader2 } from 'lucide-react'

const Login = () => {
    const [input, setInput] = useState({
        email: "",
        password: ""
    });
    const { loading } = useSelector(store => store.auth);
    const dispatch = useDispatch();
    const navigate = useNavigate();
    const changeEventHandler = (e) => {
        setInput({ ...input, [e.target.name]: e.target.value });
    }
    // api integration
    const submitHandler = async (e) => {
        e.preventDefault();
        try {
            dispatch(setLoading(true));
            const res = await axios.post(`${USER_API_END_POINT}/login`, input, {
                headers: {
                    'Content-Type': "application/json"
                },
                withCredentials: true
            });
            if (res.data.success) {
                dispatch(setAuthUser(res.data.user));
                navigate("/");
                toast.success(res.data.message);
            }
        } catch (error) {
            console.log(error);
            toast.error(error.response.data.message);
        } finally {
            dispatch(setLoading(false));
        }
    }
    return (
        <div className='flex items-center justify-center w-screen h-screen'>
            <form onSubmit={submitHandler} className='w-96 p-8 shadow-lg'>
                <div className='w-full flex justify-center mb-5'>
                    <Logo />
                </div>
                <div>
                    <Label>Email</Label>
                    <Input
                        type="email"
                        name="email"
                        value={input.email}
                        onChange={changeEventHandler}
                    />
                </div>
                <div>
                    <Label>Password</Label>
                    <Input
                        type="password"
                        name="password"
                        value={input.password}
                        onChange={changeEventHandler}
                    />
                </div>
                {
                    loading ? (
                        <Button className='w-full my-4'>
                            <Loader2 className='mr-2 h-4 w-4 animate-spin' />
                            Please wait
                        </Button>
                    ) : (
                        <Button className="w-full my-5">Login</Button>
                    )
                }

                <p className='text-center text-sm'>Don't have an account? <Link to="/signup" className="text-blue-600">Signup</Link></p>
            </form>
        </div>
    )
}

export default Login
```

### Part 6: Building Create Expense & Filter Page + Configuring Redux Toolkit

### 1. Configure Redux Toolkit

- Install Redux Toolkit: `npm install @reduxjs/toolkit react-redux`
- Setup Redux store and slices.

```jsx
import {configureStore} from "@reduxjs/toolkit";
import authSlice from "./authSlice.js";
import expenseSlice from "./expenseSlice.js";

const store = configureStore({
    reducer:{
        auth:authSlice,
        expense:expenseSlice
    }
})
export default store;
```

```jsx
// Auth Slice 
import { createSlice } from "@reduxjs/toolkit";

const authSlice = createSlice({
    name:"auth",
    initialState:{
        loading:false,
        user:null
    },
    reducers:{
        // actions
        setLoading:(state, action) => {
             state.loading = action.payload;
        },
        setAuthUser:(state,action) => {
            state.user = action.payload;
        }
    }
});
export const {
    setLoading,
    setAuthUser
} = authSlice.actions;

export default authSlice.reducer;
```

```jsx
// Expense Slice
import { createSlice } from "@reduxjs/toolkit";

const expenseSlice = createSlice({
    name:"expense",
    initialState:{
        loading:false,
        expenses:[],
        category:"",
        markAsDone:"",
        singleExpense:null
    },
    reducers:{
        // actions
        setLoading:(state, action) => {
            state.loading = action.payload;
        },
        setExpenses:(state,action) => {
            state.expenses = action.payload;
        },
        setCategory:(state,action) => {
            state.category = action.payload;
        },
        setMarkAsDone:(state,action) => {
            state.markAsDone = action.payload;
        },
        setSingleExpense:(state,action) => {
            state.singleExpense = action.payload;
        }
    }
});
export const {
    setLoading,
    setExpenses,
    setCategory,
    setMarkAsDone,
    setSingleExpense
} = expenseSlice.actions;

export default expenseSlice.reducer;
```

### 2. Create Expense Page

- Create components for adding expenses and filtering them.

```jsx
// Create Expense Page
import { Button } from "@/components/ui/button";
import {
    Dialog,
    DialogContent,
    DialogDescription,
    DialogFooter,
    DialogHeader,
    DialogTitle,
    DialogTrigger,
} from "@/components/ui/dialog";
import { Input } from "@/components/ui/input";
import { Label } from "@/components/ui/label";
import { Select, SelectContent, SelectGroup, SelectItem, SelectTrigger, SelectValue } from "./ui/select";
import { useState } from "react";
import axios from "axios";
import { EXPENSE_API_END_POINT } from "@/utils/endpoints";
import { toast } from "sonner";
import { useDispatch, useSelector } from "react-redux";
import { setExpenses, setLoading } from "@/redux/expenseSlice";
import { Loader2 } from "lucide-react";

export function CreateExpense() {
    const [formData, setFormData] = useState({
        description: "",
        amount: "",
        category: "",
    });
    const [isOpen, setIsOpen] = useState(false);
    const { expenses, loading } = useSelector(store => store.expense);
    const dispatch = useDispatch();

    const handleChange = (e) => {
        const { name, value } = e.target;
        setFormData((prevData) => ({
            ...prevData,
            [name]: value,
        }));
    };

    const handleCategoryChange = (value) => {
        setFormData((prevData) => ({
            ...prevData,
            category: value,
        }));
    };

    const handleSubmit = async (e) => {
        e.preventDefault();
        try {
            dispatch(setLoading(true));
            const res = await axios.post(`${EXPENSE_API_END_POINT}/add`, formData, {
                headers: {
                    'Content-Type': 'application/json'
                },
                withCredentials: true
            });
            if (res.data.success) {
                dispatch(setExpenses([...expenses, res.data.expense]));
                toast.success(res.data.message);
                setIsOpen(false);  // Close the dialog after success
            }
        } catch (error) {
            console.log(error);
            toast.error(error.response?.data?.message || "An error occurred");
        } finally {
            dispatch(setLoading(false));
        }
    };

    return (
        <Dialog open={isOpen} onOpenChange={setIsOpen}>
            <DialogTrigger asChild>
                <Button onClick={() => setIsOpen(true)}>Add New Expense</Button>
            </DialogTrigger>
            <DialogContent className="sm:max-w-[425px]">
                <DialogHeader>
                    <DialogTitle>Add Expense</DialogTitle>
                    <DialogDescription>
                        Create expense to here. Click add when you're done.
                    </DialogDescription>
                </DialogHeader>
                <form onSubmit={handleSubmit}>
                    <div className="grid gap-4 py-4">
                        <div className="grid grid-cols-4 items-center gap-4">
                            <Label htmlFor="description" className="text-right">
                                Description
                            </Label>
                            <Input
                                id="description"
                                placeholder="description"
                                className="col-span-3"
                                name="description"
                                value={formData.description}
                                onChange={handleChange}
                            />
                        </div>
                        <div className="grid grid-cols-4 items-center gap-4">
                            <Label htmlFor="amount" className="text-right">
                                Amount
                            </Label>
                            <Input
                                id="amount"
                                placeholder="xxx in ₹"
                                className="col-span-3"
                                name="amount"
                                value={formData.amount}
                                onChange={handleChange}
                            />
                        </div>
                        <Select onValueChange={handleCategoryChange}>
                            <SelectTrigger>
                                <SelectValue placeholder="Select a category" />
                            </SelectTrigger>
                            <SelectContent>
                                <SelectGroup>
                                    <SelectItem value="rent">Rent</SelectItem>
                                    <SelectItem value="food">Food</SelectItem>
                                    <SelectItem value="salary">Salary</SelectItem>
                                    <SelectItem value="shopping">Shopping</SelectItem>
                                    <SelectItem value="other">Other</SelectItem>
                                </SelectGroup>
                            </SelectContent>
                        </Select>
                    </div>
                    <DialogFooter>
                        {
                            loading ? (
                                <Button className='w-full my-4'>
                                    <Loader2 className='mr-2 h-4 w-4 animate-spin' />
                                    Please wait
                                </Button>
                            ) : (
                                <Button type="submit">Add</Button>
                            )
                        }
                    </DialogFooter>
                </form>
            </DialogContent>
        </Dialog>
    );
}
```

### Part 7: Fetch All Expenses (Delete & Update Expense) + Persist Redux Store

### 1. Fetch All Expenses

- Use Axios to fetch expenses from the backend and dispatch them to the Redux store.

```jsx
import { setExpenses } from "@/redux/expenseSlice"
import { EXPENSE_API_END_POINT } from "@/utils/endpoints"
import axios from "axios"
import { useEffect } from "react"
import { useDispatch, useSelector } from "react-redux"

const useGetExpenses = () => {
    const dispatch = useDispatch();
    const { category, markAsDone } = useSelector(store => store.expense);

    useEffect(() => {
        const fetchExpenses = async () => {
            try {
                axios.defaults.withCredentials = true;
                const res = await axios.get(`${EXPENSE_API_END_POINT}/getall?category=${category}&done=${markAsDone}`);
                if (res.data.success) {
                    dispatch(setExpenses(res.data.expenses));
                }
            } catch (error) {
                console.log(error);
            }
        }
        fetchExpenses();
    }, [dispatch, category, markAsDone]);
};
export default useGetExpenses;
```

### 2. Delete & Update Expense

- Create functions to delete and update expenses and integrate them into the components.

```jsx
// Update Expense
const submitHandler = async (e) => {
        e.preventDefault();
        console.log(formData);
        try {
            setLoading(true);
            const res = await axios.put(`http://localhost:8000/api/v1/expense/update/${expense._id}`, formData, {
                headers: {
                    'Content-Type': 'application/json'
                },
                withCredentials: true
            });
            if (res.data.success) { 
                const updatedExpenses = expenses.map(exp => exp._id === expense._id ? res.data.expense: exp);
                dispatch(setExpenses(updatedExpenses));
                toast.success(res.data.message);
                setIsOpen(false);
            }
        } catch (error) {
            console.log(error);
            toast.error(error.response.data.message);
        } finally {
            setLoading(false);
        }
    }
    
    // Delete Expense
    const removeExpenseHandler = async (expenseId) => {
        try {
            const res = await axios.delete(`http://localhost:8000/api/v1/expense/remove/${expenseId}`);
            if (res.data.success) {
                toast.success(res.data.message);
                // update the local state
                const filteredExpenses = localExpense.filter(expense => expense._id !== expenseId);
                setLocalExpense(filteredExpenses);
            }
        } catch (error) {
            console.log(error);
        }
    }
```

### 3. Persist Redux Store

- Use Redux Persist to save the Redux state across sessions.

```jsx
import { combineReducers, configureStore } from "@reduxjs/toolkit";
import authSlice from "./authSlice";
import expenseSlice from "./expenseSlice";
import { 
    persistReducer,
    FLUSH,
    REHYDRATE,
    PAUSE,
    PERSIST,
    PURGE,
    REGISTER,
} from 'redux-persist'
import storage from 'redux-persist/lib/storage' 

const persistConfig = {
    key: 'root',
    version: 1,
    storage,
}
const rootReducer = combineReducers({
    auth: authSlice,
    expense: expenseSlice
})
const persistedReducer = persistReducer(persistConfig, rootReducer)

const store = configureStore({
    reducer: persistedReducer,
    middleware: (getDefaultMiddleware) =>
        getDefaultMiddleware({
            serializableCheck: {
                ignoredActions: [FLUSH, REHYDRATE, PAUSE, PERSIST, PURGE, REGISTER],
            },
        }),
})
export default store;
```