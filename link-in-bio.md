# How to create link in bio for developers in Next.js ?

Link in bio is an application which serves as a single points where all the links whether to their profiles, portfolios or projects are listed in a single page. This page’s URL is then added to the bio of   various social media so that other user can see all the links and access them.

## **Setting Up the Next.js Project**

Let's start by setting up a new Next.js project. Open your terminal and execute the following commands:

```jsx
npx create-next-app linkshortner

✔ Would you like to use TypeScript with this project? … No
✔ Would you like to use ESLint with this project? … Yes
✔ Would you like to use Tailwind CSS with this project? … Yes
✔ Would you like to use `src/` directory with this project? … Yes
✔ Use App Router (recommended)? … No
✔ Would you like to customize the default import alias? … No
```

This will create a new Next.js project in a directory named **`linkshortner`**. Next, open the project in your preferred code editor.

```jsx
cd linkshortner
```

## **Installing Dependencies**

Next, we need to install the required dependencies for our project. We are using framer motion here to make a application animated. In the terminal, navigate to the project directory and run the following command:

```jsx
npm install framer-motion
npm install react-qr-code
npm install html-to-image
npm install @mui/material @emotion/styled @emotion/react @mui/icons-material
npm install mongoose 
npm install react-hot-toast
npm install lucide-react
```

Step 1 : Create folder structure inside the src folder like this :

```jsx
.
├── components
│   └── icons.js
├── images
│   └── tinyhost.png
├── lib
│   └── connectDB.js
├── model
│   └── userModel.js
├── pages
│   ├── _app.js
│   ├── _document.js
│   ├── api
│   │   ├── addlink.js
│   │   ├── getlinks.js
│   │   ├── signin.js
│   │   └── signup.js
│   ├── dashboard
│   │   └── [slug].js
│   ├── index.js
│   ├── signup
│   │   └── index.js
│   └── view
│       └── [slug].js
├── source
│   └── index.js
└── styles
    └── globals.css
```

Step 2  : Inside the **`components/icons.js`**  add the following code :

```jsx
import {
    AlertTriangle,
    ArrowRight,
    Check,
    ChevronLeft,
    ChevronRight,
    Command,
    Option,
    List,
    ArrowDownRight,
    CreditCard,
    File,
    FileText,
    HelpCircle,
    Image,
    Laptop,
    Loader2,
    LucideProps,
    Moon,
    MoreVertical,
    Pizza,
    Plus,
    Settings,
    SunMedium,
    Trash,
    Twitter,
    User,
    X,
  } from "lucide-react"
  

  
  export const Icons = {
    logo: ArrowDownRight,
    close: X,
    spinner: Loader2,
    chevronLeft: ChevronLeft,
    chevronRight: ChevronRight,
    trash: Trash,
    post: FileText,
    page: File,
    media: Image,
    settings: Settings,
    billing: CreditCard,
    ellipsis: MoreVertical,
    add: Plus,
    warning: AlertTriangle,
    user: User,
    arrowRight: ArrowRight,
    help: HelpCircle,
    pizza: Pizza,
    sun: SunMedium,
    moon: Moon,
    laptop: Laptop,
    gitHub: ({ ...props }) => (
      <svg
        aria-hidden="true"
        focusable="false"
        data-prefix="fab"
        data-icon="github"
        role="img"
        xmlns="http://www.w3.org/2000/svg"
        viewBox="0 0 496 512"
        {...props}
      >
        <path
          fill="currentColor"
          d="M165.9 397.4c0 2-2.3 3.6-5.2 3.6-3.3 .3-5.6-1.3-5.6-3.6 0-2 2.3-3.6 5.2-3.6 3-.3 5.6 1.3 5.6 3.6zm-31.1-4.5c-.7 2 1.3 4.3 4.3 4.9 2.6 1 5.6 0 6.2-2s-1.3-4.3-4.3-5.2c-2.6-.7-5.5 .3-6.2 2.3zm44.2-1.7c-2.9 .7-4.9 2.6-4.6 4.9 .3 2 2.9 3.3 5.9 2.6 2.9-.7 4.9-2.6 4.6-4.6-.3-1.9-3-3.2-5.9-2.9zM244.8 8C106.1 8 0 113.3 0 252c0 110.9 69.8 205.8 169.5 239.2 12.8 2.3 17.3-5.6 17.3-12.1 0-6.2-.3-40.4-.3-61.4 0 0-70 15-84.7-29.8 0 0-11.4-29.1-27.8-36.6 0 0-22.9-15.7 1.6-15.4 0 0 24.9 2 38.6 25.8 21.9 38.6 58.6 27.5 72.9 20.9 2.3-16 8.8-27.1 16-33.7-55.9-6.2-112.3-14.3-112.3-110.5 0-27.5 7.6-41.3 23.6-58.9-2.6-6.5-11.1-33.3 2.6-67.9 20.9-6.5 69 27 69 27 20-5.6 41.5-8.5 62.8-8.5s42.8 2.9 62.8 8.5c0 0 48.1-33.6 69-27 13.7 34.7 5.2 61.4 2.6 67.9 16 17.7 25.8 31.5 25.8 58.9 0 96.5-58.9 104.2-114.8 110.5 9.2 7.9 17 22.9 17 46.4 0 33.7-.3 75.4-.3 83.6 0 6.5 4.6 14.4 17.3 12.1C428.2 457.8 496 362.9 496 252 496 113.3 383.5 8 244.8 8zM97.2 352.9c-1.3 1-1 3.3 .7 5.2 1.6 1.6 3.9 2.3 5.2 1 1.3-1 1-3.3-.7-5.2-1.6-1.6-3.9-2.3-5.2-1zm-10.8-8.1c-.7 1.3 .3 2.9 2.3 3.9 1.6 1 3.6 .7 4.3-.7 .7-1.3-.3-2.9-2.3-3.9-2-.6-3.6-.3-4.3 .7zm32.4 35.6c-1.6 1.3-1 4.3 1.3 6.2 2.3 2.3 5.2 2.6 6.5 1 1.3-1.3 .7-4.3-1.3-6.2-2.2-2.3-5.2-2.6-6.5-1zm-11.4-14.7c-1.6 1-1.6 3.6 0 5.9 1.6 2.3 4.3 3.3 5.6 2.3 1.6-1.3 1.6-3.9 0-6.2-1.4-2.3-4-3.3-5.6-2z"
        ></path>
      </svg>
    ),
    twitter: Twitter,
    check: Check,
  }
  
```

