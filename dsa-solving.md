# How to create DSA Solving platform in Next.js?

**Daily DSA** is a platform through which you can track your DSA learning. The platform helps you track your progress in learning Data Structures and Algorithms (DSA) effectively. It provides a structured curriculum to ensure comprehensive coverage of DSA topics. It offers a wide range of practice problems tailored to different levels of difficulty. To create this platform we will using Next.js.

### **Setting Up the Next.js Project**

Let's start by setting up a new Next.js project. Open your terminal and execute the following commands:

```jsx
npx create-next-app dailydsa

✔ Would you like to use TypeScript with this project? … No
✔ Would you like to use ESLint with this project? … Yes
✔ Would you like to use Tailwind CSS with this project? … Yes
✔ Would you like to use `src/` directory with this project? … Yes
✔ Use App Router (recommended)? … No
✔ Would you like to customize the default import alias? … No
```

This will create a new Next.js project in a directory named **`dailydsa`**. Next, open the project in your preferred code editor.

```jsx
cd dailydsa
```

### **Installing Dependencies**

We will be using **material UI** icons to make our UI more appealing.

```jsx
npm install @mui/icons-material @mui/material @emotion/styled @emotion/react
```

**Step 1.** Remove all the default application code from **`src/pages/index.js`** and  **`src/styles/global.css`.** Both file should like this :

```jsx
export default function Home() {
  return (
    <div>hello</div>
  );
}
```

```jsx
@tailwind base;
@tailwind components;
@tailwind utilities;
```

**Step 2.** Create a landing page :

1. Create folder named **`components`**inside the root project and add the following files into it:

```jsx
 **components**
│   ├── blog.js
│   ├── features.js
│   ├── footer.js
│   ├── getStarted.js
│   ├── header.js
│   ├── heroSection.js
│   ├── stats.js
|   └── testimonials.js
|__
```

1. **Create  Blog section** : Add the following code in the **`blog.js`** in component folder:

```jsx
import Image from "next/image";

export default function Blog() {
  return (
    <div id="blog">
      <div className="max-w-7xl mx-auto px-6 md:px-12 xl:px-6">
        <div className="mb-12 space-y-2 text-center">
          <h2 className="text-3xl font-bold text-gray-800 md:text-4xl dark:text-white">
            Latest Articles
          </h2>
          <p className="lg:mx-auto lg:w-6/12 text-gray-600 dark:text-gray-300">
            Quam hic dolore cumque voluptate rerum beatae et quae, tempore sunt,
            debitis dolorum officia aliquid explicabo? Excepturi, voluptate?
          </p>
        </div>
        <div className="grid gap-8 md:grid-cols-2 lg :grid-cols-3">
          <div className="group p-6 sm:p-8 rounded-3xl bg-white border border-gray-100 dark:shadow-none dark:border-gray-700 dark:bg-gray-800 bg-opacity-50 shadow-2xl shadow-gray-600/10">
            <div className="relative overflow-hidden rounded-xl">
              <Image
                src="https://images.unsplash.com/photo-1661749711934-492cd19a25c3?ixlib=rb-1.2.1&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=1674&q=80"
                alt="art cover"
                loading="lazy"
                width="1000"
                height="667"
                className="h-64 w-full object-cover object-top transition duration-500 group-hover:scale-105"
              />
            </div>
            <div className="mt-6 relative">
              <h3 className="text-2xl font-semibold text-gray-800 dark:text-white">
                De fuga fugiat lorem ispum laboriosam expedita.
              </h3>
              <p className="mt-6 mb-8 text-gray-600 dark:text-gray-300">
                Voluptates harum aliquam totam, doloribus eum impedit atque!
                Temporibus...
              </p>
              <a className="inline-block" href="#">
                <span className="text-info dark:text-blue-300">Read more</span>
              </a>
            </div>
          </div>
          <div className="group p-6 sm:p-8 rounded-3xl bg-white border border-gray-100 dark:shadow-none dark:border-gray-700 dark:bg-gray-800 bg-opacity-50 shadow-2xl shadow-gray-600/10">
            <div className="relative overflow-hidden rounded-xl">
              <Image
                src="https://images.unsplash.com/photo-1491895200222-0fc4a4c35e18?ixlib=rb-1.2.1&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=1674&q=80"
                alt="art cover"
                loading="lazy"
                width="1000"
                height="667"
                className="h-64 w-full object-cover object-top transition duration-500 group-hover:scale-105"
              />
            </div>
            <div className="mt-6 relative">
              <h3 className="text-2xl font-semibold text-gray-800 dark:text-white">
                De fuga fugiat lorem ispum laboriosam expedita.
              </h3>
              <p className="mt-6 mb-8 text-gray-600 dark:text-gray-300">
                Voluptates harum aliquam totam, doloribus eum impedit atque!
                Temporibus...
              </p>
              <a className="inline-block" href="#">
                <span className="text-info dark:text-blue-300">Read more</span>
              </a>
            </div>
          </div>
          <div className="group p-6 sm:p-8 rounded-3xl bg-white border border-gray-100 dark:shadow-none dark:border-gray-700 dark:bg-gray-800 bg-opacity-50 shadow-2xl shadow-gray-600/10">
            <div className="relative overflow-hidden rounded-xl">
              <Image
                src="https://images.unsplash.com/photo-1620121692029-d088224ddc74?ixlib=rb-1.2.1&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=2832&q=80"
                alt="art cover"
                loading="lazy"
                width="1000"
                height="667"
                className="h-64 w-full object-cover object-top transition duration-500 group-hover:scale-105"
              />
            </div>
            <div className="mt-6 relative">
              <h3 className="text-2xl font-semibold text-gray-800 dark:text-white">
                De fuga fugiat lorem ispum laboriosam expedita.
              </h3>
              <p className="mt-6 mb-8 text-gray-600 dark:text-gray-300">
                Voluptates harum aliquam totam, doloribus eum impedit atque!
                Temporibus...
              </p>
              <a className="inline-block" href="#">
                <span className="text-info dark:text-blue-300">Read more</span>
              </a>
            </div>
          </div>
        </div>
      </div>
    </div>
  );
}
```

1. Create  **`images/companies`**   folder and add images for each company and add the following code in **features.js** file :

