# E-Commerce Application with React, Redux Toolkit, Tailwind CSS and Firebase

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

- `CartItem.jsx`

```jsx
import React from "react";
import { useSelector, useDispatch } from "react-redux";
import { HiOutlineArrowLeft } from "react-icons/hi";
import { Link, useNavigate } from "react-router-dom";
import {
  decrementQuantity,
  deleteItem,
  increamentQuantity,
} from "../redux/bazarSlice";

import { ToastContainer, toast } from "react-toastify";

const CartItem = () => {
  const productData = useSelector((state) => state.bazar.productData);
  const dispatch = useDispatch();

  return (
    <div className="w-full md:pr-14  h-full flex flex-col gap-4 sm:border-r-2 ">
      <h2 className="text-xl text-gray-700 font-titleFont border-b-2 py-2">
        Your Shopping Cart
      </h2>

      {productData.length > 0 ? (
        productData.map((product) => (
          <div className="border-b-2 pb-4 flex gap-4 p-1" key={product._id}>
            <figure className=" w-32 md:w-40">
              <img
                src={product.image}
                alt="productImg"
                className="max-w-full block rounded-md object-cover"
              />
            </figure>
            <div className="flex flex-col w-1/2 gap-4 py-3 text-gray-700">
              <h2
                className="text-lg md:text-2xl"
              >
                {product.title}
              </h2>
              <p className="md:text-lg">${product.price * product.quantity}</p>
              <div className="flex gap-3 border w-fit items-center">
                <button
                  onClick={() =>
                    dispatch(
                      decrementQuantity({
                        _id: product._id,
                        title: product.title,
                        image: product.image,
                        price: product.price,
                        quantity: 1,
                        description: product.description,
                      })
                    )
                  }
                  className="border h-5 font-normal text-lg flex items-center justify-center px-2 hover:bg-gray-700 hover:text-white cursor-pointer duration-300 active:bg-black"
                >
                  -
                </button>
                <span className="text-sm">{product.quantity}</span>
                <button
                  onClick={() =>
                    dispatch(
                      increamentQuantity({
                        _id: product._id,
                        title: product.title,
                        image: product.image,
                        price: product.price,
                        quantity: 1,
                        description: product.description,
                      })
                    )
                  }
                  className="border h-5 font-normal text-lg flex items-center justify-center px-2 hover:bg-gray-700 hover:text-white cursor-pointer duration-300 active:bg-black"
                >
                  +
                </button>
              </div>
              <button
                onClick={() =>
                  dispatch(deleteItem(product._id)) &
                  toast.error(`${product.title} is removed`)
                }
                className="bg-red-500 text-white py-2 mt-2 w-16 md:w-20 text-xs md:text-sm rounded hover:bg-transparent
            border border-red-500 hover:text-red-500 transition-all duration-200"
              >
                Remove
              </button>
            </div>
          </div>
        ))
      ) : (
        <div className="h-full w-full">
          <Link to="/">
            <button className="flex items-center gap-1 text-gray-400 hover:text-black duration-300">
              <span>
                <HiOutlineArrowLeft />
              </span>
              go shopping
            </button>
          </Link>
          <p className="w-full h-full grid place-content-center italic  text-gray-400">
            Your Cart is Empty
          </p>
        </div>
      )}
      <ToastContainer
        position="top-left"
        autoClose={3000}
        hideProgressBar={false}
        newestOnTop={false}
        closeOnClick
        rtl={false}
        pauseOnFocusLoss
        draggable
        pauseOnHover
        theme="dark"
      />
    </div>
  );
};

export default CartItem;

```

- `Footer.jsx`