This file contains all the icons used in the project.

Step 3 : Create a sign in page in the **`src/pages/index.js`** using the following code:
|

![Untitled](https://talktohire.blr1.digitaloceanspaces.com/af478667-23e3-4056-aee0-50a54405e4ac.png)

```jsx
import React, { useEffect } from "react";
import { useRouter } from "next/navigation";
import { Icons } from "@/components/icons";
import toast, { Toaster } from "react-hot-toast";
import { URI } from "@/source";
export default function Home() {
  const router = useRouter();
  //AUthOpen
  const [isGitHubLoading, setIsGitHubLoading] = React.useState(false);
  const [googleLoading, setGoogleLoading] = React.useState(false);
  //Local-States:
  const [email, setEmail] = React.useState("");
  const [password, setPassword] = React.useState("");

  const handleSignIn = async () => {
    const bodyObject = {
      username: email,
      password: password,
    };
    const response = await fetch(`${URI}/api/signin`, {
      method: "POST",
      headers: {
        "Content-Type": "application/json",
      },
      body: JSON.stringify(bodyObject),
    });
    console.log("Response", response);
    const result = await response.json();
    console.log("Result of login", result);
    if (response.ok) {
      toast.success("Successfully Signed In!");
      setTimeout(() => {
        router.push(`/dashboard/${result.data._id}`);
      }, 1500);
    } else {
      toast.error("Invalid Username or Password");
    }
  };

  return (
    <div className="flex justify-center items-center min-h-screen bg-gradient-to-bl from-custom-gradient-start via-custom-gradient-middle to-custom-gradient-end">
      <div className="w-10/12 h-full sm:min-w-full md:min-w-80 md:min-h-screen lg:max-w-80 xl:max-w-80">
        <div className="md:pt-2 xl:pt-3 2xl:pt-28">
          <div className="font-sans font-semibold md:mt-2 xl:mt-3 text-4xl">
            Sign In
          </div>
          <Toaster />
          <div className="font-sans text-base py-3 font-medium">
            Welcome back {`you've`} been missed
          </div>
          <div className="font-sans mt-2 text-lg font-semibold mb-1.5 ">
            Username
          </div>
          <input
            onChange={(e) => setEmail(e.target.value)}
            type="text"
            className="block w-full rounded-lg border-0  py-3.5 pl-4 pr-20 text-gray-900 ring-1 ring-inset ring-gray-300 placeholder:text-gray-400 focus:ring-2 focus:ring-inset focus:ring-indigo-600 sm:text-sm sm:leading-6"
            placeholder="Enter Username"
          />

          <div className="font-sans text-lg font-semibold md:mt-2 xl:mt-5 mb-1.5">
            Password
          </div>
          <input
            onChange={(e) => setPassword(e.target.value)}
            type="password"
            className="block w-full rounded-lg border-0 py-3.5 pl-4 pr-20 text-gray-900 ring-1 ring-inset ring-gray-300 placeholder:text-gray-400 focus:ring-2 focus:ring-inset focus:ring-indigo-600 sm:text-sm sm:leading-6"
            placeholder="Enter Password"
          />
          <div className="flex items-center xl:mt-1">
            <input
              type="checkbox"
              id="checkbox"
              className="form-checkbox h-5 w-5 rounded-lg text-purple-700"
            />
            <div className="font-sans text-base font-medium py-4 ml-3">
              Remember Me
            </div>
            <div className="font-sans text-base font-medium ml-auto py-2">
              Forgot Password ?
            </div>
          </div>
          <button
            className="bg-slate-800 text-white font-semibold text-xl pt-3 pb-4 px-4 rounded-lg w-full md:mt-4 xl:mt-5"
            onClick={handleSignIn}
          >
            Sign In
          </button>
          <div className="flex items-center justify-center md:mt-4 xl:mt-3">
            <hr className="w-1/3 border-t border-gray-300 mr-3.5" />
            <span className="font-sans text-sm font-medium">Or With</span>
            <hr className="w-1/3 border-t border-gray-300 ml-3.5" />
          </div>
          <div className="grid grid-cols-2 gap-4 md:mt-3 xl:mt-3">
            <div className="flex justify-center py-4 px-4 border-2 rounded-lg text-center text-base font-medium bg-white">
              {isGitHubLoading ? (
                <Icons.spinner className="mr-2 h-4 w-4 mt-1 animate-spin" />
              ) : (
                <Icons.gitHub className="mr-2 h-4 w-4 mt-1 " />
              )}
              <button
                type="button"
                onClick={() => {
                  setIsGitHubLoading(true);
                }}
                disabled={isGitHubLoading}
              >
                Github
              </button>
            </div>
            <div className="py-1 px-2 border-2 flex justify-center rounded-lg text-center text-sm font-medium bg-white">
              <button
                className="gsi-material-button"
                onClick={() => {
                  setGoogleLoading(true);
                }}
              >
                <div className="gsi-material-button-state"></div>
                <div className="gsi-material-button-content-wrapper flex justify-center">
                  {googleLoading ? (
                    <Icons.spinner className="mr-2 h-4 w-4 mt-1 animate-spin" />
                  ) : (
                    <div
                      className="gsi-material-button-icon mt-1"
                      style={{ width: 16, height: 12 }}
                    >
                      <svg
                        version="1.1"
                        xmlns="http://www.w3.org/2000/svg"
                        viewBox="0 0 48 48"
                        xmlnsXlink="http://www.w3.org/1999/xlink"
                        style={{ display: "block" }}
                      >
                        <path
                          fill="#EA4335"
                          d="M24 9.5c3.54 0 6.71 1.22 9.21 3.6l6.85-6.85C35.9 2.38 30.47 0 24 0 14.62 0 6.51 5.38 2.56 13.22l7.98 6.19C12.43 13.72 17.74 9.5 24 9.5z"
                        ></path>
                        <path
                          fill="#4285F4"
                          d="M46.98 24.55c0-1.57-.15-3.09-.38-4.55H24v9.02h12.94c-.58 2.96-2.26 5.48-4.78 7.18l7.73 6c4.51-4.18 7.09-10.36 7.09-17.65z"
                        ></path>
                        <path
                          fill="#FBBC05"
                          d="M10.53 28.59c-.48-1.45-.76-2.99-.76-4.59s.27-3.14.76-4.59l-7.98-6.19C.92 16.46 0 20.12 0 24c0 3.88.92 7.54 2.56 10.78l7.97-6.19z"
                        ></path>
                        <path
                          fill="#34A853"
                          d="M24 48c6.48 0 11.93-2.13 15.89-5.81l-7.73-6c-2.15 1.45-4.92 2.3-8.16 2.3-6.26 0-11.57-4.22-13.47-9.91l-7.98 6.19C6.51 42.62 14.62 48 24 48z"
                        ></path>
                        <path fill="none" d="M0 0h48v48H0z"></path>
                      </svg>
                    </div>
                  )}
                  <span className="gsi-material-button-contents text-base ml-2">
                    Google
                  </span>
                  <span style={{ display: "none" }}>Google</span>
                </div>
              </button>
            </div>
          </div>
          <div className="text-center font-sans text-base text-gray-500 md:mt-11 lg:mt-15 xl:mt-4 mb-7">
            {`Don't`} have an account ?
            <span
              className="font-sans font-medium text-base text-cyan-800 cursor-pointer  underline-offset-2 ml-2"
              onClick={() => router.push("/signup")}
            >
              Sign Up
            </span>
          </div>
        </div>
      </div>
    </div>
  );
}

