# E-Commerce Application with React, Redux Toolkit, Tailwind CSS and Firebase-Part 1

In this tutorial we will be building an E-Commerce Application using FakeStore-API, Firebase, React, Redux Toolkit and Stripe API. We will use FakeStore API to fetch data of products, Firebase for Authentication and React for frontend.

![Untitled](https://talktohire.blr1.digitaloceanspaces.com/Untitled.png)

# Setting up the Environment

## Setting up the Frontend of the App

Execute the command, open a new terminal and follow these instructions:

- Enter "frontend" when prompted for the Project name
- Select React as the framework and JavaScript as the variant

```bash
npm create vite@latest

✔ Project name: … client
✔ Select a framework: › React
✔ Select a variant: › JavaScript
```

This will create the frontend directory in your root `application` folder. Run the following commands after it.

```bash
cd application
npm install
npm run dev
```

This will install the initial dependencies and start the development server of Vite running on `localhost:5173`. Open your browser and write the `localhost` URL to see the development server.

## Setting up the server

Execute the given commands by following these instructions:

- Enter “server” as input for package name after running `npm init`
- Click return for all other options asked to maintain the default state, this will create a server directory inside your root directory

```bash
npm init
package name: (user) server
```

Before starting with the code we will setup our file directory as per the below diagram. The `client` will contain all our logic and `server` directory will contain the `Stripe` API request.

```bash
application/
├── client/
│   ├── node_modules/
│   ├── public/
│   └── src/
│       ├── api/
│       ├── assets/
│       ├── components
│       │   ├── Banner.jsx
│       │   ├── Cartitem.jsx
│       │   ├── Footer.jsx
│       │   ├── Header.jsx
│       │   ├── Product.jsx
│       │   ├── ProductCard.jsx
│       │   └── Products.jsx
│       ├── pages
│       │   ├── Cart.jsx
│       │   ├── Home.jsx
│       │   └── Login.jsx
│       ├── redux
│       │   ├── bazarSlice.jsx
│       │   ├── store.jsx
│       ├── App.css
│       ├── App.jsx
│       ├── firebase.config.js
│       ├── index.css
│       ├── index.html
│       └── main.jsx
│   ├── package-lock.json
│   ├── package.json
│   ├── postcss.config.js
│   ├── README.md
│   ├── tailwind.config.js
│   └── vite.config.js
├── server/
│   ├── node_modules/
│   ├── package-lock.json
│   ├── package.json
│   └── server.js
└── .gitignore

```

## Building the Client Side (Front-end)

### Installing Dependencies

Open the terminal and paste the following snippet

```bash
npm install @reduxjs/toolkit axios dotenv firebase lucide-react react react-dom react-icons react-redux react-router-dom react-stripe-checkout react-toastify redux-persist
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

Now we will re run the server to make it effective using `npm run dev`

### Fetching data from Fake Store API

Now once we have our dependencies installed and environment ready we will setup the API request for our product data. Inside the `api` directory create an `Api.jsx` file and paste the following code, this will fetch all the items from Fakestore API.

```jsx
import axios from "axios";
const productsData = async () => {
  const products = await axios.get(
    "https://fakestoreapiserver.reactbd.com/products"
  );

  return products;
};

export default productsData;

```

### Setting up Redux Toolkit

We will now start with setting up redux toolkit for state management, in the `bazarSlice.jsx` file inside the `redux` directory, paste the following code.

```jsx
import { createSlice } from "@reduxjs/toolkit";
const initialState = {
  productData: [],
  userInfo: null,
};

export const bazarSlice = createSlice({
  name: "bazar",
  initialState,
  reducers: {
    addToCart: (state, action) => {
      const item = state.productData.find(
        (item) => item._id === action.payload._id
      );

      if (item) {
        item.quantity += action.payload.quantity;
      } else {
        state.productData.push(action.payload);
      }
    },
    increamentQuantity: (state, action) => {
      const item = state.productData.find(
        (item) => item._id === action.payload._id
      );
      if (item) {
        item.quantity++;
      }
    },
    decrementQuantity: (state, action) => {
      const item = state.productData.find(
        (item) => item._id === action.payload._id
      );
      if (item.quantity === 1) {
        item.quantity = 1;
      } else {
        item.quantity--;
      }
    },
    deleteItem: (state, action) => {
      state.productData = state.productData.filter(
        (item) => item._id !== action.payload
      );
    },
    resetCart: (state) => {
      state.productData = [];
    },
    // =============== User Start here ==============
    addUser: (state, action) => {
      state.userInfo = action.payload;
    },
    removeUser: (state) => {
      state.userInfo = null;
    },
    // =============== User End here ================
  },
});

export const {
  addToCart,
  deleteItem,
  resetCart,
  increamentQuantity,
  decrementQuantity,
  addUser,
  removeUser,
} = bazarSlice.actions;

export default bazarSlice.reducer;
```

Now once we have created the state reducers we will create the `store.jsx` file for redux

```jsx
import { configureStore } from "@reduxjs/toolkit";
import {
  persistStore,
  persistReducer,
  FLUSH,
  REHYDRATE,
  PAUSE,
  PERSIST,
  PURGE,
  REGISTER,
} from "redux-persist";
import storage from "redux-persist/lib/storage";
import bazarReducer from "./bazarSlice";

const persistConfig = {
  key: "root",
  version: 1,
  storage,
};
const persistedReducer = persistReducer(persistConfig, bazarReducer);

export const store = configureStore({
  reducer: { bazar: persistedReducer },
  middleware: (getDefaultMiddleware) =>
    getDefaultMiddleware({
      serializableCheck: {
        ignoredActions: [FLUSH, REHYDRATE, PAUSE, PERSIST, PURGE, REGISTER],
      },
    }),
});

export let persistor = persistStore(store);

```

### Setting up the Firebase Config SDK and Authentication

We will be using Google Authentication to setup session management, to use this we will be using `Firebase`. Inside the `src` directory, create a `firebase.config.js` file and paste the following values too initialize SDK.

```jsx
// Import the functions you need from the SDKs you need
import { initializeApp } from "firebase/app";
// TODO: Add SDKs for Firebase products that you want to use
// https://firebase.google.com/docs/web/setup#available-libraries

// Your web app's Firebase configuration
const firebaseConfig = {
  apiKey: "",
  authDomain: "",
  projectId: "",
  storageBucket: "",
  messagingSenderId: "",
  appId: ""
};

// Initialize Firebase
export const app = initializeApp(firebaseConfig);
```

If you need help in finding the variables you can use the commented links for help

Now inside the `Login.jsx` component we will setup the authentication request and logic using redux and firebase configuration we just did. Paste the following code inside your Login Page

```jsx
import React from "react";
import {
  GoogleAuthProvider,
  getAuth,
  signInWithPopup,
  signOut,
} from "firebase/auth";
import { toast } from "react-toastify";
import { useDispatch, useSelector } from "react-redux";
import { addUser, removeUser } from "../redux/bazarSlice";
import { useNavigate } from "react-router-dom";

const Login = () => {
  const auth = getAuth();
  const provider = new GoogleAuthProvider();
  const dispatch = useDispatch();
  const navigate = useNavigate();
  const userInfo = useSelector((state) => state.bazar.userInfo);

  const handleGoogleLogin = (e) => {
    e.preventDefault();
    signInWithPopup(auth, provider)
      .then((result) => {
        const user = result.user;
        dispatch(
          addUser({
            _id: user.uid,
            name: user.displayName,
            email: user.email,
            image: user.photoURL,
          })
        );
        setTimeout(() => {
          navigate("/");
        }, 1500);
      })
      .catch((err) => {
        console.error(err);
      });
  };

  const handleSignOut = () => {
    signOut(auth).then(() => {
      toast.success("Signed Out Successfully");
      dispatch(removeUser());
    });
  };

  return (
    <div className="flex items-center justify-center min-h-screen bg-gray-50">
      <div className="w-full max-w-md p-8 space-y-8 bg-white shadow-lg rounded-lg">
        <div className="text-center">
          <h2 className="mt-6 text-3xl font-extrabold text-gray-900">
            Welcome to eBazaar
          </h2>
          <p className="mt-2 text-sm text-gray-600">
            Sign in to your account
          </p>
        </div>
        {!userInfo ? (
          <div className="mt-8 space-y-6">
            <button
              onClick={handleGoogleLogin}
              className="w-full flex items-center justify-center px-4 py-2 border border-transparent text-sm font-medium rounded-md text-white bg-indigo-600 hover:bg-indigo-700 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-indigo-500"
            >
              <svg
                className="w-5 h-5 mr-2"
                fill="currentColor"
                viewBox="0 0 20 20"
              >
                <path d="M10 0C4.477 0 0 4.477 0 10c0 4.411 2.865 8.138 6.839 9.465.5.092.682-.217.682-.482 0-.237-.009-.866-.013-1.7-2.782.603-3.369-1.34-3.369-1.34-.454-1.156-1.11-1.463-1.11-1.463-.908-.62.069-.608.069-.608 1.003.07 1.531 1.03 1.531 1.03.892 1.529 2.341 1.087 2.91.831.092-.646.35-1.086.636-1.336-2.22-.253-4.555-1.11-4.555-4.943 0-1.091.39-1.984 1.029-2.683-.103-.253-.446-1.27.098-2.647 0 0 .84-.269 2.75 1.025A9.564 9.564 0 0110 4.844c.85.004 1.705.115 2.504.337 1.909-1.294 2.747-1.025 2.747-1.025.546 1.377.203 2.394.1 2.647.64.699 1.028 1.592 1.028 2.683 0 3.842-2.339 4.687-4.566 4.935.359.309.678.919.678 1.852 0 1.336-.012 2.415-.012 2.743 0 .267.18.579.688.481C17.137 18.135 20 14.411 20 10c0-5.523-4.477-10-10-10z" />
              </svg>
              Sign in with Google
            </button>
          </div>
        ) : (
          <button
            onClick={handleSignOut}
            className="w-full flex justify-center py-2 px-4 border border-transparent rounded-md shadow-sm text-sm font-medium text-white bg-red-600 hover:bg-red-700 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-red-500"
          >
            Sign Out of eBazaar
          </button>
        )}
      </div>
    </div>
  );
};

export default Login;
```

### Setting up Pages and Routes

In the root `App.jsx` file setup the routes and pages using reac-router-dom

```jsx
import "./App.css";
import Home from "./pages/Home";
import Header from "./components/Header";
import Footer from "./components/Footer";
import {
  createBrowserRouter,
  Outlet,
  RouterProvider,
  ScrollRestoration,
} from "react-router-dom";
import Cart from "./pages/Cart";
import productsData from "./api/Api";
import Product from "./components/Product";
import Login from "./pages/Login";

const Layout = () => {
  return (
    <>
      <Header />
      <ScrollRestoration />
      <Outlet />
      <Footer />
    </>
  );
};

const router = createBrowserRouter([
  {
    path: "/",
    element: <Layout />,
    children: [
      {
        path: "/",
        element: <Home />,
        loader: productsData,
      },
      {
        path: "/product/:id",
        element: <Product />,
      },
      {
        path: "/cart",
        element: <Cart />,
      },
      {
        path: "login",
        element: <Login />,
      },
    ],
  },
]);

function App() {
  return (
    <>
      <RouterProvider router={router} />
    </>
  );
}

export default App;

```

We still don’t have our `Pages` ready, before completing the pages we will start with `components` first and then import them to complete the pages.

### Building the Components

Copy paste the following components inside the `components` directory

- `Banner.jsx`

```jsx
import React, { useState } from "react";
import { ChevronLeft, ChevronRight } from "lucide-react";
import {Link} from 'react-router-dom';

const Banner = () => {
  const [currentSlide, setCurrentSlide] = useState(0);
  const data = [
    {
      image: "https://images.unsplash.com/photo-1534452203293-494d7ddbf7e0?q=80&w=1744&auto=format&fit=crop&ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D",
      title: "Summer Collection",
      subtitle: "Discover our latest styles",
      cta: "Shop Now"
    },
    {
      image: "https://images.unsplash.com/photo-1521566652839-697aa473761a?q=80&w=1742&auto=format&fit=crop&ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D",
      title: "New Arrivals",
      subtitle: "Fresh looks for the season",
      cta: "Explore"
    },
    {
      image: "https://images.unsplash.com/photo-1481437156560-3205f6a55735?q=80&w=1790&auto=format&fit=crop&ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D",
      title: "Limited Edition",
      subtitle: "Exclusive pieces, limited time",
      cta: "Get It Now"
    },
    {
      image: "https://images.unsplash.com/photo-1441984904996-e0b6ba687e04?q=80&w=1740&auto=format&fit=crop&ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D",
      title: "Clearance Sale",
      subtitle: "Up to 70% off selected items",
      cta: "Shop Sale"
    },
  ];

  const prevSlide = () => {
    setCurrentSlide(currentSlide === 0 ? data.length - 1 : currentSlide - 1);
  };

  const nextSlide = () => {
    setCurrentSlide(currentSlide === data.length - 1 ? 0 : currentSlide + 1);
  };

  return (
    <div className="relative w-full h-[650px] overflow-hidden">
      <div
        className="flex transition-transform duration-500 ease-out h-full"
        style={{ transform: `translateX(-${currentSlide * 100}%)` }}
      >
        {data.map((slide, index) => (
          <div key={index} className="w-full h-full flex-shrink-0 relative">
            <img
              src={slide.image}
              alt={slide.title}
              className="w-full h-full object-cover"
            />
            <div className="absolute inset-0 bg-black bg-opacity-40 flex items-center justify-center">
              <div className="text-center text-white">
                <h2 className="text-4xl font-bold mb-2">{slide.title}</h2>
                <p className="text-xl mb-4">{slide.subtitle}</p>
                <Link to="/#shop" className="bg-white text-black py-2 px-6 rounded-full hover:bg-opacity-90 transition-colors">
                  {slide.cta}
                </Link>
              </div>
            </div>
          </div>
        ))}
      </div>
      
      <button
        onClick={prevSlide}
        className="absolute top-1/2 left-4 transform -translate-y-1/2 bg-white bg-opacity-50 p-2 rounded-full hover:bg-opacity-75 transition-colors"
      >
        <ChevronLeft className="w-6 h-6 text-black" />
      </button>
      <button
        onClick={nextSlide}
        className="absolute top-1/2 right-4 transform -translate-y-1/2 bg-white bg-opacity-50 p-2 rounded-full hover:bg-opacity-75 transition-colors"
      >
        <ChevronRight className="w-6 h-6 text-black" />
      </button>

      <div className="absolute bottom-4 left-1/2 transform -translate-x-1/2 flex space-x-2">
        {data.map((_, index) => (
          <button
            key={index}
            onClick={() => setCurrentSlide(index)}
            className={`w-3 h-3 rounded-full ${
              currentSlide === index ? "bg-white" : "bg-white bg-opacity-50"
            }`}
          />
        ))}
      </div>
    </div>
  );
};

export default Banner;
```

![Untitled](https://talktohire.blr1.digitaloceanspaces.com/Untitled_1.png)