```jsx
import React from "react";
import { Link } from "react-router-dom";

const Footer = () => {
  const currentYear = new Date().getFullYear();

  return (
    <footer className="bg-gray-950 text-gray-400">
      <div className="w-full max-w-screen-xl mx-auto px-4 py-8 md:py-12">
        <div className="flex flex-col md:flex-row md:items-center md:justify-between space-y-6 md:space-y-0">
          <div className="flex items-center">
            <span className="text-2xl md:text-3xl text-white font-bold font-titleFont relative">
              eBazaar
              <span className="absolute bottom-1 left-0 w-full h-0.5 bg-white transform scale-x-0 transition-transform duration-300 group-hover:scale-x-100"></span>
            </span>
          </div>
          <nav>
            <ul className="flex flex-wrap justify-center md:justify-end gap-4 md:gap-8 text-sm font-medium">
              {["About", "Privacy Policy", "Licensing", "Contact"].map((item) => (
                <li key={item}>
                  <Link to="#" className="hover:text-white transition-colors duration-300">
                    {item}
                  </Link>
                </li>
              ))}
            </ul>
          </nav>
        </div>
        <hr className="my-8 border-gray-700" />
        <div className="text-center text-sm">
          © {currentYear} eBazaar. All Rights Reserved.
        </div>
      </div>
    </footer>
  );
};

export default Footer;
```

- `Header.jsx`

```jsx
import React, { useState } from "react";
import logo from "../assets/img/header-logo.svg";
import cartIcon from "../assets/img/cartLogo.svg";
import { Link, useNavigate } from "react-router-dom";
import { useSelector } from "react-redux";
import { ToastContainer } from "react-toastify";
import { AiOutlineUser } from "react-icons/ai";
import { HiOutlineMenuAlt3 } from "react-icons/hi";

const Header = () => {
  const navigate = useNavigate();
  const [menu, setMenu] = useState(false);
  const productData = useSelector((state) => state.bazar.productData);
  const userInfo = useSelector((state) => state.bazar.userInfo) || "";

  const handleNavigate = () => navigate("/");
  const handleNavigateCart = () => navigate("/cart");

  const NavItems = () => (
    <>
      <li className="hover:text-gray-600 transition-colors duration-300">
        <Link to="/">Home</Link>
      </li>
      <li className="hover:text-gray-600 transition-colors duration-300">Shop</li>
      {userInfo.name && (
        <li className="text-blue-600 font-medium">{userInfo.name}</li>
      )}
      <li>
        <Link to="/login">
          <AiOutlineUser
            className="w-6 h-6 border border-black rounded-full p-1 hover:bg-gray-100 transition-colors duration-300"
          />
        </Link>
      </li>
      <li className="relative" onClick={handleNavigateCart}>
        <img src={cartIcon} alt="cart" className="w-7 h-7 hover:opacity-80 transition-opacity duration-300" />
        {productData.length > 0 && (
          <span className="absolute -top-1 -right-1 bg-red-500 text-white text-xs rounded-full w-4 h-4 flex items-center justify-center">
            {productData.length}
          </span>
        )}
      </li>
    </>
  );

  return (
    <header className="fixed w-full z-10 bg-white shadow-md">
      <div className="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
        <div className="flex justify-between items-center h-16">
          <figure className="w-36 sm:w-44">
            <img
              onClick={handleNavigate}
              src={logo}
              alt="logo"
              className="w-full cursor-pointer"
            />
          </figure>
          <nav className="hidden md:block">
            <ul className="flex items-center space-x-8">
              <NavItems />
            </ul>
          </nav>
          <button
            className="md:hidden"
            onClick={() => setMenu(!menu)}
          >
            <HiOutlineMenuAlt3 className="h-6 w-6" />
          </button>
        </div>
      </div>
      {menu && (
        <nav className="md:hidden">
          <ul className="flex flex-col items-center space-y-4 py-4 bg-white shadow-lg">
            <NavItems />
          </ul>
        </nav>
      )}
      <ToastContainer
        position="top-left"
        autoClose={2000}
        hideProgressBar={false}
        closeOnClick
        rtl={false}
        draggable
        theme="dark"
        newestOnTop={false}
      />
    </header>
  );
};

export default Header;
```

- `Product.jsx`

