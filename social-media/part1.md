# How to build Social Media Full Stack App?

Building a full-stack social media app involves developing both the frontend (client-side) and backend (server-side) components. Here's a general overview of the steps involved:

1. Create user interface in React and add all the necessary hooks like use State, use Effect, use Ref in it. Create different component like profile component, followers component, posts component etc.
2. Create a server using Node.js and Express.js . Also create API routes to handle all the request from the React.
3. Create Mongoose schema in Node.js to model your user and posts data.    
4. Connect MongoDB free Atlas database to store all the user and posts data.

## Step 1 : Create frontend using React

### **Setting Up the React.js Project**

Let's start by setting up a new React.js project. Open your terminal and execute the following commands:

```jsx
npx create-react-app social
```

This will create a new React.js project in a directory named **`social`**. Next, open the project in your preferred code editor.

```jsx
cd social
```

### **Installing Dependencies**

```jsx
npm install react-router-dom
npm install @mui/icons-material @mui/material @emotion/styled @emotion/react
npm install @iconscout/react-unicons
```

1. Create folder structure inside the root folder like the following :

```jsx
src
├── App.css
├── App.js
├── Data
│   ├── FollowersData.js
│   ├── PostsData.js
│   └── TrendData.js
├── components
│   ├── FollowersCard
│   │   ├── FollowersCard.css
│   │   └── FollowersCard.jsx
│   ├── InfoCard
│   │   ├── InfoCard.css
│   │   └── InfoCard.jsx
│   ├── LogoSearch
│   │   ├── LogoSearch.css
│   │   └── LogoSearch.jsx
│   ├── Post
│   │   ├── Post.css
│   │   └── Post.jsx
│   ├── PostShare
│   │   ├── PostShare.css
│   │   └── PostShare.jsx
│   ├── PostSide
│   │   ├── PostSide.css
│   │   └── PostSide.jsx
│   ├── Posts
│   │   ├── Posts.css
│   │   └── Posts.jsx
│   ├── ProfileCard
│   │   ├── ProfileCard.css
│   │   └── ProfileCard.jsx
│   ├── ProfileLeft
│   │   ├── ProfileLeft.css
│   │   └── ProfileLeft.jsx
│   ├── ProfileModal
│   │   └── ProfileModal.jsx
│   ├── RightSide
│   │   ├── RightSide.css
│   │   └── RightSide.jsx
│   ├── ShareModal
│   │   └── ShareModal.jsx
│   ├── TrendCard
│   │   ├── TrendCard.css
│   │   └── TrendCard.jsx
│   └── profileSide
│       ├── ProfileSide.css
│       └── ProfileSide.jsx
├── img
│   ├── comment.png
│   ├── cover.jpg
│   ├── home.png
│   ├── img-01.png
│   ├── img1.png
│   ├── img2.png
│   ├── img3.png
│   ├── img4.jpg
│   ├── like.png
│   ├── logo.png
│   ├── noti.png
│   ├── notlike.png
│   ├── postpic1.jpg
│   ├── postpic2.jpg
│   ├── postpic3.JPG
│   ├── profileImg.jpg
│   └── share.png
├── index.js
└── pages
    ├── Auth
    │   ├── Auth.css
    │   └── Auth.jsx
    ├── Profile
    │   ├── Profile.css
    │   └── Profile.jsx
    └── home
        ├── Home.css
        └── Home.jsx
```

- Add your liked images in the images folder and named them as shown.
- Data folder will show static data that will be used in the project further.
- Component folder will create segregated component files and these component will be called later in the project.

1. Add the following code to the app.js **`app.js`** file : 