```

Step 4 : Create a Sign up page in the **`pages/singup/index.js`**using the following code:

![Untitled](https://talktohire.blr1.digitaloceanspaces.com/Untitled.png)

```jsx
import { URI } from "@/source";
import { useRouter } from "next/navigation";
import { useState } from "react";
import toast, { Toaster } from "react-hot-toast";

export default function Signup() {
  const router = useRouter();
  const [userName, setUserName] = useState("");
  const [password, setPassword] = useState("");

  const handleSignup = async () => {
    const bodyObject = {
      username: userName,
      password: password,
    };
    const response = await fetch(`${URI}/api/signup`, {
      method: "POST",
      headers: {
        "Content-Type": "application/json"
      },
      body: JSON.stringify(bodyObject),
    });
    console.log("Response", response);
    if (response.ok) {
      toast.success("Successfully created User!");
      setTimeout(() => {
        router.push("/");
      }, 1500);
    }
  };
  return (
    <div className="flex justify-center items-center min-h-screen bg-gradient-to-bl from-custom-gradient-start via-custom-gradient-middle to-custom-gradient-end">
      <div className="w-10/12 h-full sm:min-w-full md:min-w-80 md:min-h-screen lg:max-w-80 xl:max-w-80">
        <div className="md:pt-2 xl:pt-3">
          <Toaster />
          <div className="font-sans font-semibold md:mt-2 xl:mt-5 text-4xl">
            Sign Up
          </div>
          <div className="font-sans text-base py-3 font-medium">
            Join us today and start exploring
          </div>
          <div className="font-sans mt-2 text-lg font-semibold mb-1.5">
            Username
          </div>
          <input
            onChange={(e) => setUserName(e.target.value)}
            type="text"
            value={userName}
            className="block w-full rounded-lg border-0  py-3.5 pl-4 pr-20 text-gray-900 ring-1 ring-inset ring-gray-300 placeholder:text-gray-400 focus:ring-2 focus:ring-inset focus:ring-indigo-600 sm:text-sm sm:leading-6"
            placeholder="Enter Username"
          />

          <div className="font-sans text-lg font-semibold md:mt-2 xl:mt-5 mb-1.5">
            Password
          </div>
          <input
            onChange={(e) => setPassword(e.target.value)}
            type="password"
            value={password}
            className="block w-full rounded-lg border-0 py-3.5 pl-4 pr-20 text-gray-900 ring-1 ring-inset ring-gray-300 placeholder:text-gray-400 focus:ring-2 focus:ring-inset focus:ring-indigo-600 sm:text-sm sm:leading-6"
            placeholder="Enter Password"
          />
          <div className="font-sans text-lg font-semibold md:mt-2 xl:mt-5 mb-1.5">
            Confirm Password
          </div>
          <input
            type="password"
            className="block w-full rounded-lg border-0 py-3.5 pl-4 pr-20 text-gray-900 ring-1 ring-inset ring-gray-300 placeholder:text-gray-400 focus:ring-2 focus:ring-inset focus:ring-indigo-600 sm:text-sm sm:leading-6"
            placeholder="Confirm Password"
          />
          <button
            onClick={handleSignup}
            className="bg-slate-800 text-white font-semibold text-xl pt-3 pb-4 px-4 rounded-lg w-full md:mt-4 xl:mt-8"
          >
            Sign Up
          </button>
          <div className="text-center font-sans text-base text-gray-500 md:mt-11 lg:mt-15 xl:mt-8 mb-7">
            Already have an account?
            <span
              className="font-sans font-medium text-base text-cyan-500 cursor-pointer  underline-offset-2 ml-2"
              onClick={() => router.push("/")}
            >
              Sign In
            </span>
          </div>
        </div>
      </div>
    </div>
  );
}

