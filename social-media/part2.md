
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