```jsx
import Image from "next/image";
import Meta from "../images/companies/Meta-Logo.png";
import Amazon from "../images/companies/Amazon.png";
import Atlassion from "../images/companies/atlassion.png";
import Uber from "../images/companies/Uber.png";

export default function Features() {
  return (
    <div id="features">
      <div className="max-w-7xl mx-auto px-6 md:px-12 xl:px-6">
        <div className="md:w-2/3 lg:w-1/2">
          <svg
            xmlns="http://www.w3.org/2000/svg"
            viewBox="0 0 24 24"
            fill="currentColor"
            className="w-6 h-6 text-secondary"
          >
            <path
              fill-rule="evenodd"
              d="M9 4.5a.75.75 0 01.721.544l.813 2.846a3.75 3.75 0 002.576 2.576l2.846.813a.75.75 0 010 1.442l-2.846.813a3.75 3.75 0 00-2.576 2.576l-.813 2.846a.75.75 0 01-1.442 0l-.813-2.846a3.75 3.75 0 00-2.576-2.576l-2.846-.813a.75.75 0 010-1.442l2.846-.813A3.75 3.75 0 007.466 7.89l.813-2.846A.75.75 0 019 4.5zM18 1.5a.75.75 0 01.728.568l.258 1.036c.236.94.97 1.674 1.91 1.91l1.036.258a.75.75 0 010 1.456l-1.036.258c-.94.236-1.674.97-1.91 1.91l-.258 1.036a.75.75 0 01-1.456 0l-.258-1.036a2.625 2.625 0 00-1.91-1.91l-1.036-.258a.75.75 0 010-1.456l1.036-.258a2.625 2.625 0 001.91-1.91l.258-1.036A.75.75 0 0118 1.5zM16.5 15a.75.75 0 01.712.513l.394 1.183c.15.447.5.799.948.948l1.183.395a.75.75 0 010 1.422l-1.183.395c-.447.15-.799.5-.948.948l-.395 1.183a.75.75 0 01-1.422 0l-.395-1.183a1.5 1.5 0 00-.948-.948l-1.183-.395a.75.75 0 010-1.422l1.183-.395c.447-.15.799-.5.948-.948l.395-1.183A.75.75 0 0116.5 15z"
              clip-rule="evenodd"
            />
          </svg>

          <h2 className="my-8 text-2xl font-bold text-gray-700 dark:text-white md:text-4xl">
            A Data-Driven Approach to DSA Mastery
          </h2>
          <p className="text-gray-600 dark:text-gray-300">
            Unlock the power of Data Structures and Algorithms with our
            cutting-edge platform. Dive into daily challenges and expert
            insights, crafted to elevate your coding skills and accelerate your
            learning journey.
          </p>
        </div>
        <div className="mt-16 grid divide-x divide-y divide-gray-100 dark:divide-gray-700 overflow-hidden rounded-3xl border border-gray-100 text-gray-600 dark:border-gray-700 sm:grid-cols-2 lg:grid-cols-4 lg:divide-y-0 xl:grid-cols-4">
          <div className="group relative bg-white dark:bg-gray-800 transition hover:z-[1] hover:shadow-2xl hover:shadow-gray-600/10">
            <div className="relative space-y-8 py-12 p-8">
              <Image
                src={Meta}
                className="w-32"
                width="512"
                height="512"
                alt="burger illustration"
              />

              <div className="space-y-2">
                <h5 className="text-xl font-semibold text-gray-700 dark:text-white transition group-hover:text-secondary">
                Meta DSA Interview Questions
                </h5>
                {/* <p className="text-gray-600 dark:text-gray-300">
                  Discover a curated collection of advanced Meta DSA questions
                  covering algorithmic complexities, optimization strategies,
                  and key concepts.
                </p> */}
              </div>
              <a
                href="#"
                className="flex items-center justify-between group-hover:text-secondary"
              >
                <span className="text-sm">Read more</span>
                <svg
                  xmlns="http://www.w3.org/2000/svg"
                  viewBox="0 0 24 24"
                  fill="currentColor"
                  className="w-5 h-5 -translate-x-4 text-2xl opacity-0 transition duration-300 group-hover:translate-x-0 group-hover:opacity-100"
                >
                  <path
                    fill-rule="evenodd"
                    d="M12.97 3.97a.75.75 0 011.06 0l7.5 7.5a.75.75 0 010 1.06l-7.5 7.5a.75.75 0 11-1.06-1.06l6.22-6.22H3a.75.75 0 010-1.5h16.19l-6.22-6.22a.75.75 0 010-1.06z"
                    clip-rule="evenodd"
                  />
                </svg>
              </a>
            </div>
          </div>
          <div className="group relative bg-white dark:bg-gray-800 transition hover:z-[1] hover:shadow-2xl hover:shadow-gray-600/10">
            <div className="relative space-y-8 py-12 p-8">
              <Image
                src={Amazon}
                className="w-40 pt-2.5"
                width="512"
                height="512"
                alt="burger illustration"
              />

              <div className="space-y-2">
                <h5 className="text-xl font-semibold text-gray-700 dark:text-white transition group-hover:text-secondary">
                  Amazon Interview Questions
                </h5>
              </div>
              <a
                href="#"
                className="flex items-center justify-between group-hover:text-secondary"
              >
                <span className="text-sm">Read more</span>
                <svg
                  xmlns="http://www.w3.org/2000/svg"
                  viewBox="0 0 24 24"
                  fill="currentColor"
                  className="w-5 h-5 -translate-x-4 text-2xl opacity-0 transition duration-300 group-hover:translate-x-0 group-hover:opacity-100"
                >
                  <path
                    fill-rule="evenodd"
                    d="M12.97 3.97a.75.75 0 011.06 0l7.5 7.5a.75.75 0 010 1.06l-7.5 7.5a.75.75 0 11-1.06-1.06l6.22-6.22H3a.75.75 0 010-1.5h16.19l-6.22-6.22a.75.75 0 010-1.06z"
                    clip-rule="evenodd"
                  />
                </svg>
              </a>
            </div>
          </div>
          <div className="group relative bg-white dark:bg-gray-800 transition hover:z-[1] hover:shadow-2xl hover:shadow-gray-600/10">
            <div className="relative space-y-8 py-12 p-8">
              <Image
                src={Atlassion}
                className="w-48 h-20"
                width="512"
                height="512"
                alt="burger illustration"
              />

              <div className="space-y-2">
                <h5 className="text-xl font-semibold text-gray-700 dark:text-white transition group-hover:text-secondary">
                  Atlassian Interview Questions
                </h5>
                {/* <p className="text-gray-600 dark:text-gray-300">
                  Neque Dolor, fugiat non cum doloribus aperiam voluptates
                  nostrum.
                </p> */}
              </div>
              <a
                href="#"
                className="flex items-center justify-between group-hover:text-secondary"
              >
                <span className="text-sm">Read more</span>
                <svg
                  xmlns="http://www.w3.org/2000/svg"
                  viewBox="0 0 24 24"
                  fill="currentColor"
                  className="w-5 h-5 -translate-x-4 text-2xl opacity-0 transition duration-300 group-hover:translate-x-0 group-hover:opacity-100"
                >
                  <path
                    fill-rule="evenodd"
                    d="M12.97 3.97a.75.75 0 011.06 0l7.5 7.5a.75.75 0 010 1.06l-7.5 7.5a.75.75 0 11-1.06-1.06l6.22-6.22H3a.75.75 0 010-1.5h16.19l-6.22-6.22a.75.75 0 010-1.06z"
                    clip-rule="evenodd"
                  />
                </svg>
              </a>
            </div>
          </div>
          <div className="group relative bg-white dark:bg-gray-900 transition hover:z-[1] hover:shadow-2xl hover:shadow-gray-600/10">
            <div className="relative space-y-8 py-12 p-8 transition duration-300 group-hover:bg-white dark:group-hover:bg-gray-800">
              <Image
                src={Uber}
                className="w-32 h-20"
                width="512"
                height="512"
                alt="burger illustration"
              />
              <div className="space-y-2">
                <h5 className="text-xl font-semibold text-gray-700 dark:text-white transition group-hover:text-secondary">
                  Uber DSA Interview Questions
                </h5>
                {/* <p className="text-gray-600 dark:text-gray-300">
                  Neque Dolor, fugiat non cum doloribus aperiam voluptates
                  nostrum.
                </p> */}
              </div>
              <a
                href="#"
                className="flex items-center justify-between group-hover:text-secondary"
              >
                <span className="text-sm">Read more</span>
                <svg
                  xmlns="http://www.w3.org/2000/svg"
                  viewBox="0 0 24 24"
                  fill="currentColor"
                  className="w-5 h-5 -translate-x-4 text-2xl opacity-0 transition duration-300 group-hover:translate-x-0 group-hover:opacity-100"
                >
                  <path
                    fill-rule="evenodd"
                    d="M12.97 3.97a.75.75 0 011.06 0l7.5 7.5a.75.75 0 010 1.06l-7.5 7.5a.75.75 0 11-1.06-1.06l6.22-6.22H3a.75.75 0 010-1.5h16.19l-6.22-6.22a.75.75 0 010-1.06z"
                    clip-rule="evenodd"
                  />
                </svg>
              </a>
            </div>
          </div>
        </div>
      </div>
    </div>
  );
}
```

1. Add code to the footer.js **`footer.js`** file : 