```

Step 5 : Create a dashboard page for displaying all the links and your access page in the **`pages/dashboard/[slug].js`**using the following code (make sure you have added logo in the image folder ):

![Untitled](https://talktohire.blr1.digitaloceanspaces.com/Untitled_1.png)

```jsx
import LinkIcon from "@mui/icons-material/Link";
import { styled } from "@mui/material/styles";
import Table from "@mui/material/Table";
import TableBody from "@mui/material/TableBody";
import TableCell, { tableCellClasses } from "@mui/material/TableCell";
import TableHead from "@mui/material/TableHead";
import TableRow from "@mui/material/TableRow";
import IconButton from "@mui/material/IconButton";
import Menu from "@mui/material/Menu";
import MenuItem from "@mui/material/MenuItem";
import MoreVertIcon from "@mui/icons-material/MoreVert";
import React, { useEffect, useState } from "react";
import LinkOffIcon from "@mui/icons-material/LinkOff";
import DeleteIcon from "@mui/icons-material/Delete";
import QrCode2Icon from "@mui/icons-material/QrCode2";
import EditIcon from "@mui/icons-material/Edit";
import ContentCopyIcon from "@mui/icons-material/ContentCopy";
import Tooltip from "@mui/material/Tooltip";
import logo from "../../images/tinyhost.png";
import Image from "next/image";
import { useRouter } from "next/router";
import { toPng } from 'html-to-image';
import QRCode from "react-qr-code";
import Backdrop from '@mui/material/Backdrop';
import Box from '@mui/material/Box';
import Modal from '@mui/material/Modal';
import Fade from '@mui/material/Fade';
import Button from '@mui/material/Button';
import Typography from '@mui/material/Typography';
import { URI } from "@/source";
import toast, { Toaster } from "react-hot-toast";

const StyledTableCell = styled(TableCell)(({ theme }) => ({
  [`&.${tableCellClasses.head}`]: {
    backgroundColor: "#f3f4f6",
    color: "#898989",
    fontSize: 12,
    border: 0,
    padding: 11,
    [theme.breakpoints.down("xl")]: {
      fontSize: 10,
      padding: 8,
    },
  },
  [`&.${tableCellClasses.body}`]: {
    fontSize: 11,
    border: 0,
    padding: 14,
    [theme.breakpoints.down("xl")]: {
      fontSize: 9,
      padding: 10,
    },
  },
}));

const StyledTableRow = styled(TableRow)(({ theme }) => ({
  "&:nth-of-type(even)": {
    backgroundColor: theme.palette.action.hover,
  },
  // hide last border
  "&:last-child td, &:last-child th": {
    border: 0,
  },
}));

function createData(name, calories, fat, carbs, protein) {
  return { name, calories, fat, carbs, protein };
}

