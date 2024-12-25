# Build Gmail Clone with MERN Stack

### **Course Description:**

Are you ready to dive deep into full-stack development with one of the most popular technology stacks? This course, "Building a Gmail Clone with the MERN Stack," will take you on an immersive journey through the creation of a fully functional email application, similar to Gmail, using MongoDB, Express.js, React, and Node.js (MERN).

### **Course Objectives:**

- **Understand the MERN Stack:** Gain comprehensive knowledge of MongoDB, Express.js, React, and Node.js, and how these technologies work together to create powerful web applications.
- **Develop a Full-Stack Application:** Build a real-world email application from scratch, covering both front-end and back-end development.
- **Implement User Authentication:** Create secure login and signup functionalities, ensuring user data is protected.
- **CRUD Operations:** Learn how to create, read, update, and delete emails in a MongoDB database.
- **State Management with Redux:** Manage the state of your application effectively using Redux Toolkit.
- **API Integration:** Integrate RESTful APIs to connect the front-end and back-end seamlessly.
- **Responsive Design:** Design a responsive user interface using Tailwind CSS that works across various devices and screen sizes.
- **Route Protection:** Implement middleware to protect routes and ensure only authenticated users can access certain parts of the application.
- **Persist State:** Ensure that user sessions are persisted, providing a seamless user experience.
- **Search Functionality:** Add search functionality to filter emails, enhancing the application's usability.

## **P-1 Project Setup (React Vite + Tailwind CSS) & Building Navbar, Sidebar, Inbox Pages**

### **1.1 Project Setup**

1. **Create a new Vite project:**
    
    ```bash
    bashCopy code
    npm create vite@latest my-email-app --template react
    cd my-email-app
    
    ```
    
2. **Install Tailwind CSS:**
    
    ```bash
    bashCopy code
    npm install -D tailwindcss postcss autoprefixer
    npx tailwindcss init -p
    
    ```
    
3. **Configure Tailwind CSS:**
    - **`tailwind.config.js`**
        
        ```jsx
        javascriptCopy code
        /** @type {import('tailwindcss').Config} */
        module.exports = {
          content: [
            "./index.html",
            "./src/**/*.{js,ts,jsx,tsx}",
          ],
          theme: {
            extend: {},
          },
          plugins: [],
        }
        
        ```
        
    - **`src/index.css`**
        
        ```css
        cssCopy code
        @tailwind base;
        @tailwind components;
        @tailwind utilities;
        
        ```
        
4. **Run the project:**
    
    ```bash
    bashCopy code
    npm install
    npm run dev
    
    ```
    

### **1.2 Building Navbar, Sidebar, Inbox Pages**

1. **Create Component Structure:**
    - Establish a folder structure for components and pages.
2. **Design Navbar:**
    - Implement a responsive navigation bar for the application.
3. **Design Sidebar:**
    - Implement a sidebar for email categories like Inbox, Sent, Drafts, and Trash.
4. **Design Inbox Page:**
    - Create an inbox page layout that includes email listings.
5. **Integrate Components:**
    - Combine Navbar, Sidebar, and Inbox components into the main page layout.

---

## **P-2 Understand React-Router-Dom and Redux Toolkit**

### **2.1 React-Router-Dom**

1. **Install React Router:**
    - Add React Router to the project to handle routing.
2. **Set Up Basic Routes:**
    - Define routes for different pages such as Inbox, Login, and Signup.
    - Use **`Link`** for navigation between pages.
    
    ### **1. Install React Router:**
    
    First, install the React Router library:
    
    ```bash
    bashCopy code
    npm install react-router-dom
    ```
    
    ```jsx
    
    import { createBrowserRouter, RouterProvider } from 'react-router-dom'
    import './App.css'
    import Inbox from './components/Inbox'
    import Navbar from './components/Navbar'
    import Sidebar from './components/Sidebar'
    import Body from './components/Body'
    import Mail from './components/Mail'
    import SendEmail from './components/SendEmail'
    import Login from './components/Login'
    import Signup from './components/Signup'
    import { Toaster } from 'react-hot-toast';
    import { useEffect } from 'react'
    import { useSelector } from 'react-redux'
    
    const appRouter = createBrowserRouter([
      {
        path: "/",
        element: <Body />,
        children: [
          {
            path: "/",
            element: <Inbox />
          },
          {
            path: "/mail/:id",
            element: <Mail />
          },
        ]
      },
      {
        path:"/login",
        element:<Login/>
      },
      {
        path:"/signup",
        element:<Signup/>
      }
    ])
    
    function App() {
      
    
      return (
        <div className='bg-[#F6F8FC] h-screen'>
          
          <RouterProvider router={appRouter} />
          <div className='absolute w-[30%] bottom-0 right-20 z-10'>
            <SendEmail />
          </div>
          <Toaster />
        </div>
      )
    }
    
    export default App
    
    ```
    

