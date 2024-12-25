# E-Commerce Application with React, Redux Toolkit, Tailwind CSS and Firebase-Part 3

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