export default function Dashboard() {
  const [anchorEl, setAnchorEl] = React.useState(null);
  const [linkData, setLinkData] = React.useState([])
  const [title, setTitle] = React.useState("")
  const [link, setLink] = React.useState("")
  const [url, seturl] = useState("");
  const qrCodeRef = React.useRef(null)
  const [qrIsVisible, setQrIsVisible] = useState(false);
  const open = anchorEl;
  const [openQr, setOpenQr] = React.useState(false);
  const [openAddLink, setOpenAddLink] = React.useState(false);

  const router = useRouter();
  const { slug } = router.query;

  useEffect(() => {
    console.log("Id", slug);
    if (typeof window !== "undefined") {
      const currentUrl = window.location.href;
      const replacedWord = currentUrl.replace('dashboard', 'view')
      seturl(replacedWord);
    }
  }, []);

  const fetchUserData = async () => {
    const bodyObject = {
      userID: slug,
    };
    const response = await fetch(`${URI}/api/getlinks`, {
      method: "POST",
      headers: {
        "Content-Type": "application/json"
      },
      body: JSON.stringify(bodyObject),
    });
    const result = await response.json();
    console.log("Result ",result)
    setLinkData(result?.data.links);
  };

  useEffect(() => {
    if (slug) {
      fetchUserData();
    }
  }, [slug]);

  const handleOpenQR = () => {
    setOpenQr(true)
    if (!url) {
      return;
    }
    setQrIsVisible(true);
  };

  const handleCloseQR = () => setOpenQr(false);

  const handleOpenAddLinks = () => {
    setOpenAddLink(true)
  };

  const handleCloseAddLinks = () => setOpenAddLink(false);

  

  const downloadQRCode = () => {
    toPng(qrCodeRef.current)
      .then(function (dataUrl) {
        const link = document.createElement("a");
        link.href = dataUrl;
        link.download = "qr-code.png";
        link.click();
      })
      .catch(function (error) {
        console.error("Error generating QR code:", error);
      });
  };

  const style = {
    position: 'absolute',
    top: '50%',
    left: '50%',
    transform: 'translate(-50%, -50%)',
    width: 400,
    bgcolor: 'background.paper',
    border: '2px solid #000',
    boxShadow: 24,
    borderRadius: 5,
    p: 4,
  };

  const styleLink = {
    position: 'absolute',
    top: '50%',
    left: '50%',
    transform: 'translate(-50%, -50%)',
    width: 500,
    bgcolor: 'background.paper',
    border: '2px solid #000',
    boxShadow: 24,
    borderRadius: 5,
    p: 4,
  };

  const handleClick = (event) => {
    setAnchorEl(event.currentTarget);
  };
  const handleClose = () => {
    setAnchorEl(null);
  };

  function copyToClipboard(text) {
    navigator.clipboard.writeText(text)
      .then(() => {
        toast.success("Link copied!")
        console.log('Text copied to clipboard');
      })
      .catch(err => {
        console.error('Error copying text: ', err);
      });
  }

  const handleLinks = async () => {
    const bodyObject = {
      userId: slug,
      title: title,
      link: link,
      status: "active"
    }
    const response = await fetch(`${URI}/api/addlink`, {
      method: "POST",
      headers: {
        "Content-Type": "application/json"
      },
      body: JSON.stringify(bodyObject)
    })
    const result = await response.json();
    console.log("Reuslt of adding", result)
    setLinkData(result.data.links)
    if (response.ok) {
      toast.success("Successfully added link");
      handleCloseAddLinks()
    } else {
      toast.error("Failed to add link");
    }
  }

  return (
    <div style={{ backgroundColor: "#fbfbfb" }}>
      <div className="flex justify-center items-center headerDiv">
        <div className="w-5/6 border rounded-2xl min-h-8 xl:p-2 2xl:p-4 mt-10 bg-white shadow-[0_12px_41px_-11px_rgba(199,199,199)]">
          <div className="flex">
            <Image
              src={logo}
              alt="logo"
              style={{ height: "11%", width: "11%" }}
            />
            <span className=" text-base cursor-pointer xl:mt-1.5 2xl:mt-4 font-sans text-slate-500 ml-10 mr-5">
              Products
            </span>
            <span className="text-base cursor-pointer xl:mt-1.5 2xl:mt-4 font-sans text-slate-500 mr-5">
              Use Cases
            </span>
            <span className="text-base cursor-pointer xl:mt-1.5 2xl:mt-4 font-sans text-slate-500 mr-5">
              Blogs
            </span>
            <button className="bg-purple-200 text-sm text-purple-700 font-medium ml-auto rounded-lg xl:h-8 xl:pt-0 pb-0 pl-1 pr-1 2xl:pt-1 pb-1 pl-2 pr-2 h-10 mt-2">
              Sign Out
            </button>
          </div>
        </div>
        <Toaster />
      </div>
      <div class="text-[#333] rounded-xl font-[sans-serif] linkDiv">
        <div className="w-full flex items-center justify-center xl:pt-8  2xl:mt-10 2xl:pt-10">
          <span class="mt-4 text-xl font-sans text-purple-700 font-medium">
            Your link :
            <span className="text-sm text-gray-600 font-sans font-medium ml-2 mr-5">
              {" "}
              {url}
            </span>
            <Tooltip title="Copy" onClick={() => copyToClipboard(url)}>
              <IconButton
                style={{
                  fontSize: 13,
                  paddingTop: 3,
                  paddingBottom: 3,
                  paddingLeft: 5,
                  paddingRight: 5,
                  borderRadius: 7,
                  border: "1px solid #19a89d",
                  color: "#19a89d",
                  fontWeight: 700,
                }}
              >
                Copy URL
                <ContentCopyIcon
                  size="sm"
                  style={{ fontSize: 17, color: "#19a89d", marginLeft: 7 }}
                />
              </IconButton>
            </Tooltip>
            <Tooltip title="Generate QR Code" onClick={handleOpenQR}>
              <IconButton
                style={{
                  fontSize: 13,
                  paddingTop: 3,
                  paddingBottom: 3,
                  paddingLeft: 5,
                  paddingRight: 5,
                  borderRadius: 7,
                  border: "1px solid #8a53fe",
                  color: "#8a53fe",
                  fontWeight: 700,
                  marginLeft: 17,
                }}
              >
                Generate QR Code
                <QrCode2Icon
                  size="sm"
                  style={{ fontSize: 22, color: "#8a53fe", marginLeft: 7 }}
                />
              </IconButton>
            </Tooltip>
            <Modal
              aria-labelledby="transition-modal-title"
              aria-describedby="transition-modal-description"
              open={openQr}
              onClose={handleCloseQR}
              closeAfterTransition
              slots={{ backdrop: Backdrop }}
              slotProps={{
                backdrop: {
                  timeout: 500,
                },
              }}
            >
              <Fade in={openQr}>
                <Box sx={style} className="border-0">
                  <Typography id="transition-modal-title" variant="h6" component="h2">
                    Your Unique link QR Code is ready
                  </Typography>
                  {qrIsVisible && (
                    <div className="qrcode__download" ref={qrCodeRef}>
                      <div className="qrcode__image" >
                        <QRCode value={url} size={200} />
                      </div>
                      <button className="bg-purple-200 text-purple-700 border rounded-lg p-3" onClick={downloadQRCode}>Download QR Code</button>
                    </div>
                  )}
                </Box>
              </Fade>
            </Modal>
          </span>
        </div>
      </div>
      <div className="flex justify-center items-center liveCard">
        <div className=" xl:h-content 2xl:h-content w-content bg-white border rounded-2xl shadow-[0_12px_41px_-11px_rgba(199,199,199)]">
          <div className="xl:mx-3 xl:mt-2 2xl:mx-6 2xl:mt-5">
            <div className="flex justify-between xl:mt-3 2xl:mt-6">
              <div className="font-sans text-2xl font-medium subpixel-antialiased flex">
                Live Links
                <div
                  className="bg-purple-200 text-purple-800 w-fit h-auto p-1 font-sans mt-1.5 ml-3 mb-2"
                  style={{ borderRadius: 10 }}
                >
                  <div className="font-sans mr-1 ml-1 text-xs font-base">
                    2/4 Live
                  </div>
                </div>
              </div>
              <button className="font-sans rounded-xl font-medium text-xs pl-3 pr-3 pt-0 pb-0 rounded-2xl bg-black text-white " onClick={handleOpenAddLinks}>
                Add Link <LinkIcon style={{ fontSize: 20, marginLeft: 2 }} />
              </button>
              <Modal
                aria-labelledby="transition-modal-title"
                aria-describedby="transition-modal-description"
                open={openAddLink}
                onClose={handleCloseAddLinks}
                closeAfterTransition
                slots={{ backdrop: Backdrop }}
                slotProps={{
                  backdrop: {
                    timeout: 500,
                  },
                }}
              >
                <Fade in={openAddLink}>
                  <Box sx={styleLink} className="border-0">
                    <Typography  alignContent={'center'} id="transition-modal-title" variant="h6" component="h2">
                      Add Link
                    </Typography>
                    <div className="mt-5">
                      <div>Title :</div>
                      <div>
                        <input type="text" className="mt-3 h-10 w-full  border-2 p-3  rounded-lg mb-5 " placeholder="Enter title" onChange={(e) => setTitle(e.target.value)} value={title}></input>
                      </div>
                      <div>Link :</div>
                      <div>
                        <input type="text" className="mt-3 h-10 w-full  border-2 p-3  rounded-lg" placeholder="Enter link" onChange={(e) => setLink(e.target.value)} value={link}></input>
                      </div>
                      <button className="ml-80 font-medium mt-5 bg-purple-200 text-purple-700 border rounded-lg pt-1 pb-1 pr-2.5 pl-2.5 h-fit" onClick={handleLinks}>Add Link</button>
                    </div>
                  </Box>
                </Fade>
              </Modal>
            </div>
            <div className="font-sans xl:mt-5 xl:mb-2 2xl:mt-10 2xl:mb-4">
              <Table
                sx={{ minWidth: 400, border: "none" }}
                aria-label="customized table"
                size="sm"
              >
                <TableHead>
                  <TableRow>
                    <StyledTableCell
                      align="left"
                      style={{
                        borderTopLeftRadius: 15,
                        borderBottomLeftRadius: 15,
                      }}
                    >
                      Status
                    </StyledTableCell>
                    <StyledTableCell align="left">Domain</StyledTableCell>
                    <StyledTableCell
                      align="center"
                      style={{
                        borderTopleftRadius: 15,
                        borderBottomleftRadius: 15,
                      }}
                    >
                      Actions
                    </StyledTableCell>
                  </TableRow>
                </TableHead>
                <TableBody>
                  {linkData?.map((row) => (
                    <StyledTableRow key={row}>
                      <StyledTableCell
                        align="left"
                        style={{
                          borderTopLeftRadius: 15,
                          borderBottomLeftRadius: 15,
                        }}
                      >{row.status ==="active" ?  
                      <div
                          className="bg-green-300 text-green-800 w-fit h-auto p-1 font-sans flex"
                          style={{ borderRadius: 10 }}
                        >
                          {" "}
                          <div className="bg-green-800 w-1 h-1 rounded-xl mt-1.5 ml-1 mr-1"></div>
                          <div className="font-sans mr-1">Active</div>
                        </div>
                        :
                        <div
                          className="bg-red-300 text-red-800 w-fit h-auto p-1 font-sans flex"
                          style={{ borderRadius: 10 }}
                        >
                          {" "}
                          <div className="bg-red-800 w-1 h-1 rounded-xl mt-1.5 ml-1 mr-1"></div>
                          <div className="font-sans mr-1">In Active</div>
                        </div>}

                      </StyledTableCell>
                      <StyledTableCell align="left">
                        <a href={row.link} className="text-purple-700 text-sm font-sans font-medium">{row.link}</a>
                      </StyledTableCell>
                      <StyledTableCell
                        align="right"
                        style={{
                          borderTopRightRadius: 15,
                          borderBottomRightRadius: 15,
                        }}
                      >
                        {" "}
                        <div>
                          <Tooltip title="Copy" onClick={copyToClipboard}>
                            <IconButton style={{ paddingLeft: 0 }}>
                              <ContentCopyIcon
                                size="sm"
                                style={{ fontSize: 17, color: "#19a89d" }}
                              />
                            </IconButton>
                          </Tooltip>
                          <IconButton
                            aria-label="more"
                            id="long-button"
                            aria-controls={open ? "long-menu" : undefined}
                            aria-expanded={open ? "true" : undefined}
                            aria-haspopup="true"
                            onClick={handleClick}
                            size="sm"
                          >
                            <MoreVertIcon size="sm" />
                          </IconButton>
                          <Menu
                            size="sm"
                            id="long-menu"
                            MenuListProps={{
                              "aria-labelledby": "long-button",
                            }}
                            anchorEl={anchorEl}
                            open={open}
                            onClose={handleClose}
                            PaperProps={{
                              style: {
                                width: "auto",
                                boxShadow: "none",
                                fontSize: 12,
                                border: "1px solid gray",
                                borderRadius: 15,
                              },
                            }}
                          >
                            <MenuItem
                              size="sm"
                              className="text-sm text-slate-500"
                              onClick={handleClose}
                            >
                              <LinkOffIcon className="mr-2 inActive text-cyan-500" /> In
                              Active
                            </MenuItem>
                            <MenuItem
                              size="sm"
                              className="text-sm text-slate-500"
                              onClick={handleClose}
                            >
                              <EditIcon className="mr-2 editOptions text-green-500" /> Edit
                            </MenuItem>
                            <MenuItem
                              size="sm"
                              className="text-sm text-slate-500"
                              onClick={handleClose}
                            >
                              <DeleteIcon className="mr-2 deleteOptions text-red-500" />{" "}
                              Delete
                            </MenuItem>
                          </Menu>
                        </div>
                      </StyledTableCell>
                    </StyledTableRow>
                  ))}
                </TableBody>
              </Table>
            </div>
          </div>
        </div>
      </div>
    </div>
  );
}
```

Step 6 : Create a View page for  accessing all the links added by you, in the **`pages/view/[slug].js`**using the following code :

![Untitled](https://talktohire.blr1.digitaloceanspaces.com/Untitled_2.png)

```jsx
import { URI } from "@/source";
import { useRouter } from "next/router";
import { useEffect, useState } from "react";
import { motion, sync, useCycle } from "framer-motion";

