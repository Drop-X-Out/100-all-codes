# E-Commerce Application with React, Redux Toolkit, Tailwind CSS and Firebase-Part 2

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