```jsx
import "./App.css";
import { Auth, SignUp } from "./pages/Auth/Auth";
import Home from "./pages/home/Home";
import Profile from "./pages/Profile/Profile";
import { BrowserRouter, Routes, Route } from "react-router-dom";

function App() {
  return (
    <BrowserRouter>
      <Routes>
        <Route
          path="/"
          element={
            <div className="App">
              <div className="blur" style={{ top: "-18%", right: "0" }}></div>
              <div className="blur" style={{ top: "36%", left: "-8rem" }}></div>
              <Auth />
            </div>
          }
        />
        <Route
          path="profile"
          element={
            <div className="App">
              <div className="blur" style={{ top: "-18%", right: "0" }}></div>
              <div className="blur" style={{ top: "36%", left: "-8rem" }}></div>
              <Profile />
            </div>
          }
        />
        <Route
          path="home"
          element={
            <div className="App">
              <div className="blur" style={{ top: "-18%", right: "0" }}></div>
              <div className="blur" style={{ top: "36%", left: "-8rem" }}></div>
              <Home />
            </div>
          }
        />
        <Route
          path="signup"
          element={
            <div className="App">
              <div className="blur" style={{ top: "-18%", right: "0" }}></div>
              <div className="blur" style={{ top: "36%", left: "-8rem" }}></div>
              <SignUp />
            </div>
          }
        />
      </Routes>
    </BrowserRouter>
  );
}

export default App;

**Add the following to the App.css :** 

:root {
  --yellow: #99dbff;
  --orange: #17a5f4;
  --black: #242d49;
  --gray: rgba(36, 45, 73, 0.65);
  --profileShadow: 0px 4px 17px 2px rgba(0, 0, 0, 0.25);
  --hrColor: #cfcdcd;
  --cardColor: rgba(255, 255, 255, 0.64);
  --buttonBg: #3db3f3;
  --inputColor: rgba(40, 52, 62, 0.07);
  --photo: #4CB256;
  --video: #4A4EB7;
  --location: #EF5757;
--shedule: #E1AE4A;
}

.App{
  overflow: hidden;
  color: var(--black);
  background: #f3f3f3;
  padding: 1rem 1rem;
}

.blur{
  position: absolute;
  width: 22rem;
  height: 14rem;
  border-radius: 50%;
  background: #a6ddf0;
  filter: blur(72px)
}

.button{
  display: flex;
  align-items: center;
  justify-content: center;
  color: white;
  border:none;
  font-size: large;
  font-weight: 500;
  border-radius: 30px;
  background: var(--buttonBg);
  transition:all 100ms ease-out;
}

.button:hover{
  cursor: pointer;
  color: var(--orange);
  background: transparent;
  border: 2px solid var(--orange);
}

::-webkit-scrollbar{
  display: none;
}
```

App.js contains all the routes to the react application. The App.js file decides which file to open on which route. For example “/signup” route will open the Sign up Component. 

1. Create Sing In/Sign up Page :

