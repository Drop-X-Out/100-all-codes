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
    