export default function View() {
  const router = useRouter();
  const [userData, setUserData] = useState([]);
  const [userName , setUsername] = useState("")
  const { slug } = router.query;

  const fetchUserData = async () => {
    const bodyObject = {
      userID: slug,
    };
    debugger;
    const response = await fetch(`${URI}/api/getlinks`, {
      method: "POST",
      headers: {
        "Content-Type": "application/json"
      },
      body: JSON.stringify(bodyObject),
    });
    const result = await response.json();
    setUserData(result.data.links);
    setUsername(result)
  };

  useEffect(() => {
    if (slug) {
      fetchUserData();
    }
  }, [slug]);

  const handlelink = (url) => {
    const newTab = window.open(url, '_blank');
    // Focus on the new tab (optional)
    if (newTab) {
      newTab.focus();
    } else {
      // If pop-up blocker prevents opening new tab, provide alternative
      console.log("Please allow pop-ups for this site to open the link.");
    }
  }
  return (
    <div className="backggroundColor flex justify-center w-full h-full">
      <div className="flex justify-center flex-col items-center w-2/6">
        <div class="circle">
          <p class="circle-inner">AY</p>
        </div>
        <div className="userName">
        @{userName?.data?.username}
        </div>
        {userData.length > 0 ? userData.map((item, index) => {
          return (
            <motion.div animate={{ y: -10 }}
              transition={{
                type: "spring", duration: 1, stiffness: 100,
                damping: 10
              }} key={index} className="cursor-pointer w-3/6 bg-white min-h-12 text-cyan-500 mb-2 border rounded-2xl mt-2 font-sans text-lg font-normal pl-12 pr-12 pt-2 pb-2">
              <div className="flex justify-center items-center bg-white font-medium font-sans" onClick={() => handlelink(item.link)}>
                {item.title}
              </div>
            </motion.div>
          )
        }) : null}
      </div>
    </div>
  );
}
```

Step 7 : Add CSS page for designing all the major component in the application, in the **`styles/global.css`**using the following code :

```css
@tailwind base;
@tailwind components;
@tailwind utilities;