### **2.2 Redux Toolkit**

1. **Install Redux Toolkit:**
    - Add Redux Toolkit and React-Redux for state management.
2. **Setup Redux Store:**
    - Create a Redux store and configure it with reducers.
3. **Create Redux Slices:**
    - Define slices for managing application state, such as emails and user authentication.
4. **Provide Store to Application:**
    - Use the **`Provider`** component to make the Redux store available throughout the application.
    
    **Install Redux Toolkit:**
    
    ```bash
    bashCopy code
    npm install @reduxjs/toolkit react-redux
    
    ```
    
    ```jsx
    // configuring our store
    
    import { configureStore } from "@reduxjs/toolkit";
    import appSlice from "./appSlice";
    
    const store = configureStore({
        reducer: {
            app:appSlice
        } 
    });
    export default store;
    ```
    
    **Creating Slice:**
    
    ```jsx
    import { createSlice } from "@reduxjs/toolkit"
    
    const appSlice = createSlice({
        name: "app",
        initialState: {
            open: false,
            user: null,
            emails: [],
            selectedEmail: null,
            searchText:"",
        },
        reducers: {
            // actions
            setOpen: (state, action) => {
                state.open = action.payload
            },
            setAuthUser: (state, action) => {
                state.user = action.payload;
            },
            setEmails: (state, action) => {
                state.emails = action.payload;
            },
            setSelectedEmail: (state, action) => {
                state.selectedEmail = action.payload;
            },
            setSearchText:(state,action) => {
                state.searchText = action.payload;
            }
        }
    });
    export const { setOpen, setAuthUser, setEmails, setSelectedEmail, setSearchText } = appSlice.actions;
    export default appSlice.reducer;
    ```
    

## **P-3 Building Login & Signup Page + Create Server & Connect with MongoDB Atlas**

### **3.1 Frontend - Login & Signup Pages**

1. **Design Login Page:**
    - Create a form for user login with email and password fields.
2. **Design Signup Page:**
    - Create a form for new user registration with email and password fields.
3. **Update Routing:**
    - Add routes for the login and signup pages.

### **3.2 Backend - Server Setup & Database Connection**

1. **Create a Node.js Server:**
    - Set up an Express.js server.
2. **Connect to MongoDB Atlas:**
    - Establish a connection to a MongoDB Atlas cluster.
    
    ```jsx
    // Connecting to MongoDB Databse
    
    import mongoose from "mongoose";
    
    const connectDB = async () => {
        try {
            await mongoose.connect(process.env.MONGO_URI);
            console.log('Mongodb connected successfully.');
        } catch (error) {
            console.log(error);
        }
    }
    export default connectDB;
    ```
    
    ```jsx
    // Creating Nodejs Server
    
    import express from "express"; // react style
    import dotenv from "dotenv";
    import connectDB from "./db/connectDB.js";
    import cookieParser from "cookie-parser";
    import cors from "cors";
    import userRoute from "./routes/user.route.js";
    import emailRoute from "./routes/email.route.js";
    
    dotenv.config({});
    connectDB();
    const PORT = 8080;
    const app = express();
    
    // middleware
    app.use(express.urlencoded({extended:true}));
    app.use(express.json());
    app.use(cookieParser());
    
    const corsOptions = {
        origin:'http://localhost:5173',
        credentials:true
    }
    app.use(cors(corsOptions));
    
    // routes
    app.use("/api/v1/user", userRoute);
    app.use("/api/v1/email", emailRoute);
    
    app.listen(PORT, ()=>{
        console.log(`Server running at port ${PORT}`);
    });
    ```
    

## **P-4 Schema Design + User Controllers & Routes for Login & Signup + Middleware**

### **4.1 Schema Design**

1. **Define User Schema:**
    - Create a Mongoose schema for user data.
    
    ```jsx
    // User Schema
    
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
        },
        profilePhoto:{
            type:String,
            required:true
        }
    },{timestamps:true});
    export const User = mongoose.model("User", userSchema);
    ```
    
    ```jsx
    // Email Schema
    
    import mongoose from "mongoose";
    
    const emailSchema = new mongoose.Schema({
        to:{
            type:String,
            required:true
        },
        subject:{
            type:String,
            required:true
        },
        message:{
            type:String,
            required:true
        },
        userId:{
            type:mongoose.Schema.Types.ObjectId,
            ref:'User'
        }
    },{timestamps:true});
    export const Email = mongoose.model("Email", emailSchema);
    ```
    