```jsx
import Image from "next/image";

export default function  Footer(){
return(
    <footer className="py-20 md:py-40">
    <div className="max-w-7xl mx-auto px-6 md:px-12 xl:px-6">
      <div className="m-auto md:w-10/12 lg:w-8/12 xl:w-6/12">
        <div className="flex flex-wrap items-center justify-between md:flex-nowrap">
          <div className="flex w-full justify-center space-x-12 text-gray-600 dark:text-gray-300 sm:w-7/12 md:justify-start">
            <ul className="list-inside list-disc space-y-8">
              <li>
                <a href="#" className="transition hover:text-primary">
                  Home
                </a>
              </li>

              <li>
                <a href="#" className="transition hover:text-primary">
                  About
                </a>
              </li>
              <li>
                <a href="#" className="transition hover:text-primary">
                  Guide
                </a>
              </li>
              <li>
                <a href="#" className="transition hover:text-primary">
                  Blocks
                </a>
              </li>
              <li>
                <a href="#" className="transition hover:text-primary">
                  Contact
                </a>
              </li>
              <li>
                <a href="#" className="transition hover:text-primary">
                  Terms of Use
                </a>
              </li>
            </ul>

            <ul role="list" className="space-y-8">
              <li>
                <a
                  href="#"
                  className="flex items-center space-x-3 transition hover:text-primary"
                >
                  <svg
                    xmlns="http://www.w3.org/2000/svg"
                    fill="currentColor"
                    className="w-5"
                    viewBox="0 0 16 16"
                  >
                    <path d="M8 0C3.58 0 0 3.58 0 8c0 3.54 2.29 6.53 5.47 7.59.4.07.55-.17.55-.38 0-.19-.01-.82-.01-1.49-2.01.37-2.53-.49-2.69-.94-.09-.23-.48-.94-.82-1.13-.28-.15-.68-.52-.01-.53.63-.01 1.08.58 1.23.82.72 1.21 1.87.87 2.33.66.07-.52.28-.87.51-1.07-1.78-.2-3.64-.89-3.64-3.95 0-.87.31-1.59.82-2.15-.08-.2-.36-1.02.08-2.12 0 0 .67-.21 2.2.82.64-.18 1.32-.27 2-.27.68 0 1.36.09 2 .27 1.53-1.04 2.2-.82 2.2-.82.44 1.1.16 1.92.08 2.12.51.56.82 1.27.82 2.15 0 3.07-1.87 3.75-3.65 3.95.29.25.54.73.54 1.48 0 1.07-.01 1.93-.01 2.2 0 .21.15.46.55.38A8.012 8.012 0 0 0 16 8c0-4.42-3.58-8-8-8z" />
                  </svg>
                  <span>Github</span>
                </a>
              </li>
              <li>
                <a
                  href="#"
                  className="flex items-center space-x-3 transition hover:text-primary"
                >
                  <svg
                    xmlns="http://www.w3.org/2000/svg"
                    fill="currentColor"
                    className="w-5"
                    viewBox="0 0 16 16"
                  >
                    <path d="M5.026 15c6.038 0 9.341-5.003 9.341-9.334 0-.14 0-.282-.006-.422A6.685 6.685 0 0 0 16 3.542a6.658 6.658 0 0 1-1.889.518 3.301 3.301 0 0 0 1.447-1.817 6.533 6.533 0 0 1-2.087.793A3.286 3.286 0 0 0 7.875 6.03a9.325 9.325 0 0 1-6.767-3.429 3.289 3.289 0 0 0 1.018 4.382A3.323 3.323 0 0 1 .64 6.575v.045a3.288 3.288 0 0 0 2.632 3.218 3.203 3.203 0 0 1-.865.115 3.23 3.23 0 0 1-.614-.057 3.283 3.283 0 0 0 3.067 2.277A6.588 6.588 0 0 1 .78 13.58a6.32 6.32 0 0 1-.78-.045A9.344 9.344 0 0 0 5.026 15z" />
                  </svg>
                  <span>Twitter</span>
                </a>
              </li>
              <li>
                <a
                  href="#"
                  className="flex items-center space-x-3 transition hover:text-primary"
                >
                  <svg
                    xmlns="http://www.w3.org/2000/svg"
                    fill="currentColor"
                    className="w-5"
                    viewBox="0 0 16 16"
                  >
                    <path d="M8.051 1.999h.089c.822.003 4.987.033 6.11.335a2.01 2.01 0 0 1 1.415 1.42c.101.38.172.883.22 1.402l.01.104.022.26.008.104c.065.914.073 1.77.074 1.957v.075c-.001.194-.01 1.108-.082 2.06l-.008.105-.009.104c-.05.572-.124 1.14-.235 1.558a2.007 2.007 0 0 1-1.415 1.42c-1.16.312-5.569.334-6.18.335h-.142c-.309 0-1.587-.006-2.927-.052l-.17-.006-.087-.004-.171-.007-.171-.007c-1.11-.049-2.167-.128-2.654-.26a2.007 2.007 0 0 1-1.415-1.419c-.111-.417-.185-.986-.235-1.558L.09 9.82l-.008-.104A31.4 31.4 0 0 1 0 7.68v-.123c.002-.215.01-.958.064-1.778l.007-.103.003-.052.008-.104.022-.26.01-.104c.048-.519.119-1.023.22-1.402a2.007 2.007 0 0 1 1.415-1.42c.487-.13 1.544-.21 2.654-.26l.17-.007.172-.006.086-.003.171-.007A99.788 99.788 0 0 1 7.858 2h.193zM6.4 5.209v4.818l4.157-2.408L6.4 5.209z" />
                  </svg>
                  <span>YouTube</span>
                </a>
              </li>

              <li>
                <a
                  href="#"
                  className="flex items-center space-x-3 transition hover:text-primary"
                >
                  <svg
                    xmlns="http://www.w3.org/2000/svg"
                    fill="currentColor"
                    className="w-5"
                    viewBox="0 0 16 16"
                  >
                    <path d="M16 8.049c0-4.446-3.582-8.05-8-8.05C3.58 0-.002 3.603-.002 8.05c0 4.017 2.926 7.347 6.75 7.951v-5.625h-2.03V8.05H6.75V6.275c0-2.017 1.195-3.131 3.022-3.131.876 0 1.791.157 1.791.157v1.98h-1.009c-.993 0-1.303.621-1.303 1.258v1.51h2.218l-.354 2.326H9.25V16c3.824-.604 6.75-3.934 6.75-7.951z" />
                  </svg>
                  <span>Facebook</span>
                </a>
              </li>
              <li>
                <a
                  href="#"
                  className="flex items-center space-x-3 transition hover:text-primary"
                >
                  <svg
                    xmlns="http://www.w3.org/2000/svg"
                    fill="currentColor"
                    className="w-5"
                    viewBox="0 0 16 16"
                  >
                    <path d="M9.025 8c0 2.485-2.02 4.5-4.513 4.5A4.506 4.506 0 0 1 0 8c0-2.486 2.02-4.5 4.512-4.5A4.506 4.506 0 0 1 9.025 8zm4.95 0c0 2.34-1.01 4.236-2.256 4.236-1.246 0-2.256-1.897-2.256-4.236 0-2.34 1.01-4.236 2.256-4.236 1.246 0 2.256 1.897 2.256 4.236zM16 8c0 2.096-.355 3.795-.794 3.795-.438 0-.793-1.7-.793-3.795 0-2.096.355-3.795.794-3.795.438 0 .793 1.699.793 3.795z" />
                  </svg>
                  <span>Medium</span>
                </a>
              </li>
              <li>
                <a
                  href="#"
                  className="flex items-center space-x-3 transition hover:text-primary"
                >
                  <svg
                    xmlns="http://www.w3.org/2000/svg"
                    width="16"
                    height="16"
                    fill="currentColor"
                    className="5"
                    viewBox="0 0 16 16"
                  >
                    <path d="M8 0a8 8 0 0 0-2.915 15.452c-.07-.633-.134-1.606.027-2.297.146-.625.938-3.977.938-3.977s-.239-.479-.239-1.187c0-1.113.645-1.943 1.448-1.943.682 0 1.012.512 1.012 1.127 0 .686-.437 1.712-.663 2.663-.188.796.4 1.446 1.185 1.446 1.422 0 2.515-1.5 2.515-3.664 0-1.915-1.377-3.254-3.342-3.254-2.276 0-3.612 1.707-3.612 3.471 0 .688.265 1.425.595 1.826a.24.24 0 0 1 .056.23c-.061.252-.196.796-.222.907-.035.146-.116.177-.268.107-1-.465-1.624-1.926-1.624-3.1 0-2.523 1.834-4.84 5.286-4.84 2.775 0 4.932 1.977 4.932 4.62 0 2.757-1.739 4.976-4.151 4.976-.811 0-1.573-.421-1.834-.919l-.498 1.902c-.181.695-.669 1.566-.995 2.097A8 8 0 1 0 8 0z" />
                  </svg>
                  <span>Pintrest</span>
                </a>
              </li>
              <li>
                <a
                  href="#"
                  className="flex items-center space-x-3 transition hover:text-primary"
                >
                  <Image
                    className="h-5 w-5"
                    width="32"
                    height="32"
                    src="https://c5.patreon.com/external/favicon/favicon.ico?v=69kMELnXkB"
                    alt="patreon icon"
                  />
                  <span>Patreon</span>
                </a>
              </li>
              <li>
                <a
                  href="#"
                  className="flex items-center space-x-3 transition hover:text-primary"
                >
                  <svg
                    xmlns="http://www.w3.org/2000/svg"
                    fill="currentColor"
                    className="w-5"
                    viewBox="0 0 16 16"
                  >
                    <path d="M8 0C5.829 0 5.556.01 4.703.048 3.85.088 3.269.222 2.76.42a3.917 3.917 0 0 0-1.417.923A3.927 3.927 0 0 0 .42 2.76C.222 3.268.087 3.85.048 4.7.01 5.555 0 5.827 0 8.001c0 2.172.01 2.444.048 3.297.04.852.174 1.433.372 1.942.205.526.478.972.923 1.417.444.445.89.719 1.416.923.51.198 1.09.333 1.942.372C5.555 15.99 5.827 16 8 16s2.444-.01 3.298-.048c.851-.04 1.434-.174 1.943-.372a3.916 3.916 0 0 0 1.416-.923c.445-.445.718-.891.923-1.417.197-.509.332-1.09.372-1.942C15.99 10.445 16 10.173 16 8s-.01-2.445-.048-3.299c-.04-.851-.175-1.433-.372-1.941a3.926 3.926 0 0 0-.923-1.417A3.911 3.911 0 0 0 13.24.42c-.51-.198-1.092-.333-1.943-.372C10.443.01 10.172 0 7.998 0h.003zm-.717 1.442h.718c2.136 0 2.389.007 3.232.046.78.035 1.204.166 1.486.275.373.145.64.319.92.599.28.28.453.546.598.92.11.281.24.705.275 1.485.039.843.047 1.096.047 3.231s-.008 2.389-.047 3.232c-.035.78-.166 1.203-.275 1.485a2.47 2.47 0 0 1-.599.919c-.28.28-.546.453-.92.598-.28.11-.704.24-1.485.276-.843.038-1.096.047-3.232.047s-2.39-.009-3.233-.047c-.78-.036-1.203-.166-1.485-.276a2.478 2.478 0 0 1-.92-.598 2.48 2.48 0 0 1-.6-.92c-.109-.281-.24-.705-.275-1.485-.038-.843-.046-1.096-.046-3.233 0-2.136.008-2.388.046-3.231.036-.78.166-1.204.276-1.486.145-.373.319-.64.599-.92.28-.28.546-.453.92-.598.282-.11.705-.24 1.485-.276.738-.034 1.024-.044 2.515-.045v.002zm4.988 1.328a.96.96 0 1 0 0 1.92.96.96 0 0 0 0-1.92zm-4.27 1.122a4.109 4.109 0 1 0 0 8.217 4.109 4.109 0 0 0 0-8.217zm0 1.441a2.667 2.667 0 1 1 0 5.334 2.667 2.667 0 0 1 0-5.334z" />
                  </svg>
                  <span>Instagram</span>
                </a>
              </li>
            </ul>
          </div>
          <div className="m-auto w-10/12 space-y-6 text-center sm:mt-auto sm:w-5/12 sm:text-left">
            <span className="block text-gray-500 dark:text-gray-400">
            Your Daily Dose of DSA Wisdom
            </span>

            <span className="block text-gray-500 dark:text-gray-400">
              Daily DSA &copy; <span id="year"></span>
            </span>

            <span className="flex justify-between text-gray-600 dark:text-white">
              <a href="#" className="font-medium">
                Terms of Use{" "}
              </a>
              <a href="#" className="font-medium">
                {" "}
                Privacy Policy
              </a>
            </span>

            <span className="block text-gray-500 dark:text-gray-400">
              Need help?{" "}
              <a href="#" className="font-semibold text-gray-600 dark:text-white">
                {" "}
                Contact Us
              </a>
            </span>
          </div>
        </div>
      </div>
    </div>
  </footer>
)}
```

1. Add the following code in the  **`getStarted.js`** file, also create   **`images/avatar`**  folder and add your avatars images.