@media (max-width: 1600px) {

    .deleteOptions {
        font-size: 19px;
        color: #f00;
    }

    .inActive {
        font-size: 19px;
        color: #9074d3;
    }

    .editOptions {
        font-size: 19px;
        color: #36c6d5;
    }
}

@media (max-width: 1200px) {

    .deleteOptions {
        font-size: 16px;
        color: #f00;
    }

    .inActive {
        font-size: 16px;
        color: #9074d3;
    }

    .editOptions {
        font-size: 16px;
        color: #36c6d5;
    }
}

.liveCard {
    height: 75vh;
}

.linkDiv {
    height: 9vh;
}

.headerDiv {
    height: 10vh;
}

@media (max-width: 1200px) {

    .liveCard {
        height: 80vh;
    }

    .linkDiv {
        height: 11vh;
    }

    .headerDiv {
        height: 10vh;
    }
}

@media (max-width: 1600px) {
    .liveCard {
        height: 80vh;
    }

    .linkDiv {
        height: 11vh;
    }

    .headerDiv {
        height: 10vh;
    }
}

.qrcode__container {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    width: 100%;
    height: 100vh;
}

.qrcode__container--parent {
    display: flex;
    gap: 10px;
    align-items: center;
    justify-content: center;
    width: 100%;
}

.qrcode__input {
    display: flex;
    flex-direction: column;
    align-items: center;
    width: 30%;
    margin-top: 20px;
}

.qrcode__input input {
    width: 100%;
    padding: 10px;
    font-size: 12px;
    outline: none;
    resize: none;
    border-radius: 5px;
    border: 1px solid #ccc;
    margin-bottom: 15px;
}

.qrcode__input button,
.qrcode__download button {
    display: inline-block;
    padding: 7px;
    cursor: pointer;
    border: none;
    border-radius: 5px;
    font-size: 15px;
    font-weight: 500;
    transition: background-color 0.2s;
}

.qrcode__download {
    display: flex;
    align-items: center;
    flex-direction: column;
    gap: 10px;
    margin-top: 10%;
}

.qrcode__download button {
    margin-top: 10px;
}

