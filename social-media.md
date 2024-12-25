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

This component will display all the post inside the post slide component.

1. Inside the **`src/component/Posts/Posts.jsx`** add the following code :

```jsx
import React, { useEffect, useState } from 'react'
import './Posts.css'
import { PostsData } from '../../Data/PostsData'
import Post from '../Post/Post'
import Comment from "../../img/comment.png";
import Share from "../../img/share.png";
import Heart from "../../img/like.png";
import NotLike from "../../img/notlike.png";

const Posts = () => {
  const [liked, setLiked] = useState('')
  const [posts, setPosts] = useState([])

  const fetchPosts = async () => {
    const response = await fetch("http://localhost:8080/api/posts/fetchAllPosts");
    const converted = await response.json()
    setPosts(converted)
  }
  useEffect(() => {
    fetchPosts()
  }, [])

  const handleLikes = async (post) => {
    console.log("Attributes", post);
    const formData = {
      _id: post._doc._id,
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
      fetchPosts()
    }
  }

  return (
    <div className="Posts">
      {posts.length > 0 ? posts.reverse().map((post, id) => {
        // return <Post data={post.postBase64} id={id} attribute={post._doc} />
        return (<div className="Post" id={id}>
          {post._doc.format === "image" ? <img src={post._doc.format === "image" ? `data:image/jpeg;base64,${post.postBase64}` : `data:video/mp4;base64,${post.postBase64}`} alt="imagePreviewx" /> : <video src={post._doc.format === "image" ? `data:image/jpeg;base64,${post.postBase64}` : `data:video/mp4;base64,${post.postBase64}`} alt="videoplayer" controls style={{ maxWidth: '100%' }} />}

          <div className="postReact">
            <img src={post._doc.likedUser.includes(localStorage.getItem("userId")) ? Heart : NotLike} alt="" style={{ cursor: "pointer" }} onClick={() => handleLikes(post)} />
            <img src={Comment} alt="" style={{ cursor: "pointer" }} />
            <img src={Share} alt="" style={{ cursor: "pointer" }} />
          </div>
          <span style={{ color: "var(--gray)", fontSize: "12px" }}>
            {post._doc.likes} likes 
          </span>
          <div className="detail">
            <span>
              <b>{post._doc.name}</b>
            </span>
            <span> {post._doc.desc === undefined ? post._doc.desc : null}</span>
          </div>
        </div>)
      }) : null}
    </div>
  )
}

export default Posts
Add the following code to the Posts.css
.Posts{
    display: flex;
    flex-direction: column;
    gap: 1rem;
    
}

.commentinput{
    width: 60%;
    margin-left: 25%;
    margin-right: auto;
    height: 30px;
    /* margin-left: 10%; */
    border: 1px solid rgb(4, 115, 143);
    border-radius: 9px;
}
```