```jsx
import React, { useEffect, useState } from "react";
import { useLocation } from "react-router-dom";
import { MdOutlineStar } from "react-icons/md";
import { useDispatch } from "react-redux";
import { addToCart } from "../redux/bazarSlice";
import { ToastContainer, toast } from "react-toastify";

const Product = () => {
  const dispatch = useDispatch();
  const [details, setDetails] = useState({});
  let [baseQty, setBaseQty] = useState(1);
  const location = useLocation();
  useEffect(() => {
    setDetails(location.state.item);
  }, [location]);

  return (
    <div className="mt-24 min-h-[70vh]">
      <div className="max-w-screen-xl mx-auto my-10 flex flex-col items-center sm:flex-row gap-10 px-4">
        <div className="w-full sm:w-2/5 relative">
          <img
            className="w-full h-[350px] lg:h-[550px] object-cover rounded-lg shadow-md"
            src={details.image}
            alt={details.title}
          />
          <div className="absolute top-4 right-0">
            {details.isNew && (
              <p className="bg-black text-white font-semibold font-titleFont px-6 py-1 rounded-l-lg">
                Sale
              </p>
            )}
          </div>
        </div>
        <div className="w-full sm:w-3/5 flex flex-col justify-center gap-8">
          <div>
            <h2 className="text-3xl md:text-4xl font-semibold mb-2">{details.title}</h2>
            <div className="flex items-center gap-4">
              <p className="line-through text-base text-gray-500">
                ${details.oldPrice}
              </p>
              <p className="text-2xl font-medium text-gray-900">
                ${details.price}
              </p>
            </div>
          </div>
          <div className="flex items-center gap-2">
            <div className="flex text-yellow-400">
              {[...Array(5)].map((_, index) => (
                <MdOutlineStar key={index} />
              ))}
            </div>
            <p className="text-sm text-gray-500">(1 Customer review)</p>
          </div>
          <p className="text-base text-gray-600">{details.description}</p>
          <div className="flex flex-col sm:flex-row gap-4">
            <div className="flex gap-3 items-center justify-between text-gray-500 border border-gray-300 rounded p-3">
              <p className="text-sm ">Quantity</p>
              <div className="flex items-center gap-2 text-sm font-semibold">
                <button
                  onClick={() => setBaseQty(Math.max(1, baseQty - 1))}
                  className="border h-8 w-8 flex items-center justify-center hover:bg-gray-100 rounded-full transition-colors duration-300"
                >
                  -
                </button>
                <span>{baseQty}</span>
                <button
                  onClick={() => setBaseQty(baseQty + 1)}
                  className="border h-8 w-8 flex items-center justify-center hover:bg-gray-100 rounded-full transition-colors duration-300"
                >
                  +
                </button>
              </div>
            </div>
            <button
              onClick={() => {
                dispatch(
                  addToCart({
                    _id: details._id,
                    title: details.title,
                    image: details.image,
                    price: details.price,
                    quantity: baseQty,
                    description: details.description,
                  })
                );
                toast.success(`${details.title} is added`);
              }}
              className="bg-black text-white py-3 px-6 rounded-lg hover:bg-gray-800 transition-colors duration-300"
            >
              Add to Cart
            </button>
          </div>
          <p className="text-sm text-gray-500">
            Category: <span className="font-medium capitalize">{details.category}</span>
          </p>
        </div>
      </div>
      <ToastContainer
        position="top-left"
        autoClose={2000}
        hideProgressBar={false}
        newestOnTop={false}
        closeOnClick
        rtl={false}
        pauseOnFocusLoss
        draggable
        pauseOnHover
        theme="light"
      />
    </div>
  );
};

export default Product;
```

- `ProductCard.jsx`

