# How to create Minimalistic Portfolio Template?

A portfolio for developers is a crucial tool that showcases their skills, experience, and expertise through a curated collection of projects and code samples. It goes beyond the traditional resume by providing tangible proof of what a developer can achieve, highlighting unique projects, innovative solutions, and command of relevant technologies. By regularly updating and maintaining a user-friendly portfolio, developers can attract new opportunities, connect with industry professionals, and effectively communicate their professional journey.

![Untitled](https://talktohire.blr1.digitaloceanspaces.com/Untitled.png)

[port.mp4](https://drive.google.com/file/d/1F6H-DjBTRaAC_cQp-Ec9kjOwe4Vvn5hK/view?usp=sharing)

Demo  : [Portfolio](https://portfolio-alpha-sand-49.vercel.app/)

## **Setting Up the Next.js Project**

Let's start by setting up a new Next project. Open your terminal and execute the following commands:

```jsx
npx create-next-app portfolio

✔ Would you like to use TypeScript with this project? … No
✔ Would you like to use ESLint with this project? … Yes
✔ Would you like to use Tailwind CSS with this project? … Yes
✔ Would you like to use `src/` directory with this project? … No
✔ Use App Router (recommended)? … Yes
✔ Would you like to customize the default import alias? … No
```

This will create a new Next project in a directory named **`portfolio`**. Next, open the project in your preferred code editor.

```jsx
cd **portfolio**
```

## **Installing Dependencies**

Next, we need to install the required dependencies for our project. We are using framer motion here to animate out HTML elements. In the terminal, navigate to the project directory and run the following command:

```jsx
npm i **framer-motion**
```

**Step 1.** Remove all the default application code from **`/app/page.js`** and **`app/globals.css`.** Both file should like this :

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

**Step 2.** Create folder structure for app folder similar to this :

```jsx
**.app**
├── **blogs**      
│   └── page.js
├── **contact**    
│   └── page.js
├── favicon.ico
├── globals.css
├── layout.js  
├── **newsletter** 
│   └── page.js
├── page.js    
└── **uses**
    └── page.js
```

Your overall structure should look like this :

![Untitled](https://talktohire.blr1.digitaloceanspaces.com/Untitled_1.png)

**Step 3 : Create component files :** 

In the **`/components`**and the following code **:**

- social.js file is used to social navigation buttons
- profile.js file is used to display profile image name and designation.
- navbar.js file is used to show the navigation tabs.
- aboutsection.js file is used to show the current employee information.
- blogsection.js file is used to display blogs.
- userSection.js file is used in user navigation for displaying user specific information

```jsx
**social.js**
import Image from "next/image";
import XLogo from "../images/x.png";
import linkedLogo from "../images/linkedin.png";
import githubLogo from "../images/github.png";

export default function Social() {
  return (
    <div className="flex flex-row justify-between items-center mt-6">
      <div className="flex flex-row gap-x-3">
        <a target="_blank" rel="noreferrer" href="#">
          <Image
            alt="Twitter"
            loading="lazy"
            width="18"
            height="18"
            decoding="async"
            data-nimg="1"
            src={XLogo}
            style={{color: "transparent"}}
            className="mt-1 "
          />
        </a>
        <a target="_blank" rel="noreferrer" href="#">
          <Image
            alt="Github"
            loading="lazy"
            width="22"
            height="22"
            decoding="async"
            data-nimg="1"
            src={githubLogo}
            style={{color: "transparent"}}
            className="ml-1"
          />
        </a>
        <a target="_blank" rel="noreferrer" href="#">
          <Image
            alt="Linkedin"
            loading="lazy"
            width="22"
            height="22"
            decoding="async"
            data-nimg="1"
            src={linkedLogo}
            style={{color: "transparent"}}
            className="mt-0.5 ml-1"
          />
        </a>
      </div>
    </div>
  );
}

**profile.js**
import Image from "next/image";
import ProfileImage from "../images/image.png";

export default function Profile (){
    return(
        <div>
            <Image width="120" height="200" decoding="async" src={ProfileImage} loading="lazy"  alt="Profile" className="rounded-full object-fit w-[50px] h-[50px]"/>
            <h1 className="font-medium text-gray-900 mt-2 text-xl font-heading">Saige Fuentes</h1>
            <p className="text-gray-500">Software Engineer</p>
        </div>
    )
} 

**navbar.js**
"use client";
import { usePathname } from "next/navigation";
import { motion } from "framer-motion";
import Link from "next/link";

export default function Navbar() {
  const router = usePathname();
  console.log(router);
  const navItems = [
    { href: "/", label: "About" },
    { href: "/blogs", label: "Blogs" },
    { href: "/uses", label: "Uses" },
    { href: "/newsletter", label: "Newsletter" },
    { href: "/contact", label: "Contact" },
  ];

  return (
    <div className="flex items-center flex-wrap gap-2 mb-8">
      {navItems.map((item) => (
        <Link key={item.href} href={item.href}>
          <motion.div
            whileTap={{ scale: 0.2 }}
            transition={{ ease: "easeOut", duration: 1 }}
            className={`relative text-sm transition-colors px-2 py-1 rounded-md ${
              router === item.href ? "text-white" : "text-black"
            }`}
          >
            <span className="relative z-10">{item.label}</span>
            <motion.span
              className={`absolute inset-0 z-0 rounded-md ${
                router === item.href ? "bg-gray-900" : "bg-white"
              }`}
              initial={{ opacity: 0 }}
              animate={{ opacity: 1 }}
              transition={{ ease: "easeOut", duration: 0.5 }}
            ></motion.span>
          </motion.div>
        </Link>
      ))}
    </div>
  );
}

**common.js**
import Navbar from "./navbar";
import Profile from "./profile";
import Social from "./social";

export default function Common() {
    return (
        <div className="w-full max-w-3xl">
            <Profile />
            <Social />
            <div className="border-b w-full my-8"></div>
            <Navbar />
        </div>
    )
} 

**blogSection.js**
"use client"
import { motion } from "framer-motion";
import { useEffect, useState } from "react";
export default function BlogSection() {
    const [isLargeScreen, setIsLargeScreen] = useState(false);
    const [isOpen, setIsOpen] = useState(false);
    const [selected, setSelected] = useState('Share')

    useEffect(() => {
        const checkScreenSize = () => {
            setIsLargeScreen(window.innerWidth >= 1024); // Adjust breakpoint as needed
        };

        checkScreenSize();
        window.addEventListener("resize", checkScreenSize);

        return () => {
            window.removeEventListener("resize", checkScreenSize);
        };
    }, []);

    const toggleMenu = (e) => {
        setIsOpen(!isOpen);
    };
    return (<div className="mt-4 mb-auto">
        <div className="mb-5">
            <div
                id="speed-dial-menu-default"
                className="group flex ml-2 items-center space-x-2"
            >
                <button
                    type="button"
                    onClick={(e) => toggleMenu(e)}
                    // onMouseLeave={(e) => toggleMenu(e)}
                    aria-controls="speed-dial-menu-default"
                    aria-expanded={isOpen}
                    style={{ outline: 'none' }}
                    className="flex mr-5 text-white bg-slate-800 rounded-lg pl-2 pr-2 pt-1 pb-1 text-sm hover:bg-slate-900 dark:bg-blue-600 dark:hover:bg-blue-700 focus:outline-none focus:ring-0"
                >
                    Filters
                    <svg
                        className={`w-3.5 h-3.5 ml-1.5 mt-1 mb-0.5 transition-transform  ${isOpen ? 'rotate-45' : ''}`}
                        aria-hidden="true"
                        xmlns="http://www.w3.org/2000/svg"
                        fill="none"
                        viewBox="0 0 18 18"
                    >
                        <path
                            stroke="currentColor"
                            strokeLinecap="round"
                            strokeLinejoin="round"
                            strokeWidth="2"
                            d="M9 1v16M1 9h16"
                        />
                    </svg>
                </button>
                {isOpen &&
                    [
                        {
                            tooltip: "Share",
                            icon: "M14.419 10.581a3.564 ...",
                            ariaLabel: "Share",
                        },
                        {
                            tooltip: "Print",
                            icon: "M5 20h10a1 1 ...",
                            ariaLabel: "Print",
                        },
                        {
                            tooltip: "Download",
                            icon: "M14.707 7.793a1 1 ...",
                            ariaLabel: "Download",
                        },
                        {
                            tooltip: "Copy",
                            icon: "M5 9V4.13a2.96 ...",
                            ariaLabel: "Copy",
                        },
                    ].map((item, index) => (
                        <motion.div
                            key={index}
                            initial={{ opacity: 0, x: -10 }}
                            animate={{ opacity: 1, x: 0, transition: { type: "easeOut", duration: 0.9 } }}
                            exit={{ opacity: 0, x: 10, transition: { type: "easeOut", duration: 0.5 } }}
                            className="relative"
                        >
                            <button
                                type="button"
                                data-tooltip-target={`tooltip-${item.tooltip.toLowerCase()}`}
                                data-tooltip-placement="top"
                                onClick={() => setSelected(item.ariaLabel)}
                                className={`${item.ariaLabel === selected ? 'bg-slate-800 text-white' : 'bg-white text-black'} flex rounded-lg pl-2 pr-2 pt-1 pb-1 text-sm outline-none hover:bg-slate-900 hover:text-white dark:bg-blue-600 dark:hover:bg-blue-700 focus:ring-0 focus:ring-blue-300 focus:outline-none dark:focus:ring-blue-800`}
                            >{item.ariaLabel}
                            </button>
                            <div
                                id={`tooltip-${item.tooltip.toLowerCase()}`}
                                role="tooltip"
                                className="absolute z-10 invisible inline-block w-auto px-3 py-2 text-sm font-medium text-white transition-opacity duration-300 bg-gray-900 rounded-lg shadow-sm opacity-0 tooltip dark:bg-gray-700"
                            >
                                {item.tooltip}
                                <div className="tooltip-arrow" data-popper-arrow></div>
                            </div>
                        </motion.div>
                    ))}
            </div>
        </div>

        <div className="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-2 gap-6 mb-auto mt-2">
            <motion.div
                initial={{ opacity: 0 }}
                animate={isLargeScreen ? { x: 6, opacity: 1 } : { opacity: 1 }}
                transition={{ type: "spring" }}
                className=" max-w-sm bg-white border border-gray-200 rounded-lg shadow dark:bg-gray-800 dark:border-gray-700 hover:shadow-lg cursor-pointer transition-shadow"
            >
                <div className="p-5">
                    <a href="#">
                        <h5 className="mb-2 text-lg font-bold tracking-tight text-gray-900 dark:text-white">
                            Noteworthy technology acquisitions 2021
                        </h5>
                    </a>
                    <p className="mb-3 font-normal text-sm text-gray-700 dark:text-gray-400">
                        Here are the biggest enterprise technology acquisitions of 2021 so
                        far, in reverse chronological order.
                    </p>
                    <button className="inline-flex items-center pl-2 pr-2 pt-1 pb-1 text-sm  text-center text-white bg-slate-800 rounded-lg  focus:ring-4 focus:outline-none ">
                        Read more
                        <svg
                            className="rtl:rotate-180 w-3 h-3.5 ms-2"
                            ariaHidden="true"
                            xmlns="http://www.w3.org/2000/svg"
                            fill="none"
                            viewBox="0 0 14 10"
                        >
                            <path
                                stroke="currentColor"
                                strokeLinecap="round"
                                strokeLinejoin="round"
                                stroke-width="2"
                                d="M1 5h12m0 0L9 1m4 4L9 9"
                            />
                        </svg>
                    </button>
                </div>
            </motion.div>

            <motion.div
                initial={{ opacity: 0 }}
                animate={isLargeScreen ? { x: -6, opacity: 1 } : { opacity: 1 }}
                transition={{ type: "spring" }}
                className=" max-w-sm bg-white border border-gray-200 rounded-lg shadow dark:bg-gray-800 dark:border-gray-700 hover:shadow-lg cursor-pointer transition-shadow"
            >
                <div className="p-5">
                    <a href="#">
                        <h5 className="mb-2 text-lg font-bold tracking-tight text-gray-900 dark:text-white">
                            Noteworthy technology acquisitions 2021
                        </h5>
                    </a>
                    <p className="mb-3 font-normal text-sm text-gray-700 dark:text-gray-400">
                        Here are the biggest enterprise technology acquisitions of 2021 so
                        far, in reverse chronological order.
                    </p>
                    <button className="inline-flex items-center pl-2 pr-2 pt-1 pb-1 text-sm  text-center text-white bg-slate-800 rounded-lg  focus:ring-4 focus:outline-none ">
                        Read more
                        <svg
                            className="rtl:rotate-180 w-3 h-3.5 ms-2"
                            ariaHidden="true"
                            xmlns="http://www.w3.org/2000/svg"
                            fill="none"
                            viewBox="0 0 14 10"
                        >
                            <path
                                stroke="currentColor"
                                stroke-linecap="round"
                                stroke-linejoin="round"
                                stroke-width="2"
                                d="M1 5h12m0 0L9 1m4 4L9 9"
                            />
                        </svg>
                    </button>
                </div>
            </motion.div>

            <motion.div
                initial={{ opacity: 0 }}
                animate={{ x: 6, opacity: 1 }}
                transition={{ type: "spring" }}
                className=" max-w-sm bg-white border border-gray-200 rounded-lg shadow dark:bg-gray-800 dark:border-gray-700 hover:shadow-lg cursor-pointer transition-shadow"
            >
                <div className="p-5">
                    <a href="#">
                        <h5 className="mb-2 text-lg font-bold tracking-tight text-gray-900 dark:text-white">
                            Noteworthy technology acquisitions 2021
                        </h5>
                    </a>
                    <p className="mb-3 font-normal text-sm text-gray-700 dark:text-gray-400">
                        Here are the biggest enterprise technology acquisitions of 2021 so
                        far, in reverse chronological order.
                    </p>
                    <button className="inline-flex items-center pl-2 pr-2 pt-1 pb-1 text-sm  text-center text-white bg-slate-800 rounded-lg  focus:ring-4 focus:outline-none ">
                        Read more
                        <svg
                            className="rtl:rotate-180 w-3 h-3.5 ms-2"
                            ariaHidden="true"
                            xmlns="http://www.w3.org/2000/svg"
                            fill="none"
                            viewBox="0 0 14 10"
                        >
                            <path
                                stroke="currentColor"
                                stroke-linecap="round"
                                stroke-linejoin="round"
                                stroke-width="2"
                                d="M1 5h12m0 0L9 1m4 4L9 9"
                            />
                        </svg>
                    </button>
                </div>
            </motion.div>

            <motion.div
                initial={{ opacity: 0 }}
                animate={isLargeScreen ? { x: -6, opacity: 1 } : { opacity: 1 }}
                transition={{ type: "spring" }}
                className=" max-w-sm bg-white border border-gray-200 rounded-lg shadow dark:bg-gray-800 dark:border-gray-700 hover:shadow-lg cursor-pointer transition-shadow"
            >
                <div className="p-5">
                    <a href="#">
                        <h5 className="mb-2 text-lg font-bold tracking-tight text-gray-900 dark:text-white">
                            Noteworthy technology acquisitions 2021
                        </h5>
                    </a>
                    <p className="mb-3 font-normal text-sm text-gray-700 dark:text-gray-400">
                        Here are the biggest enterprise technology acquisitions of 2021 so
                        far, in reverse chronological order.
                    </p>
                    <button className="inline-flex items-center pl-2 pr-2 pt-1 pb-1 text-sm  text-center text-white bg-slate-800 rounded-lg  focus:ring-4 focus:outline-none ">
                        Read more
                        <svg
                            className="rtl:rotate-180 w-3 h-3.5 ms-2"
                            ariaHidden="true"
                            xmlns="http://www.w3.org/2000/svg"
                            fill="none"
                            viewBox="0 0 14 10"
                        >
                            <path
                                stroke="currentColor"
                                stroke-linecap="round"
                                stroke-linejoin="round"
                                stroke-width="2"
                                d="M1 5h12m0 0L9 1m4 4L9 9"
                            />
                        </svg>
                    </button>
                </div>
            </motion.div>
        </div>
    </div>
    )
}
**aboutSection.js**
"use client"
import { AnimatePresence, motion } from "framer-motion";
import { useEffect, useState } from "react";
import close from "../images/close.svg"
import Image from "next/image";
import md from "../images/md.svg";
import next from "../images/next.svg";
import node from "../images/node.svg";
import react from "../images/react.svg";
import stripe from "../images/stripe.svg";
import supabase from "../images/supabase.svg";
import js from "../images/js.svg";
import Link from "next/link";

export default function AboutSection() {
    const [isLargeScreen, setIsLargeScreen] = useState(false);
    const [selectedId, setSelectedId] = useState(null)

    useEffect(() => {
        const checkScreenSize = () => {
            setIsLargeScreen(window.innerWidth >= 1024); // Adjust breakpoint as needed
        };

        checkScreenSize();
        window.addEventListener("resize", checkScreenSize);

        return () => {
            window.removeEventListener("resize", checkScreenSize);
        };
    }, []);

    const projects = [
        {
            id: 1,
            title: 'Meditations App',
            description: 'An app for organizing book notes and personal reflections.',
        },
        {
            id: 2,
            title: 'Personal Blog',
            description: 'A platform for sharing thoughts and insights on various topics.',
        },
        {
            id: 3,
            title: 'Creative Portfolio',
            description: 'Showcase your work and achievements in a visually appealing portfolio.',
        },
        {
            id: 4,
            title: 'Quote Vault',
            description: 'Curate and store your favorite quotes for inspiration and motivation.',
        }
    ];

    return (
        <motion.div
            initial={{ opacity: 0, y: 10 }}
            animate={{ opacity: 1, y: 0 }}
            transition={{ type: "spring", stiffness: 300 }}
            className="max-w-3xl mb-auto">
            <div >
                <p className="text-gray-500">
                    Transform your projects with a Full Stack Developer skilled in
                    JavaScript, React.js, Node.js, and Next.js. Let{"'"}s create
                    something exceptional!
                </p>
            </div>
            <div className="mt-16">
                <h1 className="font-medium text-gray-900 mb-4 text-lg">Experience</h1>
                <ol className="relative border-s border-gray-200">
                    <li className="mb-10 ms-4">
                        <div className="flex flex-row items-center gap-2">
                            <div className="absolute w-3 h-3 bg-gray-500 rounded-full mt-1.5 -start-1.5 border "></div>
                            <div className="text-md font-medium text-gray-900">
                                Senior Software Engineer
                            </div>
                            <div className="inline-block ml-2  text-xs rounded-full px-1 py-0  text-gray-500">
                                Present
                            </div>
                        </div>
                        <div className="mb-4 text-sm font-normal text-gray-500">
                            Google India
                        </div>
                    </li>
                    <li className="mb-10 ms-4">
                        <div className="absolute w-3 h-3 bg-gray-200 rounded-full mt-1.5 -start-1.5 border "></div>
                        <div className="text-md font-medium text-gray-900">
                            Software Engineer
                        </div>
                        <div className="mb-4 text-sm font-normal text-gray-500">
                            Microsoft
                        </div>
                    </li>
                </ol>
            </div>
            <div class="mt-16">
                <h1 class="font-medium text-gray-900 mb-4 text-lg">Projects</h1><div class="relative grid grid-cols-1 md:grid-cols-2 lg:grid-cols-2 w-full gap-x-3 gap-y-2">
                    {projects.map((project) => (
                        <motion.div
                            whileHover={{ boxShadow: '0px 10px 20px rgba(0, 0, 0, 0.4)' }}
                            transition={{ duration: 0.3 }}
                            key={project.id}
                            layoutId={project.id}
                            onClick={() => setSelectedId(project.id)}
                            className="  mt-5 cursor-pointer p-4 rounded-lg"
                        >
                            <div className="text-sm mb-1 font-medium text-gray-900">
                                {project.title}
                            </div>
                            <div className="max-w-xs text-sm font-normal text-gray-500">
                                {project.description}
                            </div>
                        </motion.div>
                    ))}
                    {isLargeScreen ? <AnimatePresence >
                        {selectedId && projects.filter(project => project.id === selectedId).map(project =>
                            <motion.div
                                animate={{ boxShadow: '0px 5px 13px rgba(0, 0, 0, 0.4)' }}
                                layoutId={selectedId}
                                className="absolute top-[29%] left-[9%] shadow-lg p-6 bg-white rounded-md"
                            >
                                <div className="flex justify-between">
                                    <motion.h2 className="text-xl font-medium">{project.title}</motion.h2>
                                    <motion.button onClick={() => setSelectedId(null)} className="border p-1 rounded-lg  hover:bg-slate-100">
                                        <Image src={close} width={15} height={15} alt="close" />
                                    </motion.button>
                                </div>
                                <motion.h5 className="mt-2 text-sm">{project.description}</motion.h5>
                                <motion.h5 className="mt-5 text-sm font-bold">Technologies Used</motion.h5>
                                <motion.div className="flex justify-between items-center mx-5">
                                    <Image src={js} height={50} width={50} alt="Tech" />
                                    <Image src={react} height={50} width={50} alt="Tech" />
                                    <Image src={next} height={50} width={50} alt="Tech" />
                                    <Image src={md} height={120} width={120} alt="Tech" />
                                </motion.div>
                                <motion.div className="flex justify-between items-center mx-10">
                                    <Image src={node} height={50} width={50} alt="Tech" />
                                    <Image src={stripe} height={70} width={70} alt="Tech" />
                                    <Image src={supabase} height={150} width={150} alt="supabase" />
                                </motion.div>
                                <div className="flex justify-center items-center mx-auto mt-4">
                                    <Link className="bg-gray-900 hover:bg-gray-700 text-xs text-white px-4 py-2 rounded-md disabled:opacity-80 mr-6" href={'/'}>Demo</Link>
                                    <Link className="bg-gray-900 hover:bg-gray-700 text-xs text-white px-4 py-2 rounded-md disabled:opacity-80" href={'/'}>Github</Link>
                                </div>
                            </motion.div>
                        )}
                    </AnimatePresence> : null}
                </div>
            </div>
            <div class="mt-16"><div class="mt-1"><h1 class="font-medium text-gray-900 mb-4 text-lg">Open Source Contributions</h1>
                <p class="text-gray-500 mt-4">As a dedicated JavaScript developer, I actively contribute to various projects on GitHub. My contributions reflect my passion for open-source software and my commitment to continuous learning and community collaboration. </p>
            </div></div>

            <div class="mt-16"><div class="mt-1"><h1 class="font-medium text-gray-900 mb-4 text-lg">Meditations Newsletter</h1>
                <p class="text-gray-500 mt-4">Stay ahead of the curve with my monthly newsletter called Meditations. Receive valuable insights on the latest trends, techniques, and tools in web development and design.</p>
                <form class="relative">
                    <input class="border w-full mt-4 px-2 py-3 rounded-md text-sm focus:outline-none focus:ring-2 focus:ring-gray-900 focus:border-transparent" placeholder="Enter your email" value="" />
                    <button class="bg-gray-900 hover:bg-gray-700 inline-block top-6 text-xs right-1 text-white px-2 py-2 rounded-md absolute disabled:opacity-80" type="submit" disabled="">Subscribe</button>
                </form></div></div>

        </motion.div >
    )
}
**contactSection.js**
export default function ContactSection() {
  return (
    <motion.div
      initial={{ opacity: 0, y: 10 }}
      animate={{ opacity: 1, y: 0 }}
      transition={{ type: "spring", stiffness: 300 }}
      className="max-w-3xl mb-auto"
    >
      <div className="flex items-center justify-between  w-full flex-col mb-auto mt-5">
        <h1 className="font-medium text-gray-900 mt-2 text-xl font-heading mb-4">
          Get in Touch
        </h1>
        <input
          type="text"
          className="pl-4 p-2 border rounded-lg w-11/12 lg:w-[342px] text-sm outline-none mb-4"
          placeholder="Enter your name"
        ></input>
        <input
          type="text"
          className="pl-4 p-2 border rounded-lg w-11/12 lg:w-[342px] text-sm outline-none mb-4"
          placeholder="Enter your email"
        ></input>
        <textarea
          type="text"
          rows={5}
          className="pl-4 p-2 border rounded-lg w-11/12 lg:w-[342px] text-sm outline-none mb-4"
          placeholder="How can I help you?"
        ></textarea>
        <button className="text-white bg-black text-sm px-3 py-2 rounded-md relative">
          <span className="relative z-10">Submit</span>
        </button>
      </div>
    </motion.div>
  );
}
**newsletterSection.js**
"use client"
import { motion } from "framer-motion";
export default function Newsletter() {
  return (
    <motion.div
      initial={{ opacity: 0, y: 10 }}
      animate={{ opacity: 1, y: 0 }}
      transition={{ type: "spring", stiffness: 300 }}
      className="max-w-3xl mb-auto"
    >
      <div class="mt-4">
        <div class="mt-1">
          <h1 class="font-medium text-gray-900 mb-4 text-lg">
            Meditations Newsletter
          </h1>
          <p class="text-gray-500 mt-4">
            Stay ahead of the curve with my monthly newsletter called
            Meditations. Receive valuable insights on the latest trends,
            techniques, and tools in web development and design.
          </p>
          <form class="relative">
            <input
              class="border w-full mt-4 px-2 py-3 rounded-md text-sm focus:outline-none focus:ring-2 focus:ring-gray-900 focus:border-transparent"
              placeholder="Enter your email"
              value=""
            />
            <button
              class="bg-gray-900 hover:bg-gray-700 inline-block top-6 text-xs right-1 text-white px-2 py-2 rounded-md absolute disabled:opacity-80"
              type="submit"
              disabled=""
            >
              Subscribe
            </button>
          </form>
        </div>
      </div>
    </motion.div>
  );
}
**userSection.js**
"use client"
import { motion } from 'framer-motion';

const UsesSection = () => {
  return (
    <motion.section
      className="text-black bg-white max-w-3xl mb-auto"
      initial={{ opacity: 0 }}
      animate={{ opacity: 1 }}
      transition={{ duration: 1 }}
    >
      <motion.h2 className="text-xl font-bold mb-6" initial={{ y: -20 }} animate={{ y: 0 }} transition={{ duration: 0.5 }}>
        Uses
      </motion.h2>
      <motion.p className="mb-6 text-gray-600" initial={{ y: -20 }} animate={{ y: 0 }} transition={{ duration: 0.5, delay: 0.1 }}>
        Welcome to the Uses section of my portfolio! Here, I highlight the tools, technologies, and software that I rely on to build and enhance my portfolio. Each item plays a crucial role in my development process, ensuring a seamless and professional online presence.
      </motion.p>

      <Section
        title="Code Editor"
        items={[
          { name: "VS Code", description: "My go-to code editor, equipped with numerous extensions and customization options that streamline my coding experience." },
        ]}
      />

      <Section
        title="Frameworks & Libraries"
        items={[
          { name: "React", description: "A powerful JavaScript library for building user interfaces, enabling me to create dynamic and responsive components." },
          { name: "Framer Motion", description: "An animation library for React, allowing me to add smooth and engaging animations to my projects." },
          { name: "Tailwind CSS", description: "A utility-first CSS framework that helps me quickly design modern and responsive layouts." },
        ]}
      />

      <Section
        title="MERN Stack"
        items={[
          { name: "MongoDB", description: "A NoSQL database that provides flexibility and scalability for storing and managing data." },
          { name: "Express.js", description: "A minimal and flexible Node.js web application framework that provides a robust set of features for web and mobile applications." },
          { name: "React", description: "A JavaScript library for building user interfaces, particularly single-page applications." },
          { name: "Node.js", description: "A JavaScript runtime built on Chromes V8 JavaScript engine, allowing me to build scalable network applications." },
        ]}
      />

      <Section
        title="Version Control"
        items={[
          { name: "Git", description: "Essential for tracking changes and collaborating on projects. Git keeps my work organized and allows for efficient version management." },
          { name: "GitHub", description: "My preferred platform for hosting and sharing code repositories, facilitating collaboration and showcasing my projects." },
        ]}
      />

      <Section
        title="Design Tools"
        items={[
          { name: "Figma", description: "A versatile design tool that aids in creating wireframes, prototypes, and visual designs, ensuring a cohesive look and feel for my projects." },
          { name: "Adobe XD", description: "Another powerful tool for designing and prototyping user interfaces, helping me bring my design ideas to life." },
        ]}
      />

      <Section
        title="Deployment"
        items={[
          { name: "Netlify", description: "A reliable platform for deploying and hosting my portfolio, providing continuous deployment and fast, secure hosting." },
          { name: "Vercel", description: "An alternative deployment platform that specializes in hosting front-end frameworks and static sites, ensuring optimal performance and scalability." },
        ]}
      />
    </motion.section>
  );
};

const Section = ({ title, items }) => {
  return (
    <motion.div className="mb-8" initial={{ opacity: 0 }} animate={{ opacity: 1 }} transition={{ duration: 0.5, delay: 0.2 }}>
      <h3 className="text-lg font-semibold mb-4">{title}</h3>
      <ul className="list-disc list-inside text-gray-600">
        {items.map((item, index) => (
          <motion.li key={index} className="mb-2" initial={{ x: -20 }} animate={{ x: 0 }} transition={{ duration: 0.5, delay: index * 0.1 }}>
            <strong>{item.name}</strong> - {item.description}
          </motion.li>
        ))}
      </ul>
    </motion.div>
  );
};

export default UsesSection;

```

**Step 4 : Add code for the about section:**

In the **`/app/page.js`** add the following code **:**

```jsx
import Common from "@/components/common";
import AboutSection from "@/components/aboutSection";

export default function Home() {
  return (
    <main className="flex items-center justify-between w-full flex-col p-8 min-h-screen">
      <Common />
      <AboutSection />
    </main>
  );
}
```

Output :

![Untitled](https://talktohire.blr1.digitaloceanspaces.com/Untitled_2.png)

**Step 5 : Add code for the blog section:**

In the **`/app/blogs/page.js`** add the following code **:**

```jsx
"use server"
import Common from "@/components/common";
import BlogSection from "@/components/blogSection";
export default async function Blogs() {
  return (
    <main className="flex items-center justify-between w-full flex-col p-8 min-h-screen">
      <Common />
      <BlogSection/>
    </main>
  );
}
```

Output :

![Untitled](https://talktohire.blr1.digitaloceanspaces.com/Untitled_3.png)

**Step 6 : Add code for the contact section:**

In the **`/app/contact/page.js`** add the following code **:**

```jsx
import ContactSection from "@/components/contactSection";
import Common from "@/components/common";
export default async function Contact() {
    return (
        <main className="flex items-center justify-between w-full flex-col p-8 min-h-screen">
            <Common />
            <ContactSection/>
        </main>
    )
}
```

Output :

![Untitled](https://talktohire.blr1.digitaloceanspaces.com/Untitled_4.png)

**Step 7 : Add code for the newsletter section:**

In the **`/app/newsletter/page.js`** add the following code **:**

```jsx
import Common from "@/components/common";
import Newsletter from "@/components/newsletterSection";
export default function Contact() {
    return (
        <main className="flex items-center justify-between w-full flex-col p-8 min-h-screen">
            <Common />
            <Newsletter/>
        </main>
    )
}
```

Output :

![Untitled](https://talktohire.blr1.digitaloceanspaces.com/Untitled_5.png)

**Step 8 : Add code for the User section:**

In the **`/app/user/page.js`** add the following code **:**

```jsx
import Common from "@/components/common";
import UsesSection from "@/components/userSection";
export default function Contact() {
    return (
        <main className="flex items-center justify-between w-full flex-col p-8 min-h-screen">
            <Common />
            <UsesSection/>
        </main>
    )
}
```

Output :

![Untitled](https://talktohire.blr1.digitaloceanspaces.com/Untitled_6.png)

Your portfolio is complete, Happy Coding!