![Untitled](https://talktohire.blr1.digitaloceanspaces.com/Untitled_5.png)

1. Inside the **`src/component/PostShare/PostShare.jsx`** add the following code :

```jsx
import React, { useState, useRef } from "react";
import ProfileImage from "../../img/profileImg.jpg";
import "./PostShare.css";
import { UilScenery } from "@iconscout/react-unicons";
import { UilPlayCircle } from "@iconscout/react-unicons";
import { UilLocationPoint } from "@iconscout/react-unicons";
import { UilSchedule } from "@iconscout/react-unicons";
import { UilTimes } from "@iconscout/react-unicons";

const PostShare = () => {
  const [image, setImage] = useState(null);
  const imageRef = useRef();
  const videoRef = useRef();
  const [video, setVideo] = useState(null)
  const [desc, setDesc] = useState()

  const onImageChange = async (event) => {
    if (event.target.files && event.target.files[0]) {
      let img = event.target.files[0];
      let reader = new FileReader();
      reader.onload = function (event) {
        // `event.target.result` contains the base64 string representing the image
        setImage({
          image: URL.createObjectURL(img),
          base64String: event.target.result
        });
      };
      reader.readAsDataURL(img);
      event.target.value = null;
    }
  }

  const postImage = async (e) => {
    e.preventDefault();
    if (image !== null) {
      console.log("Hit in the image");
      try {
        const formData = new FormData();
        formData.append("images", image.base64String);
        formData.append("name", "Tzuyu");
        formData.append("userId", localStorage.getItem("userId"));
        formData.append("desc", desc);
        formData.append("likes", 0);
        formData.append("liked", false);

        const response = await fetch("http://localhost:8080/api/posts/upload", {
          method: "POST",
          body: formData,
        });

        if (response.ok) {
          console.log("Image uploaded successfully:");
        } else {
          console.error("Error uploading image:");
        }
      } catch (error) {
        console.error("Error uploading image:", error);
      }
    }
    else {
      console.log("Hit in the video")
      try {
        const formData = new FormData();
        formData.append("images", video.base64String);
        formData.append("name", "Tzuyu");
        formData.append("userId", localStorage.getItem("userId"));
        formData.append("desc", desc);
        formData.append("likes", 0);
        formData.append("liked", false);

        const response = await fetch("http://localhost:8080/api/posts/upload/video", {
          method: "POST",
          body: formData,
        });

        if (response.ok) {
          imageRef.current = null;
          setImage()
          setDesc("")
          console.log("Image uploaded successfully:");
        } else {
          console.error("Error uploading image:");
        }
      } catch (error) {
        console.error("Error uploading image:", error);
      }
    }
    imageRef.current = null;
    setImage(null)
    setDesc("")
    setVideo(null)
    videoRef.current = null;
  };

  const onVideoChange = async (event) => {
    event.preventDefault();
    if (event.target.files && event.target.files[0]) {
      let img = event.target.files[0];
      let reader = new FileReader();
      reader.onload = function (event) {
        setVideo({
          video: URL.createObjectURL(img),
          base64String: event.target.result
        });
      };
      reader.readAsDataURL(img);
      // event.target.value = null;
    }

  }

  return (
    <div className="PostShare">
      <img src={localStorage.getItem("image")} alt="" />
      <div>
        <div className="InputContainer">
          <input
            placeholder="What's happening ?!"
            type="text"
            id="files"
            name="file"
            className="input"
            value={desc}
            onChange={(e) => setDesc(e.target.value)}
          />
        </div>
        <div className="postOptions">
          <div
            className="option"
            style={{ color: "var(--photo)" }}
            onClick={() => imageRef.current.click()}
          >
            <UilScenery style={{ marginRight: 5 }} />
            Photo
          </div>
          <div className="option" style={{ color: "var(--video)" }} onClick={() => videoRef.current.click()}>
            <UilPlayCircle style={{ marginRight: 5 }} />
            Video
          </div>{" "}
          <button className="button-share " onClick={postImage}>
            Share
          </button>
          <div style={{ display: "none" }}>
            <input
              type="file"
              name="file"
              ref={imageRef}
              onChange={onImageChange}
            />
            <input
              type="file"
              // accept="video/*"
              name="videoFile"
              ref={videoRef}
              onChange={onVideoChange}
            />
          </div>
        </div>
        {image && (
          <div className="previewImage">
            <UilTimes onClick={() => setImage(null)} />
            <img src={image.image} alt="" />
          </div>
        )}

        {video && (
          <div className="previewImage">
            <video src={video.video} controls style={{ maxWidth: '100%' }}>
              Your browser does not support the video tag.
            </video>
          </div>
        )}
      </div>
    </div>
  );
};

export default PostShare;
**Add the following code in the PostShare.css file :**
.PostShare {
    display: flex;
    gap: 1rem;
    background-color: var(--cardColor);
    padding: 1rem;
    border-radius: 1rem;
}

.PostShare>img {
    border-radius: 50%;
    width: 3rem;
    height: 3rem;
}

.PostShare>div {
    display: flex;
    flex-direction: column;
    width: 90%;
    gap: 1rem;
}

.PostShare>div>input {
    background-color: var(--inputColor);
    border-radius: 10px;
    padding: 10px;
    font-size: 17px;
    border: none;
    outline: none;
}

.postOptions {
    display: flex;
    justify-content: space-around;
}

.option {
    padding: 5px;
    padding-left: 10px;
    padding-right: 10px;
    border-radius: 10px;
    display: flex;
    align-items: center;
    justify-content: center;
    font-size: 16px;
    font-family: sans-serif;
}

.option:hover {
    cursor: pointer;
}

.ps-button {
    padding: 5px;
    padding-left: 20px;
    padding-right: 20px;
    font-size: 12px
}

.previewImage {
    position: relative;
}

.previewImage>svg {
    position: absolute;
    right: 1rem;
    top: 0.5rem;
    cursor: pointer;
}

.previewImage>img {
    width: 100%;
    max-height: 20rem;
    object-fit: cover;
    border-radius: 0.5rem;
}

.button-share {
    display: flex;
    align-items: center;
    justify-content: center;
    color: white;
    border: 2px solid var(--orange);
    padding-top: 10px;
    padding-bottom: 10px;
    padding-left: 15px;
    padding-right: 15px;
    font-size: medium;
    font-weight: 500;
    border-radius: 25px;
    background: var(--buttonBg);
    transition: all 100ms ease-out;
}

.button-share:hover {
    cursor: pointer;
    color: var(--orange);
    background: transparent;
    border: 2px solid var(--orange);
}

.InputContainer {
    width: 100%;
    height: 50px;
    display: flex;
    align-items: center;
    justify-content: center;
    background: #b7e6ff;
    border-radius: 30px;
    overflow: hidden;
    cursor: pointer;
    box-shadow: 2px 2px 10px rgba(0, 0, 0, 0.075);
  }
  
  .input {
    width: 100%;
    height: 45px;
    border: none;
    outline: none;
    caret-color: rgb(255, 81, 0);
    background-color: rgb(255, 255, 255);
    border-radius: 30px;
    padding-left: 15px;
    letter-spacing: 0.8px;
    color: rgb(19, 19, 19);
    font-size: 13.4px;
  }
```

![Untitled](https://talktohire.blr1.digitaloceanspaces.com/Untitled_6.png)

1. Inside the **`src/component/PostSlide/PostSlide.jsx`** add the following code :

```jsx
import React from 'react'
import Posts from '../Posts/Posts'
import PostShare from '../PostShare/PostShare'
import './PostSide.css'
const PostSide = () => {
  return (
   <div className="PostSide">
       <PostShare/>
       <Posts/>
   </div>
  )
}

export default PostSide
Add the following code in the PostSlide.css file:
.PostSide{
    display: flex;
    flex-direction: column;
    gap: 1rem;
    height: 100vh;
    overflow: auto;
}
```

1. Inside the **`src/component/ProfileCard/ProfileCard.jsx`** add the following code :

```jsx
import React, { useEffect, useState } from "react";
import Cover from "../../img/cover.jpg";
import Profile from "../../img/profileImg.jpg";
import "./ProfileCard.css";

const ProfileCard = () => {
const [profileData,setProfileData] = useState('')
  const fetchInfo = async () => {
    const formData = {
      userId : localStorage.getItem("userId")
    }
    const response = await fetch('http://localhost:8080/api/profile/data', {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json'
      },
      body: JSON.stringify(formData),
    });
    console.log("Fecthing profile info response", response)
    if (response.ok) {
      const resp = await response.json();
      setProfileData(resp)
    }
  }

  useEffect(() => {
    fetchInfo()
  }, [])

  const ProfilePage = true;
  return (
    <div className="ProfileCard">
      <div className="ProfileImages">
        <img src={Cover} alt="" />
        <img src={localStorage.getItem("image")} alt="" />
      </div>

      <div className="ProfileName">
        <span>{localStorage.getItem("name")}</span>
      </div>

      <div className="followStatus">
        <div>
          <div className="follow">
            <span>{profileData?.followings}</span>
            <span className="textbased">Followings</span>
          </div>
          {/* <div className="vl"></div> */}
          <div className="follow">
            <span>{profileData?.followers}</span>
            <span>Followers</span>
          </div>

          {ProfilePage && (
            <>
              {/* <div className="vl"></div> */}
              <div className="follow">
                <span>{profileData?.posts}</span>
                <span>Posts</span>
              </div>
            </>
          )}
        </div>
      </div>
      {ProfilePage ? "" : <span>My Profile</span>}
    </div>
  );
};

export default ProfileCard;
Add the following code to Profile.css file :
.ProfileCard{
    border-radius: 1.5rem;
    display: flex;
    flex-direction: column;
    position: relative;
    gap: 1rem;
    overflow-x: clip;
    background: var(--cardColor);
}

.ProfileImages{
    position: relative;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
}

.ProfileImages>img:nth-of-type(1){
    width: 100%;
}

.ProfileImages>img:nth-of-type(2)
{
    width: 6rem;
    border-radius: 50%;
    position: absolute;
    bottom: -3rem;
    box-shadow: var(--profileShadow);
}

.ProfileName{
    display: flex;
    flex-direction: column;
    align-items: center;
    margin-top: 3rem;
    gap: 10px;
}

.ProfileName>span:nth-of-type(1)
{
    font-weight: bold;
}

/* Follow Status */
.followStatus {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    gap: 0.75rem;
    padding: 20px;
  }
  
  .followStatus > hr {
    width: 85%;
    border: 1px solid var(--hrColor);
  }
  
  .followStatus > div {
    display: flex;
    gap: 1rem;
    width: 80%;
    justify-content: space-around;
    align-items: center;
  }
  
  .follow {
    display: flex;
    flex-direction: column;
    gap: 0.4rem;
    align-items: center;
    justify-content: center;
    font-weight: 500;
  }
  .follow > span:nth-of-type(1) {
    font-weight: bold;
  }
  
  .follow > span:nth-of-type(2) {
    color: var(--gray);
    font-size: 13px;
  }
  
  .vl {
    height: 150%;
    border-left: 2px solid var(--hrColor);
  }

  .ProfileCard>span{
      font-weight: bold;
      color: orange;
      align-self: center;
      margin-bottom: 1rem;
      cursor: pointer;
  }

  .textbased{
    font-weight: 500;
  }
```

1. Inside the **`src/component/ProfileLeft/ProfileLeft.jsx`** add the following code :

```jsx
import React from 'react';
import { BrowserRouter as Router, Route, Link } from "react-router-dom";
import FollowersCard from '../FollowersCard/FollowersCard'
import InfoCard from '../InfoCard/InfoCard';
import HomeIcon from "@mui/icons-material/Home";
import LogoSearch from '../LogoSearch/LogoSearch'
import ProfileSide from '../profileSide/ProfileSide';
import { UilSetting } from "@iconscout/react-unicons";
import AccountCircleIcon from "@mui/icons-material/AccountCircle";
import LogoutIcon from "@mui/icons-material/Logout";

const ProfileLeft = () => {
  return (
    <div className="ProfileSide">
      <InfoCard />
      <div className="Menu">
        <Link to="/home">
          <div className="menu-items">
            <HomeIcon style={{ marginRight: 10, color: "#3db3f3" }} />
            Home
          </div>
        </Link>
        <Link to="/profile">
          <div className="menu-items">
            <AccountCircleIcon
              style={{ marginRight: 10, color: "#3db3f3" }}
            />
            Profile
          </div>
        </Link>
        <Link to="/settings">
          <div className="menu-items">
            <UilSetting style={{ marginRight: 10, color: "#3db3f3" }} />
            Settings
          </div>
        </Link>
        <Link to="/">
          <div className="menu-items">
            <LogoutIcon style={{ marginRight: 10, color: "#3db3f3" }} />
            Log Out
          </div>
        </Link>
      </div>
    </div>
  )
}

export default ProfileLeft;
```

![Untitled](https://talktohire.blr1.digitaloceanspaces.com/Untitled_7.png)

1. Inside the **`src/component/ProfileModal/ProfileModal.jsx`** add the following code :

```jsx
import { Modal, useMantineTheme } from "@mantine/core";

function ProfileModal({ modalOpened, setModalOpened }) {
  const theme = useMantineTheme();

  return (
    <Modal
      overlayColor={
        theme.colorScheme === "dark"
          ? theme.colors.dark[9]
          : theme.colors.gray[2]
      }
      overlayOpacity={0.55}
      overlayBlur={3}
      size="55%"
      opened={modalOpened}
      onClose={() => setModalOpened(false)}
    >
      <form className="infoForm">
        <h3>Your info</h3>

        <div>
          <input
            type="text"
            className="infoInput"
            name="FirstName"
            placeholder="First Name"
          />

          <input
            type="text"
            className="infoInput"
            name="LastName"
            placeholder="Last Name"
          />
        </div>

        <div>
          <input
            type="text"
            className="infoInput"
            name="worksAT"
            placeholder="Works at"
          />
        </div>

        <div>
          <input
            type="text"
            className="infoInput"
            name="livesIN"
            placeholder="LIves in"
          />

          <input
            type="text"
            className="infoInput"
            name="Country"
            placeholder="Country"
          />
        </div>

        <div>
          <input
            type="text"
            className="infoInput"
            placeholder="RelationShip Status"
          />
        </div>

        <div>
            Profile Image 
            <input type="file" name='profileImg'/>
            Cover Image
            <input type="file" name="coverImg" />
        </div>

        <button className="button infoButton">Update</button>
      </form>
    </Modal>
  );
}

export default ProfileModal;
```

1. Inside the **`src/component/ProfileSide/ProfileSide.jsx`** add the following code :

```jsx
import React from "react";
import ProfileCard from "../ProfileCard.jsx/ProfileCard";
import { UilSetting } from "@iconscout/react-unicons";
import HomeIcon from "@mui/icons-material/Home";
import { BrowserRouter as Router, Route, Link } from "react-router-dom";
import AccountCircleIcon from "@mui/icons-material/AccountCircle";
import LogoutIcon from "@mui/icons-material/Logout";

import "./ProfileSide.css";
const ProfileSide = () => {
  return (
      <div className="ProfileSide">
        <ProfileCard />
        <div className="Menu">
          <Link to="/">
            <div className="menu-items">
              <HomeIcon style={{ marginRight: 10, color: "#3db3f3" }} />
              Home
            </div>
          </Link>
          <Link to="/profile">
            <div className="menu-items">
              <AccountCircleIcon
                style={{ marginRight: 10, color: "#3db3f3" }}
              />
              Profile
            </div>
          </Link>
          <Link to="/settings">
            <div className="menu-items">
              <UilSetting style={{ marginRight: 10, color: "#3db3f3" }} />
              Settings
            </div>
          </Link>
          <Link to="/" >
            <div className="menu-items" >
              <LogoutIcon style={{ marginRight: 10, color: "#3db3f3" }} />
              Log Out
            </div>
          </Link>
        </div>
      </div>
  );
};

export default ProfileSide;
Add the following code in the ProfileSide.css
.ProfileSide {
    display: flex;
    flex-direction: column;
}

.Menu {
    margin-top: 15%;
    display: flex;
    flex-direction: column;
    gap: 3rem;
    margin-left: auto;
    margin-right: auto;
}

.menu-items {
    padding-top: 10px;
    padding-bottom: 10px;
    padding-left: 30px;
    padding-right: 30px;
    border-radius: 10px;
    cursor: pointer;
    display: flex;
    flex-direction: row;
    font-family: sans-serif;
    font-weight: 600;
    font-size: larger;
    color:#4f5050 ; 
}
```

![Untitled](https://talktohire.blr1.digitaloceanspaces.com/Untitled_8.png)

1. Inside the **`src/component/RightSide/RightSide.jsx`** add the following code :

```jsx
import React, { useState } from "react";
import "./RightSide.css";
import TrendCard from "../TrendCard/TrendCard";
import ShareModal from "../ShareModal/ShareModal";
import FollowersCard from "../FollowersCard/FollowersCard";

const RightSide = () => {
  const [modalOpened, setModalOpened] = useState(false);
  return (
    <div className="RightSide">
      <FollowersCard />
      <button className="button r-button" onClick={() => setModalOpened(true)}>
        Share
      </button>
      <ShareModal modalOpened={modalOpened} setModalOpened={setModalOpened} />
      <TrendCard />
    </div>
  );
};

export default RightSide;
Add the following code in the RightSide.css file:
.RightSide{
    display: flex;
    flex-direction: column;
    gap: 2rem;
}
.navIcons{
    margin-top: 1rem;
    display: flex;
    justify-content: space-between;
}
.navIcons>img{
    width: 1.5rem;
    height: 1.5rem;
}
.r-button{
    height: 3rem;
    width: 80%;
    align-self: center;
}
```

![Untitled](https://talktohire.blr1.digitaloceanspaces.com/Untitled_9.png)

1. Inside the **`src/component/ShareModal/ShareModal.jsx`** add the following code :

```jsx
import { Modal, useMantineTheme } from "@mantine/core";
import PostShare from "../PostShare/PostShare";

function ShareModal({ modalOpened, setModalOpened }) {
  const theme = useMantineTheme();

  return (
    <Modal
      overlayColor={
        theme.colorScheme === "dark"
          ? theme.colors.dark[9]
          : theme.colors.gray[2]
      }
      overlayOpacity={0.55}
      overlayBlur={3}
      size="55%"
      opened={modalOpened}
      onClose={() => setModalOpened(false)}
    >
    <PostShare/>
    </Modal>
  );
}

export default ShareModal;
```

1. Inside the **`src/component/TrendCard/TrendCard.jsx`** add the following code :

```jsx
import React from 'react'
import './TrendCard.css'

import {TrendData} from '../../Data/TrendData.js'
const TrendCard = () => {
  return (
    <div className="TrendCard">
            <h3>Trends for you</h3>
            {TrendData.map((trend)=>{
                return(
                    <div className="trend">
                        <span>#{trend.name}</span>
                        <span>{trend.shares}k shares</span>
                    </div>
                )
            })}

    </div>
  )
}

export default TrendCard
Add the following code to TrendCard.css file : 
.TrendCard{
    display: flex;
    flex-direction: column;
    gap: 1rem;
    background-color: var(--cardColor);
    padding: 1rem;
    border-radius: 1rem;
    padding-left: 2rem;
}

.trend{
    display: flex;
    flex-direction: column;
    gap: 0.5rem;
}

.trend>span:nth-of-type(1)
{
    font-weight: bold;
}
.trend>span:nth-of-type(2)
{
    font-size: 13px;
}
```

![Untitled](https://talktohire.blr1.digitaloceanspaces.com/Untitled_10.png)

1. Inside the **`src/Data/TrendData.jsx`** add the following code :

```jsx
export const TrendData= [
    {
      name: "Minions",
      shares: 97,
    },
    {
      name: "Avangers",
      shares: 80.5,
    },
    {
      name: "Zainkeepscode",
      shares: 75.5,
    },
    {
      name: "Reactjs",
      shares: 72,
    },
    {
      name: "Elon Musk",
      shares: 71.9,
    },
    {
      name: "Need for Speed",
      shares: 20,
    },
  ];
 
```

This data will be used in the trends right card.

## Step 2 : Create Backend using Node.js and Express

### **Setting Up the Node.js Project**

Let's start by setting up a new Node.js project. Create a new folder named server inside the root folder ( i.e. : outside the src folder ) Open your terminal and inside the server folder execute the following commands:

```jsx
npm init -y
```

### **Installing Dependencies**

```jsx
npm install express
npm install cors
npm install body-parser mongodb mongoose multer fs-extra
```

1. Create a folder structure like this in the server folder :

```jsx
.
├── models
│   ├── postModels.js
│   └── userModel.js 
├── package-lock.json
├── package.json     
├── routes
│   ├── post.js      
│   ├── profile.js   
│   ├── uploads
│   │   ├── Tzuyu-163559.png  //Random Data
│   │   └── video
│   │       ├── Tzuyu-100208.mp4//Random Data
│   │       └── Tzuyu-385501.mp4//Random Data
│   └── user.js
├── server.js
└── uploads
```

1. Inside the **`server/models/postModels.js`** file add the following code : 

```jsx
const mongoose = require('mongoose');

const postSchema = new mongoose.Schema({
  imgPath: {
    type: String,
    required: true
  },
  name: {
    type: String,
    required: true
  },
  desc: {
    type: String,
    required: true
  },
  format: {
    type: String,
    required: true
  },
  likes: {
    type: Number,
    default: 0
  },
  liked: {
    type: Boolean,
    default: false
  },
  userId: {
    type: mongoose.Schema.Types.ObjectId,
    ref: 'User',
    required: true
  },
  likedUser : [{type : String , default : 0}]
});

const postModel = mongoose.model('Post', postSchema);

module.exports = postModel;
```

This file will create a model for post schema.

1.  Inside the **`server/models/userModel.js`** file add the following code : 

```jsx
const mongoose = require('mongoose');

const userSchema = new mongoose.Schema({
  firstName: String,
  lastName: String,
  username: { type: String, unique: true },
  password: String,
  followings: { type: Number, default: 0 },
  followers: { type: Number, default: 0 },
  followersList: [{ type: String, default: 0 }]
});

module.exports = mongoose.model('User', userSchema);
```

This module will create user schema.

1. Inside the **`server/routes/post.js`** file add the following code : 

```jsx
const express = require('express')
const multer = require('multer')
const path = require("path")
const fs = require('fs');
const postModel = require("../models/postModels");

const app = express()

var upload = multer({ dest: "./uploads" });

function generateRandomNumber() {
  return Math.floor(100000 + Math.random() * 900000);
}

app.get("/fetchAllPosts", async (req, res) => {
  try {
    console.log("Hit")
    const data = await postModel.find();
    const modifiedData = await Promise.all(data.map((post) => {
      // Read the video file content
      const newObj = {...post};
      const videoContent = fs.readFileSync(post.imgPath);
      console.log("Path", post.imgPath)
      // Convert the video content to base64
      const base64Video = videoContent.toString('base64');
      newObj.postBase64 = base64Video;
      return newObj;
    }));
    res.status(200).send(modifiedData)
  }
  catch (err) {
    console.log("Error ", err)
    res.status(500).send({ message: err })
  }
})

app.post('/upload', upload.array('images', 5), (req, res) => {
  try {
    const imageBlob = req.body.images;
    const imageName = `${req.body.name}`;
    const userId  = req.body.userId

    // Create directory if it doesn't exist
    const uploadDir = path.join(__dirname, 'uploads');
    if (!fs.existsSync(uploadDir)) {
      fs.mkdirSync(uploadDir);
    }

    // Construct image path
    const randomSixDigitNumber = generateRandomNumber();

    const imagePath = path.join(uploadDir, `${imageName}-${randomSixDigitNumber}.png`);
    let base64Image = imageBlob.split(';base64,').pop();

    // Write image data to file
    fs.writeFile(imagePath, base64Image, { encoding: 'base64' }, async (err) => {
      if (err) {
        console.error('Error saving image:', err);
        return res.status(500).send('Failed to save image.');
      }
      const postdata = new postModel({
        imgPath: imagePath,
        name: imageName,
        format: "image",
        userId : userId,
        desc: req.body.desc,
        likes: req.body.likes,
        liked: req.body.liked
      });
      await postdata.save();

      console.log('Image saved successfully:', imagePath);
      res.status(200).send('Image uploaded successfully.');
    })
  }
  catch (err) {
    console.log("Error ", err)
    res.status(500).send({ message: err })
  }
});

app.post('/upload/video', upload.array('video', 5), (req, res) => {
  try {
    console.log("Video")
    const imageBlob = req.body.images;
    const imageName = `${req.body.name}`;

    // Create directory if it doesn't exist
    const uploadDir = path.join(__dirname, 'uploads/video');
    if (!fs.existsSync(uploadDir)) {
      fs.mkdirSync(uploadDir);
    }

    // Construct image path
    const randomSixDigitNumber = generateRandomNumber();

    const imagePath = path.join(uploadDir, `${imageName}-${randomSixDigitNumber}.mp4`);
    let base64Image = imageBlob.split(';base64,').pop();

    // Write image data to file
    fs.writeFile(imagePath, base64Image, { encoding: 'base64' }, async (err) => {
      if (err) {
        console.error('Error saving image:', err);
        return res.status(500).send('Failed to save image.');
      }
      const postdata = new postModel({
        imgPath: imagePath,
        name: imageName,
        format: "video",
        desc: req.body.desc,
        likes: req.body.likes,
        liked: req.body.liked,
        userId : req.body.userId
      });
      await postdata.save();

      console.log('Image saved successfully:', imagePath);
      res.status(200).send('Image uploaded successfully.');
    })
  }
  catch (err) {
    console.log("Error ", err)
    res.status(500).send({ message: err })
  }
});

module.exports = app;
```

This module will create three API routes which will handle all the post upload operations.

1. Inside the **`server/routes/profile.js`** file add the following code : 

```jsx
const User = require('./../models/userModel');
const Post = require("../models/postModels");
const express = require("express");
const bodyParser = require("body-parser");
const app = express();

app.use(bodyParser.urlencoded({ extended: true }));
app.use(bodyParser.json());

app.post("/data", async (req, res) => {
    try {
        const { userId } = req.body;
        const user = await Post.countDocuments({ userId });
        const userData = await User.findOne({ _id: userId })
        console.log("USerData", userData)
        const profileData = {
            posts: user,
            followers: userData.followers,
            followings: userData.followings
        }
        res.status(200).json(profileData);
    } catch (error) {
        console.error(error);
        res.status(500).json({ error: "Internal server error" });
    }
});
app.post("/follow", async (req, res) => {
    try {
        const { userId, followerId } = req.body;
        console.log("Getting", req.body)
        const updatedUser = await User.findOneAndUpdate(
            { _id: userId },
            { $push: { followersList: followerId }, $inc: { followings: 1 } },
            { new: true } // This option ensures that the updated document is returned
        );

        console.log("updatedUser", updatedUser)
        res.status(200).json(updatedUser);
    } catch (error) {
        console.error(error);
        res.status(500).json({ error: "Internal server error" });
    }
});
app.post("/like", async (req, res) => {
    try {
        const { _id, userId } = req.body;
        console.log("Getting", req.body);

        // Check if the user has already liked the post
        const userLiked = await Post.findOne({ _id: _id, likedUser: userId });

        if (userLiked) {
            const updatedUser = await Post.findOneAndUpdate(
                { _id: _id, likedUser: userId },
                { $pull: { likedUser: userId }, $inc: { likes: -1 } }, 
                { new: true }
            );

            console.log("updatedUser dec", updatedUser);
            res.status(200).json(updatedUser);
        } else {
            // If the user has not already liked the post, increase the likes count
            const updatedUser = await Post.findOneAndUpdate(
                { _id: _id },
                { $push: { likedUser: userId }, $inc: { likes: 1 } },
                { new: true }
            );

            console.log("updatedUser inc", updatedUser);
            res.status(200).json(updatedUser);
        }
    } catch (error) {
        console.error(error);
        res.status(500).json({ error: "Internal server error" });
    }
});

module.exports = app
```

This api routes will handle all the profile operations.

1. Inside the **`server/routes/user.js`** file add the following code : 

```jsx
const User = require('./../models/userModel');
const express = require("express");
const bodyParser = require("body-parser");

const app = express();

app.use(bodyParser.urlencoded({ extended: true }));
app.use(bodyParser.json());

// Define the route to handle form data
app.post("/signup", async (req, res) => {
    try {
        const { firstName, lastName, username, password } = req.body;
        // Validate input (e.g., check if required fields are provided)

        // Check if the username already exists
        const existingUser = await User.findOne({ username });
        if (existingUser) {
            return res.status(400).json({ error: 'Username already exists' });
        }

        // Create a new user
        const newUser = new User({ firstName, lastName, username, password });

        // Save the user to the database
        await newUser.save();

        res.status(201).json({ message: 'User created successfully' });
    } catch (error) {
        console.error(error);
        res.status(500).json({ error: 'Internal server error' });
    }
});

// Login endpoint
app.post("/login", async (req, res) => {
    try {
        const { username, password } = req.body;
        console.log("data", req.body)
        // Check if the user exists
        const user = await User.findOne({ username });
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
});

app.get("/fetchUsers", async (req, res) => {
    try {
        // Check if the user exists
        const user = await User.find();

        res.status(200).json(user);
    } catch (error) {
        console.error(error);
        res.status(500).json({ error: "Internal server error" });
    }
});

module.exports = app
```

These api routes will handle all the api routes for user operations.

1. Inside the **`server/server.js`** file add the following code : 

```jsx
const express = require("express");
const { MongoClient } = require("mongodb");
const mongoose = require("mongoose")
const cors = require("cors");
const postApi = require("./routes/post");
const userApi = require("./routes/user");
const profileApi = require("./routes/profile");
const bodyparser  = require("body-parser")

const app = express();
const PORT = process.env.PORT || 8080;
app.use(cors({
  origin: ['http://localhost:3000']
}));
app.use(bodyparser.urlencoded({ extended: true }))
app.use(bodyparser.json())
app.use(express.json());
app.use("/api/posts", postApi);
app.use("/api/users", userApi);
app.use("/api/profile",profileApi);

app.get("/", (req, res) => {
  res.send("Hello from Express!");
});

mongoose.connect( //Your own mongodb Atlas Connection string ( FREE )
  "mongodb+srv://root:t@cluster0.0mlseyi.mongodb.net/?retryWrites=true&w=majority&appName=Cluster0",
  {
    bufferCommands: false,
    useNewUrlParser: true,
    useUnifiedTopology: true

  }
)
  .then((client) => {
    console.log("Connected to MongoDB Atlas");

    app.listen(PORT, () => {
      console.log(`Server is running on http://localhost:${PORT}`);
    });
  })
  .catch((err) => {
    console.error("Error connecting to MongoDB Atlas", err);
  });
```

This will run server on 8080. when you run the command **node server** in the terminal.

## Step 3  : Test the application

To test the application run the command **npm start** in the project root and **node server** inside the server folder terminal. After creating 

You will be able to perform the following operations : 

- Like photo and  Unlike photo.
- Follow and unfollow other users.
- Upload photo in the application.
- Upload video in the application.

[socialrun1.mp4](https://drive.google.com/file/d/1oJShYQYacxEFLIGHKy00TTrMS0ymWUK-/view?usp=sharing)