```jsx
import Image from "next/image";
import Avatar from "../images/avatars/avatar.webp";
import Avatar2 from "../images/avatars/avatar-1.webp";
import Avatar3 from "../images/avatars/avatar-2.webp";
import Avatar4 from "../images/avatars/avatar-3.webp";
import Avatar5 from "../images/avatars/avatar-4.webp";

export default function GetStarted() {
  return (
    <div className="relative py-16">
      <div
        aria-hidden="true"
        className="absolute inset-0 h-max w-full m-auto grid grid-cols-2 -space-x-52 opacity-40 dark:opacity-20"
      >
        <div className="blur-[106px] h-56 bg-gradient-to-br from-primary to-violet-600 dark:from-blue-700"></div>
        <div className="blur-[106px] h-32 bg-gradient-to-r from-violet-50 to-violet-700 dark:to-indigo-600"></div>
      </div>
      <div className="max-w-7xl mx-auto px-6 md:px-12 xl:px-6">
        <div className="relative">
          <div className="flex items-center justify-center -space-x-2">
            <Image
              loading="lazy"
              width="400"
              height="400"
              src={Avatar}
              alt="member photo"
              className="h-8 w-8 rounded-full object-cover"
            />
            <Image
              loading="lazy"
              width="200"
              height="200"
              src={Avatar2}
              alt="member photo"
              className="h-12 w-12 rounded-full object-cover"
            />
            <Image
              loading="lazy"
              width="200"
              height="200"
              src={Avatar3}
              alt="member photo"
              className="z-10 h-16 w-16 rounded-full object-cover"
            />
            <Image
              loading="lazy"
              width="200"
              height="200"
              src={Avatar4}
              alt="member photo"
              className="relative h-12 w-12 rounded-full object-cover"
            />
            <Image
              loading="lazy"
              width="200"
              height="200"
              src={Avatar5}
              alt="member photo"
              className="h-8 w-8 rounded-full object-cover"
            />
          </div>
          <div className="mt-6 m-auto space-y-6 md:w-8/12 lg:w-7/12">
            <h1 className="text-center text-4xl font-bold text-gray-800 dark:text-white md:text-5xl">
              Get Started now
            </h1>
            <p className="text-center text-xl text-gray-600 dark:text-gray-300">
              Be part of millions people around the world using Daily DSA.
            </p>
            <div className="flex flex-wrap justify-center gap-6">
              <a
                href="#"
                className="relative flex h-12 w-full items-center justify-center px-8 before:absolute before:inset-0 before:rounded-full before:bg-primary before:transition before:duration-300 hover:before:scale-105 active:duration-75 active:before:scale-95 sm:w-max"
              >
                <span className="relative text-lg	 font-bold pl-6 pr-6 pb-3 pt-3 rounded-lg bg-violet-50  text-violet-600 mt-40">
                  Get Started
                </span>
              </a>
            </div>
          </div>
        </div>
      </div>
    </div>
  );
}
```

1. Add the following code in the  **`header.js`** file, also add your favorite logo for the application.

```jsx
import Image from "next/image";
import Logo from "../images/ddlogo.png";
import Link from "next/link";

export default function Header() {
  return (
    <header>
      <nav className="z-10 w-full absolute">
        <div className="max-w-7xl mx-auto px-6 md:px-12 xl:px-6">
          <div className="flex flex-wrap items-center justify-between py-2 gap-6 md:py-4 md:gap-0 relative">
            <input
              aria-hidden="true"
              type="checkbox"
              name="toggle_nav"
              id="toggle_nav"
              className="hidden peer"
            />
            <div className="relative z-20 w-full flex justify-between lg:w-max md:px-0">
              <Image src={Logo} className="h-14 w-40" alt="Logo" />

              <div className="relative flex items-center lg:hidden max-h-10">
                <label
                  role="button"
                  for="toggle_nav"
                  aria-label="humburger"
                  id="hamburger"
                  className="relative  p-6 -mr-6"
                >
                  <div
                    aria-hidden="true"
                    id="line"
                    className="m-auto h-0.5 w-5 rounded bg-sky-900 dark:bg-gray-300 transition duration-300"
                  ></div>
                  <div
                    aria-hidden="true"
                    id="line2"
                    className="m-auto mt-2 h-0.5 w-5 rounded bg-sky-900 dark:bg-gray-300 transition duration-300"
                  ></div>
                </label>
              </div>
            </div>
            <div
              aria-hidden="true"
              className="fixed z-10 inset-0 h-screen w-screen bg-white/70 backdrop-blur-2xl origin-bottom scale-y-0 transition duration-500 peer-checked:origin-top peer-checked:scale-y-100 lg:hidden dark:bg-gray-900/70"
            ></div>
            <div
              className="flex-col z-20 flex-wrap gap-6 p-8 rounded-3xl border border-gray-100 bg-white shadow-2xl shadow-gray-600/10 justify-end w-full invisible opacity-0 translate-y-1  absolute top-full left-0 transition-all duration-300 scale-95 origin-top 
                            lg:relative lg:scale-100 lg:peer-checked:translate-y-0 lg:translate-y-0 lg:flex lg:flex-row lg:items-center lg:gap-0 lg:p-0 lg:bg-transparent lg:w-7/12 lg:visible lg:opacity-100 lg:border-none
                            peer-checked:scale-100 peer-checked:opacity-100 peer-checked:visible lg:shadow-none 
                            dark:shadow-none dark:bg-gray-800 dark:border-gray-700"
            >
              <div className="text-gray-600 dark:text-gray-300 lg:pr-4 lg:w-auto w-full lg:pt-0">
                <ul className="tracking-wide font-medium lg:text-sm flex-col flex lg:flex-row gap-6 lg:gap-0">
                  <li>
                    <Link href="/problems" legacyBehavior>
                      <a
                        href="#features"
                        className="block md:px-4 transition hover:text-primary"
                      >
                        <span>Problems</span>
                      </a>
                    </Link>
                  </li>
                  <li>
                    <Link href="/discuss" legacyBehavior>
                      <a
                        href="#solution"
                        className="block md:px-4 transition hover:text-primary"
                      >
                        <span>Discuss</span>
                      </a>
                    </Link>
                  </li>
                  <li>
                    <Link href="/contest" legacyBehavior>
                      <a
                        href="#testimonials"
                        className="block md:px-4 transition hover:text-primary"
                      >
                        <span>Contest</span>
                      </a>
                    </Link>
                  </li>
                  <li>
                    <Link href="/blog" legacyBehavior>
                      <a
                        href="#blog"
                        className="block md:px-4 transition hover:text-primary"
                      >
                        <span>Blog</span>
                      </a>
                    </Link>
                  </li>
                </ul>
              </div>

              <div className="mt-12 lg:mt-0">
                <Link href="/problems" legacyBehavior>
                  <a
                    href="#"
                    className="relative flex h-9 w-full items-center justify-center px-4 before:absolute before:inset-0 before:rounded-full before:bg-primary before:transition before:duration-300 hover:before:scale-105 active:duration-75 active:before:scale-95 sm:w-max"
                  >
                    <span className="relative rounded-lg bg-transparent text-sm font-bold text-violet-700">
                      Login
                    </span>
                  </a>
                </Link>
              </div>
            </div>
          </div>
        </div>
      </nav>
    </header>
  );
}
```

1. Add the following code in the  **`herosection.js`** file, also add your favorite client images for the application.

```jsx
import Image from "next/image";
import Client1 from "../images/clients/google.svg";
import Client2 from "../images/clients/google-cloud.svg";
import Client3 from "../images/clients/microsoft.svg";
import Client4 from "../images/clients/netflix.svg";
import Client5 from "../images/clients/ge.svg";
import Client6 from "../images/clients/airbnb.svg";

export default function HeroSection() {
  return (
    <div className="relative" id="home">
      <div
        aria-hidden="true"
        className="absolute inset-0 grid grid-cols-2 -space-x-52 opacity-40 dark:opacity-20"
      >
        <div className="blur-[106px] h-56 bg-gradient-to-br from-primary to-violet-400 dark:from-blue-700"></div>
        <div className="blur-[106px] h-32 bg-gradient-to-r from-violet-400 to-violet-300 dark:to-indigo-600"></div>
      </div>
      <div className="max-w-7xl mx-auto px-6 md:px-12 xl:px-6">
        <slot />
        <div className="relative pt-36 ml-auto">
          <div className="lg:w-2/3 text-center mx-auto">
            <h1 className="text-gray-900 dark:text-white font-bold text-5xl md:text-6xl xl:text-7xl">
              Your Daily Dose of Data Structures.
            </h1>
            <p className="mt-8 text-gray-700 dark:text-gray-300">
              Your daily fix of coding challenges and insights into Data
              Structures and Algorithms. Level up your coding skills with our
              curated challenges and expert explanations. Join us on a journey
              of growth and mastery in the world of algorithms
            </p>
            <div className="mt-16 flex flex-wrap justify-center gap-y-4 gap-x-6">
              <a
                href="#"
                className="relative flex h-11 w-full items-center justify-center px-6 before:absolute before:inset-0 before:rounded-full before:bg-primary before:transition before:duration-300 hover:before:scale-105 active:duration-75 active:before:scale-95 sm:w-max"
              >
                <span className="relative font-sans text-lg font-bold	 pl-6 pr-6 pb-3 pt-3 rounded-lg bg-violet-50 text-violet-600">
                  Get Started
                </span>
              </a>
            </div>
            <div className="hidden py-8 mt-16 border-y border-gray-100 dark:border-gray-800 sm:flex justify-between">
              <div className="text-left">
                <h6 className="text-lg font-semibold text-gray-700 dark:text-white">
                  Personalized Learning Paths
                </h6>
              </div>
              <div className="text-left">
                <h6 className="text-lg font-semibold text-gray-700 dark:text-white">
                  Accessible Anytime, Anywhere
                </h6>
              </div>
              <div className="text-left">
                <h6 className="text-lg font-semibold text-gray-700 dark:text-white">
                  Engaging Community
                </h6>
              </div>
            </div>
          </div>
          <div className="mt-12 grid grid-cols-3 sm:grid-cols-4 md:grid-cols-6">
            <div className="p-4 ">
              <Image
                src={Client1}
                className="h-12 w-auto mx-auto"
                loading="lazy"
                alt="client logo"
                width=""
                height=""
              />
            </div>
            <div className="p-4 ">
              <Image
                src={Client2}
                className="h-12 w-auto mx-auto"
                loading="lazy"
                alt="client logo"
                width=""
                height=""
              />
            </div>
            <div className="p-4 ">
              <Image
                src={Client3}
                className="h-9 w-auto m-auto"
                loading="lazy"
                alt="client logo"
                width=""
                height=""
              />
            </div>
            <div className="p-4 ">
              <Image
                src={Client4}
                className="h-12 w-auto mx-auto"
                loading="lazy"
                alt="client logo"
                width=""
                height=""
              />
            </div>
            <div className="p-4 ">
              <Image
                src={Client5}
                className="h-8 w-auto m-auto"
                loading="lazy"
                alt="client logo"
                width=""
                height=""
              />
            </div>
            <div className="p-4 ">
              <Image
                src={Client6}
                className="h-12 w-auto mx-auto"
                loading="lazy"
                alt="client logo"
                width=""
                height=""
              />
            </div>
          </div>
        </div>
      </div>
    </div>
  );
}

```