.backggroundColor {
    background: linear-gradient(-45deg, #ee7752, #e73c7e, #23a6d5, #23d5ab);
    background-size: 400% 400%;
    animation: gradient 15s ease infinite;
    height: 100vh;
}

@keyframes gradient {
    0% {
        background-position: 0% 50%;
    }

    50% {
        background-position: 100% 50%;
    }

    100% {
        background-position: 0% 50%;
    }
}

.circle {
    display: inline-block;
    background-color: #a0eefc;
    margin-left: 10px;
    margin-right: 10px;
    margin-top: 1px;
    margin-bottom: 10px;
    border-radius: 50%;
}

.circle-inner {
    color: rgb(0, 179, 211);
    display: table-cell;
    vertical-align: middle;
    text-align: center;
    text-decoration: none;
    height: 100px;
    width: 100px;
    font-size: 34px;
}

.userName{
    margin-bottom: 20%;
    font-size: 20px;
    font-weight: 600;
    font-family: 'Lucida Sans', 'Lucida Sans Regular', 'Lucida Grande', 'Lucida Sans Unicode', Geneva, Verdana, sans-serif;
    color: white;
}
```

Step 8 : Create Model in **`src/model/userModel.js` and add the following code  :** 

```jsx
const mongoose = require("mongoose");

const userSchema = new mongoose.Schema({
  username: { type: String, unique: true },
  password: String,
  links: [
    {
      title: { type: String },
      link: { type: String },
      status: { type: String },
    },
  ],
});
module.exports =mongoose.models.User || mongoose.model('User', userSchema);
```

Step 9 : Create Next.js API in the **`src/pages/api/getlinks.js` `src/pages/api/addlink.js` `src/pages/api/signin.js` `src/pages/api/signup.js` files :** 

```jsx
singup.js : 
import userModel from "../../model/userModel";
import connectDB from "../../lib/connectDB";

export default async function handler(req, res) {
  try {
    const { username, password } = req.body;
    console.log("Getting" , req.body.username , req.body)
    // Validate input (e.g., check if required fields are provided)
    await connectDB();
    // Check if the username already exists
    const existingUser = await userModel.findOne({ username });
    if (existingUser) {
      return res.status(400).json({ error: "Username already exists" });
    }

    // Create a new user
    const newUser = new userModel({ username, password });

    // Save the user to the database
    await newUser.save();

    res.status(201).json({ message: "User created successfully" });
  } catch (error) {
    console.error(error);
    res.status(500).json({ error: "Internal server error" });
  }
}

singin.js
import userModel from "../../model/userModel";
import connectDB from "../../lib/connectDB";
export default async function handler(req, res) {
  try {
    const { username, password } = req.body;
    console.log("data", req.body);
    await connectDB()
    // Check if the user exists
    const user = await userModel.findOne({ username });
    if (!user) {
      return res.status(401).json({ error: "Invalid username or password" });
    }

    // Check if the password matches
    if (user.password !== password) {
      return res.status(401).json({ error: "Invalid username or password" });
    }
    res.status(200).json({ message: "Login successful", data: user });
  } catch (error) {
    console.error(error);
    res.status(500).json({ error: "Internal server error" });
  }
}

addlinks.js
import userModel from "@/model/userModel";
import connectDB from "@/lib/connectDB";
export default async function handler(req, res) {
  try {
    await connectDB()
    const { title, link, status, userId } = req.body;
    console.log("Data getting" , title, link, status, userId)
    // Check if the user exists
    const user = await userModel.findOne({_id : userId});
    if (!user) {
      return res.status(401).json({ error: "User not found" });
    }
console.log("Data found" , user)
    // Push new link object to the links array
    user.links.push({ title, link, status });

    // Save the updated user document
    await user.save();

    res.status(200).json({ message: "Link added successfully", data: user });
  } catch (error) {
    console.error(error);
    res.status(500).json({ error: "Internal server error" });
  }
}

getlinks.js
import connectDB from "@/lib/connectDB";
import userModel from "@/model/userModel";
export default async function handler(req, res) {
  try {
    await connectDB()
    const { userID } = req.body;
    console.log("data", userID,req.body);
    // Check if the user exists
    const user = await userModel.findOne({ _id : userID });
    console.log("data" , user)
    res.status(200).json({ message: "linkData", data: user });
  } catch (error) {
    console.error(error);
    res.status(500).json({ error: "Internal server error" });
  }
}
```

Step 10 : Connect to mongodb atlas DBusing the following code in the **`src/lib/connectDB.js` :**

```jsx
import mongoose from "mongoose";

let cached = global.mongoose;

if (!cached) {
  cached = global.mongoose = { conn: null, promise: null };
}

async function connectDB() {
  if (cached.conn) {
    return cached.conn;
  }

  if (!cached.promise) {
    const opts = {
      bufferCommands: false,
    };

    cached.promise = mongoose
      .connect(
      //Add yout own connect string from the mongo db atlas
        "mongodb+srv://t@cluster0.nknrg9k.mongodb.net/?retryWrites=true&w=majority&appName=Cluster0",
        opts
      )
      .then((mongoose) => {
        console.log("Db Connected!");
        return mongoose;
      });
  }
  cached.conn = await cached.promise;
  return cached.conn;
}

export default connectDB;
```

Step 11 : Create URL link file in the **`source/index.js`** and add the following code ( add your won vercel link for deployment ) :

```jsx
const liveLink = "https://linkshortner-psi.vercel.app"
const localURI = "http://localhost:8000"
export const URI = `${localURI}`
```

Final step Run your application using the following code : 

```jsx
npm run dev -p 8000
```