### **4.2 User Controllers & Routes**

1. **Create User Controllers:**
    - Implement controller functions for user registration and login.
2. **Set Up Routes:**
    - Define API routes for login and signup.
    
    ```jsx
    // User Api Routes
    
    import express from "express";
    import { login, logout, register } from "../controllers/user.controller.js";
    
    const router = express.Router();
    
    router.route("/register").post(register);
    router.route("/login").post(login);
    router.route("/logout").get(logout);
    
    export default router;
    
    ```
    

### **4.3 Middleware**

1. **Authentication Middleware:**
    - Create middleware to handle authentication and protect routes.
    
    ```jsx
    // Middleware 
    
    import jwt from "jsonwebtoken";
    
    const isAuthenticated = async (req,res,next) => {
        try {
            const token = req.cookies.token;
           
            if(!token){
                return res.status(401).json({message:"User not authenticated"});
            }
    
            const decode = await jwt.verify(token, process.env.SECRET_KEY);
            if(!decode) {
                return res.status(401).json({message:"Invalid token"});
            }
            req.id = decode.userId;
            next();
        } catch (error) {
            console.log(error);
        }
    }
    export default isAuthenticated;
    
    ```
    

---

## **P-5 Creating Email Controller & API Endpoints (Create, Get, Delete Email)**

### **5.1 Email Controller**

1. **Implement Email Logic:**
    - Create controller functions for creating, fetching, and deleting emails.

### **5.2 API Endpoints**

1. **Define Email Routes:**
    - Set up API endpoints for email operations (create, get, delete).
    
    ```jsx
    // Email Api Routes
    
    import express from "express"; 
    import { createEmail, deleteEmail, getAllEmailById } from "../controllers/email.controller.js";
    import isAuthenticated from "../middleware/isAuthenticated.js";
    
    const router = express.Router();
    
    router.route("/create").post(isAuthenticated, createEmail);
    router.route("/:id").delete(isAuthenticated, deleteEmail);
    router.route("/getallemails").get(isAuthenticated, getAllEmailById);
    
    export default router;
    
    ```
    

---

## **P-6 API Integration with Frontend (Login, Signup, Fetch, Create & Delete Email)**

### **6.1 Frontend Integration**

1. **Connect Login & Signup:**
    - Integrate frontend forms with backend authentication API.
    
    ```jsx
    // Signup 
    
    import axios from 'axios';
    import React, { useState } from 'react'
    import toast from 'react-hot-toast';
    import { useDispatch } from 'react-redux';
    import { Link, useNavigate } from 'react-router-dom'
    import { setAuthUser } from '../redux/appSlice';
    
    const Login = () => {
    
        const [input, setInput] = useState({
            email:"",
            password:""
        });
        const dispatch = useDispatch();
        const navigate = useNavigate();
    
        const changeHandler = (e) => {
            setInput({...input, [e.target.name]:e.target.value});
        }
    
        const submitHandler = async (e) => {
            e.preventDefault();
            console.log(input);
            try {
            // Api Integration
                const res = await axios.post("http://localhost:8080/api/v1/user/login", input, {
                    headers:{
                        'Content-Type':"application/json"
                    },
                    withCredentials:true
                });
    
                if(res.data.success){
                    dispatch(setAuthUser(res.data.user));
                    navigate("/");
                    toast.success(res.data.message);
                }
            } catch (error) {
                console.log(error);
                toast.error(error.response.data.message);
            }
        }
    
        return (
            <div className='flex items-center justify-center w-screen h-screen'>
                <form onSubmit={submitHandler} className='flex flex-col gap-3 bg-white p-4 w-[20%]'>
                    <h1 className='font-bold text-2xl uppercase my-2'>Login</h1>
                    <input onChange={changeHandler} value={input.email} name="email" type='email' placeholder='Email' className='border border-gray-400 rounded-md px-2 py-1' />
                    <input onChange={changeHandler} value={input.password} name="password" type='password' placeholder='Password' className='border border-gray-400 rounded-md px-2 py-1' />
                    <button type="submit" className='bg-gray-800 p-2 text-white my-2 rounded-md'>Login</button>
                    <p>Don't have an account? <Link to={"/signup"} className='text-blue-600'>Signup</Link></p>
                </form>
            </div>
        )
    }
    
    export default Login
    ```
    
    ```jsx
    // Signup 
    
    import React, { useState } from 'react'
    import { Link, useNavigate } from 'react-router-dom'
    import axios from "axios";
    import toast from 'react-hot-toast';
    
    const Signup = () => {
        const [input, setInput] = useState({
            fullname:"",
            email:"",
            password:""
        });
    
        const navigate = useNavigate();
    
        const changeHandler = (e) => {
            setInput({...input, [e.target.name]:e.target.value});
        }
    
        const submitHandler = async (e) => {
            e.preventDefault();
            try {
            // Api Integration
                const res = await axios.post("http://localhost:8080/api/v1/user/register", input, {
                    headers:{
                        'Content-Type':"application/json"
                    },
                    withCredentials:true
                });
                if(res.data.success){
                    navigate("/login");
                    toast.success(res.data.message);
                }
            } catch (error) {
                console.log(error);
                toast.error(error.response.data.message);
            }
        }
    
        return (
            <div className='flex items-center justify-center w-screen h-screen'>
                <form onSubmit={submitHandler} className='flex flex-col gap-3 bg-white p-4 w-[20%]'>
                    <h1 className='font-bold text-2xl uppercase my-2'>Signup</h1>
                    <input onChange={changeHandler} value={input.fullname} name='fullname' type='text' placeholder='Name' className='border border-gray-400 rounded-md px-2 py-1' />
                    <input onChange={changeHandler} value={input.email} name='email' type='email' placeholder='Email' className='border border-gray-400 rounded-md px-2 py-1' />
                    <input onChange={changeHandler} value={input.password} name='password' type='password' placeholder='Password' className='border border-gray-400 rounded-md px-2 py-1' />
                    <button type="submit" className='bg-gray-800 p-2 text-white my-2 rounded-md'>Signup</button>
                    <p>Already have an account? <Link to={"/login"} className='text-blue-600'>Login</Link></p>
                </form>
            </div>
        )
    }
    
    export default Signup
    ```
    