1. Add the following code in the **`stats.js`** file, also add your favorite client images for the application.

```jsx
import Image from "next/image";
import Client from "../images/pie.svg";
export default function Stats() {
  return (
    <div id="solution">
      <div class="max-w-7xl mx-auto px-6 md:px-12 xl:px-6">
        <svg
          xmlns="http://www.w3.org/2000/svg"
          viewBox="0 0 24 24"
          fill="currentColor"
          className="w-6 h-6 text-sky-500"
        >
          <path
            fill-rule="evenodd"
            d="M2.25 13.5a8.25 8.25 0 018.25-8.25.75.75 0 01.75.75v6.75H18a.75.75 0 01.75.75 8.25 8.25 0 01-16.5 0z"
            clip-rule="evenodd"
          />
          <path
            fill-rule="evenodd"
            d="M12.75 3a.75.75 0 01.75-.75 8.25 8.25 0 018.25 8.25.75.75 0 01-.75.75h-7.5a.75.75 0 01-.75-.75V3z"
            clip-rule="evenodd"
          />
        </svg>

        <div className="space-y-6 justify-between text-gray-600 md:flex flex-row-reverse md:gap-6 md:space-y-0 lg:gap-12 lg:items-center">
          <div className="md:5/12 lg:w-1/2">
            <Image
              src={Client}
              alt="image"
              loading="lazy"
              width=""
              height=""
              className="w-full"
            />
          </div>
          <div className="md:7/12 lg:w-1/2">
            <h2 className="text-3xl font-bold text-gray-900 md:text-4xl dark:text-white">
              Daily DSA development is carried out by passionate developers
            </h2>
            <p className="my-8 text-gray-600 dark:text-gray-300">
              {" "}
              Daily DSA development is driven by a team of passionate developers
              committed to delivering high-quality content that enriches and
              challenges users{"'"} understanding of Data Structures and
              Algorithms.
              <br /> <br /> With a dedication to excellence and a deep-rooted
              enthusiasm for problem-solving, our team meticulously crafts each
              daily challenge, ensuring that learners are continually engaged
              and empowered to enhance their coding skills.
            </p>
            <div className="divide-y space-y-4 divide-gray-100 dark:divide-gray-800">
              <div className="mt-8 flex gap-4 md:items-center">
                <div className="w-12 h-12 flex gap-4 rounded-full bg-indigo-100 dark:bg-indigo-900/20">
                  <svg
                    xmlns="http://www.w3.org/2000/svg"
                    viewBox="0 0 24 24"
                    fill="currentColor"
                    className="w-6 h-6 m-auto text-indigo-500 dark:text-indigo-400"
                  >
                    <path
                      fill-rule="evenodd"
                      d="M4.848 2.771A49.144 49.144 0 0112 2.25c2.43 0 4.817.178 7.152.52 1.978.292 3.348 2.024 3.348 3.97v6.02c0 1.946-1.37 3.678-3.348 3.97a48.901 48.901 0 01-3.476.383.39.39 0 00-.297.17l-2.755 4.133a.75.75 0 01-1.248 0l-2.755-4.133a.39.39 0 00-.297-.17 48.9 48.9 0 01-3.476-.384c-1.978-.29-3.348-2.024-3.348-3.97V6.741c0-1.946 1.37-3.68 3.348-3.97zM6.75 8.25a.75.75 0 01.75-.75h9a.75.75 0 010 1.5h-9a.75.75 0 01-.75-.75zm.75 2.25a.75.75 0 000 1.5H12a.75.75 0 000-1.5H7.5z"
                      clip-rule="evenodd"
                    />
                  </svg>
                </div>
                <div className="w-5/6">
                  <h4 className="font-semibold text-lg text-gray-700 dark:text-indigo-300">
                    Daily DSA Question in Email
                  </h4>
                  <p className="text-gray-500 dark:text-gray-400">
                    Maintain consistent learning
                  </p>
                </div>
              </div>
              <div className="pt-4 flex gap-4 md:items-center">
                <div className="w-12 h-12 flex gap-4 rounded-full bg-teal-100 dark:bg-teal-900/20">
                  <svg
                    xmlns="http://www.w3.org/2000/svg"
                    viewBox="0 0 24 24"
                    fill="currentColor"
                    className="w-6 h-6 m-auto text-teal-600 dark:text-teal-400"
                  >
                    <path
                      fill-rule="evenodd"
                      d="M11.54 22.351l.07.04.028.016a.76.76 0 00.723 0l.028-.015.071-.041a16.975 16.975 0 001.144-.742 19.58 19.58 0 002.683-2.282c1.944-1.99 3.963-4.98 3.963-8.827a8.25 8.25 0 00-16.5 0c0 3.846 2.02 6.837 3.963 8.827a19.58 19.58 0 002.682 2.282 16.975 16.975 0 001.145.742zM12 13.5a3 3 0 100-6 3 3 0 000 6z"
                      clip-rule="evenodd"
                    />
                  </svg>
                </div>
                <div className="w-5/6">
                  <h4 className="font-semibold text-lg text-gray-700 dark:text-teal-300">
                    Trackable Progress
                  </h4>
                  <p className="text-gray-500 dark:text-gray-400">
                    Track your progress over time, identifying areas for
                    improvement
                  </p>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  );
}
```

1. Add the following code in the a **`testimonials.js`** file, also add your favorite avatars images for the application.