```jsx
import React from "react";
import { BsArrowBarRight } from "react-icons/bs";
import { useDispatch } from "react-redux";

import { useNavigate } from "react-router-dom";
import { addToCart } from "../redux/bazarSlice";
import { toast } from "react-toastify";

const ProductCard = ({ product }) => {
  const dispatch = useDispatch();
  const navigate = useNavigate();
  const _id = product.title;
  const _idString = (_id) => {
    return String(_id).toLowerCase().split(" ").join("");
  };
  const rootId = _idString(_id);

  const handleDetails = () => {
    navigate(`/product/${rootId}`, {
      state: {
        item: product,
      },
    });
  };

  return (
    <div className="group relative rounded-2xl overflow-hidden border-b">
      <figure
        onClick={handleDetails}
        className="w-60 sm:w-72 lg:w-full h-96 hover:cursor-pointer overflow-hidden "
      >
        <img
          src={product.image}
          alt="productImg"
          className="lg:w-full h-full object-cover group-hover:scale-110 duration-500"
        />
      </figure>
      <div className="info border border-b-0 px-2 py-4 w-full flex flex-col gap-0.5 ">
        <div className="flex justify-between">
          <div>
            <h2>{product.title.substring(0, 15)}</h2>
          </div>
          <div className="flex justify-center gap-2 text-sm relative overflow-hidden w-28 hover:cursor-pointer">
            <div className="flex gap-2 relative justify-end transform group-hover:translate-x-24 duration-500">
              <p className="line-through text-gray-500">${product.oldPrice}</p>
              <p className="font-semibold">${product.price}</p>
            </div>
            <p
              onClick={() =>
                dispatch(
                  addToCart({
                    _id: product._id,
                    title: product.title,
                    image: product.image,
                    price: product.price,
                    quantity: 1,
                    description: product.description,
                  })
                ) && toast.success(`${product.title} added to Cart`)
              }
              className="absolute z-20 w-[100px] top-0 transform -translate-x-32 group-hover:translate-x-0 duration-500 text-gray-500 flex items-center "
            >
              add to cart &nbsp;
              <span>
                <BsArrowBarRight />
              </span>
            </p>
          </div>
        </div>
        <div>
          <p className="text-sm text-gray-700 capitalize">{product.category}</p>
        </div>
        <div className="absolute top-4 right-0">
          {product.isNew && (
            <p className="bg-slate-900 text-gray-100 text-sm text-center font-titleFont font-semibold rounded-l-lg  px-4 py-0.5 ">
              Sale
            </p>
          )}
        </div>
      </div>
    </div>
  );
};

export default ProductCard;

```

- `Products.jsx`

```jsx
import React from "react";
import ProductCard from "./ProductCard";

const Products = ({ products }) => {
  return (
    <div id="shop" className="py-10">
      <div className="text-center mt-12 mb-6">
          <h2 className="text-3xl font-bold mb-4">Our Products</h2>
          <div className="w-24 h-1 bg-blue-600 mx-auto mb-4"></div>
          <p className="text-gray-600 max-w-2xl mx-auto">
            Discover our curated collection of high-quality products. From everyday essentials to unique finds, we've got something for everyone.
          </p>
        </div>
      <div  className=" max-w-screen-xl mx-auto flex justify-center flex-wrap lg:grid grid-cols-4 gap-10 py-10 ">
        {products.map((item) => (
          <ProductCard key={item._id} product={item} />
        ))}
      </div>
    </div>
  );
};

export default Products;

```