![Untitled](https://talktohire.blr1.digitaloceanspaces.com/Untitled.png)

In the  **`src/Auth/Auth.jsx`** file add the following code :

```jsx
import React, { useEffect, useState } from "react";
import "./Auth.css";
import Logo from "../../img/logo.png";
import { Link, useNavigate } from "react-router-dom";

const Auth = () => {
  return (
    <div className="Auth">
      <LogIn />
    </div>
  );
};

function LogIn() {
  const [username, setUsername] = useState('');
  const [password, setPassword] = useState('');
  const navigate = useNavigate();
  useEffect(() => {
    if (localStorage.getItem("userId")) {
      localStorage.removeItem("userId")
    }
  }, [])
  const handleLogin = async (e) => {
    e.preventDefault(); // Prevent the default form submission

    const formData = {
      username: username,
      password: password
    };

    console.log("FormData", formData)
    try {
      const response = await fetch('http://localhost:8080/api/users/login', {
        method: 'POST',
        headers: {
          'Content-Type': 'application/json'
        },
        body: JSON.stringify(formData),
      });

      if (response.ok) {
        const resp = await response.json();
        localStorage.setItem("userId", resp.data._id);
        localStorage.setItem("image", resp.data.img);
        localStorage.setItem("followersList", resp.data.followersList);
        localStorage.setItem("name", resp.data.firstName +' ' +resp.data.lastName);
        navigate("/home")
        console.log('User signed up successfully', resp.data)
      } else {
        const errorData = await response.json();
        console.error('Error:', errorData.error);
       }
    } catch (error) {
      console.error('Error:', error);
    }
  }

  return (
    <div className="a-right">
      <form className="infoForm authForm" onSubmit={handleLogin}>
        <h3>Log In</h3>

        <div>
          <input
            type="text"
            placeholder="Username"
            className="infoInput"
            name="username"
            value={username}
            onChange={(e) => setUsername(e.target.value)}
          />
        </div>

        <div>
          <input
            type="password"
            className="infoInput"
            placeholder="Password"
            name="password"
            value={password}
            onChange={(e) => setPassword(e.target.value)}
          />
        </div>

        <div>
          <span style={{ fontSize: "12px" }}>
            Don't have an account <Link to={"/signUp"} >Sign up</Link>
          </span>
          <button className="button infoButton">Login</button>
        </div>
      </form>
    </div>
  );
}

const SignUp = () => {
  useEffect(() => {
    if (localStorage.getItem("userId")) {
      localStorage.removeItem("userId")
    }
  }, [])
  return (
    <Authenticate />
  )
}

function Authenticate() {
  const [username, setUsername] = useState('');
  const [lastname, setLastname] = useState('');
  const [firstName, setFirstName] = useState('');
  const [password, setPassword] = useState('')

  const handleSignup = async (event) => {
    event.preventDefault(); // Prevent the default form submission

    const formData = {
      firstName: firstName,
      lastName: lastname,
      username: username,
      password: password
    };

    console.log("FormData", formData)
    try {
      const response = await fetch('http://localhost:8080/api/users/signup', {
        method: 'POST',
        headers: {
          'Content-Type': 'application/json'
        },
        body: JSON.stringify(formData),
      });

      if (response.ok) {
        console.log('User signed up successfully');
        // Redirect or show success message
      } else {
        const errorData = await response.json();
        console.error('Error:', errorData.error);
        // Handle error (e.g., show error message to the user)
      }
    } catch (error) {
      console.error('Error:', error);
      // Handle error (e.g., show error message to the user)
    }
  };

  return (
    <div className="a-right">
      <form className="infoForm authForm" onSubmit={handleSignup}>
        <h3>Sign up</h3>

        <div>
          <input
            type="text"
            placeholder="First Name"
            className="infoInput"
            name="firstName"
            value={firstName}
            onChange={(e) => setFirstName(e.target.value)}
          />
          <input
            type="text"
            placeholder="Last Name"
            className="infoInput"
            name="lastName"
            value={lastname}
            onChange={(e) => setLastname(e.target.value)}
          />
        </div>

        <div>
          <input
            type="text"
            className="infoInput"
            name="username"
            placeholder="Username"
            value={username}
            onChange={(e) => setUsername(e.target.value)}
          />
        </div>

        <div>
          <input
            type="password"
            className="infoInput"
            name="password"
            placeholder="Password"
            value={password}
            onChange={(e) => setPassword(e.target.value)}
          />
          <input
            type="password"
            className="infoInput"
            name="confirmPassword"
            placeholder="Confirm Password"

          />
        </div>

        <div>
          <span style={{ fontSize: '12px' }}>Already have an account.<Link to={"/"}>Login</Link> </span>
        </div>
        <button className="button infoButton" >Signup</button>
      </form>
    </div>
  );
}

export { Auth, SignUp };

```

This will probably give some error because we have not added any backend and images in the images folder. You will be able to see the login page like the following when project get complete :

 

![Untitled](https://talktohire.blr1.digitaloceanspaces.com/e1bad6fa-b971-4a99-bec6-6f4df3442022.png)

1. Create home page by adding the following code in the **`src/home/Home.jsx` :**

```jsx
import React, { useEffect } from 'react'
import PostSide from '../../components/PostSide/PostSide'
import ProfileSide from '../../components/profileSide/ProfileSide'
import RightSide from '../../components/RightSide/RightSide'
import './Home.css';
import { useNavigate } from 'react-router-dom';
const Home = () => {
  const navigate = useNavigate()
  useEffect(() => {
    if (!localStorage.getItem("userId")) {
      navigate("/")
    }
  }, [])
  return (
    <div className="Home">
      <ProfileSide />
      <PostSide />
      <RightSide />
    </div>
  )
}

export default Home

**Add the following code to the home.css file :**
.Home{
    position: relative;
    display: grid;
    grid-template-columns: 18rem auto 20rem;
    gap: 1rem;
}
```

This code simply imports component from component folder and then adds into home [component.](http://component.Now) Since we haven’t added any component folder code it will show an error. Simply ignore it.

![Untitled](https://talktohire.blr1.digitaloceanspaces.com/Untitled_2.png)

1. Create a profile page by adding the following code in the **`src/Profile/Profile.jsx` :**

```jsx
import React, { useEffect } from 'react'
import PostSide from '../../components/PostSide/PostSide'
import ProfileCard from '../../components/ProfileCard.jsx/ProfileCard'
import ProfileLeft from '../../components/ProfileLeft/ProfileLeft'
import RightSide from '../../components/RightSide/RightSide'
import './Profile.css'
import ProfileSide from '../../components/profileSide/ProfileSide'
import { useNavigate } from 'react-router-dom'
const Profile = () => {
  const navigate = useNavigate()
  useEffect(() => {
    if (!localStorage.getItem("userId")) {
      navigate("/")
    }
  }, [])
  return (
    <div className="Profile">
      <ProfileLeft />
      <div className="Profile-center">
        <ProfileCard />
        <PostSide />
      </div>

      <RightSide />
    </div>
  )
}

export default Profile
**Add the following in the Profile.css file :**
.Profile{
    position: relative;
    display: grid;
    grid-template-columns: 18rem auto 20rem;
    gap: 1rem;
}

.Profile-center{
    display: flex;
    flex-direction: column;
    gap: 1rem;
}
```

![Untitled](https://talktohire.blr1.digitaloceanspaces.com/Untitled_2.png)

1. Now we will be adding code inside the component folder. In **`src/component/FollowersCard/FollowersCard.jsx`** add the following code :

```jsx
import React, { useEffect, useState } from 'react'
import './FollowersCard.css'

const FollowersCard = () => {
    const [users, setUsers] = useState([]);
    const [unfollow, setUnfollow] = useState([])
    const fetchUsers = async () => {
        const response = await fetch("http://localhost:8080/api/users/fetchUsers");
        const converted = await response.json()
        console.log("all users", converted)
        setUsers(converted)
    }
    useEffect(() => {
        fetchUsers()
        setUnfollow(localStorage.getItem("followersList"))
    }, [])

    const handleFollow = async (follwoerId) => {
        const formData = {
            userId: localStorage.getItem("userId"),
            followerId: follwoerId._id

        }
        const response = await fetch('http://localhost:8080/api/profile/follow', {
            method: 'POST',
            headers: {
                'Content-Type': 'application/json'
            },
            body: JSON.stringify(formData),
        });
        const userData = await response.json()
        setUnfollow(userData.followersList)
        localStorage.setItem("followersList" ,userData.followersList )
        console.log("Posting..... info response", userData);
    }
    return (
        <div className="FollowersCard">
            <h3>People you may follow</h3>

            {users.length > 0 ? users.filter(item=>!localStorage.getItem("name").startsWith(item.firstName)).map((follower, id) => {
                return (
                    <div className="follower">
                        <div>
                            <img src={follower.img} alt="" className='followerImage' />
                            <div className="name">
                                <span>{follower.firstName}</span>
                                <span>@{follower.username}</span>
                            </div>
                        </div>
                        <button className='button fc-button' onClick={() => handleFollow(follower)}>
                           {unfollow?.includes(follower._id) ? 'Unfollow' : 'Follow'} 
                        </button>
                    </div>
                )
            }) : null}
        </div>
    )
}

export default FollowersCard
**Add the Follwing in the FollowersCard.css file :**

.FollowersCard{
    width: 100%;
    border-radius: 0.7rem;
    gap: 1rem;
    display: flex;
    flex-direction: column;
    font-size: 13px;
}

.follower{
    display: flex;
    justify-content: space-between;
    align-items: center;
}

.follower>div{
    display: flex;
    gap: 10px;
}

.followerImage{
    width: 3.2rem;
    height: 3.2rem;
    border-radius: 50%;
}

.name{
    display: flex;
    flex-direction: column;
    align-items: flex-start;
    justify-content: center;
}

.name>span:nth-of-type(1){
    font-weight: bold;
}

.fc-button{
    height: 2rem;
    padding-left: 20px;
    padding-right: 20px;
}
```

This component is used to show followers in the home section.

![Untitled](https://talktohire.blr1.digitaloceanspaces.com/Untitled_3.png)

1. Inside the **`src/component/InfoCard/InfoCard.jsx`** add the following code :

```jsx
import React, { useState } from "react";
import "./InfoCard.css";
import { UilPen } from "@iconscout/react-unicons";
import ProfileModal from "../ProfileModal.jsx/ProfileModal";

const InfoCard = () => {
  const [modalOpened, setModalOpened] = useState(false);
  return (
    <div className="InfoCard">
      <div className="infoHead">
        <h4>Your Info</h4>
        <div>
          <UilPen
            width="2rem"
            height="1.2rem"
            onClick={() => setModalOpened(true)}
          />
          <ProfileModal
            modalOpened={modalOpened}
            setModalOpened={setModalOpened}
          />
        </div>
      </div>

      <div className="info">
        <span>
          <b>Status </b>
        </span>
        <span>in Relationship</span>
      </div>

      <div className="info">
        <span>
          <b>Lives in </b>
        </span>
        <span>Multan</span>
      </div>

      <div className="info">
        <span>
          <b>Works at </b>
        </span>
        <span>Zainkeepscode inst</span>
      </div>

      <button className="button logout-button">Logout</button>
    </div>
  );
};

export default InfoCard;
**Add the follwoing code in Infocard.css file :**

.InfoCard
{
    display: flex;
    flex-direction: column;
    gap: 0.75rem;
    background-color: var(--cardColor);
    padding: 1rem;
    border-radius: 1rem;
    width: 90%;
}

.infoHead{
    display: flex;
    justify-content: space-between;
    align-items: center;
}

.infoHead>div:hover{
    cursor: pointer;
}

.logout-button{
    width: 7rem;
    height: 2rem;
    margin-top: 6rem;
    align-self: flex-end;
}
```

This component is used to show information in the profile section.

![Untitled](https://talktohire.blr1.digitaloceanspaces.com/Untitled_4.png)

1. Inside the **`src/component/LogoSearch/LogoSearch.jsx`** add the following code :

```jsx
import React from 'react'
import Logo from '../../img/logo.png'
import './LogoSearch.css'
const LogoSearch = () => {
  return (
   <div className="LogoSearch">
       <img src={Logo} alt="" />
   </div>
  )
}

export default LogoSearch
**Add the following code in the LogoSearch.css file:**
.LogoSearch{
    display: flex;
    gap: 0.75rem;
}

.Search{
    display: flex;
    background-color: var(--inputColor);
    border-radius: 10px;
    padding: 5px;
}

.Search>input{
    background-color: transparent;
    border:none;
    outline: none;
}

.s-icon{
    display: flex;
    align-items: center;
    justify-content: center;
    background: linear-gradient(106.23deg, #f99827, #f95f35 100%);
    border-radius: 5px;
    padding: 4px;
    color: white;
}

.s-icon:hover{
    cursor: pointer;
}
```

This component contains code that will be used in the home component for displaying header photo.

1. Inside the **`src/component/Post/Post.jsx`** add the following code :

```jsx
import React, { useEffect, useState } from "react";
import "./Post.css";
import Comment from "../../img/comment.png";
import Share from "../../img/share.png";
import Heart from "../../img/like.png";
import NotLike from "../../img/notlike.png";

const Post = ({ data, attribute }) => {
  const [liked, setLiked] = useState('')
  const url = attribute.format === "image" ? `data:image/jpeg;base64,${data}` : `data:video/mp4;base64,${data}`;

  const handleLikes = async () => {
    console.log("Attributes", attribute);
    const formData = {
      _id: attribute._id,
      userId: localStorage.getItem("userId")
    }
    const response = await fetch('http://localhost:8080/api/profile/like', {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json'
      },
      body: JSON.stringify(formData),
    });
    console.log("like response", response)
    if (response.ok) {
      const resp = await response.json();
      console.log("Result : ", resp)
    }
  }

  return (
    <div className="Post">
      {attribute.format === "image" ? <img src={url} alt="imagePreviewx" /> : <video src={url} alt="videoplayer" controls style={{ maxWidth: '100%' }} />}

      <div className="postReact">
        <img src={attribute.likedUser.includes()? Heart : NotLike} alt="" style={{ cursor: "pointer" }} onClick={handleLikes} />
        <img src={Comment} alt="" style={{ cursor: "pointer" }} />
        <img src={Share} alt="" style={{ cursor: "pointer" }} />
      </div>
      <span style={{ color: "var(--gray)", fontSize: "12px" }}>
        {liked.likes} likes
      </span>
      <div className="detail">
        <span>
          <b>{attribute.name}</b>
        </span>
        <span> {attribute.desc === undefined ? attribute.desc : null}</span>
      </div>
    </div>
  );
};

export default Post;
**Add the following code to the Post.css file:**
.Post
{
    display: flex;
    flex-direction: column;
    padding: 1rem;
    background-color: var(--cardColor);
    border-radius: 1rem;
    gap: 1rem;
}

.Post>img{
    width: 100%;
    max-height: 20rem;
    object-fit: cover;
    border-radius: 0.5rem;
}

.postReact{
    display: flex;
    align-items: flex-start;
    gap: 1.5rem;
}
```