```jsx
import Avatar from "../images/avatars/avatar.webp";
import Avatar2 from "../images/avatars/avatar-1.webp";
import Avatar3 from "../images/avatars/avatar-2.webp";
import Avatar4 from "../images/avatars/avatar-3.webp";
import Avatar5 from "../images/avatars/avatar-4.webp";

import Image from "next/image";
export default function Testimonials() {
  return (
    <div className="text-gray-600 dark:text-gray-300" id="testimonials">
      <div className="max-w-7xl mx-auto px-6 md:px-12 xl:px-6">
        <div className="mb-20 space-y-4 px-6 md:px-0">
          <h2 className="text-center text-2xl font-bold text-gray-800 dark:text-white md:text-4xl">
            We have some fans.
          </h2>
        </div>
        <div className="md:columns-2 lg:columns-3 gap-8 space-y-8">
          <div className="aspect-auto p-8 border border-gray-100 rounded-3xl bg-white dark:bg-gray-800 dark:border-gray-700 shadow-2xl shadow-gray-600/10 dark:shadow-none">
            <div className="flex gap-4">
              <Image
                className="w-12 h-12 rounded-full"
                src={Avatar}
                alt="user avatar"
                width="400"
                height="400"
                loading="lazy"
              />
              <div>
                <h6 className="text-lg font-medium text-gray-700 dark:text-white">
                  Daniella Doe
                </h6>
                <p className="text-sm text-gray-500 dark:text-gray-300">
                  Mobile dev
                </p>
              </div>
            </div>
            <p className="mt-8">
              Lorem ipsum dolor sit amet consectetur adipisicing elit. Illum
              aliquid quo eum quae quos illo earum ipsa doloribus nostrum minus
              libero aspernatur laborum cum, a suscipit, ratione ea totam ullam!
              Lorem ipsum dolor sit amet consectetur, adipisicing elit.
              Architecto laboriosam deleniti aperiam ab veniam sint non cumque
              quis tempore cupiditate. Sint libero voluptas veniam at
              reprehenderit, veritatis harum et rerum.
            </p>
          </div>
          <div className="aspect-auto p-8 border border-gray-100 rounded-3xl bg-white dark:bg-gray-800 dark:border-gray-700 shadow-2xl shadow-gray-600/10 dark:shadow-none">
            <div className="flex gap-4">
              <Image
                className="w-12 h-12 rounded-full"
                src={Avatar2}
                alt="user avatar"
                width="200"
                height="200"
                loading="lazy"
              />
              <div>
                <h6 className="text-lg font-medium text-gray-700 dark:text-white">
                  Jane doe
                </h6>
                <p className="text-sm text-gray-500 dark:text-gray-300">
                  Marketing
                </p>
              </div>
            </div>
            <p className="mt-8">
              {" "}
              Lorem ipsum dolor laboriosam deleniti aperiam ab veniam sint non
              cumque quis tempore cupiditate. Sint libero voluptas veniam at
              reprehenderit, veritatis harum et rerum.
            </p>
          </div>
          <div className="aspect-auto p-8 border border-gray-100 rounded-3xl bg-white dark:bg-gray-800 dark:border-gray-700 shadow-2xl shadow-gray-600/10 dark:shadow-none">
            <div className="flex gap-4">
              <Image
                className="w-12 h-12 rounded-full"
                src={Avatar3}
                alt="user avatar"
                width="200"
                height="200"
                loading="lazy"
              />
              <div>
                <h6 className="text-lg font-medium text-gray-700 dark:text-white">
                  Yanick Doe
                </h6>
                <p className="text-sm text-gray-500 dark:text-gray-300">
                  Developer
                </p>
              </div>
            </div>
            <p className="mt-8">
              Lorem ipsum dolor sit amet consectetur, adipisicing elit.
              Architecto laboriosam deleniti aperiam ab veniam sint non cumque
              quis tempore cupiditate. Sint libero voluptas veniam at
              reprehenderit, veritatis harum et rerum.
            </p>
          </div>
          <div className="aspect-auto p-8 border border-gray-100 rounded-3xl bg-white dark:bg-gray-800 dark:border-gray-700 shadow-2xl shadow-gray-600/10 dark:shadow-none">
            <div className="flex gap-4">
              <Image
                className="w-12 h-12 rounded-full"
                src={Avatar4}
                alt="user avatar"
                width="200"
                height="200"
                loading="lazy"
              />
              <div>
                <h6 className="text-lg font-medium text-gray-700 dark:text-white">
                  Jane Doe
                </h6>
                <p className="text-sm text-gray-500 dark:text-gray-300">
                  Mobile dev
                </p>
              </div>
            </div>
            <p className="mt-8">
              Lorem ipsum dolor sit amet consectetur, adipisicing elit.
              Architecto laboriosam deleniti aperiam ab veniam sint non cumque
              quis tempore cupiditate. Sint libero voluptas veniam at
              reprehenderit, veritatis harum et rerum.
            </p>
          </div>
          <div className="aspect-auto p-8 border border-gray-100 rounded-3xl bg-white dark:bg-gray-800 dark:border-gray-700 shadow-2xl shadow-gray-600/10 dark:shadow-none">
            <div className="flex gap-4">
              <Image
                className="w-12 h-12 rounded-full"
                src={Avatar5}
                alt="user avatar"
                width="200"
                height="200"
                loading="lazy"
              />
              <div>
                <h6 className="text-lg font-medium text-gray-700 dark:text-white">
                  Andy Doe
                </h6>
                <p className="text-sm text-gray-500 dark:text-gray-300">
                  Manager
                </p>
              </div>
            </div>
            <p className="mt-8">
              {" "}
              Lorem ipsum dolor sit amet consectetur, adipisicing elit.
              Architecto laboriosam deleniti aperiam ab veniam sint non cumque
              quis tempore cupiditate. Sint libero voluptas veniam at
              reprehenderit, veritatis harum et rerum.
            </p>
          </div>
          <div className="aspect-auto p-8 border border-gray-100 rounded-3xl bg-white dark:bg-gray-800 dark:border-gray-700 shadow-2xl shadow-gray-600/10 dark:shadow-none">
            <div className="flex gap-4">
              <Image
                className="w-12 h-12 rounded-full"
                src={Avatar2}
                alt="user avatar"
                width="400"
                height="400"
                loading="lazy"
              />
              <div>
                <h6 className="text-lg font-medium text-gray-700 dark:text-white">
                  Yanndy Doe
                </h6>
                <p className="text-sm text-gray-500 dark:text-gray-300">
                  Mobile dev
                </p>
              </div>
            </div>
            <p className="mt-8">
              Lorem ipsum dolor sit amet consectetur, adipisicing elit.
              Architecto laboriosam deleniti aperiam ab veniam sint non cumque
              quis tempore cupiditate. Sint libero voluptas veniam at
              reprehenderit, veritatis harum et rerum.
            </p>
          </div>
        </div>
      </div>
    </div>
  );
}
```

1. Finally import all the components into one file i.e  **`src/pages/index.js`** 

```jsx
import Features from "@/components/features";
import Header from "@/components/header";
import Footer from "@/components/footer";
import HeroSection from "@/components/heroSection";
import Stats from "@/components/stats";
import Testimonials from "@/components/testimonials";
import GetStarted from "@/components/getStarted";
export default function Home() {
  return (
    <>
      <Header />
      <main className="space-y-40 mb-40">
        <HeroSection />
        <Features />
        <Stats />
        <GetStarted />
        <Testimonials />
      </main>
      <Footer />
    </>
  );
}

```

**Output** : you can run your application using **`npm run dev`** 