2. **Email Operations:**
    - Implement frontend logic for fetching, creating, and deleting emails through the API.

### **6.2 Redux Integration**

1. **Update Redux Store:**
    - Update store to handle new actions related to email operations.

---

## **P-7 Persist Our Store + Searching Emails + Protecting Routes**

### **7.1 Persist Redux Store**

1. **Persist State:**
    - Use middleware to persist the Redux store state across sessions.

## Steps to Persist Redux Toolkit State

- **Install redux-persist:**
    
    ```bash
    npm install redux-persist
    ```
    
- **Configure redux-persist:** Update `store.js` to use `redux-persist` for persisting the state.
    
    ```jsx
    jsxCopy code
    import { configureStore } from '@reduxjs/toolkit';
    import {
      persistStore,
      persistReducer,
      FLUSH,
      REHYDRATE,
      PAUSE,
      PERSIST,
      PURGE,
      REGISTER,
    } from 'redux-persist';
    import storage from 'redux-persist/lib/storage';
    import { combineReducers } from 'redux';
    import emailsReducer from './slices/emailsSlice';
    import authReducer from './slices/authSlice';
    
    const persistConfig = {
      key: 'root',
      storage,
    };
    
    const rootReducer = combineReducers({
      emails: emailsReducer,
      auth: authReducer,
    });
    
    const persistedReducer = persistReducer(persistConfig, rootReducer);
    
    const store = configureStore({
      reducer: persistedReducer,
      middleware: (getDefaultMiddleware) =>
        getDefaultMiddleware({
          serializableCheck: {
            ignoredActions: [FLUSH, REHYDRATE, PAUSE, PERSIST, PURGE, REGISTER],
          },
        }),
    });
    
    export const persistor = persistStore(store);
    export default store;
    
    ```
    
- **Wrap the Application with PersistGate:** Update `index.js` to include `PersistGate`.
    
    ```jsx
    jsxCopy code
    import React from 'react';
    import ReactDOM from 'react-dom';
    import { Provider } from 'react-redux';
    import { PersistGate } from 'redux-persist/integration/react';
    import store, { persistor } from './store';
    import App from './App';
    
    ReactDOM.render(
      <Provider store={store}>
        <PersistGate loading={null} persistor={persistor}>
          <App />
        </PersistGate>
      </Provider>,
      document.getElementById('root')
    );
    
    ```
    

### **7.2 Search Functionality**

1. **Implement Search:**
    - Add a search feature to filter emails based on keywords.

### **7.3 Route Protection**

1. **Protect Routes:**
    - Use authentication middleware to protect sensitive routes in the application.