![Untitled](https://talktohire.blr1.digitaloceanspaces.com/Untitled_2.png)

### Setting up the Pages

- `Cart.jsx`

```jsx
import React, { useEffect, useState } from "react";
import { useSelector } from "react-redux";

import CartItem from "../components/CartItem";
import { ToastContainer, toast } from "react-toastify";
import StripeCheckout from "react-stripe-checkout";
import axios from "axios";

const Cart = () => {
  const productData = useSelector((state) => state.bazar.productData);
  const [totalAmt, setTotalAmt] = useState(null);
  const [payNow, setPayNow] = useState(false);
  const userInfo = useSelector((state) => state.bazar.userInfo);

  useEffect(() => {
    let price = 0;
    productData.map((product) => (price += product.price * product.quantity));
    setTotalAmt(price.toFixed(2));
  }, [productData]);

  const payment = async (token) => {
    await axios.post("https://eBazaar-api-backend/pay", {
      amount: totalAmt * 100,
      token: token,
    });
  };

  const handleCheckout = () => {
    if (userInfo) {
      setPayNow(true);
    } else {
      toast.error("Please sign in to Checkout");
    }
  };
  return (
    <div className="mt-24 flex flex-col items-center min-h-screen px-1">
      <div className="flex gap-3 flex-wrap sm:flex-nowrap justify-center sm:justify-start max-w-screen-xl p-4 my-1">
        <div className="w-5/6 sm:w-1/2  md:w-2/3">
          <CartItem />
        </div>
        <div className="w-5/6 sm:w-1/2 pl-5 md:pl-0 md:w-1/3 text-sm lg:text-base flex flex-col gap-3 items-center text-gray-800  pt-3">
          <h2 className="w-full font-titleFont font-semibold  text-gray-900 text-2xl mb-3">
            Cart Total
          </h2>
          <div className="w-full flex items-center gap-3 mb-1">
            <span className="w-1/5">Subtotal</span>
            <p className="w-4/5 text-lg">${totalAmt}</p>
          </div>
          <div className="flex gap-3 w-full mb-4">
            <span className="w-1/5">Shipping</span>
            <p className="w-4/5">
              Lorem ipsum dolor sit amet consectetur adipisicing elit. Aperiam
              consequuntur hic unde porro culpa dolores reprehenderit provident{" "}
            </p>
          </div>
          <div className="border border-gray-600 w-full px-2 mb-4"></div>
          <div className="flex  justify-between w-full items-center">
            <span>Total</span>
            <p className="text-lg font-semibold">$ {totalAmt}</p>
          </div>

          <button
            className="bg-gray-900 text-white w-full py-2"
            onClick={handleCheckout}
          >
            Proceed to Checkout
          </button>
          {payNow && (
            <div className="w-full mt-2 flex items-center justify-center">
              <StripeCheckout
                stripeKey="pk_test_51NPN6ZSAdCrwmZUXYsOcpBg6s7nFA90QIR1frvzXZKrHQo5wL5spmSyC6zVbqOWQEfAaPgeSLoviCfSmaCwUZvJT00Ufprot9c"
                name="Bazar Online Shopping"
                amount={totalAmt * 100}
                label="Pay to bazar"
                description={`Your Payment amount is $${totalAmt}`}
                token={payment}
                email={userInfo.email}
                className="w-full"
              />
            </div>
          )}
        </div>
      </div>
      <ToastContainer
        position="top-left"
        autoClose={2000}
        hideProgressBar={false}
        newestOnTop={false}
        closeOnClick
        rtl={false}
        pauseOnFocusLoss
        draggable
        pauseOnHover
        theme="dark"
      />
    </div>
  );
};

export default Cart;

```

![Untitled](https://talktohire.blr1.digitaloceanspaces.com/Untitled_3.png)

- `Home.jsx`

```jsx
import { useEffect, useState } from "react";
import { useLoaderData } from "react-router-dom";
import Banner from "../components/Banner";
import Products from "../components/Products";

const Home = () => {
  const response = useLoaderData();
  const [products, setProducts] = useState([]);

  useEffect(() => {
    setProducts(response.data);
  }, [response]);

  return (
    <div>
      <Banner />
      <Products products={products} />
    </div>
  );
};

export default Home;

```

![Untitled](https://talktohire.blr1.digitaloceanspaces.com/Untitled_4.png)

- `Login.jsx`- We created the Login Page when we setup the Firebase SDK

![Untitled](https://talktohire.blr1.digitaloceanspaces.com/Untitled_5.png)

## Setting up the Stripe and Express Server

We will be setting up an express server to setup the Stripe payments, to get started with server code start by installing the dependencies.

### Installing the Dependencies

```jsx
npm install body-parser cors dotenv express nodemon path stripe
```

### Creating the Express Server

```jsx
const express = require("express");
const app = express();
const cors = require("cors");
const path = require("path");

const bodyParser = require("body-parser");
require("dotenv").config();
const port = process.env.PORT;
const Stripe = require("stripe")(process.env.STRIPE_SECRET_KEY);

app.use(
  cors({
    origin: ["https://myebazarstore.onrender.com"],
  })
);
app.use(bodyParser.json());
app.use(bodyParser.urlencoded({ extended: true }));

app.use(express.static(path.join(__dirname, "..", "client", "dist")));

app.get('/*', (req, res) => {
	res.sendFile(path.join(__dirname, '../client/dist/index.html'), (err) => {
		if (err) {
			console.error('Error sending file:', err);
		}
	});
});

app.post("/pay", async (req, res) => {
  console.log(req.body.token);
  await Stripe.charges.create({
    source: req.body.token.id,
    amount: req.body.amount,
    currency: "usd",
  });
});

app.listen(port, () => {
  console.log(`Server is running on Port ${port}`);
});

```

### Setting Environment Variables

```jsx
STRIPE_SECRET_KEY=
//Stripe in Test Mode, get yours from stripe.com
PORT=
```

### Run the Server

```jsx
npm run start
```

The Stripe and Express server is now running and will work perfectly fine now.