![Untitled](https://talktohire.blr1.digitaloceanspaces.com/Untitled.png)

**Step 3 :** Add DSA question data for your application in new folder **`src/data/arrayData.js`** for reference access https://github.com/Ank61/dailyDSA/blob/master/src/data/arrayData.js. Also add **`random.js`** for random DSA Question.

  

**Step 4 :** Create a new folder named **problems** and your folder structure should look like this:

```jsx
├── _app.js
│   ├── _document.js
│   ├── **api**
│   │   └── hello.js
│   ├── index.js
│   └── **problems**
│       ├── **[slug]**
│       │   └── index.js
│       └── index.js
```

**Step 5 :** Add the following code in **`problems/index.js`** file:

```jsx
import Footer from "@/components/footer";
import Header from "@/components/header";
import { useRouter } from "next/router";
import Link from "next/link";
import Table from "@mui/material/Table";
import TableBody from "@mui/material/TableBody";
import TableCell, { tableCellClasses } from "@mui/material/TableCell";
import TableContainer from "@mui/material/TableContainer";
import TableHead from "@mui/material/TableHead";
import Box from "@mui/material/Box";
import TableRow from "@mui/material/TableRow";
import Paper from "@mui/material/Paper";
import { styled } from "@mui/material/styles";
import { arrayData } from "@/data/arrayData";
import TablePagination from "@mui/material/TablePagination";
import { useState } from "react";
import { randomQuestion } from "@/data/randomData";

export default function Problems() {
  const router = useRouter();
  const StyledTableCell = styled(TableCell)(({ theme }) => ({
    [`&.${tableCellClasses.head}`]: {
      backgroundColor: "#ebe6ff",
      color: theme.palette.common.black,
      fontWeight: 600,
      fontSize: 15,
      //   fontFamily: "ui-sans-serif",
    },
    [`&.${tableCellClasses.body}`]: {
      fontSize: 14,
      border: 0,
    },
  }));

  const StyledTableRow = styled(TableRow)(({ theme }) => ({
    "&:nth-of-type(odd)": {
      //   backgroundColor: theme.palette.action.hover,
    },
    // hide last border
    "&:last-child td, &:last-child th": {
      //   border: 0,
    },
  }));

  return (
    <>
      <Header />
      <div className="pt-32 text-gray-600 dark:text-gray-300" id="testimonials">
        <div className="max-w-7xl mx-auto px-6 md:px-12 xl:px-6">
          <div className="text-2xl font-bold font-sans text-violet-600 mb-7">
            Topic Based Questions
          </div>
          <div className="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-4 gap-8">
            <div
              className="group relative bg-white dark:bg-gray-800 border-2  border-violet-100 rounded-2xl transition hover:z-[1] hover:shadow-2xl hover:shadow-violet-600/10 cursor-pointer"
              onClick={() => router.push("/problems/array")}
            >
              <div className="relative space-y-8 py-12 p-8">
                <div className="space-y-2">
                  <h5 className="text-xl font-sans font-semibold text-gray-700 dark:text-white transition group-hover:text-secondary">
                    Arrays
                  </h5>
                </div>
                <Link
                  href="/problems/array"
                  className="flex items-center justify-between group-hover:text-secondary"
                >
                  <span className="text-sm font-sans text-violet-600">
                    Solve{" "}
                  </span>
                  <svg
                    xmlns="http://www.w3.org/2000/svg"
                    viewBox="0 0 24 24"
                    fill="#5e3aee"
                    className="w-5 h-5 -translate-x-4 text-2xl opacity-0 transition duration-300 group-hover:translate-x-0 group-hover:opacity-100"
                  >
                    <path
                      fill-rule="evenodd"
                      d="M12.97 3.97a.75.75 0 011.06 0l7.5 7.5a.75.75 0 010 1.06l-7.5 7.5a.75.75 0 11-1.06-1.06l6.22-6.22H3a.75.75 0 010-1.5h16.19l-6.22-6.22a.75.75 0 010-1.06z"
                      clip-rule="evenodd"
                    />
                  </svg>
                </Link>
              </div>
            </div>
            <div className="group relative bg-white dark:bg-gray-800 border-2 border-violet-100 rounded-2xl transition hover:z-[1] hover:shadow-2xl hover:shadow-violet-600/10 cursor-pointer">
              <div className="relative space-y-8 py-12 p-8">
                <div className="space-y-2">
                  <h5 className="text-xl font-sans font-semibold text-gray-700 dark:text-white transition group-hover:text-secondary">
                    Backtracking
                  </h5>
                </div>
                <Link
                  href="#"
                  className="flex items-center justify-between group-hover:text-secondary"
                >
                  <span className="text-sm font-sans">Solve </span>
                  <svg
                    xmlns="http://www.w3.org/2000/svg"
                    viewBox="0 0 24 24"
                    fill="#5e3aee"
                    className="w-5 h-5 -translate-x-4 text-2xl opacity-0 transition duration-300 group-hover:translate-x-0 group-hover:opacity-100"
                  >
                    <path
                      fill-rule="evenodd"
                      d="M12.97 3.97a.75.75 0 011.06 0l7.5 7.5a.75.75 0 010 1.06l-7.5 7.5a.75.75 0 11-1.06-1.06l6.22-6.22H3a.75.75 0 010-1.5h16.19l-6.22-6.22a.75.75 0 010-1.06z"
                      clip-rule="evenodd"
                    />
                  </svg>
                </Link>
              </div>
            </div>
            <div className="group relative bg-white dark:bg-gray-800 border-2 border-violet-100 rounded-2xl transition hover:z-[1] hover:shadow-2xl hover:shadow-violet-600/10 cursor-pointer">
              <div className="relative space-y-8 py-12 p-8">
                <div className="space-y-2">
                  <h5 className="text-xl font-sans font-semibold text-gray-700 dark:text-white transition group-hover:text-secondary">
                    Dynamic Programing
                  </h5>
                </div>
                <Link
                  href="#"
                  className="flex items-center justify-between group-hover:text-secondary"
                >
                  <span className="text-sm font-sans">Solve </span>
                  <svg
                    xmlns="http://www.w3.org/2000/svg"
                    viewBox="0 0 24 24"
                    fill="#5e3aee"
                    className="w-5 h-5 -translate-x-4 text-2xl opacity-0 transition duration-300 group-hover:translate-x-0 group-hover:opacity-100"
                  >
                    <path
                      fill-rule="evenodd"
                      d="M12.97 3.97a.75.75 0 011.06 0l7.5 7.5a.75.75 0 010 1.06l-7.5 7.5a.75.75 0 11-1.06-1.06l6.22-6.22H3a.75.75 0 010-1.5h16.19l-6.22-6.22a.75.75 0 010-1.06z"
                      clip-rule="evenodd"
                    />
                  </svg>
                </Link>
              </div>
            </div>
            <div className="group relative bg-white dark:bg-gray-800 border-2 border-violet-100 rounded-2xl transition hover:z-[1] hover:shadow-2xl hover:shadow-violet-600/10 cursor-pointer">
              <div className="relative space-y-8 py-12 p-8">
                <div className="space-y-2">
                  <h5 className="text-xl font-sans font-semibold text-gray-700 dark:text-white transition group-hover:text-secondary">
                    String
                  </h5>
                </div>
                <Link
                  href="#"
                  className="flex items-center justify-between group-hover:text-secondary"
                >
                  <span className="text-sm font-sans">Solve </span>
                  <svg
                    xmlns="http://www.w3.org/2000/svg"
                    viewBox="0 0 24 24"
                    fill="#5e3aee"
                    className="w-5 h-5 -translate-x-4 text-2xl opacity-0 transition duration-300 group-hover:translate-x-0 group-hover:opacity-100"
                  >
                    <path
                      fill-rule="evenodd"
                      d="M12.97 3.97a.75.75 0 011.06 0l7.5 7.5a.75.75 0 010 1.06l-7.5 7.5a.75.75 0 11-1.06-1.06l6.22-6.22H3a.75.75 0 010-1.5h16.19l-6.22-6.22a.75.75 0 010-1.06z"
                      clip-rule="evenodd"
                    />
                  </svg>
                </Link>
              </div>
            </div>
            <div className="group relative bg-white dark:bg-gray-800 border-2 border-violet-100 rounded-2xl transition hover:z-[1] hover:shadow-2xl hover:shadow-violet-600/10 cursor-pointer">
              <div className="relative space-y-8 py-12 p-8">
                <div className="space-y-2">
                  <h5 className="text-xl font-sans font-semibold text-gray-700 dark:text-white transition group-hover:text-secondary">
                    Hashing
                  </h5>
                </div>
                <Link
                  href="#"
                  className="flex items-center justify-between group-hover:text-secondary"
                >
                  <span className="text-sm font-sans">Solve </span>
                  <svg
                    xmlns="http://www.w3.org/2000/svg"
                    viewBox="0 0 24 24"
                    fill="#5e3aee"
                    className="w-5 h-5 -translate-x-4 text-2xl opacity-0 transition duration-300 group-hover:translate-x-0 group-hover:opacity-100"
                  >
                    <path
                      fill-rule="evenodd"
                      d="M12.97 3.97a.75.75 0 011.06 0l7.5 7.5a.75.75 0 010 1.06l-7.5 7.5a.75.75 0 11-1.06-1.06l6.22-6.22H3a.75.75 0 010-1.5h16.19l-6.22-6.22a.75.75 0 010-1.06z"
                      clip-rule="evenodd"
                    />
                  </svg>
                </Link>
              </div>
            </div>
            <div className="group relative bg-white dark:bg-gray-800 border-2 border-violet-100 rounded-2xl transition hover:z-[1] hover:shadow-2xl hover:shadow-violet-600/10 cursor-pointer">
              <div className="relative space-y-8 py-12 p-8">
                <div className="space-y-2">
                  <h5 className="text-xl font-sans font-semibold text-gray-700 dark:text-white transition group-hover:text-secondary">
                    Stacks & Queues
                  </h5>
                </div>
                <Link
                  href="#"
                  className="flex items-center justify-between group-hover:text-secondary"
                >
                  <span className="text-sm font-sans">Solve </span>
                  <svg
                    xmlns="http://www.w3.org/2000/svg"
                    viewBox="0 0 24 24"
                    fill="#5e3aee"
                    className="w-5 h-5 -translate-x-4 text-2xl opacity-0 transition duration-300 group-hover:translate-x-0 group-hover:opacity-100"
                  >
                    <path
                      fill-rule="evenodd"
                      d="M12.97 3.97a.75.75 0 011.06 0l7.5 7.5a.75.75 0 010 1.06l-7.5 7.5a.75.75 0 11-1.06-1.06l6.22-6.22H3a.75.75 0 010-1.5h16.19l-6.22-6.22a.75.75 0 010-1.06z"
                      clip-rule="evenodd"
                    />
                  </svg>
                </Link>
              </div>
            </div>
            <div className="group relative bg-white dark:bg-gray-800 border-2 border-violet-100 rounded-2xl transition hover:z-[1] hover:shadow-2xl hover:shadow-violet-600/10 cursor-pointer">
              <div className="relative space-y-8 py-12 p-8">
                <div className="space-y-2">
                  <h5 className="text-xl font-sans font-semibold text-gray-700 dark:text-white transition group-hover:text-secondary">
                    Trees
                  </h5>
                </div>
                <Link
                  href="#"
                  className="flex items-center justify-between group-hover:text-secondary"
                >
                  <span className="text-sm font-sans">Solve </span>
                  <svg
                    xmlns="http://www.w3.org/2000/svg"
                    viewBox="0 0 24 24"
                    fill="#5e3aee"
                    className="w-5 h-5 -translate-x-4 text-2xl opacity-0 transition duration-300 group-hover:translate-x-0 group-hover:opacity-100"
                  >
                    <path
                      fill-rule="evenodd"
                      d="M12.97 3.97a.75.75 0 011.06 0l7.5 7.5a.75.75 0 010 1.06l-7.5 7.5a.75.75 0 11-1.06-1.06l6.22-6.22H3a.75.75 0 010-1.5h16.19l-6.22-6.22a.75.75 0 010-1.06z"
                      clip-rule="evenodd"
                    />
                  </svg>
                </Link>
              </div>
            </div>
            <div className="group relative bg-white dark:bg-gray-800 border-2 border-violet-100 rounded-2xl transition hover:z-[1] hover:shadow-2xl hover:shadow-violet-600/10 cursor-pointer">
              <div className="relative space-y-8 py-12 p-8">
                <div className="space-y-2">
                  <h5 className="text-xl font-sans font-semibold text-gray-700 dark:text-white transition group-hover:text-secondary">
                    Graphs
                  </h5>
                </div>
                <Link
                  href="#"
                  className="flex items-center justify-between group-hover:text-secondary"
                >
                  <span className="text-sm font-sans">Solve </span>
                  <svg
                    xmlns="http://www.w3.org/2000/svg"
                    viewBox="0 0 24 24"
                    fill="#5e3aee"
                    className="w-5 h-5 -translate-x-4 text-2xl opacity-0 transition duration-300 group-hover:translate-x-0 group-hover:opacity-100"
                  >
                    <path
                      fill-rule="evenodd"
                      d="M12.97 3.97a.75.75 0 011.06 0l7.5 7.5a.75.75 0 010 1.06l-7.5 7.5a.75.75 0 11-1.06-1.06l6.22-6.22H3a.75.75 0 010-1.5h16.19l-6.22-6.22a.75.75 0 010-1.06z"
                      clip-rule="evenodd"
                    />
                  </svg>
                </Link>
              </div>
            </div>
            <div className="group relative bg-white dark:bg-gray-800 border-2 border-violet-100 rounded-2xl transition hover:z-[1] hover:shadow-2xl hover:shadow-violet-600/10 cursor-pointer">
              <div className="relative space-y-8 py-12 p-8">
                <div className="space-y-2">
                  <h5 className="text-xl font-sans font-semibold text-gray-700 dark:text-white transition group-hover:text-secondary">
                    Greedy
                  </h5>
                </div>
                <Link
                  href="#"
                  className="flex items-center justify-between group-hover:text-secondary"
                >
                  <span className="text-sm font-sans">Solve </span>
                  <svg
                    xmlns="http://www.w3.org/2000/svg"
                    viewBox="0 0 24 24"
                    fill="#5e3aee"
                    className="w-5 h-5 -translate-x-4 text-2xl opacity-0 transition duration-300 group-hover:translate-x-0 group-hover:opacity-100"
                  >
                    <path
                      fill-rule="evenodd"
                      d="M12.97 3.97a.75.75 0 011.06 0l7.5 7.5a.75.75 0 010 1.06l-7.5 7.5a.75.75 0 11-1.06-1.06l6.22-6.22H3a.75.75 0 010-1.5h16.19l-6.22-6.22a.75.75 0 010-1.06z"
                      clip-rule="evenodd"
                    />
                  </svg>
                </Link>
              </div>
            </div>
          </div>
          <div className="text-2xl font-bold font-sans text-violet-600 mb-3 mt-12">
            All Topics
          </div>
          <TableContainer
            component={Paper}
            className="lg:w-8/12 lg:mx-auto mt-14 border-0 flex justify-center item-center"
          >
            <Table sx={{ minWidth: 400 }} aria-label="customized table">
              <TableHead>
                <TableRow>
                  {/* <StyledTableCell>LeetCode Id</StyledTableCell> */}
                  <StyledTableCell align="left">Title</StyledTableCell>
                  <StyledTableCell align="left">Acceptance</StyledTableCell>
                  <StyledTableCell align="left">Difficulty</StyledTableCell>
                  {/* <StyledTableCell align="left">Status</StyledTableCell> */}
                </TableRow>
              </TableHead>
              <TableBody>
                {randomQuestion.questions.map((row, index) => (
                  <StyledTableRow key={index}>
                    {/* <StyledTableCell component="th" scope="row">
                      {row.questionId}
                    </StyledTableCell> */}
                    <StyledTableCell
                      align="left"
                      className="hover:text-violet-700 cursor-pointer"
                      onClick={() =>
                        window.open(
                          `https://leetcode.com/problems/${row.titleSlug}/`,
                          "_blank",
                          "noopener,noreferrer"
                        )
                      }
                    >
                      {row.title}
                    </StyledTableCell>
                    <StyledTableCell align="">{row.acRate}</StyledTableCell>
                    <StyledTableCell align="center">
                      {row.difficulty === "Easy" ? (
                        <div className="pl-3 pr-3 font-semibold	 pt-1 pb-1 w-fit bg-green-100 text-green-500 text-xs rounded-2xl">
                          Easy
                        </div>
                      ) : row.difficulty === "Hard" ? (
                        <div className="pl-3 pr-3 font-semibold pt-1 pb-1 w-fit bg-red-100 text-red-500 text-xs rounded-2xl">
                          Hard
                        </div>
                      ) : (
                        <div className="pl-3 pr-3  font-semibold pt-1 pb-1 w-fit bg-yellow-100 text-yellow-500 text-xs rounded-2xl">
                          Medium
                        </div>
                      )}
                    </StyledTableCell>
                    {/* <StyledTableCell align="center">
                      <input type="checkbox" className="" />
                    </StyledTableCell> */}
                  </StyledTableRow>
                ))}
              </TableBody>
            </Table>
          </TableContainer>
          {/* <TablePagination
            className="lg:w-8/12 lg:mx-auto"
            rowsPerPageOptions={[5, 10, 25]}
            component="div"
            count={arrayData.length}
            rowsPerPage={rowsPerPage}
            page={page}
            onPageChange={handleChangePage}
            onRowsPerPageChange={handleChangeRowsPerPage}
          /> */}
        </div>
      </div>
      <Footer />
    </>
  );
}
```

**Step 6 : Test** the problems page through this route : [http://localhost:3000/problems](http://localhost:3000/problems)

![Untitled](https://talktohire.blr1.digitaloceanspaces.com/Untitled_1.png)

![Untitled](https://talktohire.blr1.digitaloceanspaces.com/Untitled_2.png)

**Step 7 :** Add the following code in the **`src/pages/[slug]/index.js`** This page will be for the arrays section.

```jsx
import { useRouter } from "next/router";
import Header from "@/components/header";
import Footer from "@/components/footer";
import Bookmark from "../../../images/companies/bookmark.png";
import Image from "next/image";
import Table from "@mui/material/Table";
import TableBody from "@mui/material/TableBody";
import TableCell, { tableCellClasses } from "@mui/material/TableCell";
import TableContainer from "@mui/material/TableContainer";
import TableHead from "@mui/material/TableHead";
import Box from "@mui/material/Box";
import TableRow from "@mui/material/TableRow";
import Paper from "@mui/material/Paper";
import { styled } from "@mui/material/styles";
import { arrayData } from "@/data/arrayData";
import TablePagination from "@mui/material/TablePagination";
import { useState } from "react";

