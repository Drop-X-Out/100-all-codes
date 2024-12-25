# Step-by-Step Guide to Creating a Startup SaaS Template in Next.js with Supabase and Stripe

In this guide, we’ll walk you through the process of creating a Next.js SaaS Startup template. You’ll learn how to incorporate Supabase for seamless data management, Stripe for secure transactions, Framer Motion to beautifully animate your application and comprehensive user authentication. By the end, you'll have a fully functional sign-in and sign-up page ready to power your startup with payment gateway attached to it.

Before starting with the guide, it is essential to learn how to configure **Supabase** and **Stripe**. We would recommend going through the following section to familiarize yourself with these configurations. If you are already knowledgeable about the basics, you may proceed with the guide.

## **1. Configuring Supabase and Stripe**

In this section, you'll learn how to set up Supabase and Stripe to power your Next.js SaaS application. We'll guide you through the process of creating a Supabase project for seamless data management and configuring Stripe for secure payment processing. By following these steps, you'll ensure that your application is equipped with robust backend services and a reliable payment gateway, setting a strong foundation for your startup.

# **Stripe:**

To integrate Stripe, you'll need to sign up for a Stripe account and obtain the necessary API keys. Follow these steps to get started:

![Untitled](https://talktohire.blr1.digitaloceanspaces.com/Untitled.png)

### 1.Sign Up for a Stripe Account

1. **Visit the Stripe Website**:
    - Go to [Stripe's website](https://stripe.com/).
2. **Create an Account**:
    - Click on the "Start now" or "Sign up" button.
    - Fill in your email address, full name, and country of operation, then create a password.
    - Click on "Create account."
3. **Verify Your Email**:
    - Stripe will send a verification email to the address you provided.
    - Open the email and click on the verification link.
4. **Set Up Your Stripe Account**:
    - Provide additional details about your business, such as the business name, type, and address.
    - Enter your bank account details for payouts.
    - Complete the account setup by following the prompts on the dashboard.

### 2. Obtain API Keys

1. **Log In to Your Stripe Dashboard**:
    - Go to [Stripe Dashboard](https://dashboard.stripe.com/) and log in with your credentials.
2. **Access API Keys**:
    - In the left-hand sidebar, navigate to the "Developers" section.
    - Click on "API keys."
3. **Get Your Publishable and Secret Keys**:
    - Under the "Standard keys" section, you will see the `Publishable key` and `Secret key`.
    - The `Publishable key` is labeled as `pk_test_xxx` (for test mode) and the `Secret key` is labeled as `sk_test_xxx` (for test mode).
    
    ![Untitled](https://talktohire.blr1.digitaloceanspaces.com/22d617ba-82b7-41ae-9931-8ed16c008d04.png)
    
4. **Copy the Keys**:
    - Click on the "Reveal test key token" button to view the `Secret key`.
    - Copy both keys to a secure location. These will be used as your `NEXT_PUBLIC_STRIPE_PUBLISHABLE_KEY` and `STRIPE_SECRET_KEY`.

### 3. Set Up Webhooks

1. **Create a Webhook Endpoint**:
    - In the Stripe Dashboard, go to "Developers" > "Webhooks."
    - Click on the "Add endpoint" button.
2. **Configure the Webhook**:
    - In the "Endpoint URL" field, enter the URL of your Next.js application where you will handle Stripe webhook events (e.g., `https://yourdomain.com/api/webhooks/stripe`).
    - Select the events you want to listen for, such as `checkout.session.completed`, `invoice.payment_succeeded`, and `customer.subscription.updated`. (You might also need ngrok to establish local connection)
3. **Obtain the Webhook Secret**:
    - After creating the webhook endpoint, click on the endpoint's details to reveal the `Signing secret`.
    - Copy the `Signing secret`. This will be used as your `STRIPE_WEBHOOK_SECRET`.

### 4. Store Your Keys Securely

1. **Environment Variables**:
    - In your Next.js project, create a `.env.local` file in the root directory.
    - Add the following environment variables to the file:
        
        ```jsx
        NEXT_PUBLIC_STRIPE_PUBLISHABLE_KEY=your_publishable_key
        STRIPE_SECRET_KEY=your_secret_key
        STRIPE_WEBHOOK_SECRET=your_webhook_secret
        ```
        
2. **Replace with Your Keys**:
    - Replace `your_publishable_key`, `your_secret_key`, and `your_webhook_secret` with the actual keys obtained from Stripe.

### 5. Create Product to sell in stripe:

- **Create a New Product**:
    - Click on the "Add product" button.
    - Fill in the product details:
        - **Name**: Enter the name of your product.
        - **Description**: Optionally, add a description.
        - **Price**: Set the price and currency. You can also choose between a one-time payment or a recurring subscription.
    - Click on "Save product."
- **Repeat for Additional Products**:
    - Repeat the above steps to create the additional products. For example, you might create "Basic Plan," "Standard Plan," and "Premium Plan."
    
    ![Untitled](https://talktohire.blr1.digitaloceanspaces.com/Untitled_1.png)
    
- **Obtain Product IDs**:
    - After creating the products, click on each product to view its details.
    - Note down the `Product ID` (it starts with `prod_`) for each product.
    - 
    
    ![Untitled](https://talktohire.blr1.digitaloceanspaces.com/9573ef2e-18d7-4c21-b1d2-57629c3cb246.png)
    

## Supabase

![Untitled](https://talktohire.blr1.digitaloceanspaces.com/Untitled_2.png)

### 1. Sign Up and Set Up a Supabase Project

1. **Visit the Supabase Website**:
    - Go to [Supabase](https://supabase.io/).
2. **Create an Account**:
    - Click on "Sign up" and create an account using your email, GitHub, or other available options.
    - Complete the sign-up process by following the prompts.
3. **Create a New Project**:
    - After logging in, click on "New Project."
    - Fill in the project details, such as the name, organization, database password, and region.
    - Click "Create new project" and wait for Supabase to initialize your project. This may take a few minutes.

### 2. Obtain the Project URL and API Keys

1. **Navigate to Project Settings**:
    - In your Supabase project dashboard, click on "Settings" in the left-hand sidebar.
2. **Get the `NEXT_PUBLIC_SUPABASE_URL` and `NEXT_PUBLIC_SUPABASE_ANON_KEY`**:
    - Go to the "API" tab under "Settings."
    - The `URL` and `anon key` can be found in the "Project API keys" section.
    - The `URL` is your `NEXT_PUBLIC_SUPABASE_URL`.
    - The `anon key` is your `NEXT_PUBLIC_SUPABASE_ANON_KEY`.
3. **Get the `SUPABASE_SERVICE_ROLE_KEY`**:
    - In the same "API" tab, you will find the `service_role` key.
    - The `service_role` key is your `SUPABASE_SERVICE_ROLE_KEY`.

### 3. Set Environment Variables in Your Next.js Project

1. **Create a `.env.local` File**:
    - In your Next.js project, create a `.env.local` file in the root directory if it doesn't already exist.
2. **Add the Environment Variables**:
    - Open the `.env.local` file and add the following lines, replacing the placeholders with your actual Supabase project URL and keys:
        
        ```
        NEXT_PUBLIC_SUPABASE_URL=your_supabase_url
        NEXT_PUBLIC_SUPABASE_ANON_KEY=your_supabase_anon_key
        SUPABASE_SERVICE_ROLE_KEY=your_supabase_service_role_key
        ```
        
    - Replace `your_supabase_url`, `your_supabase_anon_key`, and `your_supabase_service_role_key` with the actual values obtained from the Supabase dashboard.
    
    For now just add both the Stripe and Supabase links in the .env.local file and run the project.
    
    you can add data in the Supabase like below :
    
    ![Untitled](https://talktohire.blr1.digitaloceanspaces.com/Untitled_3.png)
    

*If you encounter any difficulties completing this guide, please set up our project locally and add the provided URLs to the `.env.local` file. Running the project locally will help you understand how the components interact and function.*

# 2. Step by Step Guide

To ensure a smooth and structured learning experience, We have divided this guide into three distinct phases:

1. **Creating the Landing Page**:
    - This phase provides a solid foundation for your SaaS template by guiding you through the creation of an engaging and functional landing page.
2. **Supabase and Authentication**:
    - In this phase, we’ll delve into integrating Supabase and implementing authentication, ensuring secure and efficient user management.
3. **Payment Gateway Integration**:
    - The final phase covers the integration of the Stripe payment gateway, enabling seamless and secure transactions.

This phased approach simplifies the process, allowing you to build and understand each component step-by-step.Once you complete this guide, you'll have a fully functional SaaS template ready for deployment.

[Video : Play](https://drive.google.com/file/d/1FSuQGNNYSIhOsJjW7DNPQ0t2kfj_ZfHa/view?usp=sharing)

## **Setting Up the Next.js Project**

Let's start by setting up a new Next.js project. Open your terminal and execute the following commands:

```jsx
npx create-next-app saas

✔ Would you like to use TypeScript with this project? … No
✔ Would you like to use ESLint with this project? … Yes
✔ Would you like to use Tailwind CSS with this project? … Yes
✔ Would you like to use `src/` directory with this project? … Yes
✔ Use App Router (recommended)? … Yes
✔ Would you like to customize the default import alias? … No
```

This will create a new Next.js project in a directory named **`saas`**. Next, open the project in your preferred code editor.

```jsx
cd saas
```

## **Installing Dependencies**

Next, we need to install the required dependencies for our project. 

- **Framer Motion**: For adding smooth and interactive animations to the application.
- **Stripe**: For secure payment processing.
- **Supabase**: For database management and user authentication.
- **React Toast**: Toast notifications for user alerts and messages.
- **Class Names**: Utility for conditionally joining CSS class names.
- **Framer Motion**: Library for creating animations in React applications.
- **Lucide React**: Icon library for React.
- **Tailwind Merge**: Utility for merging Tailwind CSS classes.
- **CLSX**: Utility for constructing class name strings conditionally.
- **React Intersection Observer**: React hook to observe when an element enters or leaves the viewport.
- **React Merge Refs**: Utility for merging multiple refs into a single ref.

In the terminal, navigate to the project directory and run the following command:

```jsx
npm install @radix-ui/react-toast@^1.1.5
npm install @react-spring/web@^9.7.3
npm install @stripe/stripe-js@^3.5.0
npm install @supabase/ssr@^0.3.0
npm install @supabase/supabase-js@^2.43.4
npm install classnames@^2.5.1
npm install framer-motion@^11.2.10
npm install lucide-react@^0.395.0
npm install next@14.1.0
npm install react@^18.2.0
npm install tailwind-merge@^2.2.1
npm install clsx@^2.1.0
npm install react-dom@^18.2.0
npm install class-variance-authority@^0.7.0
npm install react-intersection-observer@^9.10.3
npm install react-merge-refs@^2.1.1
npm install stripe@^15.10.0
```

## Phase 1 : Creating a landing page

1. **Configure Tailwind CSS** by editing `tailwind.config.js`:
    
    ```jsx
    /** @type {import('tailwindcss').Config} */
    module.exports = {
      content: [
        "./pages/**/*.{js,ts,jsx,tsx,mdx}",
        "./components/**/*.{js,ts,jsx,tsx,mdx}",
        "./app/**/*.{js,ts,jsx,tsx,mdx}",
      ],
      plugins: [],
    };
    ```
    
2. **Add Tailwind directives** to your CSS file `styles/globals.css`:
    
    ```css
    @tailwind base;
    @tailwind components;
    @tailwind utilities;
    ```
    
3. **Your  package.json** file should look like this (you can run supabase init):

```jsx
{
  "name": "saas",
  "version": "0.1.0",
  "private": true,
  "scripts": {
    "dev": "next dev  --turbo",
    "build": "next build",
    "start": "next start",
    "lint": "next lint",
    "stripe:login": "stripe login",
    "stripe:listen": "stripe listen --forward-to=localhost:3000/api/webhooks",
    "stripe:fixtures": "stripe fixtures fixtures/stripe-fixtures.json",
    "supabase:start": "npx supabase start",
    "supabase:stop": "npx supabase stop",
    "supabase:status": "npx supabase status",
    "supabase:restart": "npm run supabase:stop && npm run supabase:start",
    "supabase:reset": "npx supabase reset",
    "supabase:link": "npx supabase link",
    "supabase:generate-types": "npx supabase gen types typescript --local --schema public > types_db.ts",
    "supabase:generate-migration": "npx supabase db diff | npx supabase migration new",
    "supabase:generate-seed": "npx supabase db dump --data-only -f supabase/seed.sql",
    "supabase:push": "npx supabase push",
    "supabase:pull": "npx supabase pull"
  },
  "dependencies": {
    "@radix-ui/react-toast": "^1.1.5",
    "@react-spring/web": "^9.7.3",
    "@stripe/stripe-js": "^3.5.0",
    "@supabase/ssr": "^0.3.0",
    "@supabase/supabase-js": "^2.43.4",
    "classnames": "^2.5.1",
    "framer-motion": "^11.2.10",
    "lucide-react": "^0.395.0",
    "next": "14.1.0",
    "react": "^18.2.0",
    "tailwind-merge": "^2.2.1",
    "clsx": "^2.1.0",
    "react-dom": "^18.2.0",
    "class-variance-authority": "^0.7.0",
    "react-intersection-observer": "^9.10.3",
    "react-merge-refs": "^2.1.1",
    "stripe": "^15.10.0"
  },
  "devDependencies": {
    "@types/node": "^20.11.17",
    "@types/react": "^18.2.55",
    "@types/react-dom": "^18.2.19",
    "eslint": "^8",
    "eslint-config-next": "14.2.3",
    "postcss": "^8",
    "supabase": "^1.142.2",
    "tailwindcss": "^3.4.1",
    "typescript": "^5.3.3"
  }
}

```

1. **Create a folder structure similar to this:**

![Untitled](https://talktohire.blr1.digitaloceanspaces.com/Untitled_4.png)

```jsx
//Add folders and file in your A**pp Folder** like this:
│   favicon.ico
│   globals.css
│   layout.js
│   page.js
│
├───**account**
│       page.tsx
│
├──**─api**
│   └───webhooks
│           route.ts
│
├───**auth**
│   ├───callback
│   │       route.ts
│   │
│   └───reset_password
│           route.ts
│
└───**signin**
    │   page.tsx
    │
    └───[id]
            page.tsx
//Add folders and file in your **Component Folder** like this:
├───**footer**
│       index.js
│
├──**─header**
│       index.js
│
├───**images**
│       QuickStart.png
│
├───**landingPage**
│       index.js
│
└───**ui**
    ├───**AccountForms**
    │       CustomerPortalForm.tsx
    │       EmailForm.tsx
    │       NameForm.tsx
    │
    ├───**AuthForms**
    │       EmailSignIn.tsx
    │       ForgotPassword.tsx
    │       OauthSignIn.tsx
    │       PasswordSignIn.tsx
    │       Separator.tsx
    │       Signup.tsx
    │       UpdatePassword.tsx
    │
    └───**Toasts**
            toast.tsx
            toaster.tsx
            use-toast.ts
//For phase 1 we will be only adding data in these files.
```

1. **Add the following date in the following files:**
- Add the following code in  /components/header/index.js

```jsx
"use client";
import { usePathname, useRouter } from "next/navigation";
import Logo from "../images/QuickStart.png";
import Image from "next/image";
import { useState } from "react";
export const Header = ({ user = null } ) => {
  const [isMenuOpen, setIsMenuOpen] = useState(false);
  const [isSubmenuOpen, setIsSubmenuOpen] = useState(false);

  const toggleMenu = () => {
    setIsMenuOpen(!isMenuOpen);
  };
  const toggleSubmenu = () => {
    setIsSubmenuOpen(!isSubmenuOpen);
  };

  const router = useRouter();
  return (
    <header className="header bg-transparent absolute top-0 left-0 z-40 w-full flex items-center transition">
      <div className="container">
        <div className="flex mx-[-16px] items-center justify-between relative">
          <div className="px-4 w-60 max-w-full">
            <button
              onClick={() => router.push("/")}
              className="header-logo w-full block py-5"
            >
              <Image src={Logo} alt="logo" className="w-full" />
            </button>
          </div>
          <div className="flex px-4 justify-between items-center w-full">
            <div>
              <button
                id="navbarToggler"
                onClick={toggleMenu}
                className="block absolute right-4 top-1/2 translate-y-[-50%] lg:hidden focus:ring-2 ring-primary px-3 py-[6px] rounded-lg"
              >
                <span className="relative w-[30px] h-[2px] my-[6px] block bg-dark"></span>
                <span className="relative w-[30px] h-[2px] my-[6px] block bg-dark"></span>
                <span className="relative w-[30px] h-[2px] my-[6px] block bg-dark"></span>
              </button>
              <nav
                id="navbarCollapse"
                className={`absolute py-5 lg:py-0 lg:px-4 xl:px-6 bg-white lg:bg-transparent shadow-lg rounded-lg max-w-[250px] w-full lg:max-w-full lg:w-full right-4 top-full ${
                  isMenuOpen ? "block" : "hidden"
                } lg:block lg:static lg:shadow-none`}
              >
                <ul className="block lg:flex">
                  <li className="relative group">
                    <a
                      href="/#home"
                      className="menu-scroll text-base text-black group-hover:text-primary py-2 lg:py-6 lg:inline-flex lg:px-0 flex mx-8 lg:mr-0"
                    >
                      Home
                    </a>
                  </li>
                  <li className="relative group">
                    <a
                      href="/#about"
                      className="menu-scroll text-base text-black group-hover:text-primary py-2 lg:py-6 lg:inline-flex lg:px-0 flex mx-8 lg:mr-0 lg:ml-8 xl:ml-12"
                    >
                      About
                    </a>
                  </li>
                  <li className="relative group">
                    <a
                      href="/#features"
                      className="menu-scroll text-base text-black group-hover:text-primary py-2 lg:py-6 lg:inline-flex lg:px-0 flex mx-8 lg:mr-0 lg:ml-8 xl:ml-12"
                    >
                      Features
                    </a>
                  </li>
                  <li className="relative group">
                    <a
                      href="/#pricing"
                      className="menu-scroll text-base text-black group-hover:text-primary py-2 lg:py-6 lg:inline-flex lg:px-0 flex mx-8 lg:mr-0 lg:ml-8 xl:ml-12"
                    >
                      Pricing
                    </a>
                  </li>
                  <li className="relative group">
                    <a
                      href="/#contact"
                      className="menu-scroll text-base text-black group-hover:text-primary py-2 lg:py-6 lg:inline-flex lg:px-0 flex mx-8 lg:mr-0 lg:ml-8 xl:ml-12"
                    >
                      Support
                    </a>
                  </li>
                  <li className="relative group submenu-item">
                    <a
                      href="javascript:void(0)"
                      onClick={toggleSubmenu}
                      className="text-base text-black group-hover:text-primary py-2 lg:py-6 lg:inline-flex lg:pl-0 lg:pr-4 flex mx-8 lg:mr-0 lg:ml-8 xl:ml-12 relative after:absolute after:w-2 after:h-2 after:border-b-2 after:border-r-2 after:border-current after:rotate-45 lg:after:right-0 after:right-1 after:top-1/2 after:translate-y-[-50%] after:mt-[-2px]"
                    >
                      Pages
                    </a>
                    <div
                      className={`submenu ${
                        isSubmenuOpen ? "block" : "hidden"
                      } relative lg:absolute w-[250px] top-full lg:top-[110%] left-0 rounded-sm lg:shadow-lg p-4 lg:block lg:opacity-0 lg:invisible group-hover:opacity-100 lg:group-hover:visible lg:group-hover:top-full bg-white transition-[top] duration-300`}
                    >
                      <a
                        href="/#about"
                        className="block text-sm text-black rounded hover:text-primary py-[10px] px-4"
                      >
                        About Page
                      </a>
                      <a
                        href="/#contact"
                        className="block text-sm text-black rounded hover:text-primary py-[10px] px-4"
                      >
                        Contact Page
                      </a>
                      <a
                        href="/#blog"
                        className="block text-sm text-black rounded hover:text-primary py-[10px] px-4"
                      >
                        Blog Grid Page
                      </a>
                      <a
                        href="/signin"
                        className="block text-sm text-black rounded hover:text-primary py-[10px] px-4"
                      >
                        Sign In Page
                      </a>
                      <a
                        href="/signin/signup"
                        className="block text-sm text-black rounded hover:text-primary py-[10px] px-4"
                      >
                        Sign Up Page
                      </a>
                    </div>
                  </li>
                </ul>
              </nav>
            </div>
            {!user ? (
              <div className="sm:flex justify-end hidden pr-16 lg:pr-0">
                <button
                  onClick={() => router.push("/signin")}
                  className="text-base font-semibold text-body-color hover:text-primary py-3 px-7 transition"
                >
                  {" "}
                  Sign In{" "}
                </button>
                <button
                  onClick={() => router.push("/signin/signup")}
                  className="text-base font-bold text-white bg-black py-3 px-8 md:px-9 lg:px-8 xl:px-9 hover:shadow-signUp rounded-sm transition ease-in-out duration-300"
                >
                  Sign Up
                </button>
              </div>
            ) : (
              <form >
                <input type="hidden" name="pathName" value={"/signin"} />
                <button
                  type="submit"
                  className="text-base font-bold text-white bg-black py-3 px-8 md:px-9 lg:px-8 xl:px-9 hover:shadow-signUp rounded-sm transition ease-in-out duration-300"
                >
                  Sign out
                </button>
              </form>
            )}
          </div>
        </div>
      </div>
    </header>
  );
};
```

- Add the following the code in the /components/footer/index.js

```jsx
"use client"
import { useRouter } from "next/navigation";
import Logo from "../images/QuickStart.png"
import Image from "next/image";
export const  Footer= ()=> {
  const router = useRouter();
  return (
    <footer className="pt-20 pb-12">
      <div className="container">
        <div className="flex flex-wrap mx-[-16px]">
          <div className="w-full lg:w-4/12 2xl:w-5/12 px-5">
            <div className="mb-6">
              <button onClick={()=>router.push("/")} className="inline-block mb-5">
                <img
                  src={Logo}
                  alt="logo"
                   className="w-full h-12"
                />
              </button>
              <p className="text-base text-body-color font-medium">
                © 2025 SaaS Tailwind
              </p>
            </div>
          </div>
          <div className="w-full md:w-8/12 lg:w-5/12 2xl:w-5/12 px-4">
            <div className="mb-6 mt-4">
              <h3 className="font-medium text-black text-xl mb-4">
                Quick Links
              </h3>
              <div className="flex flex-wrap items-center">
                <a
                  href="javascript:void(0)"
                  className="text-base text-body-color hover:text-primary font-medium mr-8"
                >
                  {" "}
                  Contact{" "}
                </a>
                <a
                  href="javascript:void(0)"
                  className="text-base text-body-color hover:text-primary font-medium mr-8"
                >
                  {" "}
                  About us{" "}
                </a>
                <a
                  href="javascript:void(0)"
                  className="text-base text-body-color hover:text-primary font-medium mr-8"
                >
                  {" "}
                  FAQ{"'"}s{" "}
                </a>
                <a
                  href="javascript:void(0)"
                  className="text-base text-body-color hover:text-primary font-medium"
                >
                  {" "}
                  Support{" "}
                </a>
              </div>
            </div>
          </div>
          <div className="w-full md:w-4/12 lg:w-3/12 2xl:w-2/12 px-4">
            <div className="mb-6 mt-4 md:text-right">
              <h3 className="font-medium text-black text-xl mb-4">Follow Us</h3>
              <div className="flex flex-wrap md:justify-end items-center">
                <a
                  href="javascript:void(0)"
                  className="text-body-color hover:text-primary"
                >
                  <svg
                    width="9"
                    height="18"
                    viewBox="0 0 9 18"
                    className="fill-current"
                  >
                    <path d="M8.13643 7.00049H6.78036H6.29605V6.43597V4.68597V4.12146H6.78036H7.79741C8.06378 4.12146 8.28172 3.89565 8.28172 3.55694V0.565004C8.28172 0.254521 8.088 0.000488281 7.79741 0.000488281H6.02968C4.11665 0.000488281 2.78479 1.58113 2.78479 3.92388V6.37952V6.94404H2.30048H0.65382C0.314802 6.94404 0 7.25452 0 7.70613V9.73839C0 10.1336 0.266371 10.5005 0.65382 10.5005H2.25205H2.73636V11.065V16.7384C2.73636 17.1336 3.00273 17.5005 3.39018 17.5005H5.66644C5.81174 17.5005 5.93281 17.4158 6.02968 17.3029C6.12654 17.19 6.19919 16.9924 6.19919 16.8231V11.0932V10.5287H6.70771H7.79741C8.11222 10.5287 8.35437 10.3029 8.4028 9.9642V9.93597V9.90775L8.74182 7.96017C8.76604 7.76258 8.74182 7.53678 8.59653 7.31097C8.54809 7.16984 8.33016 7.02871 8.13643 7.00049Z"></path>
                  </svg>
                </a>
                <a
                  href="javascript:void(0)"
                  className="text-body-color hover:text-primary ml-6"
                >
                  <svg
                    width="19"
                    height="14"
                    viewBox="0 0 19 14"
                    className="fill-current"
                  >
                    <path d="M16.3024 2.26076L17.375 1.02789C17.6855 0.693982 17.7702 0.437132 17.7984 0.308707C16.9516 0.771036 16.1613 0.925146 15.6532 0.925146H15.4556L15.3427 0.822406C14.6653 0.283022 13.8185 0.000488281 12.9153 0.000488281C10.9395 0.000488281 9.3871 1.49021 9.3871 3.2111C9.3871 3.31384 9.3871 3.46795 9.41532 3.57069L9.5 4.08439L8.90726 4.05871C5.29435 3.95597 2.33065 1.13063 1.85081 0.642612C1.06048 1.92686 1.5121 3.15973 1.99194 3.93028L2.95161 5.36864L1.42742 4.59809C1.45565 5.67686 1.90726 6.52446 2.78226 7.1409L3.54435 7.6546L2.78226 7.93713C3.2621 9.24706 4.33468 9.78645 5.125 9.99193L6.16935 10.2488L5.18145 10.8652C3.60081 11.8926 1.625 11.8156 0.75 11.7385C2.52823 12.8686 4.64516 13.1255 6.1129 13.1255C7.21371 13.1255 8.03226 13.0227 8.22984 12.9457C16.1331 11.2505 16.5 4.82926 16.5 3.54501V3.36521L16.6694 3.26248C17.629 2.44056 18.0242 2.00391 18.25 1.74706C18.1653 1.77275 18.0524 1.82412 17.9395 1.8498L16.3024 2.26076Z"></path>
                  </svg>
                </a>
                <a
                  href="javascript:void(0)"
                  className="text-body-color hover:text-primary ml-6"
                >
                  <svg
                    width="18"
                    height="14"
                    viewBox="0 0 18 14"
                    className="fill-current"
                  >
                    <path d="M17.5058 2.07168C17.3068 1.24929 16.7099 0.609661 15.9423 0.396451C14.5778 0.000488354 9.0627 0.000488281 9.0627 0.000488281C9.0627 0.000488281 3.54766 0.000488354 2.18311 0.396451C1.41555 0.609661 0.818561 1.24929 0.619565 2.07168C0.25 3.56415 0.25 6.61002 0.25 6.61002C0.25 6.61002 0.25 9.68634 0.619565 11.1484C0.818561 11.9707 1.41555 12.6104 2.18311 12.8236C3.54766 13.2195 9.0627 13.2195 9.0627 13.2195C9.0627 13.2195 14.5778 13.2195 15.9423 12.8236C16.7099 12.6104 17.3068 11.9707 17.5058 11.1484C17.8754 9.68634 17.8754 6.61002 17.8754 6.61002C17.8754 6.61002 17.8754 3.56415 17.5058 2.07168ZM7.30016 9.44267V3.77736L11.8771 6.61002L7.30016 9.44267Z"></path>
                  </svg>
                </a>
                <a
                  href="javascript:void(0)"
                  className="text-body-color hover:text-primary ml-6"
                >
                  <svg
                    width="17"
                    height="16"
                    viewBox="0 0 17 16"
                    className="fill-current"
                  >
                    <path d="M15.2196 0.000488281H1.99991C1.37516 0.000488281 0.875366 0.49798 0.875366 1.11984V14.3034C0.875366 14.9004 1.37516 15.4227 1.99991 15.4227H15.1696C15.7943 15.4227 16.2941 14.9252 16.2941 14.3034V1.09497C16.3441 0.49798 15.8443 0.000488281 15.2196 0.000488281ZM5.44852 13.1094H3.17444V5.77139H5.44852V13.1094ZM4.29899 4.75153C3.54929 4.75153 2.97452 4.15454 2.97452 3.43318C2.97452 2.71182 3.57428 2.11483 4.29899 2.11483C5.02369 2.11483 5.62345 2.71182 5.62345 3.43318C5.62345 4.15454 5.07367 4.75153 4.29899 4.75153ZM14.07 13.1094H11.796V9.55232C11.796 8.70659 11.771 7.58723 10.5964 7.58723C9.39693 7.58723 9.222 8.53246 9.222 9.4777V13.1094H6.94792V5.77139H9.17202V6.79124H9.19701C9.52188 6.19425 10.2466 5.59727 11.3711 5.59727C13.6952 5.59727 14.12 7.08974 14.12 9.12945V13.1094H14.07Z"></path>
                  </svg>
                </a>
              </div>
            </div>
          </div>
        </div>
      </div>
    </footer>
  );
}
```

- Add the both the Header code and footer code in the /components/landing/index.js :

```jsx
"use client";
import { motion } from "framer-motion";
import { Suspense, useEffect, useState } from "react";
import { Footer } from "../footer/index";
import { Header } from "../header/index";
//import { createClient } from "../../utils/supabase/server";
import { useInView } from "react-intersection-observer";
//import { User } from "@supabase/supabase-js";
//import { getStripe } from "@/utils/stripe/client";
//import { getErrorRedirect } from "@/utils/helpers";
import { useRouter, usePathname } from "next/navigation";
//import { checkoutWithStripe } from "@/utils/stripe/server";
//import { Toaster } from "../ui/Toasts/toaster";
import Image from "next/image";

export default function LandingPage() {
  const router = useRouter();
  const [billingInterval, setBillingInterval] = useState("month");
  const [priceIdLoading, setPriceIdLoading] = useState();
  const currentPath = usePathname();
  const [subscriptions, setSubscriptions] = useState("");

 // const handleStripeCheckout = async (price) => {
 //  debugger;
 //   setPriceIdLoading(price.id);

   // if (!user) {
   //   setPriceIdLoading(undefined);
    //  return router.push("/signin/signup");
   // }

    //const { errorRedirect, sessionId } = await checkoutWithStripe(
    //  price[0],
     // currentPath
    //);

    //if (errorRedirect) {
     // setPriceIdLoading(undefined);
     // return router.push(errorRedirect);
    //}

    //if (!sessionId) {
     // setPriceIdLoading(undefined);
      //return router.push(
       // getErrorRedirect(
        //  currentPath,
         // "An unknown error occurred.",
          //"Please try again later or contact a system administrator."
        //)
      //);
   // }
    //const stripe = await getStripe();
    //stripe?.redirectToCheckout({ sessionId });
    //setPriceIdLoading(undefined);
 // };

  const [activePanel, setActivePanel] = useState(0);
  const { ref, inView } = useInView({ threshold: 0, triggerOnce: true });
  const { ref: refFeature, inView: viewFeature } = useInView({
    threshold: 0,
    triggerOnce: true,
  });
  const { ref: refTestimonials, inView: viewTestimonials } = useInView({
    threshold: 0,
    triggerOnce: true,
  });
  const { ref: refPricing, inView: viewPricing } = useInView({
    threshold: 0,
    triggerOnce: true,
  });
  const { ref: refBlog, inView: viewBlog } = useInView({
    threshold: 0,
    triggerOnce: true,
  });
  const { ref: refContact, inView: viewContact } = useInView({
    threshold: 0,
    triggerOnce: true,
  });

  const showPanel = (index) => {
    setActivePanel(index);
  };

  return (
    <>
      <Header  />
      <section
        id="home"
        className="relative overflow-hidden z-10 pt-[120px] pb-16 md:pt-[150px] xl:pt-[180px]"
      >
        <div className="container">
          <div className="flex flex-wrap mx-[-16px]">
            <div className="w-full px-4">
              <motion.div
                initial={{ y: 15, opacity: 0 }}
                animate={{ y: -15, opacity: 1 }}
                transition={{ type: "easeIn", stiffness: 30, duration: 0.6 }}
                className="mx-auto max-w-[720px] text-center"
              >
                <h1 className="text-black font-bold text-3xl sm:text-4xl md:text-5xl leading-tight sm:leading-tight md:leading-tight mb-5">
                  Complete Tailwind CSS Template for SaaS Website
                </h1>
                <p className="font-medium text-lg md:text-xl leading-relaxed md:leading-relaxed text-body-color opacity-90 mb-12">
                  SaaS Tailwind is a complete Tailwind CSS template crafted
                  specially for SaaS, Software Mobile App and Web App Sites.
                  Comes with all essential components and pages you need to
                  kickstart your SaaS websites.
                </p>
                <div className="flex items-center justify-center">
                  <a
                    href="#features"
                    className="menu-scroll text-base font-semibold text-white bg-primary py-4 px-7 hover:shadow-signUp mx-2 rounded-sm transition ease-in-out duration-300"
                  >
                    Explore Features
                  </a>
                </div>
              </motion.div>
              <motion.div
                initial={{ y: 15, opacity: 0 }}
                animate={{ y: -15, opacity: 1 }}
                transition={{ type: "easeIn", stiffness: 30, duration: 0.6 }}
                className="text-center"
              >
                <div className="mx-auto mt-10 lg:mt-14 relative z-20 wow fadeInUp inline-block bg-white shadow-image">
                  <img
                    src="https://saas-tailwind.preview.uideck.com/images/hero-image.png"
                    alt="image"
                    className="max-w-full mx-auto"
                  />
                  <div className="absolute right-[-16px] bottom-[-16px] z-[-1]">
                    <svg
                      width="98"
                      height="98"
                      viewBox="0 0 98 98"
                      fill="none"
                      xmlns="http://www.w3.org/2000/svg"
                    >
                      <path
                        fillRule="evenodd"
                        clipRule="evenodd"
                        d="M49 98C76.0572 98 98 76.0618 98 49C98 21.9382 76.0572 0 49 0C21.9428 0 0 21.9382 0 49C0 76.0618 21.9428 98 49 98Z"
                        fill="url(#paint0_radial_77:31)"
                      />
                      <defs>
                        <radialGradient
                          id="paint0_radial_77:31"
                          cx="0"
                          cy="0"
                          r="1"
                          gradientUnits="userSpaceOnUse"
                          gradientTransform="translate(49) rotate(90) scale(98)"
                        >
                          <stop stopColor="white" />
                          <stop offset="0.569" stopColor="#F0F4FD" />
                          <stop offset="0.993" stopColor="#D9E0F0" />
                        </radialGradient>
                      </defs>
                    </svg>
                  </div>
                </div>
              </motion.div>
            </div>
          </div>
        </div>
        <div className="absolute right-0 top-24 z-[-1]">
          <svg
            width="111"
            height="176"
            viewBox="0 0 111 176"
            fill="none"
            xmlns="http://www.w3.org/2000/svg"
          >
            <path
              fillRule="evenodd"
              clipRule="evenodd"
              d="M87.9999 176C136.614 176 176 136.602 176 88C176 39.3978 136.614 0 87.9999 0C39.4096 0 -0.00012207 39.3978 -0.00012207 88C-0.00012207 136.602 39.4096 176 87.9999 176Z"
              fill="url(#paint0_radial_2:29)"
            />
            <defs>
              <radialGradient
                id="paint0_radial_2:29"
                cx="0"
                cy="0"
                r="1"
                gradientUnits="userSpaceOnUse"
                gradientTransform="translate(87.9999) rotate(90) scale(176)"
              >
                <stop stopColor="white" />
                <stop offset="0.569" stopColor="#F0F4FD" />
                <stop offset="0.993" stopColor="#D9E0F0" />
              </radialGradient>
            </defs>
          </svg>
        </div>
        <div className="absolute left-0 bottom-0 z-[-1]">
          <svg
            width="731"
            height="1028"
            viewBox="0 0 731 1028"
            fill="none"
            xmlns="http://www.w3.org/2000/svg"
          >
            <path
              fillRule="evenodd"
              clipRule="evenodd"
              d="M341.5 630C366.624 630 387 610.077 387 585.5C387 560.923 366.624 541 341.5 541C316.375 541 296 560.923 296 585.5C296 610.077 316.375 630 341.5 630Z"
              fill="#FFDDF7"
            />
            <path
              fillRule="evenodd"
              clipRule="evenodd"
              d="M325.845 560.409C325.305 555.541 323.685 550.779 321.165 546.532C318.605 542.26 315.191 538.563 311.136 535.672C310.956 535.541 310.776 535.4 310.583 535.276L310.017 534.903C309.632 534.656 309.259 534.396 308.873 534.174C308.475 533.944 308.089 533.71 307.69 533.485L306.482 532.85C304.85 532.033 303.161 531.336 301.429 530.764C299.682 530.212 297.899 529.779 296.093 529.471C294.28 529.19 292.441 529.034 290.603 529C288.777 528.996 286.938 529.104 285.113 529.35C283.287 529.617 281.487 530.007 279.713 530.522C277.951 531.059 276.228 531.722 274.557 532.501C272.893 533.292 271.291 534.205 269.761 535.231C269.35 535.523 268.925 535.795 268.514 536.102L267.318 537.024L266.148 538.003C265.762 538.334 265.402 538.687 265.029 539.029C263.567 540.431 262.221 541.949 261.005 543.57C260.722 543.982 260.414 544.385 260.131 544.805L259.295 546.077L258.524 547.386C258.279 547.822 258.061 548.279 257.816 548.725C256.904 550.531 256.147 552.411 255.553 554.345C252.789 563.241 253.766 573.14 258.061 581.377C258.729 582.694 259.514 583.944 260.324 585.168L260.979 586.064L261.301 586.511L261.648 586.943L262.342 587.806C262.574 588.089 262.818 588.361 263.062 588.639C263.538 589.202 264.065 589.72 264.579 590.253C264.837 590.523 265.107 590.763 265.377 591.019C265.647 591.27 265.904 591.532 266.187 591.771C266.752 592.247 267.305 592.747 267.897 593.189L268.771 593.871L269.671 594.514C270.224 594.925 270.815 595.275 271.394 595.65C271.677 595.842 271.969 596.002 272.259 596.187L273.175 596.709L274.1 597.181C274.703 597.496 275.336 597.747 275.962 598.012L277.13 598.441L278.298 598.831C279.531 599.229 280.772 599.575 282.03 599.86C283.272 600.155 284.533 600.39 285.809 600.543C287.086 600.696 288.378 600.77 289.674 600.773C290.973 600.76 292.27 600.687 293.553 600.531C294.849 600.362 296.134 600.118 297.403 599.822L298.446 599.566L299.497 599.286L300.52 598.97L301.526 598.629C304.151 597.734 306.676 596.532 309.08 595.05L310.845 593.84L312.573 592.491L314.242 591.014L315.984 589.372L316.708 588.654L317.42 587.904L318.069 587.104L318.785 586.159L319.418 585.33L320.086 584.326L320.721 583.225L321.342 582.089L321.928 580.953L322.473 579.797C323.389 577.583 324.092 575.245 324.569 572.875C325.048 570.482 325.297 568.048 325.312 565.605C325.316 563.764 325.194 561.906 325.127 560.051L325.063 559.205L325.009 558.351L325.845 560.409Z"
              fill="url(#paint1_radial_2:29)"
            />
            <defs>
              <radialGradient
                id="paint1_radial_2:29"
                cx="0"
                cy="0"
                r="1"
                gradientUnits="userSpaceOnUse"
                gradientTransform="translate(341.5 541) rotate(90) scale(89)"
              >
                <stop stopColor="white" />
                <stop offset="0.495" stopColor="#F0F4FD" />
                <stop offset="1" stopColor="#D9E0F0" />
              </radialGradient>
            </defs>
          </svg>
        </div>
      </section>
      <section id="about" className="pt-[100px]">
        <div className="container">
          <div className="flex flex-wrap mx-[-16px]">
            <motion.div
              ref={ref}
              initial={{ y: 15, opacity: 0 }}
              animate={inView ? { y: -15, opacity: 1 } : {}}
              transition={{
                type: "easeIn",
                stiffness: 30,
                duration: 0.6,
                delay: 0.3,
              }}
              className="w-full px-4"
            >
              <div className="mx-auto max-w-[655px] text-center mb-12">
                <span className="text-lg font-semibold text-primary mb-2 block">
                  Know About Our Product
                </span>
                <h2 className="text-black font-bold text-3xl sm:text-4xl md:text-[45px] mb-5">
                  About Our Software
                </h2>
                <p className="text-body-color text-base md:text-lg leading-relaxed md:leading-relaxed max-w-[570px] mx-auto">
                  There are many variations of passages of Lorem Ipsum available
                  but the majority have suffered alteration in some form.
                </p>
              </div>
            </motion.div>
          </div>
          <div className="flex flex-wrap justify-center mx-[-16px]">
            <div className="w-full px-4">
              <div className="flex justify-center">
                <div className="inline-flex flex-wrap justify-center items-center bg-white shadow-md rounded-sm py-2 mb-[60px] tabButtons">
                  <a
                    href="javascript:void(0)"
                    onClick={() => showPanel(0)}
                    className="flex items-center text-body-color hover:text-primary text-base md:text-lg px-4 md:px-6 my-2 border-r border-body-color border-opacity-30 text-primary"
                  >
                    <span className="mr-3">
                      <svg
                        width="20"
                        height="20"
                        viewBox="0 0 20 20"
                        fill="none"
                        xmlns="http://www.w3.org/2000/svg"
                      >
                        <circle
                          opacity="0.5"
                          cx="4.11765"
                          cy="4.11814"
                          r="4.11765"
                          fill="#4A6CF7"
                        ></circle>
                        <circle
                          cx="15.8824"
                          cy="4.11814"
                          r="4.11765"
                          fill="#4A6CF7"
                        ></circle>
                        <circle
                          opacity="0.5"
                          cx="15.8824"
                          cy="15.8828"
                          r="4.11765"
                          fill="#4A6CF7"
                        ></circle>
                        <circle
                          cx="4.11765"
                          cy="15.8828"
                          r="4.11765"
                          fill="#4A6CF7"
                        ></circle>
                      </svg>
                    </span>
                    SaaS
                  </a>
                  <a
                    href="javascript:void(0)"
                    onClick={() => showPanel(1)}
                    className="flex items-center text-body-color hover:text-primary text-base md:text-lg px-4 md:px-6 my-2 border-r border-body-color border-opacity-30"
                  >
                    <span className="mr-3">
                      <svg
                        width="32"
                        height="20"
                        viewBox="0 0 32 20"
                        fill="none"
                        xmlns="http://www.w3.org/2000/svg"
                      >
                        <circle
                          opacity="0.5"
                          cx="22"
                          cy="10.0005"
                          r="10"
                          fill="#4A6CF7"
                        ></circle>
                        <path
                          d="M22 10.0005L12 17.0005L12 3.00049L22 10.0005Z"
                          fill="#4A6CF7"
                        ></path>
                        <rect
                          y="9.00049"
                          width="12"
                          height="2"
                          fill="#4A6CF7"
                        ></rect>
                      </svg>
                    </span>
                    Business
                  </a>
                  <a
                    href="javascript:void(0)"
                    onClick={() => showPanel(2)}
                    className="flex items-center text-body-color hover:text-primary text-base md:text-lg px-4 md:px-6 my-2 border-r border-body-color border-opacity-30"
                  >
                    <span className="mr-3">
                      <svg
                        width="20"
                        height="20"
                        viewBox="0 0 20 20"
                        fill="none"
                        xmlns="http://www.w3.org/2000/svg"
                      >
                        <path
                          fill-rule="evenodd"
                          clip-rule="evenodd"
                          d="M10 11.563V20.0005L19.9994 14.2192V5.78174L10 11.563Z"
                          fill="#4A6CF7"
                        ></path>
                        <path
                          fill-rule="evenodd"
                          clip-rule="evenodd"
                          d="M0 14.3755L10 20.0005V11.563L0 5.78174V14.3755Z"
                          fill="#4A6CF7"
                        ></path>
                        <path
                          fill-rule="evenodd"
                          clip-rule="evenodd"
                          d="M10 8.12549L0 14.3755L10 20.0005L20 14.3755L10 8.12549Z"
                          fill="url(#paint0_linear_3:20)"
                          fill-opacity="0.64"
                        ></path>
                        <path
                          fill-rule="evenodd"
                          clip-rule="evenodd"
                          d="M10 0.000488281L0 5.78174L10 11.563L20 5.78174L10 0.000488281Z"
                          fill="url(#paint1_linear_3:20)"
                        ></path>
                        <defs>
                          <linearGradient
                            id="paint0_linear_3:20"
                            x1="-3.86893e-09"
                            y1="11.9781"
                            x2="19.8302"
                            y2="17.6066"
                            gradientUnits="userSpaceOnUse"
                          >
                            <stop
                              stop-color="white"
                              stop-opacity="0.299"
                            ></stop>
                            <stop
                              offset="1"
                              stop-color="#7587E4"
                              stop-opacity="0"
                            ></stop>
                          </linearGradient>
                          <linearGradient
                            id="paint1_linear_3:20"
                            x1="3.7182"
                            y1="0.000488354"
                            x2="11.1258"
                            y2="15.7396"
                            gradientUnits="userSpaceOnUse"
                          >
                            <stop stop-color="#7587E4"></stop>
                            <stop offset="1" stop-color="#CCD4FF"></stop>
                          </linearGradient>
                        </defs>
                      </svg>
                    </span>
                    Software
                  </a>
                  <a
                    href="javascript:void(0)"
                    onClick={() => showPanel(3)}
                    className="flex items-center text-body-color hover:text-primary text-base md:text-lg px-4 md:px-6 my-2"
                  >
                    <span className="mr-3">
                      <svg
                        width="20"
                        height="20"
                        viewBox="0 0 20 20"
                        fill="none"
                        xmlns="http://www.w3.org/2000/svg"
                      >
                        <path
                          opacity="0.5"
                          d="M20 5.71484V20.0006H5.71429V15.9189H15.9184V5.71484H20Z"
                          fill="#4A6CF7"
                        ></path>
                        <rect
                          y="0.000488281"
                          width="14.2857"
                          height="14.2857"
                          fill="#4A6CF7"
                        ></rect>
                      </svg>
                    </span>
                    Mobile App
                  </a>
                </div>
              </div>
              <div className="text-center">
                <motion.div
                  key={0}
                  initial={{ opacity: 0 }}
                  animate={{ opacity: activePanel === 0 ? 1 : 0 }}
                  transition={{ duration: 0.5 }}
                  className={`tabPanel mx-auto bg-white shadow-image ${
                    activePanel !== 0 ? "hidden" : ""
                  }`}
                >
                  <img
                    src="https://saas-tailwind.preview.uideck.com/images/about-image.png"
                    alt="image"
                    className="max-w-full mx-auto"
                  />
                </motion.div>
                <motion.div
                  key={1}
                  initial={{ opacity: 0 }}
                  animate={{ opacity: activePanel === 1 ? 1 : 0 }}
                  transition={{ duration: 0.5 }}
                  className={`tabPanel mx-auto bg-white shadow-image ${
                    activePanel !== 1 ? "hidden" : ""
                  }`}
                >
                  <img
                    src="https://saas-tailwind.preview.uideck.com/images/hero-image.png
"
                    alt="image"
                    className="max-w-full mx-auto"
                  />
                </motion.div>
                <motion.div
                  key={2}
                  initial={{ opacity: 0 }}
                  animate={{ opacity: activePanel === 2 ? 1 : 0 }}
                  transition={{ duration: 0.5 }}
                  className={`tabPanel mx-auto bg-white shadow-image ${
                    activePanel !== 2 ? "hidden" : ""
                  }`}
                >
                  <img
                    src="https://saas-tailwind.preview.uideck.com/images/about-image.png"
                    alt="image"
                    className="max-w-full mx-auto"
                  />
                </motion.div>
                <motion.div
                  key={3}
                  initial={{ opacity: 0 }}
                  animate={{ opacity: activePanel === 3 ? 1 : 0 }}
                  transition={{ duration: 0.5 }}
                  className={`tabPanel mx-auto bg-white shadow-image ${
                    activePanel !== 3 ? "hidden" : ""
                  }`}
                >
                  <img
                    src="https://saas-tailwind.preview.uideck.com/images/hero-image.png"
                    alt="image"
                    className="max-w-full mx-auto"
                  />
                </motion.div>
              </div>
            </div>
          </div>
        </div>
      </section>
      <section id="features" className="pt-[100px]">
        <div className="container">
          <motion.div
            ref={refFeature}
            initial={{ y: 15, opacity: 0 }}
            animate={viewFeature ? { y: -15, opacity: 1 } : {}}
            transition={{
              type: "easeIn",
              stiffness: 30,
              duration: 0.6,
              delay: 0.2,
            }}
            className="flex flex-wrap mx-[-16px]"
          >
            <div className="w-full px-4">
              <div className="mx-auto max-w-[655px] text-center mb-20">
                <span className="text-lg font-semibold text-primary mb-2 block">
                  {" "}
                  Our Software{"'"}s Core Features{" "}
                </span>
                <h2 className="text-black font-bold text-3xl sm:text-4xl md:text-[45px] mb-5">
                  SaaS Features
                </h2>
                <p className="text-body-color text-base md:text-lg leading-relaxed md:leading-relaxed max-w-[570px] mx-auto">
                  passages of Lorem Ipsum available but the majority have
                  suffered alteration in some form.
                </p>
              </div>
            </div>
          </motion.div>
          <motion.div
            ref={refFeature}
            initial={{ y: 15, opacity: 0 }}
            animate={viewFeature ? { y: -15, opacity: 1 } : {}}
            transition={{
              type: "easeIn",
              stiffness: 30,
              duration: 0.6,
              delay: 0.3,
            }}
            className="pb-12 border-b border-[#E9ECF8]"
          >
            <div className="flex flex-wrap mx-[-16px]">
              <div className="w-full md:w-1/2 lg:w-1/3 px-4">
                <div className="mb-[70px] text-center 2xl:px-5">
                  <div className="mb-9 flex justify-center">
                    <svg
                      width="50"
                      height="50"
                      viewBox="0 0 50 50"
                      fill="none"
                      xmlns="http://www.w3.org/2000/svg"
                    >
                      <path
                        opacity="0.5"
                        d="M27.2727 0.000488281V15.9096H34.0909L25 27.2732L15.9091 15.9096H22.7273V0.000488281H27.2727Z"
                        fill="#4A6CF7"
                      ></path>
                      <path
                        d="M43.1818 0.000488281H34.0909V4.54594H43.1818C43.7846 4.54594 44.3627 4.78539 44.7889 5.21161C45.2151 5.63783 45.4545 6.21591 45.4545 6.81867V31.8187H31.8182V38.6369H18.1818V31.8187H4.54545V6.81867C4.54545 6.21591 4.7849 5.63783 5.21112 5.21161C5.63734 4.78539 6.21542 4.54594 6.81818 4.54594H15.9091V0.000488281H6.81818C5.00989 0.000488281 3.27566 0.71883 1.997 1.99749C0.718341 3.27614 0 5.01038 0 6.81867V43.1823C0 44.9906 0.718341 46.7248 1.997 48.0035C3.27566 49.2821 5.00989 50.0005 6.81818 50.0005H43.1818C44.9901 50.0005 46.7243 49.2821 48.003 48.0035C49.2817 46.7248 50 44.9906 50 43.1823V6.81867C50 5.01038 49.2817 3.27614 48.003 1.99749C46.7243 0.71883 44.9901 0.000488281 43.1818 0.000488281Z"
                        fill="#4A6CF7"
                      ></path>
                    </svg>
                  </div>
                  <h3 className="font-bold text-black text-xl sm:text-2xl lg:text-xl xl:text-2xl mb-5">
                    SaaS Focused
                  </h3>
                  <p className="text-body-color text-base leading-relaxed">
                    Deleniti nemo temporibus minima iusto, voluptatem sint
                    ratione eveniet maiores nihil maxime repellendus, dolorum
                    dolorem atque delectus laborum.
                  </p>
                </div>
              </div>
              <div className="w-full md:w-1/2 lg:w-1/3 px-4">
                <div className="mb-[70px] text-center 2xl:px-5 ">
                  <div className="mb-9 flex justify-center">
                    <svg
                      width="50"
                      height="50"
                      viewBox="0 0 50 50"
                      fill="none"
                      xmlns="http://www.w3.org/2000/svg"
                    >
                      <path
                        d="M27.5265 0.000488281H2.11742C0.948003 0.000488281 0 0.948491 0 2.11791V33.8793C0 35.0487 0.948003 35.9967 2.11742 35.9967H27.5265C28.6959 35.9967 29.6439 35.0487 29.6439 33.8793V2.11791C29.6439 0.948491 28.6959 0.000488281 27.5265 0.000488281Z"
                        fill="#4A6CF7"
                      ></path>
                      <path
                        opacity="0.5"
                        d="M48.4022 15.1783L34.5141 11.4199L33.4088 15.5087L45.2663 18.7145L38.0692 45.2839L13.541 38.6542L12.4378 42.743L39.0115 49.9274C39.2799 49.9998 39.5601 50.0187 39.8358 49.9829C40.1116 49.9471 40.3776 49.8573 40.6186 49.7187C40.8597 49.5801 41.0711 49.3953 41.2407 49.175C41.4104 48.9547 41.535 48.7031 41.6074 48.4346L49.8929 17.7743C50.0393 17.2324 49.9645 16.6545 49.685 16.1677C49.4054 15.681 48.9441 15.3251 48.4022 15.1783Z"
                        fill="#4A6CF7"
                      ></path>
                    </svg>
                  </div>
                  <h3 className="font-bold text-black text-xl sm:text-2xl lg:text-xl xl:text-2xl mb-5">
                    Developer Friendly
                  </h3>
                  <p className="text-body-color text-base leading-relaxed">
                    Deleniti nemo temporibus minima iusto, voluptatem sint
                    ratione eveniet maiores nihil maxime repellendus, dolorum
                    dolorem atque delectus laborum.
                  </p>
                </div>
              </div>
              <div className="w-full md:w-1/2 lg:w-1/3 px-4">
                <div className="mb-[70px] text-center 2xl:px-5">
                  <div className="mb-9 flex justify-center">
                    <svg
                      width="50"
                      height="50"
                      viewBox="0 0 50 50"
                      fill="none"
                      xmlns="http://www.w3.org/2000/svg"
                    >
                      <path
                        d="M18.1818 20.455H2.27273C1.66996 20.455 1.09188 20.2156 0.665665 19.7894C0.239446 19.3631 0 18.7851 0 18.1823V2.27322C0 1.67045 0.239446 1.09237 0.665665 0.666153C1.09188 0.239934 1.66996 0.000488281 2.27273 0.000488281H18.1818C18.7846 0.000488281 19.3627 0.239934 19.7889 0.666153C20.2151 1.09237 20.4545 1.67045 20.4545 2.27322V18.1823C20.4545 18.7851 20.2151 19.3631 19.7889 19.7894C19.3627 20.2156 18.7846 20.455 18.1818 20.455Z"
                        fill="#4A6CF7"
                      ></path>
                      <path
                        d="M18.1818 50.0004H2.27273C1.66996 50.0004 1.09188 49.761 0.665665 49.3348C0.239446 48.9086 0 48.3305 0 47.7277V31.8186C0 31.2159 0.239446 30.6378 0.665665 30.2116C1.09188 29.7853 1.66996 29.5459 2.27273 29.5459H18.1818C18.7846 29.5459 19.3627 29.7853 19.7889 30.2116C20.2151 30.6378 20.4545 31.2159 20.4545 31.8186V47.7277C20.4545 48.3305 20.2151 48.9086 19.7889 49.3348C19.3627 49.761 18.7846 50.0004 18.1818 50.0004Z"
                        fill="#4A6CF7"
                      ></path>
                      <path
                        opacity="0.5"
                        d="M27.2727 2.27319H50V6.81865H27.2727V2.27319ZM27.2727 13.6368H50V18.1823H27.2727V13.6368ZM27.2727 31.8186H50V36.3641H27.2727V31.8186ZM27.2727 43.1823H50V47.7277H27.2727V43.1823Z"
                        fill="#4A6CF7"
                      ></path>
                    </svg>
                  </div>
                  <h3 className="font-bold text-black text-xl sm:text-2xl lg:text-xl xl:text-2xl mb-5">
                    Essential Components
                  </h3>
                  <p className="text-body-color text-base leading-relaxed">
                    Deleniti nemo temporibus minima iusto, voluptatem sint
                    ratione eveniet maiores nihil maxime repellendus, dolorum
                    dolorem atque delectus laborum.
                  </p>
                </div>
              </div>
              <div className="w-full md:w-1/2 lg:w-1/3 px-4">
                <div className="mb-[70px] text-center 2xl:px-5">
                  <div className="mb-9 flex justify-center">
                    <svg
                      width="46"
                      height="50"
                      viewBox="0 0 46 50"
                      fill="none"
                      xmlns="http://www.w3.org/2000/svg"
                    >
                      <path
                        d="M43.75 6.25049H25V10.4172H43.75C44.3025 10.4172 44.8324 10.1977 45.2231 9.80696C45.6138 9.41626 45.8333 8.88636 45.8333 8.33382C45.8333 7.78129 45.6138 7.25138 45.2231 6.86068C44.8324 6.46998 44.3025 6.25049 43.75 6.25049Z"
                        fill="#4A6CF7"
                      ></path>
                      <path
                        d="M2.08333 10.4172H12.5V16.6672H20.8333V0.000488281H12.5V6.25049H2.08333C1.5308 6.25049 1.00089 6.46998 0.610193 6.86068C0.219492 7.25138 0 7.78129 0 8.33382C0 8.88636 0.219492 9.41626 0.610193 9.80696C1.00089 10.1977 1.5308 10.4172 2.08333 10.4172Z"
                        fill="#4A6CF7"
                      ></path>
                      <path
                        d="M43.75 39.5839H25V43.7505H43.75C44.3025 43.7505 44.8324 43.531 45.2231 43.1403C45.6138 42.7496 45.8333 42.2197 45.8333 41.6672C45.8333 41.1147 45.6138 40.5848 45.2231 40.1941C44.8324 39.8034 44.3025 39.5839 43.75 39.5839Z"
                        fill="#4A6CF7"
                      ></path>
                      <path
                        d="M2.08333 43.7505H12.5V50.0005H20.8333V33.3339H12.5V39.5839H2.08333C1.5308 39.5839 1.00089 39.8034 0.610193 40.1941C0.219492 40.5848 0 41.1147 0 41.6672C0 42.2197 0.219492 42.7496 0.610193 43.1403C1.00089 43.531 1.5308 43.7505 2.08333 43.7505Z"
                        fill="#4A6CF7"
                      ></path>
                      <path
                        opacity="0.5"
                        d="M43.75 22.9171H37.5V27.0838H43.75C44.3025 27.0838 44.8324 26.8643 45.2231 26.4736C45.6138 26.0829 45.8333 25.553 45.8333 25.0004C45.8333 24.4479 45.6138 23.918 45.2231 23.5273C44.8324 23.1366 44.3025 22.9171 43.75 22.9171ZM2.08333 27.0838H25V33.3338H33.3333V16.6671H25V22.9171H2.08333C1.5308 22.9171 1.00089 23.1366 0.610193 23.5273C0.219492 23.918 0 24.4479 0 25.0004C0 25.553 0.219492 26.0829 0.610193 26.4736C1.00089 26.8643 1.5308 27.0838 2.08333 27.0838Z"
                        fill="#4A6CF7"
                      ></path>
                    </svg>
                  </div>
                  <h3 className="font-bold text-black text-xl sm:text-2xl lg:text-xl xl:text-2xl mb-5">
                    Easy to Customize
                  </h3>
                  <p className="text-body-color text-base leading-relaxed">
                    Deleniti nemo temporibus minima iusto, voluptatem sint
                    ratione eveniet maiores nihil maxime repellendus, dolorum
                    dolorem atque delectus laborum.
                  </p>
                </div>
              </div>
              <div className="w-full md:w-1/2 lg:w-1/3 px-4">
                <div className="mb-[70px] text-center 2xl:px-5 wow fadeInUp">
                  <div className="mb-9 flex justify-center">
                    <svg
                      width="40"
                      height="50"
                      viewBox="0 0 40 50"
                      fill="none"
                      xmlns="http://www.w3.org/2000/svg"
                    >
                      <path
                        opacity="0.5"
                        d="M39.8228 2.21287C39.8228 0.88544 38.9379 0.000488281 37.6104 0.000488281H2.87607L28.7609 14.1597V37.6109H37.6104C38.9379 37.6109 39.8228 36.726 39.8228 35.3986V2.21287Z"
                        fill="#4A6CF7"
                      ></path>
                      <path
                        d="M24.3362 16.8148L0 3.54053V35.3988C0 36.2837 0.442476 36.9475 1.10619 37.3899L24.3362 50.0005V16.8148Z"
                        fill="#4A6CF7"
                      ></path>
                    </svg>
                  </div>
                  <h3 className="font-bold text-black text-xl sm:text-2xl lg:text-xl xl:text-2xl mb-5">
                    High-quality Design
                  </h3>
                  <p className="text-body-color text-base leading-relaxed">
                    Deleniti nemo temporibus minima iusto, voluptatem sint
                    ratione eveniet maiores nihil maxime repellendus, dolorum
                    dolorem atque delectus laborum.
                  </p>
                </div>
              </div>
              <div className="w-full md:w-1/2 lg:w-1/3 px-4">
                <div className="mb-[70px] text-center 2xl:px-5">
                  <div className="mb-9 flex justify-center">
                    <svg
                      width="46"
                      height="50"
                      viewBox="0 0 46 50"
                      fill="none"
                      xmlns="http://www.w3.org/2000/svg"
                    >
                      <path
                        opacity="0.5"
                        d="M35.7142 42.8576C32.4285 42.8576 29.7618 40.191 29.7618 36.9053C29.7618 36.0793 29.923 35.2891 30.2299 34.5658C30.3139 34.3676 30.2782 34.136 30.1261 33.9838L28.2169 32.0746C27.9938 31.8515 27.6217 31.8881 27.4637 32.1613C26.6592 33.5524 26.1904 35.1726 26.1904 36.9053C26.1904 42.1672 30.4523 46.4291 35.7142 46.4291V48.7934C35.7142 49.2388 36.2528 49.4619 36.5678 49.1469L40.7178 44.9969C40.9131 44.8016 40.9131 44.4851 40.7178 44.2898L36.5678 40.1398C36.2528 39.8248 35.7142 40.0479 35.7142 40.4933V42.8576ZM35.7142 27.3815V25.0172C35.7142 24.5717 35.1756 24.3486 34.8607 24.6636L30.7106 28.8136C30.5154 29.0089 30.5154 29.3255 30.7106 29.5207L34.8607 33.6708C35.1756 33.9857 35.7142 33.7627 35.7142 33.3172V30.9529C38.9999 30.9529 41.6666 33.6196 41.6666 36.9053C41.6666 37.7313 41.5054 38.5215 41.1985 39.2448C41.1145 39.4429 41.1502 39.6746 41.3024 39.8268L43.2115 41.7359C43.4346 41.959 43.8067 41.9224 43.9647 41.6493C44.7692 40.2581 45.238 38.638 45.238 36.9053C45.238 31.6434 40.9761 27.3815 35.7142 27.3815Z"
                        fill="#4A6CF7"
                      ></path>
                      <path
                        d="M21.4285 36.9052C21.4285 40.2265 22.4724 43.2906 24.2586 45.8081C24.7693 46.5279 24.2883 47.6194 23.4058 47.6194H4.76189C2.11904 47.6194 0 45.5004 0 42.8575V4.76238C0 2.14334 2.11904 0.000488281 4.76189 0.000488281H7.14284H19.0476H33.3333C35.9523 0.000488281 38.0951 2.11953 38.0951 4.76238V20.306C38.0951 20.9267 37.5253 21.429 36.9047 21.429C28.3571 21.429 21.4285 28.3576 21.4285 36.9052Z"
                        fill="#4A6CF7"
                      ></path>
                    </svg>
                  </div>
                  <h3 className="font-bold text-black text-xl sm:text-2xl lg:text-xl xl:text-2xl mb-5">
                    Regular Updates
                  </h3>
                  <p className="text-body-color text-base leading-relaxed">
                    Deleniti nemo temporibus minima iusto, voluptatem sint
                    ratione eveniet maiores nihil maxime repellendus, dolorum
                    dolorem atque delectus laborum.
                  </p>
                </div>
              </div>
            </div>
          </motion.div>
        </div>
      </section>
      <section
        id="testimonial"
        className="pt-[120px] pb-20 bg-gradient-to-t from-blue-50 to-white"
      >
        <div className="container">
          <motion.div
            ref={refTestimonials}
            initial={{ y: 15, opacity: 0 }}
            animate={viewTestimonials ? { y: -15, opacity: 1 } : {}}
            transition={{
              type: "easeIn",
              stiffness: 30,
              duration: 0.6,
              delay: 0.3,
            }}
            className="flex flex-wrap mx-[-16px]"
          >
            <div className="w-full px-4">
              <div
                className="mx-auto max-w-[655px] text-center mb-20 wow fadeInUp"
                data-wow-delay=".1s"
              >
                <span className="text-lg font-semibold text-primary mb-2 block">
                  {" "}
                  Testimonials{" "}
                </span>
                <h2 className="text-black font-bold text-3xl sm:text-4xl md:text-[45px] mb-5">
                  What Clients Says
                </h2>
                <p className="text-body-color text-base md:text-lg leading-relaxed md:leading-relaxed max-w-[570px] mx-auto">
                  There are many variations of passages of Lorem Ipsum available
                  but the majority have suffered alteration in some form.
                </p>
              </div>
            </div>
          </motion.div>
          <div className="flex flex-wrap justify-center mx-[-16px]">
            <div className="w-full lg:w-1/2 px-4">
              <div
                className="bg-white relative z-10 overflow-hidden rounded-sm p-8 lg:px-6 xl:px-8 mb-10 wow fadeInUp"
                data-wow-delay=".1s"
              >
                <div className="sm:flex justify-between lg:block xl:flex">
                  <div className="w-full flex items-center">
                    <div className="rounded-full overflow-hidden w-[60px] h-[60px] mt-2 mr-4">
                      <img
                        src="https://saas-tailwind.preview.uideck.com/images/author-01.png"
                        alt="image"
                      />
                    </div>
                    <div className="w-full">
                      <h5 className="text-base md:text-lg lg:text-base xl:text-lg text-black font-medium mb-1">
                        Musharof Chowdhury
                      </h5>
                      <p className="text-base font-medium text-body-color">
                        Founder @ TailGrids
                      </p>
                    </div>
                  </div>
                  <div className="max-w-[150px] w-full flex items-center sm:justify-end lg:justify-start xl:justify-end mt-4 sm:mt-0 lg:mt-4 xl:mt-0">
                    <div>
                      <div className="flex items-center">
                        <span className="text-yellow mr-1 block">
                          <svg
                            width="18"
                            height="16"
                            viewBox="0 0 18 16"
                            className="fill-current"
                          >
                            <path d="M9.09815 0.361679L11.1054 6.06601H17.601L12.3459 9.59149L14.3532 15.2958L9.09815 11.7703L3.84309 15.2958L5.85035 9.59149L0.595291 6.06601H7.0909L9.09815 0.361679Z"></path>
                          </svg>
                        </span>
                        <span className="text-yellow mr-1 block">
                          <svg
                            width="18"
                            height="16"
                            viewBox="0 0 18 16"
                            className="fill-current"
                          >
                            <path d="M9.09815 0.361679L11.1054 6.06601H17.601L12.3459 9.59149L14.3532 15.2958L9.09815 11.7703L3.84309 15.2958L5.85035 9.59149L0.595291 6.06601H7.0909L9.09815 0.361679Z"></path>
                          </svg>
                        </span>
                        <span className="text-yellow mr-1 block">
                          <svg
                            width="18"
                            height="16"
                            viewBox="0 0 18 16"
                            className="fill-current"
                          >
                            <path d="M9.09815 0.361679L11.1054 6.06601H17.601L12.3459 9.59149L14.3532 15.2958L9.09815 11.7703L3.84309 15.2958L5.85035 9.59149L0.595291 6.06601H7.0909L9.09815 0.361679Z"></path>
                          </svg>
                        </span>
                        <span className="text-yellow mr-1 block">
                          <svg
                            width="18"
                            height="16"
                            viewBox="0 0 18 16"
                            className="fill-current"
                          >
                            <path d="M9.09815 0.361679L11.1054 6.06601H17.601L12.3459 9.59149L14.3532 15.2958L9.09815 11.7703L3.84309 15.2958L5.85035 9.59149L0.595291 6.06601H7.0909L9.09815 0.361679Z"></path>
                          </svg>
                        </span>
                        <span className="text-yellow mr-1 block">
                          <svg
                            width="18"
                            height="16"
                            viewBox="0 0 18 16"
                            className="fill-current"
                          >
                            <path d="M9.09815 0.361679L11.1054 6.06601H17.601L12.3459 9.59149L14.3532 15.2958L9.09815 11.7703L3.84309 15.2958L5.85035 9.59149L0.595291 6.06601H7.0909L9.09815 0.361679Z"></path>
                          </svg>
                        </span>
                      </div>
                      <p className="mt-2 text-sm font-medium text-body-color">
                        Client{"'"}s Review
                      </p>
                    </div>
                  </div>
                </div>
                <p className="text-lg font-medium text-body-color leading-relaxed pt-8 mt-6 border-t border-[#eee]">
                  Lorem ipsum dolor sit amet, consectetur adipiscing elit. Fusce
                  commodo urna massa, nec dictum velit dignissim ac. Curabitur
                  non cursus tortor. Nunc dignissim accumsan commodo.
                </p>
                <div className="absolute right-0 bottom-0 z-[-1]">
                  <svg
                    width="49"
                    height="60"
                    viewBox="0 0 49 60"
                    fill="none"
                    xmlns="http://www.w3.org/2000/svg"
                  >
                    <circle
                      opacity="0.4"
                      cx="37"
                      cy="37"
                      r="36"
                      transform="rotate(-165 37 37)"
                      fill="url(#paint0_linear_77:14)"
                    ></circle>
                    <defs>
                      <linearGradient
                        id="paint0_linear_77:14"
                        x1="36.3685"
                        y1="91.4954"
                        x2="36.3685"
                        y2="5.62385"
                        gradientUnits="userSpaceOnUse"
                      >
                        <stop stop-color="#4A6CF7"></stop>
                        <stop
                          offset="1"
                          stop-color="#4A6CF7"
                          stop-opacity="0"
                        ></stop>
                      </linearGradient>
                    </defs>
                  </svg>
                </div>
              </div>
            </div>
            <div className="w-full lg:w-1/2 px-4">
              <div
                className="bg-white relative z-10 overflow-hidden rounded-sm p-8 lg:px-6 xl:px-8 mb-10 wow fadeInUp"
                data-wow-delay=".15s"
              >
                <div className="sm:flex justify-between lg:block xl:flex">
                  <div className="w-full flex items-center">
                    <div className="rounded-full overflow-hidden w-[60px] h-[60px] mt-2 mr-4">
                      <img
                        src="https://saas-tailwind.preview.uideck.com/images/author-03.png"
                        alt="image"
                      />
                    </div>
                    <div className="w-full">
                      <h5 className="text-base md:text-lg lg:text-base xl:text-lg text-black font-medium mb-1">
                        Devid Miller
                      </h5>
                      <p className="text-base font-medium text-body-color">
                        Graphic Designer
                      </p>
                    </div>
                  </div>
                  <div className="max-w-[150px] w-full flex items-center sm:justify-end lg:justify-start xl:justify-end mt-4 sm:mt-0 lg:mt-4 xl:mt-0">
                    <div>
                      <div className="flex items-center">
                        <span className="text-yellow mr-1 block">
                          <svg
                            width="18"
                            height="16"
                            viewBox="0 0 18 16"
                            className="fill-current"
                          >
                            <path d="M9.09815 0.361679L11.1054 6.06601H17.601L12.3459 9.59149L14.3532 15.2958L9.09815 11.7703L3.84309 15.2958L5.85035 9.59149L0.595291 6.06601H7.0909L9.09815 0.361679Z"></path>
                          </svg>
                        </span>
                        <span className="text-yellow mr-1 block">
                          <svg
                            width="18"
                            height="16"
                            viewBox="0 0 18 16"
                            className="fill-current"
                          >
                            <path d="M9.09815 0.361679L11.1054 6.06601H17.601L12.3459 9.59149L14.3532 15.2958L9.09815 11.7703L3.84309 15.2958L5.85035 9.59149L0.595291 6.06601H7.0909L9.09815 0.361679Z"></path>
                          </svg>
                        </span>
                        <span className="text-yellow mr-1 block">
                          <svg
                            width="18"
                            height="16"
                            viewBox="0 0 18 16"
                            className="fill-current"
                          >
                            <path d="M9.09815 0.361679L11.1054 6.06601H17.601L12.3459 9.59149L14.3532 15.2958L9.09815 11.7703L3.84309 15.2958L5.85035 9.59149L0.595291 6.06601H7.0909L9.09815 0.361679Z"></path>
                          </svg>
                        </span>
                        <span className="text-yellow mr-1 block">
                          <svg
                            width="18"
                            height="16"
                            viewBox="0 0 18 16"
                            className="fill-current"
                          >
                            <path d="M9.09815 0.361679L11.1054 6.06601H17.601L12.3459 9.59149L14.3532 15.2958L9.09815 11.7703L3.84309 15.2958L5.85035 9.59149L0.595291 6.06601H7.0909L9.09815 0.361679Z"></path>
                          </svg>
                        </span>
                        <span className="text-yellow mr-1 block">
                          <svg
                            width="18"
                            height="16"
                            viewBox="0 0 18 16"
                            className="fill-current"
                          >
                            <path d="M9.09815 0.361679L11.1054 6.06601H17.601L12.3459 9.59149L14.3532 15.2958L9.09815 11.7703L3.84309 15.2958L5.85035 9.59149L0.595291 6.06601H7.0909L9.09815 0.361679Z"></path>
                          </svg>
                        </span>
                      </div>
                      <p className="mt-2 text-sm font-medium text-body-color">
                        Client{"'"}s Review
                      </p>
                    </div>
                  </div>
                </div>
                <p className="text-lg font-medium text-body-color leading-relaxed pt-8 mt-6 border-t border-[#eee]">
                  Lorem ipsum dolor sit amet, consectetur adipiscing elit. Fusce
                  commodo urna massa, nec dictum velit dignissim ac. Curabitur
                  non cursus tortor. Nunc dignissim accumsan commodo.
                </p>
                <div className="absolute right-0 bottom-0 z-[-1]">
                  <svg
                    width="49"
                    height="60"
                    viewBox="0 0 49 60"
                    fill="none"
                    xmlns="http://www.w3.org/2000/svg"
                  >
                    <circle
                      opacity="0.4"
                      cx="37"
                      cy="37"
                      r="36"
                      transform="rotate(-165 37 37)"
                      fill="url(#paint0_linear_77:14)"
                    ></circle>
                    <defs>
                      <linearGradient
                        id="paint0_linear_77:14"
                        x1="36.3685"
                        y1="91.4954"
                        x2="36.3685"
                        y2="5.62385"
                        gradientUnits="userSpaceOnUse"
                      >
                        <stop stop-color="#4A6CF7"></stop>
                        <stop
                          offset="1"
                          stop-color="#4A6CF7"
                          stop-opacity="0"
                        ></stop>
                      </linearGradient>
                    </defs>
                  </svg>
                </div>
              </div>
            </div>
            <div className="w-full lg:w-1/2 px-4">
              <div
                className="bg-white relative z-10 overflow-hidden rounded-sm p-8 lg:px-6 xl:px-8 mb-10 wow fadeInUp"
                data-wow-delay=".2s"
              >
                <div className="sm:flex justify-between lg:block xl:flex">
                  <div className="w-full flex items-center">
                    <div className="rounded-full overflow-hidden w-[60px] h-[60px] mt-2 mr-4">
                      <img
                        src="https://saas-tailwind.preview.uideck.com/images/author-04.png"
                        alt="image"
                      />
                    </div>
                    <div className="w-full">
                      <h5 className="text-base md:text-lg lg:text-base xl:text-lg text-black font-medium mb-1">
                        Jonathon Smith
                      </h5>
                      <p className="text-base font-medium text-body-color">
                        UI/UX Designer
                      </p>
                    </div>
                  </div>
                  <div className="max-w-[150px] w-full flex items-center sm:justify-end lg:justify-start xl:justify-end mt-4 sm:mt-0 lg:mt-4 xl:mt-0">
                    <div>
                      <div className="flex items-center">
                        <span className="text-yellow mr-1 block">
                          <svg
                            width="18"
                            height="16"
                            viewBox="0 0 18 16"
                            className="fill-current"
                          >
                            <path d="M9.09815 0.361679L11.1054 6.06601H17.601L12.3459 9.59149L14.3532 15.2958L9.09815 11.7703L3.84309 15.2958L5.85035 9.59149L0.595291 6.06601H7.0909L9.09815 0.361679Z"></path>
                          </svg>
                        </span>
                        <span className="text-yellow mr-1 block">
                          <svg
                            width="18"
                            height="16"
                            viewBox="0 0 18 16"
                            className="fill-current"
                          >
                            <path d="M9.09815 0.361679L11.1054 6.06601H17.601L12.3459 9.59149L14.3532 15.2958L9.09815 11.7703L3.84309 15.2958L5.85035 9.59149L0.595291 6.06601H7.0909L9.09815 0.361679Z"></path>
                          </svg>
                        </span>
                        <span className="text-yellow mr-1 block">
                          <svg
                            width="18"
                            height="16"
                            viewBox="0 0 18 16"
                            className="fill-current"
                          >
                            <path d="M9.09815 0.361679L11.1054 6.06601H17.601L12.3459 9.59149L14.3532 15.2958L9.09815 11.7703L3.84309 15.2958L5.85035 9.59149L0.595291 6.06601H7.0909L9.09815 0.361679Z"></path>
                          </svg>
                        </span>
                        <span className="text-yellow mr-1 block">
                          <svg
                            width="18"
                            height="16"
                            viewBox="0 0 18 16"
                            className="fill-current"
                          >
                            <path d="M9.09815 0.361679L11.1054 6.06601H17.601L12.3459 9.59149L14.3532 15.2958L9.09815 11.7703L3.84309 15.2958L5.85035 9.59149L0.595291 6.06601H7.0909L9.09815 0.361679Z"></path>
                          </svg>
                        </span>
                        <span className="text-yellow mr-1 block">
                          <svg
                            width="18"
                            height="16"
                            viewBox="0 0 18 16"
                            className="fill-current"
                          >
                            <path d="M9.09815 0.361679L11.1054 6.06601H17.601L12.3459 9.59149L14.3532 15.2958L9.09815 11.7703L3.84309 15.2958L5.85035 9.59149L0.595291 6.06601H7.0909L9.09815 0.361679Z"></path>
                          </svg>
                        </span>
                      </div>
                      <p className="mt-2 text-sm font-medium text-body-color">
                        Client{"'"}s Review
                      </p>
                    </div>
                  </div>
                </div>
                <p className="text-lg font-medium text-body-color leading-relaxed pt-8 mt-6 border-t border-[#eee]">
                  Lorem ipsum dolor sit amet, consectetur adipiscing elit. Fusce
                  commodo urna massa, nec dictum velit dignissim ac. Curabitur
                  non cursus tortor. Nunc dignissim accumsan commodo.
                </p>
                <div className="absolute right-0 bottom-0 z-[-1]">
                  <svg
                    width="49"
                    height="60"
                    viewBox="0 0 49 60"
                    fill="none"
                    xmlns="http://www.w3.org/2000/svg"
                  >
                    <circle
                      opacity="0.4"
                      cx="37"
                      cy="37"
                      r="36"
                      transform="rotate(-165 37 37)"
                      fill="url(#paint0_linear_77:14)"
                    ></circle>
                    <defs>
                      <linearGradient
                        id="paint0_linear_77:14"
                        x1="36.3685"
                        y1="91.4954"
                        x2="36.3685"
                        y2="5.62385"
                        gradientUnits="userSpaceOnUse"
                      >
                        <stop stop-color="#4A6CF7"></stop>
                        <stop
                          offset="1"
                          stop-color="#4A6CF7"
                          stop-opacity="0"
                        ></stop>
                      </linearGradient>
                    </defs>
                  </svg>
                </div>
              </div>
            </div>
            <div className="w-full lg:w-1/2 px-4">
              <div
                className="bg-white relative z-10 overflow-hidden rounded-sm p-8 lg:px-6 xl:px-8 mb-10 wow fadeInUp"
                data-wow-delay=".25s"
              >
                <div className="sm:flex justify-between lg:block xl:flex">
                  <div className="w-full flex items-center">
                    <div className="rounded-full overflow-hidden w-[60px] h-[60px] mt-2 mr-4">
                      <img
                        src="https://saas-tailwind.preview.uideck.com/images/author-01.png"
                        alt="image"
                      />
                    </div>
                    <div className="w-full">
                      <h5 className="text-base md:text-lg lg:text-base xl:text-lg text-black font-medium mb-1">
                        Jubin Nutial
                      </h5>
                      <p className="text-sm font-medium text-body-color">
                        App Developer
                      </p>
                    </div>
                  </div>
                  <div className="max-w-[150px] w-full flex items-center sm:justify-end lg:justify-start xl:justify-end mt-4 sm:mt-0 lg:mt-4 xl:mt-0">
                    <div>
                      <div className="flex items-center">
                        <span className="text-yellow mr-1 block">
                          <svg
                            width="18"
                            height="16"
                            viewBox="0 0 18 16"
                            className="fill-current"
                          >
                            <path d="M9.09815 0.361679L11.1054 6.06601H17.601L12.3459 9.59149L14.3532 15.2958L9.09815 11.7703L3.84309 15.2958L5.85035 9.59149L0.595291 6.06601H7.0909L9.09815 0.361679Z"></path>
                          </svg>
                        </span>
                        <span className="text-yellow mr-1 block">
                          <svg
                            width="18"
                            height="16"
                            viewBox="0 0 18 16"
                            className="fill-current"
                          >
                            <path d="M9.09815 0.361679L11.1054 6.06601H17.601L12.3459 9.59149L14.3532 15.2958L9.09815 11.7703L3.84309 15.2958L5.85035 9.59149L0.595291 6.06601H7.0909L9.09815 0.361679Z"></path>
                          </svg>
                        </span>
                        <span className="text-yellow mr-1 block">
                          <svg
                            width="18"
                            height="16"
                            viewBox="0 0 18 16"
                            className="fill-current"
                          >
                            <path d="M9.09815 0.361679L11.1054 6.06601H17.601L12.3459 9.59149L14.3532 15.2958L9.09815 11.7703L3.84309 15.2958L5.85035 9.59149L0.595291 6.06601H7.0909L9.09815 0.361679Z"></path>
                          </svg>
                        </span>
                        <span className="text-yellow mr-1 block">
                          <svg
                            width="18"
                            height="16"
                            viewBox="0 0 18 16"
                            className="fill-current"
                          >
                            <path d="M9.09815 0.361679L11.1054 6.06601H17.601L12.3459 9.59149L14.3532 15.2958L9.09815 11.7703L3.84309 15.2958L5.85035 9.59149L0.595291 6.06601H7.0909L9.09815 0.361679Z"></path>
                          </svg>
                        </span>
                        <span className="text-yellow mr-1 block">
                          <svg
                            width="18"
                            height="16"
                            viewBox="0 0 18 16"
                            className="fill-current"
                          >
                            <path d="M9.09815 0.361679L11.1054 6.06601H17.601L12.3459 9.59149L14.3532 15.2958L9.09815 11.7703L3.84309 15.2958L5.85035 9.59149L0.595291 6.06601H7.0909L9.09815 0.361679Z"></path>
                          </svg>
                        </span>
                      </div>
                      <p className="mt-2 text-sm font-medium text-body-color">
                        Client{"'"}s Review
                      </p>
                    </div>
                  </div>
                </div>
                <p className="text-lg font-medium text-body-color leading-relaxed pt-8 mt-6 border-t border-[#eee]">
                  Lorem ipsum dolor sit amet, consectetur adipiscing elit. Fusce
                  commodo urna massa, nec dictum velit dignissim ac. Curabitur
                  non cursus tortor. Nunc dignissim accumsan commodo.
                </p>
                <div className="absolute right-0 bottom-0 z-[-1]">
                  <svg
                    width="49"
                    height="60"
                    viewBox="0 0 49 60"
                    fill="none"
                    xmlns="http://www.w3.org/2000/svg"
                  >
                    <circle
                      opacity="0.4"
                      cx="37"
                      cy="37"
                      r="36"
                      transform="rotate(-165 37 37)"
                      fill="url(#paint0_linear_77:14)"
                    ></circle>
                    <defs>
                      <linearGradient
                        id="paint0_linear_77:14"
                        x1="36.3685"
                        y1="91.4954"
                        x2="36.3685"
                        y2="5.62385"
                        gradientUnits="userSpaceOnUse"
                      >
                        <stop stop-color="#4A6CF7"></stop>
                        <stop
                          offset="1"
                          stop-color="#4A6CF7"
                          stop-opacity="0"
                        ></stop>
                      </linearGradient>
                    </defs>
                  </svg>
                </div>
              </div>
            </div>
          </div>
        </div>
      </section>
      <section id="pricing" className="pt-[120px] pb-20">
        <div className="container">
          <motion.div
            ref={refPricing}
            initial={{ y: 15, opacity: 0 }}
            animate={viewPricing ? { y: -15, opacity: 1 } : {}}
            transition={{
              type: "easeIn",
              stiffness: 30,
              duration: 0.6,
              delay: 0.3,
            }}
            className="flex flex-wrap mx-[-16px]"
          >
            <div className="w-full px-4">
              <div
                className="mx-auto max-w-[655px] text-center mb-20 wow fadeInUp"
                data-wow-delay=".1s"
              >
                <span className="text-lg font-semibold text-primary mb-2 block">
                  {" "}
                  Affordable Pricing{" "}
                </span>
                <h2 className="text-black font-bold text-3xl sm:text-4xl md:text-[45px] mb-5">
                  Our Pricing Plans
                </h2>
                <p className="text-body-color text-base md:text-lg leading-relaxed md:leading-relaxed max-w-[570px] mx-auto">
                  Our pricing plans are designed to be affordable, flexible and
                  tailored to your unique needs.
                </p>
              </div>
            </div>
          </motion.div>
          <div className="flex flex-wrap justify-center mx-[-16px]">
            {products?.map((product, index) => (
              <>
                <div className="w-full sm:w-3/4 md:w-1/2 lg:w-1/3 px-4">
                  <div className="bg-white shadow-pricing p-10 md:px-8 lg:py-10 lg:px-6 xl:p-10 text-center rounded-sm relative z-10 mb-10">
                    <h4 className="font-bold text-[#4a6cf7] text-[26px] mb-2">
                      {product?.name}
                    </h4>
                    <div className="flex justify-center items-end">
                      <h2 className="font-bold text-black text-[42px] mb-2">
                        {product?.prices[0].currency === "dol" ? "$" : ""}
                        {product?.prices[0].unit_amount}
                        <span className="text-lg font-medium text-body-color">
                          {" "}
                          /{product?.prices[0].interval}{" "}
                        </span>
                      </h2>
                    </div>
                    <p className="text-base text-body-color leading-relaxed font-medium pb-9 xl:pb-9 lg:pb-6 mb-9 lg:mb-6 xl:mb-9 border-b border-[#E9ECF8]">
                      {product?.description}
                    </p>
                    {index === 0 ? (
                      <>
                        <div className="mb-8">
                          <p className="font-medium text-base text-body-color mb-3">
                            SEO Optimization
                          </p>
                          <p className="font-medium text-base text-body-color mb-3">
                            TailwindCSS Integration
                          </p>
                          <p className="font-medium text-base text-body-color mb-3">
                            Basic Auth
                          </p>
                          <p className="font-medium text-base text-body-color mb-3">
                            Email Support
                          </p>
                          <p className="font-medium text-base text-body-color mb-3">
                            Responsive Templates
                          </p>
                          <p className="font-medium text-base text-body-color mb-3">
                            Basic Analytics
                          </p>
                        </div>
                        <div className="flex items-center justify-center">
                          <button
                            onClick={() =>
                              handleStripeCheckout(product?.prices)
                            }
                            className="flex items-center justify-center p-3 rounded-sm bg-primary text-semibold text-base text-white hover:shadow-signUp transition duration-300 ease-in-out"
                          >
                            Purchase Now
                          </button>
                        </div>
                      </>
                    ) : index === 1 ? (
                      <>
                        <div className="mb-8">
                          <p className="font-medium text-base text-body-color mb-3">
                            Enhanced SEO tools
                          </p>
                          <p className="font-medium text-base text-body-color mb-3">
                            Supabase Integration
                          </p>
                          <p className="font-medium text-base text-body-color mb-3">
                            Priority Email and Chat Support
                          </p>
                          <p className="font-medium text-base text-body-color mb-3">
                            Custom Domains
                          </p>
                          <p className="font-medium text-base text-body-color mb-3">
                            Lifetime Access
                          </p>
                          <p className="font-medium text-base text-body-color mb-3">
                            In-depth analytics
                          </p>
                        </div>
                        <div className="flex items-center justify-center">
                          <button
                            onClick={() =>
                              handleStripeCheckout(product?.prices)
                            }
                            className="flex items-center justify-center p-3 rounded-sm bg-primary text-semibold text-base text-white hover:shadow-signUp transition duration-300 ease-in-out"
                          >
                            Purchase Now
                          </button>
                        </div>
                      </>
                    ) : (
                      <>
                        <div className="mb-8">
                          <p className="font-medium text-base text-body-color mb-3">
                            Comprehensive SEO Suite
                          </p>
                          <p className="font-medium text-base text-body-color mb-3">
                            Supabase + Custom API
                          </p>
                          <p className="font-medium text-base text-body-color mb-3">
                            Commercial Use
                          </p>
                          <p className="font-medium text-base text-body-color mb-3">
                            Dedicated Support
                          </p>
                          <p className="font-medium text-base text-body-color mb-3">
                            Unlimited Custom Domains
                          </p>
                          <p className="font-medium text-base text-body-color mb-3">
                            Enterprise-Grade Analytics
                          </p>
                        </div>
                        <div className="flex items-center justify-center">
                          <button
                            onClick={() =>
                              handleStripeCheckout(product?.prices)
                            }
                            className="flex items-center justify-center p-3 rounded-sm bg-primary text-semibold text-base text-white hover:shadow-signUp transition duration-300 ease-in-out"
                          >
                            Purchase Now
                          </button>
                        </div>
                      </>
                    )}
                    <div className="absolute right-0 top-0 z-[-1]">
                      <svg
                        width="37"
                        height="154"
                        viewBox="0 0 37 154"
                        fill="none"
                        xmlns="http://www.w3.org/2000/svg"
                      >
                        <rect
                          opacity="0.1"
                          x="17.5237"
                          y="78.0005"
                          width="24.7822"
                          height="24.7822"
                          rx="4"
                          transform="rotate(45 17.5237 78.0005)"
                          fill="#4A6CF7"
                        ></rect>
                        <rect
                          opacity="0.3"
                          x="47.585"
                          y="92.9392"
                          width="41.1567"
                          height="45.1468"
                          rx="5"
                          transform="rotate(45 47.585 92.9392)"
                          fill="#4A6CF7"
                        ></rect>
                        <rect
                          opacity="0.8"
                          x="57.7432"
                          y="-2.99951"
                          width="78.8328"
                          height="78.8328"
                          rx="10"
                          transform="rotate(45 57.7432 -2.99951)"
                          fill="#4A6CF7"
                        ></rect>
                      </svg>
                    </div>
                    <div className="absolute left-0 bottom-0 z-[-1]">
                      <svg
                        width="229"
                        height="182"
                        viewBox="0 0 229 182"
                        fill="none"
                        xmlns="http://www.w3.org/2000/svg"
                      >
                        <rect
                          opacity="0.05"
                          x="-0.639709"
                          y="21.1696"
                          width="46.669"
                          height="45.7644"
                          rx="10"
                          transform="rotate(45 -0.639709 21.1696)"
                          fill="url(#paint0_linear_25:114)"
                        ></rect>
                        <rect
                          opacity="0.05"
                          x="40.1691"
                          y="0.000488281"
                          width="34.3355"
                          height="33.67"
                          rx="5"
                          transform="rotate(45 40.1691 0.000488281)"
                          fill="url(#paint1_linear_25:114)"
                        ></rect>
                        <rect
                          opacity="0.07"
                          x="60.9458"
                          y="42.1696"
                          width="237"
                          height="380.347"
                          rx="22"
                          transform="rotate(45 60.9458 42.1696)"
                          fill="url(#paint2_linear_25:114)"
                        ></rect>
                        <defs>
                          <linearGradient
                            id="paint0_linear_25:114"
                            x1="29.5176"
                            y1="14.0581"
                            x2="14.35"
                            y2="82.5021"
                            gradientUnits="userSpaceOnUse"
                          >
                            <stop stop-color="#4A6CF7"></stop>
                            <stop
                              offset="0.822917"
                              stop-color="#4A6CF7"
                              stop-opacity="0"
                            ></stop>
                          </linearGradient>
                          <linearGradient
                            id="paint1_linear_25:114"
                            x1="62.3565"
                            y1="-5.23156"
                            x2="51.1974"
                            y2="45.1244"
                            gradientUnits="userSpaceOnUse"
                          >
                            <stop stop-color="#4A6CF7"></stop>
                            <stop
                              offset="0.822917"
                              stop-color="#4A6CF7"
                              stop-opacity="0"
                            ></stop>
                          </linearGradient>
                          <linearGradient
                            id="paint2_linear_25:114"
                            x1="214.094"
                            y1="-16.9334"
                            x2="120.415"
                            y2="348.232"
                            gradientUnits="userSpaceOnUse"
                          >
                            <stop stop-color="#4A6CF7"></stop>
                            <stop
                              offset="0.822917"
                              stop-color="#4A6CF7"
                              stop-opacity="0"
                            ></stop>
                          </linearGradient>
                        </defs>
                      </svg>
                    </div>
                  </div>
                </div>
              </>
            ))}
          </div>
        </div>
      </section>
      <section id="brands">
        <div className="container">
          <div className="flex flex-wrap mx-[-16px]">
            <div className="w-full px-4">
              <div
                className="border-t border-b border-[#E9ECF8] flex flex-wrap items-center justify-center py-8 px-8 sm:px-10 md:py-[40px] md:px-[50px] xl:p-[50px] 2xl:px-[70px] wow fadeInUp"
                data-wow-delay=".1s
              "
              >
                <a
                  href="https://uideck.com"
                  target="_blank"
                  rel="nofollow noreferrer"
                  className="flex items-center justify-center lg:max-w-[130px] xl:max-w-[150px] 2xl:max-w-[160px] mx-3 sm:mx-4 xl:mx-6 2xl:mx-8 py-[15px] grayscale hover:grayscale-0 transition"
                >
                  <img
                    src="https://saas-tailwind.preview.uideck.com/images/uideck.svg"
                    alt="uideck"
                  />
                </a>
                <a
                  href="https://tailgrids.com"
                  target="_blank"
                  rel="nofollow noreferrer"
                  className="flex items-center justify-center lg:max-w-[130px] xl:max-w-[150px] 2xl:max-w-[160px] mx-3 sm:mx-4 xl:mx-6 2xl:mx-8 py-[15px] grayscale hover:grayscale-0 transition"
                >
                  <img
                    src="	https://saas-tailwind.preview.uideck.com/images/tailgrids.svg"
                    alt="tailgrids"
                  />
                </a>
                <a
                  href="https://lineicons.com"
                  target="_blank"
                  rel="nofollow noreferrer"
                  className="flex items-center justify-center lg:max-w-[130px] xl:max-w-[150px] 2xl:max-w-[160px] mx-3 sm:mx-4 xl:mx-6 2xl:mx-8 py-[15px] grayscale hover:grayscale-0 transition"
                >
                  <img
                    src="	https://saas-tailwind.preview.uideck.com/images/lineicons.svg"
                    alt="lineicons"
                  />
                </a>
                <a
                  href="https://ayroui.com"
                  target="_blank"
                  rel="nofollow noreferrer"
                  className="flex items-center justify-center lg:max-w-[130px] xl:max-w-[150px] 2xl:max-w-[160px] mx-3 sm:mx-4 xl:mx-6 2xl:mx-8 py-[15px] grayscale hover:grayscale-0 transition"
                >
                  <img
                    src="https://saas-tailwind.preview.uideck.com/images/ayroui.svg"
                    alt="ayroui"
                  />
                </a>
                <a
                  href="https://plainadmin.com"
                  target="_blank"
                  rel="nofollow noreferrer"
                  className="flex items-center justify-center lg:max-w-[130px] xl:max-w-[150px] 2xl:max-w-[160px] mx-3 sm:mx-4 xl:mx-6 2xl:mx-8 py-[15px] grayscale hover:grayscale-0 transition"
                >
                  <img
                    src="https://saas-tailwind.preview.uideck.com/images/plainadmin.svg"
                    alt="plainadmin"
                  />
                </a>
              </div>
            </div>
          </div>
        </div>
      </section>
      <section id="blog" className="pt-[120px]">
        <div className="container">
          <motion.div
            ref={refBlog}
            initial={{ y: 15, opacity: 0 }}
            animate={viewBlog ? { y: -15, opacity: 1 } : {}}
            transition={{
              type: "easeIn",
              stiffness: 30,
              duration: 0.6,
              delay: 0.3,
            }}
            className="flex flex-wrap mx-[-16px]"
          >
            <div className="w-full px-4">
              <div
                className="mx-auto max-w-[570px] text-center mb-[100px] wow fadeInUp"
                data-wow-delay=".1s"
              >
                <span className="text-lg font-semibold text-primary mb-2 block">
                  {" "}
                  Latest Updates{" "}
                </span>
                <h2 className="text-black font-bold text-3xl sm:text-4xl md:text-[45px] mb-4">
                  From The Blog
                </h2>
                <p className="text-body-color text-base md:text-lg leading-relaxed md:leading-relaxed">
                  There are many variations of passages of Lorem Ipsum available
                  but the majority have suffered alteration in some form.
                </p>
              </div>
            </div>
          </motion.div>
          <div className="border-b border-[#E9ECF8] pb-20">
            <div className="flex flex-wrap mx-[-16px] justify-center">
              <div className="w-full md:w-2/3 lg:w-1/2 xl:w-1/3 px-4">
                <div
                  className="relative rounded-sm shadow-pricing overflow-hidden mb-10 wow fadeInUp"
                  data-wow-delay=".1s"
                >
                  <a
                    href="blog-details.html"
                    className="w-full block relative p-3 pb-0"
                  >
                    <img
                      src="https://saas-tailwind.preview.uideck.com/images/blog-02.jpg"
                      alt="image"
                      className="w-full"
                    />
                  </a>
                  <div className="p-6 sm:p-8 md:py-8 md:px-6 lg:p-8 xl:py-8 xl:px-5 2xl:p-8">
                    <span className="bg-primary rounded-full inline-flex items-center justify-center py-2 px-4 font-semibold text-sm text-white mb-2">
                      {" "}
                      Computer{" "}
                    </span>
                    <h3>
                      <a
                        href="blog-details.html"
                        className="font-bold text-black text-xl sm:text-2xl block mb-4 hover:text-primary transition"
                      >
                        Best UI components for modern websites
                      </a>
                    </h3>
                    <p className="text-base text-body-color font-medium pb-6 mb-6 border-b border-[#E9ECF8]">
                      Lorem ipsum dolor sit amet, consectetur adipiscing elit.
                      Cras sit amet dictum neque, laoreet dolor.
                    </p>
                    <div className="flex items-center">
                      <div className="flex items-center pr-5 mr-5 xl:pr-3 2xl:pr-5 xl:mr-3 2xl:mr-5 border-r border-[#E9ECF8]">
                        <div className="max-w-[40px] w-full h-[40px] rounded-full overflow-hidden mr-4">
                          <img
                            src="https://saas-tailwind.preview.uideck.com/images/auth-01.png"
                            alt="author"
                            className="w-full"
                          />
                        </div>
                        <div className="w-full">
                          <h4 className="text-sm font-medium text-black mb-1">
                            By
                            <a
                              href="javascript:void(0)"
                              className="text-black hover:text-primary"
                            >
                              {" "}
                              Samuyl Joshi{" "}
                            </a>
                          </h4>
                          <p className="text-xs text-body-color">
                            Graphic Designer
                          </p>
                        </div>
                      </div>
                      <div className="inline-block">
                        <h4 className="text-sm font-medium text-black mb-1">
                          Date
                        </h4>
                        <p className="text-xs text-body-color">15 Dec, 2023</p>
                      </div>
                    </div>
                  </div>
                </div>
              </div>
              <div className="w-full md:w-2/3 lg:w-1/2 xl:w-1/3 px-4">
                <div
                  className="relative rounded-sm shadow-pricing overflow-hidden mb-10 wow fadeInUp"
                  data-wow-delay=".15s"
                >
                  <a
                    href="blog-details.html"
                    className="w-full block relative p-3 pb-0"
                  >
                    <img
                      src="https://saas-tailwind.preview.uideck.com/images/blog-01.jpg"
                      alt="image"
                      className="w-full"
                    />
                  </a>
                  <div className="p-6 sm:p-8 md:py-8 md:px-6 lg:p-8 xl:py-8 xl:px-5 2xl:p-8">
                    <span className="bg-primary rounded-full inline-flex items-center justify-center py-2 px-4 font-semibold text-sm text-white mb-2">
                      {" "}
                      Design{" "}
                    </span>
                    <h3>
                      <a
                        href="blog-details.html"
                        className="font-bold text-black text-xl sm:text-2xl block mb-4 hover:text-primary transition"
                      >
                        9 simple ways to improve your design skills
                      </a>
                    </h3>
                    <p className="text-base text-body-color font-medium pb-6 mb-6 border-b border-[#E9ECF8]">
                      Lorem ipsum dolor sit amet, consectetur adipiscing elit.
                      Cras sit amet dictum neque, laoreet dolor.
                    </p>
                    <div className="flex items-center">
                      <div className="flex items-center pr-5 mr-5 xl:pr-3 2xl:pr-5 xl:mr-3 2xl:mr-5 border-r border-[#E9ECF8]">
                        <div className="max-w-[40px] w-full h-[40px] rounded-full overflow-hidden mr-4">
                          <img
                            src="https://saas-tailwind.preview.uideck.com/images/auth-02.png"
                            alt="author"
                            className="w-full"
                          />
                        </div>
                        <div className="w-full">
                          <h4 className="text-sm font-medium text-black mb-1">
                            By
                            <a
                              href="javascript:void(0)"
                              className="text-black hover:text-primary"
                            >
                              {" "}
                              Musharof Chy{" "}
                            </a>
                          </h4>
                          <p className="text-xs text-body-color">
                            Content Writer
                          </p>
                        </div>
                      </div>
                      <div className="inline-block">
                        <h4 className="text-sm font-medium text-black mb-1">
                          Date
                        </h4>
                        <p className="text-xs text-body-color">15 Dec, 2023</p>
                      </div>
                    </div>
                  </div>
                </div>
              </div>
              <div className="w-full md:w-2/3 lg:w-1/2 xl:w-1/3 px-4">
                <div
                  className="relative rounded-sm shadow-pricing overflow-hidden mb-10 wow fadeInUp"
                  data-wow-delay=".2s"
                >
                  <a
                    href="blog-details.html"
                    className="w-full block relative p-3 pb-0"
                  >
                    <img
                      src="	https://saas-tailwind.preview.uideck.com/images/blog-03.jpg"
                      alt="image"
                      className="w-full"
                    />
                  </a>
                  <div className="p-6 sm:p-8 md:py-8 md:px-6 lg:p-8 xl:py-8 xl:px-5 2xl:p-8">
                    <span className="bg-primary rounded-full inline-flex items-center justify-center py-2 px-4 font-semibold text-sm text-white mb-2">
                      {" "}
                      Development{" "}
                    </span>
                    <h3>
                      <a
                        href="blog-details.html"
                        className="font-bold text-black text-xl sm:text-2xl block mb-4 hover:text-primary transition"
                      >
                        Tips to quickly improve your coding speed.
                      </a>
                    </h3>
                    <p className="text-base text-body-color font-medium pb-6 mb-6 border-b border-[#E9ECF8]">
                      Lorem ipsum dolor sit amet, consectetur adipiscing elit.
                      Cras sit amet dictum neque, laoreet dolor.
                    </p>
                    <div className="flex items-center">
                      <div className="flex items-center pr-5 mr-5 xl:pr-3 2xl:pr-5 xl:mr-3 2xl:mr-5 border-r border-[#E9ECF8]">
                        <div className="max-w-[40px] w-full h-[40px] rounded-full overflow-hidden mr-4">
                          <img
                            src="	https://saas-tailwind.preview.uideck.com/images/auth-03.png"
                            alt="author"
                            className="w-full"
                          />
                        </div>
                        <div className="w-full">
                          <h4 className="text-sm font-medium text-black mb-1">
                            By
                            <a
                              href="javascript:void(0)"
                              className="text-black hover:text-primary"
                            >
                              {" "}
                              Lethium Deo{" "}
                            </a>
                          </h4>
                          <p className="text-xs text-body-color">
                            App Developer
                          </p>
                        </div>
                      </div>
                      <div className="inline-block">
                        <h4 className="text-sm font-medium text-black mb-1">
                          Date
                        </h4>
                        <p className="text-xs text-body-color">15 Dec, 2023</p>
                      </div>
                    </div>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>
      </section>
      <section id="contact" className="py-[120px]">
        <div className="container">
          <motion.div
            ref={refContact}
            initial={{ y: 15, opacity: 0 }}
            animate={viewContact ? { y: -15, opacity: 1 } : {}}
            transition={{
              type: "easeIn",
              stiffness: 30,
              duration: 0.6,
              delay: 0.3,
            }}
            className="flex flex-wrap mx-[-16px]"
          >
            <div className="w-full px-4">
              <div
                className="mx-auto max-w-[570px] text-center mb-[100px] wow fadeInUp"
                data-wow-delay=".1s"
              >
                <span className="text-lg font-semibold text-primary mb-2 block">
                  {" "}
                  Need Any Help?{" "}
                </span>
                <h2 className="text-black font-bold text-3xl sm:text-4xl md:text-[45px] mb-4">
                  Contact With Us
                </h2>
                <p className="text-body-color text-base md:text-lg leading-relaxed md:leading-relaxed">
                  There are many variations of passages of Lorem Ipsum available
                  but the majority have suffered alteration in some form.
                </p>
              </div>
            </div>
          </motion.div>
          <div className="flex flex-wrap justify-center mx-[-16px]">
            <div className="w-full lg:w-10/12 xl:w-8/12 px-4">
              <div>
                <form>
                  <div className="flex flex-wrap mx-[-16px]">
                    <div className="w-full md:w-1/2 px-4">
                      <div className="mb-8">
                        <label className="block text-sm font-medium text-body-color mb-3">
                          {" "}
                          First Name <span className="text-red-500">
                            *
                          </span>{" "}
                        </label>
                        <input
                          type="text"
                          name="fname"
                          placeholder="Enter your first name"
                          className="w-full border border-[#DEE3F7] rounded-sm py-3 px-6 text-body-color text-base placeholder-body-color outline-none focus-visible:shadow-none focus:border-primary transition"
                        />
                      </div>
                    </div>
                    <div className="w-full md:w-1/2 px-4">
                      <div className="mb-8">
                        <label className="block text-sm font-medium text-body-color mb-3">
                          {" "}
                          Last Name <span className="text-red-500">*</span>{" "}
                        </label>
                        <input
                          type="text"
                          name="lname"
                          placeholder="Enter your last name"
                          className="w-full border border-[#DEE3F7] rounded-sm py-3 px-6 text-body-color text-base placeholder-body-color outline-none focus-visible:shadow-none focus:border-primary transition"
                        />
                      </div>
                    </div>
                    <div className="w-full md:w-1/2 px-4">
                      <div className="mb-8">
                        <label className="block text-sm font-medium text-body-color mb-3">
                          {" "}
                          Phone Number <span className="text-red-500">
                            *
                          </span>{" "}
                        </label>
                        <input
                          type="text"
                          name="phone"
                          placeholder="Enter your phone Number"
                          className="w-full border border-[#DEE3F7] rounded-sm py-3 px-6 text-body-color text-base placeholder-body-color outline-none focus-visible:shadow-none focus:border-primary transition"
                        />
                      </div>
                    </div>
                    <div className="w-full md:w-1/2 px-4">
                      <div className="mb-8">
                        <label className="block text-sm font-medium text-body-color mb-3">
                          {" "}
                          Your Email <span className="text-red-500">
                            *
                          </span>{" "}
                        </label>
                        <input
                          type="email"
                          placeholder="Enter your email"
                          className="w-full border border-[#DEE3F7] rounded-sm py-3 px-6 text-body-color text-base placeholder-body-color outline-none focus-visible:shadow-none focus:border-primary transition"
                        />
                      </div>
                    </div>
                    <div className="w-full px-4">
                      <div className="mb-8">
                        <label className="block text-sm font-medium text-body-color mb-3">
                          {" "}
                          Your Message <span className="text-red-500">
                            *
                          </span>{" "}
                        </label>
                        <textarea
                          name="message"
                          rows="5"
                          placeholder="Enter your Message"
                          className="w-full border border-[#DEE3F7] rounded-sm py-3 px-6 text-body-color text-base placeholder-body-color outline-none focus-visible:shadow-none focus:border-primary transition resize-none"
                        ></textarea>
                      </div>
                    </div>
                    <div className="w-full mx-auto text-center px-4">
                      <button className="text-base font-medium text-white bg-primary py-4 px-11 hover:shadow-signUp rounded-sm inline-flex mx-auto transition duration-300 ease-in-out">
                        Submit Ticket
                      </button>
                    </div>
                  </div>
                </form>
              </div>
            </div>
          </div>
        </div>
      </section>
      <section id="newsletter">
        <div className="container">
          <div className="bg-primary overflow-hidden relative z-10 rounded-sm py-10 px-12 sm:px-8 lg:flex items-center justify-between">
            <div className="mb-6 lg:mb-0 md:max-w-[400px] w-full">
              <h2 className="font-bold text-white text-2xl sm:text-3xl leading-snug sm:leading-snug">
                Want to get all the latest updates from SaaS?
              </h2>
            </div>
            <div className="lg:max-w-[520px] w-full">
              <form className="sm:flex items-center justify-between">
                <input
                  type="email"
                  placeholder="Enter Your Email"
                  className="lg:max-w-[350px] w-full sm:mr-4 mb-4 sm:mb-0 bg-white rounded-sm py-3 px-6 text-body-color placeholder-body-color outline-none focus-visible:shadow-none"
                />
                <button
                  type="submit"
                  className="bg-black text-white hover:shadow-signUp transition rounded-sm py-3 px-12 sm:px-8 md:px-10 text-base font-semibold duration-300 ease-in-out"
                >
                  Subscribe
                </button>
              </form>
            </div>
            <div className="absolute w-full h-full top-0 left-0 z-[-1]">
              <svg
                width="1170"
                height="180"
                viewBox="0 0 1170 180"
                fill="none"
                xmlns="http://www.w3.org/2000/svg"
              >
                <mask
                  id="mask0_32:22"
                  maskUnits="userSpaceOnUse"
                  x="0"
                  y="0"
                  width="1170"
                  height="181"
                >
                  <rect
                    y="0.000488281"
                    width="1170"
                    height="180"
                    rx="2"
                    fill="#4A6CF7"
                  ></rect>
                </mask>
                <g mask="url(#mask0_32:22)">
                  <path
                    opacity="0.1"
                    fill-rule="evenodd"
                    clip-rule="evenodd"
                    d="M517.924 0.362991C474.865 29.7973 337.151 43.6819 333.289 -65.9782C332.164 -98.1277 176.271 18.7043 123.154 -42.7352C91.2589 -32.0459 9.36359 -61.1568 9.36359 -61.1568C119.505 -13.6184 74.9786 -4.84753 74.9786 -4.84753C148.455 20.9409 182.866 59.4467 257.666 70.5996L258.92 70.786C341.923 82.9137 427.15 70.7086 517.626 0.518265L517.924 0.362991Z"
                    fill="url(#paint0_linear_32:22)"
                  ></path>
                  <path
                    opacity="0.1"
                    fill-rule="evenodd"
                    clip-rule="evenodd"
                    d="M116.56 135.053C73.5018 164.488 -64.2122 178.372 -68.0741 68.7122C-69.1992 36.5627 -225.092 153.395 -278.21 91.9552C-310.105 102.645 -392 73.5336 -392 73.5336C-281.859 121.072 -326.385 129.843 -326.385 129.843C-252.909 155.631 -218.497 194.137 -143.697 205.29L-142.444 205.476C-59.4408 217.604 25.7864 205.399 116.262 135.209L116.56 135.053Z"
                    fill="url(#paint1_linear_32:22)"
                  ></path>
                  <path
                    opacity="0.1"
                    fill-rule="evenodd"
                    clip-rule="evenodd"
                    d="M859.047 123.309C793.674 165.5 597.387 170.368 609.423 -7.857C612.962 -60.107 374.646 109.284 309.583 2.9043C262.931 16.1446 152.159 -41.5239 152.159 -41.5239C299.8 49.6279 235.653 58.1412 235.653 58.1412C335.089 109.347 377.446 176.161 481.081 203.816L482.818 204.279C597.858 234.563 719.91 225.696 858.602 123.522L859.047 123.309Z"
                    fill="url(#paint2_linear_32:22)"
                  ></path>
                  <path
                    opacity="0.2"
                    fill-rule="evenodd"
                    clip-rule="evenodd"
                    d="M1000 38.936C1046.89 8.71569 1187.67 5.22891 1179.04 132.886C1176.5 170.311 1347.43 48.9816 1394.09 125.178C1427.55 115.694 1507 157 1507 157C1401.11 91.7113 1447.12 85.6135 1447.12 85.6135C1375.8 48.9366 1345.42 1.07942 1271.09 -18.7287L1269.84 -19.0601C1187.33 -40.7523 1099.79 -34.4009 1000.32 38.7831L1000 38.936Z"
                    fill="url(#paint3_linear_32:22)"
                  ></path>
                  <path
                    fill-rule="evenodd"
                    clip-rule="evenodd"
                    d="M439 216C456.118 216 470 202.345 470 185.5C470 168.656 456.118 155 439 155C421.882 155 408 168.656 408 185.5C408 202.345 421.882 216 439 216Z"
                    fill="#FFDDF7"
                  ></path>
                  <path
                    fill-rule="evenodd"
                    clip-rule="evenodd"
                    d="M23.7408 -12.6863C21.3674 -15.9137 18.272 -18.6247 14.7499 -20.6024C11.1896 -22.5818 7.25873 -23.8046 3.20375 -24.1941C3.0226 -24.2134 2.83781 -24.2391 2.65018 -24.2486L2.09689 -24.2813C1.72268 -24.2984 1.35241 -24.33 0.988197 -24.3298C0.61172 -24.3298 0.242724 -24.3378 -0.13165 -24.3341L-1.24763 -24.2897C-2.73739 -24.2005 -4.21866 -24.0036 -5.67997 -23.7004C-7.14348 -23.3773 -8.58317 -22.9543 -9.98887 -22.4344C-11.3877 -21.8915 -12.7544 -21.2506 -14.0705 -20.5223C-15.3654 -19.7783 -16.6237 -18.9498 -17.8167 -18.0292C-19.0006 -17.093 -20.1164 -16.0809 -21.1625 -14.9899C-22.191 -13.8897 -23.1401 -12.7151 -24.0055 -11.4795C-24.8602 -10.2388 -25.6226 -8.93694 -26.2865 -7.58448C-26.4586 -7.20945 -26.6476 -6.84287 -26.8134 -6.45691L-27.2835 -5.31476L-27.7122 -4.14311C-27.8497 -3.75037 -27.9607 -3.3536 -28.085 -2.95883C-28.5476 -1.36754 -28.8799 0.258775 -29.0785 1.90401C-29.1101 2.31206 -29.164 2.72337 -29.1924 3.13688L-29.2644 4.37944L-29.2756 5.62213C-29.2704 6.0308 -29.2381 6.44446 -29.2287 6.86042C-29.1369 8.51276 -28.9042 10.1543 -28.5332 11.767C-26.8531 19.1994 -22.1123 25.8119 -15.7011 29.891C-14.6891 30.5502 -13.6223 31.1147 -12.5479 31.6505L-11.7169 32.0171L-11.3062 32.2026L-10.8836 32.3667L-10.0389 32.6939C-9.75931 32.7996 -9.47472 32.8927 -9.18808 32.9896C-8.62076 33.1939 -8.03547 33.3454 -7.45345 33.5121C-7.16086 33.5982 -6.87126 33.6581 -6.57534 33.7289C-6.28153 33.7961 -5.99208 33.8768 -5.69389 33.9305C-5.09858 34.0361 -4.50232 34.1642 -3.90245 34.2356L-3.00445 34.3608L-2.104 34.4481C-1.54408 34.5135 -0.982016 34.5193 -0.418604 34.5486C-0.139873 34.5686 0.143884 34.5554 0.424358 34.5574L1.27074 34.5508L2.1174 34.5028C2.39857 34.485 2.68443 34.4754 2.96682 34.4386C3.52512 34.375 4.08589 34.3366 4.64663 34.2351C6.8821 33.9002 9.07112 33.3164 11.1771 32.5049C11.5227 32.3673 11.8523 32.2231 12.1948 32.0801C12.3601 32.0089 12.5287 31.9432 12.701 31.8631L13.1952 31.6252C13.5164 31.4664 13.8552 31.3169 14.1782 31.1402L15.138 30.6152C15.2965 30.5322 15.4589 30.4348 15.61 30.339L16.0763 30.0528C16.3899 29.8597 16.6969 29.6763 16.9978 29.4613L17.9014 28.8388L18.7685 28.1742C19.9091 27.268 21.0113 26.2952 21.9969 25.2258C22.9958 24.1585 23.9275 23.0377 24.7398 21.8364C26.8381 18.8087 28.3322 15.4044 29.1402 11.8104C29.9301 8.29603 30.034 4.66221 29.4464 1.10845C29.182 -0.485233 28.7992 -2.07361 28.2972 -3.61627C27.7775 -5.1475 27.1551 -6.64621 26.3621 -8.01968C26.2944 -8.15792 26.2224 -8.28271 26.1475 -8.3913L25.9384 -8.69072C25.8373 -8.83876 25.7271 -8.98033 25.6085 -9.11472C25.4085 -9.3138 25.2714 -9.36212 25.1656 -9.2719C25.0599 -9.18168 25.0825 -8.76366 25.1871 -8.24599C25.3018 -7.73175 25.4412 -7.09088 25.6587 -6.56679C26.2175 -5.12953 26.6751 -3.65498 27.0284 -2.15394C27.1032 -1.77206 27.1993 -1.39526 27.2735 -1.01429C27.3381 -0.628969 27.4134 -0.246181 27.4897 0.138433C27.5442 0.527186 27.6092 0.913431 27.6668 1.30765C27.7234 1.70005 27.7682 2.09313 27.8177 2.49443C28.2844 6.35271 27.8374 10.3742 26.5377 14.1388C25.4727 17.2232 23.885 20.1013 21.844 22.6473C20.6945 24.0639 19.4371 25.3777 18.0298 26.5581C16.6312 27.7324 15.0912 28.7672 13.4244 29.6032L11.9314 30.362C10.3461 31.1154 8.68714 31.7029 6.98093 32.1151C6.69925 32.1951 6.41454 32.249 6.13198 32.3064L5.27461 32.4832L4.41618 32.6158L3.99118 32.679C3.84689 32.698 3.70222 32.7139 3.55725 32.7267L2.68783 32.8195C2.39913 32.8453 2.11163 32.8521 1.81925 32.8716C1.24727 32.9116 0.660716 32.9054 0.0901288 32.9057C-0.206459 32.9179 -0.490423 32.8887 -0.784739 32.8837C-1.07363 32.8672 -1.35627 32.8615 -1.64989 32.8368C-2.21945 32.7758 -2.8063 32.7478 -3.37183 32.6517C-3.66213 32.6117 -3.94983 32.5762 -4.23207 32.529L-5.07966 32.3651L-5.51251 32.2883L-5.93462 32.1884L-6.77013 31.9822L-7.63999 31.7375L-8.07523 31.6148L-8.4991 31.4697L-9.35579 31.1847L-10.1888 30.8568L-10.6103 30.6946L-11.0286 30.5171L-11.8473 30.1507L-12.6508 29.7475L-13.0577 29.5476L-13.451 29.329L-14.2303 28.8839L-14.9922 28.4057L-15.3736 28.1657C-15.5046 28.0859 -15.6185 27.9938 -15.7501 27.9131L-16.4861 27.3957C-16.7246 27.214 -16.9555 27.0243 -17.1945 26.8417L-17.5491 26.5642C-17.664 26.4703 -17.7725 26.3665 -17.8885 26.2708L-18.5688 25.6812C-18.7842 25.4764 -19.0023 25.2671 -19.2102 25.0544C-19.3404 24.934 -19.4705 24.8137 -19.5915 24.6881C-19.7217 24.5677 -19.8348 24.435 -19.9564 24.3085C-20.1996 24.0555 -20.4505 23.8105 -20.6755 23.5469C-20.9102 23.2877 -21.1443 23.0295 -21.3693 22.7659C-21.4904 22.6403 -21.5943 22.5023 -21.7079 22.3687L-22.0288 21.9602L-22.3593 21.5559C-22.4638 21.4171 -22.5597 21.2721 -22.6641 21.1333C-22.8644 20.8494 -23.0739 20.5707 -23.2752 20.285C-23.6416 19.6926 -24.0373 19.1124 -24.3537 18.4802C-25.146 17.0238 -25.8966 15.4922 -26.54 14.0202C-26.718 13.6068 -26.8807 13.0936 -27.024 12.614C-27.4005 11.2466 -27.6719 9.85131 -27.8631 8.44736C-28.0261 7.0355 -28.0999 5.61477 -28.0844 4.19362C-28.071 3.92236 -28.0673 3.65548 -28.0635 3.38857C-28.0657 3.25865 -28.0698 3.12507 -28.0618 2.99171L-28.0378 2.59168C-28.0213 2.32588 -28.0052 2.05915 -27.979 1.789C-27.9528 1.51886 -27.9016 1.25001 -27.8615 0.982808C-27.6513 -0.209646 -27.3641 -1.3737 -27.0496 -2.53283C-26.8721 -3.1089 -26.7117 -3.6934 -26.5151 -4.25742C-26.4136 -4.54434 -26.3264 -4.83511 -26.2217 -5.11656L-25.9007 -5.96983C-25.3192 -7.50781 -24.4558 -8.95716 -23.5838 -10.3495C-22.4561 -12.143 -21.1196 -13.7963 -19.6024 -15.2749C-18.0919 -16.7603 -16.4092 -18.0524 -14.6152 -19.1732C-10.4876 -21.7627 -5.70083 -23.2949 -0.882395 -23.6153C1.21771 -23.764 3.33145 -23.6364 5.4063 -23.303C7.47572 -22.9579 9.50721 -22.4051 11.4976 -21.7132C12.0656 -21.5287 12.6225 -21.2794 13.1894 -21.0336C13.4673 -20.9099 13.7466 -20.7627 14.0217 -20.6227C14.2989 -20.4792 14.5831 -20.3445 14.8484 -20.1795C15.1212 -20.0225 15.3941 -19.8655 15.6583 -19.7023C15.9322 -19.5435 16.1935 -19.3641 16.4513 -19.1911C16.9845 -18.8565 17.4847 -18.4738 17.9852 -18.1118C19.2875 -17.1353 20.5016 -16.0462 21.6132 -14.857C22.7228 -13.6712 23.7379 -12.3969 24.612 -11.0302C24.7803 -10.8018 24.9297 -10.5639 25.0946 -10.3203C25.2601 -10.0758 25.4409 -9.82556 25.6553 -9.55949C25.0446 -10.7225 24.4641 -11.728 23.7408 -12.6863Z"
                    fill="#FF88E4"
                    fill-opacity="0.48"
                  ></path>
                </g>
                <defs>
                  <linearGradient
                    id="paint0_linear_32:22"
                    x1="239.699"
                    y1="-19.4459"
                    x2="377.873"
                    y2="146.769"
                    gradientUnits="userSpaceOnUse"
                  >
                    <stop stop-color="#E2DDFE" stop-opacity="0.01"></stop>
                    <stop
                      offset="1"
                      stop-color="#E2DDFE"
                      stop-opacity="0.88"
                    ></stop>
                  </linearGradient>
                  <linearGradient
                    id="paint1_linear_32:22"
                    x1="-161.665"
                    y1="115.244"
                    x2="-23.4902"
                    y2="281.46"
                    gradientUnits="userSpaceOnUse"
                  >
                    <stop stop-color="#E2DDFE" stop-opacity="0.01"></stop>
                    <stop
                      offset="1"
                      stop-color="#E2DDFE"
                      stop-opacity="0.88"
                    ></stop>
                  </linearGradient>
                  <linearGradient
                    id="paint2_linear_32:22"
                    x1="470.113"
                    y1="55.5749"
                    x2="674.521"
                    y2="314.05"
                    gradientUnits="userSpaceOnUse"
                  >
                    <stop stop-color="#E2DDFE" stop-opacity="0.01"></stop>
                    <stop
                      offset="1"
                      stop-color="#E2DDFE"
                      stop-opacity="0.88"
                    ></stop>
                  </linearGradient>
                  <linearGradient
                    id="paint3_linear_32:22"
                    x1="1235.5"
                    y1="11.5005"
                    x2="1047.87"
                    y2="-80.0782"
                    gradientUnits="userSpaceOnUse"
                  >
                    <stop stop-color="#E2DDFE" stop-opacity="0.01"></stop>
                    <stop
                      offset="1"
                      stop-color="#E2DDFE"
                      stop-opacity="0.88"
                    ></stop>
                  </linearGradient>
                </defs>
              </svg>
            </div>
          </div>
        </div>
      </section>
      <Footer />
    </>
  );
}

```