export default function ProblemName() {
  const router = useRouter();
  const [rowsPerPage, setRowsPerPage] = useState(20);
  const [page, setPage] = useState(0);

  const handleChangePage = (event, newPage) => {
    setPage(newPage);
  };

  const handleChangeRowsPerPage = (event) => {
    setRowsPerPage(parseInt(event.target.value, 10));
    setPage(0);
  };

  const StyledTableCell = styled(TableCell)(({ theme }) => ({
    [`&.${tableCellClasses.head}`]: {
      backgroundColor: "#ebe6ff",
      color: theme.palette.common.black,
      fontWeight: 600,
      fontSize: 15,
      //   fontFamily: "ui-sans-serif",
    },
    [`&.${tableCellClasses.body}`]: {
      fontSize: 14,
      border: 0,
    },
  }));

  const StyledTableRow = styled(TableRow)(({ theme }) => ({
    "&:nth-of-type(odd)": {
      //   backgroundColor: theme.palette.action.hover,
    },
    // hide last border
    "&:last-child td, &:last-child th": {
      //   border: 0,
    },
  }));

  const startIndex = page * rowsPerPage;
  const endIndex = startIndex + rowsPerPage;
  const displayedData = arrayData.slice(startIndex, endIndex);

  return (
    <>
      <Header />
      <div className="pt-28 text-gray-600 dark:text-gray-300" id="testimonials">
        <div className="max-w-7xl mx-auto px-6 md:px-12 xl:px-6 ">
          <div className="flex flex-row ">
            <Image src={Bookmark} alt="logo" className="w-7 h-6 mt-1.5" />
            <p className="ml-2 text-2xl font-medium font-sans ">
              {router.query.slug &&
                router.query.slug.charAt(0).toUpperCase() +
                  router.query.slug.slice(1)}
            </p>
            <div></div>
          </div>
          <TableContainer
            component={Paper}
            className="lg:w-8/12 lg:mx-auto mt-14 border-0 flex justify-center item-center"
          >
            <Table sx={{ minWidth: 400 }} aria-label="customized table">
              <TableHead>
                <TableRow>
                  <StyledTableCell>LeetCode Id</StyledTableCell>
                  <StyledTableCell align="left">Title</StyledTableCell>
                  <StyledTableCell align="left">Acceptance</StyledTableCell>
                  <StyledTableCell align="left">Difficulty</StyledTableCell>
                  <StyledTableCell align="left">Status</StyledTableCell>
                </TableRow>
              </TableHead>
              <TableBody>
                {displayedData.map((row, index) => (
                  <StyledTableRow key={index}>
                    <StyledTableCell component="th" scope="row">
                      {row.questionId}
                    </StyledTableCell>
                    <StyledTableCell
                      align="left"
                      className="hover:text-violet-700 cursor-pointer"
                      onClick={() =>
                        window.open(
                          `https://leetcode.com/problems/${row.titleSlug}/`,
                          "_blank",
                          "noopener,noreferrer"
                        )
                      }
                    >
                      {row.title}
                    </StyledTableCell>
                    <StyledTableCell align="">{row.acRate}</StyledTableCell>
                    <StyledTableCell align="center">
                      {row.difficulty === "Easy" ? (
                        <div className="pl-3 pr-3 font-semibold	 pt-1 pb-1 w-fit bg-green-100 text-green-500 text-xs rounded-2xl">
                          Easy
                        </div>
                      ) : row.difficulty === "Hard" ? (
                        <div className="pl-3 pr-3 font-semibold pt-1 pb-1 w-fit bg-red-100 text-red-500 text-xs rounded-2xl">
                          Hard
                        </div>
                      ) : (
                        <div className="pl-3 pr-3  font-semibold pt-1 pb-1 w-fit bg-yellow-100 text-yellow-500 text-xs rounded-2xl">
                          Medium
                        </div>
                      )}
                    </StyledTableCell>
                    <StyledTableCell align="center">
                      <input type="checkbox" className=""/>
                    </StyledTableCell>
                  </StyledTableRow>
                ))}
              </TableBody>
            </Table>
          </TableContainer>
          <TablePagination
          className="lg:w-8/12 lg:mx-auto"
            rowsPerPageOptions={[5, 10, 25]}
            component="div"
            count={arrayData.length}
            rowsPerPage={rowsPerPage}
            page={page}
            onPageChange={handleChangePage}
            onRowsPerPageChange={handleChangeRowsPerPage}
          />
        </div>
      </div>
      <Footer />
    </>
  );
}
```

Step 8 : If you run the link [http://localhost:3000/problems/array](http://localhost:3000/problems/array) you should be able to see the output:

![Untitled](https://talktohire.blr1.digitaloceanspaces.com/Untitled.png)

You can now create all the data structure section just like arrays. Hope this article helps you.

### **Happy Coding!**