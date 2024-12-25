# How to build Trello clone using Next.js (Beginner)

Trello is a project management tool created by Atlassian. It have boards and card design which lets you manage project in an effective manner. In this tutorial we will be creating a clone of it using next.js. This tutorial is divided into three levels Beginner , Intermediate and Advance. Starting from beginner we will be gradually moving to intermediate and advance level.

## **Setting Up the Next.js Project**

Let's start by setting up a new Next.js project. Open your terminal and execute the following commands:

```jsx
npx create-next-app trello-clone

✔ Would you like to use TypeScript with this project? … No
✔ Would you like to use ESLint with this project? … Yes
✔ Would you like to use Tailwind CSS with this project? … Yes
✔ Would you like to use `src/` directory with this project? … Yes
✔ Use App Router (recommended)? … No
✔ Would you like to customize the default import alias? … No
```

This will create a new Next.js project in a directory named **`trello-clone`**. Next, open the project in your preferred code editor.

```jsx
cd trello-clone
```

## **Installing Dependencies**

Next, we need to install the required dependencies for our project. We are using framer motion here to make a application animated. In the terminal, navigate to the project directory and run the following command:

```jsx
npm install framer-motion
npm install cuid
npm install date-fns
npm install @mui/material @emotion/styled @emotion/react
npm install lucide
```

## **Level 1 : Beginner**

In this beginner phase, we will be creating a static frontend with Trello like UI and full Trello working functionality (without database).

**Step 1.** Remove all the default next.js code from **`src/pages/index.js`** and  **`src/styles/global.css`.** Both file should like this :

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

**Step 2. Creating Drag and Drop functionality :** 

1. Inside the  **`src/pages/index.js`**  create a state named boards and add an array of two boards in it. These boards will be our boards from which we will be dragging the items and dropping items. Inside each board add an object named items which will contain an array of all the title that will be dragged and dropped. Your code should look like this:

```jsx
import { useState } from "react";

export default function Home() {
  const [boards, setBoards] = useState(
    [
      {
        cardName: "Stuff to Try (this is a list)",
        items: [
          { title: "Swipe left or right to see other lists on this board." }
        ],
      },
      {
        cardName: "Try it ( Another Board )",
        items: [
          { title: "Done with this board? Tap Archive board in the board settings menu to close it." },
          { title: "Tap and hold a card to pick it up and move it. Try it now!" },
          { title: "Create as many cards as you want, we've got an unlimited supply!" },
          { title: "Tap this card to open it and see more details." },
          { title: "Start using Trello!" }
        ],
      }]
  );

  return (
    <div>hello</div>
  );
}
```

1. Now we have our data for the board now we want to display this data with proper UI. We will be using Tailwind CSS for this. Create a grid of 4 columns and map the boards state into it. This will show each card separately. Your return should look like this after mapping boards state :

```jsx
  return (
    <div className="bg-cyan-400 min-h-screen">
      <div className="grid grid-cols-4 gap-4 pt-4 pl-7">
        {boards.map(element =>
          <div
            className="flex justify-center items-center flex-col w-5/6 h-fit bg-white rounded-lg">
            {element.cardName}
          </div>
        )}
      </div>
    </div>
  );
```

Save the file and run the server using the following command:

```bash
npm run dev
```

Your application should look like this :

![Untitled](https://talktohire.blr1.digitaloceanspaces.com/64a4b424-bed1-4062-91f4-31d22ab3f819.png)

1. Now add each boards items inside the board div. You can achieve this by mapping items array inside the boards map. Also import motion from framer motion to add animation to the items. Your return should look like this:

```jsx
 return (
    <div className="bg-cyan-300 min-h-screen">
      <div className="grid grid-cols-4 gap-4 pt-4 pl-7 ">
        {boards?.map((elements, index) =>
          <>
            <div className="flex justify-center items-center flex-col w-5/6 h-fit bg-white rounded-lg pb-3">
              <p className="font-sans text-sm font-medium mt-2 mb-3">{elements.cardName}</p>
              {elements.items.map((ele, indexKey) =>
                <motion.div
                  animate={{ y: 10 }}
                  transition={{ type: "spring", duration: 1, }}
                  className="flex justify-center cursor-pointer  items-center w-11/12 h-auto mb-2 bg-cyan-100 rounded-lg mt-2 font-sans text-sm font-normal break-words pl-4 pr-4 pt-2 pb-2">
                  {ele.title}
                </motion.div>)
              }
            </div>
          </>)
        }
      </div>
    </div>
  );
```

Again save, and you should be able to see the animated cards:

![Untitled](https://talktohire.blr1.digitaloceanspaces.com/Untitled_1.png)

1. Now we will be adding draggable feature inside our boards. To achieve this we will first add draggable attribute to items div. This will make item div draggable. Also we will add ‘on Drag Start’ attribute to call a function when ever we are dragging an item. This will give us information about the dragging item. Also we will be adding ‘on Drop’ attribute to the board div. This will trigger a function that will receive information about the dropping div. Your return should look like this:

```jsx
  return (
    <div className="bg-cyan-300 min-h-screen">
      <div className="grid grid-cols-4 gap-4 pt-4 pl-7 ">
        {boards?.map((elements, index) =>
          <>
            <div
              id="dropStarting"
              key={index}
              onDrop={() => handleDropped(elements, index)}
              onDragOver={(event) => allowDrag(event)}
              className="flex justify-center items-center flex-col w-5/6 h-fit bg-white rounded-lg pb-6">
              <p className="font-sans text-sm font-medium mt-2 mb-5">{elements.cardName}</p>
              {elements.items.map((ele, indexKey) =>
                <motion.div
                  animate={{ y: 10 }}
                  transition={{
                    type: "spring", duration: 1, stiffness: 100,
                    damping: 10
                  }}
                  id="dragStarting"
                  draggable
                  onDragStart={() => handleDragStart(elements.cardName, indexKey, ele)}
                  key={indexKey}
                  onDrop={() => { rIndex = indexKey }}
                  className="flex justify-center cursor-pointer  items-center w-11/12 h-auto mb-2 bg-cyan-100 rounded-lg mt-2 font-sans text-sm font-normal break-words pl-4 pr-4 pt-2 pb-2">
                  {ele.title}
                </motion.div>
              )}
            </div>
          </>)
        }
      </div>
    </div>
  );
```

1. Add functions for each attribute and add states to store the dragged element. Your code should look like this  :

```jsx
import { useState } from "react";
import { motion } from "framer-motion";

export default function Home() {
  const [picked, setPicked] = useState({});
  const [selectedIndex, setSelectedIndex] = useState(0);
  const [selectedTitle, setSelectedTitle] = useState();
  const [boards, setBoards] = useState(
    [
      {
        cardName: "Stuff to Try (this is a list)",
        items: [
          { title: "Swipe left or right to see other lists on this board." }
        ],
      },
      {
        cardName: "Try it ( Another Board )",
        items: [
          { title: "Done with this board? Tap Archive board in the board settings menu to close it." },
          { title: "Tap and hold a card to pick it up and move it. Try it now!" },
          { title: "Create as many cards as you want, we've got an unlimited supply!" },
          { title: "Tap this card to open it and see more details." },
          { title: "Start using Trello!" }
        ],
      }]
  );

  let rIndex;

  const handleDropped = (recievedElements, index) => {
    //Make changes in the original 
    boards.map(item => {
      if (item.cardName === picked) {
        //Delete the element
        item.items.splice(selectedIndex, 1);
      }
      if (item.cardName === recievedElements.cardName) {
        //Adding the dropped element
        item.items.splice(rIndex, 0, selectedTitle);
        rIndex = 0;
      }
    });
    //Save the changes back in state
    setBoards([...boards]);
  }

  const allowDrag = (event) => {
    event.preventDefault();
  }

  const handleDragStart = (element, index, ele) => {
    //Store the dragged item
    setPicked(element);
    setSelectedIndex(index);
    setSelectedTitle(ele);
  }

  return (
    <div className="bg-cyan-300 min-h-screen">
      <div className="grid grid-cols-4 gap-4 pt-4 pl-7 ">
        {boards?.map((elements, index) =>
          <>
            <div
              id="dropStarting"
              key={index}
              onDrop={() => handleDropped(elements, index)}
              onDragOver={(event) => allowDrag(event)}
              className="flex justify-center items-center flex-col w-5/6 h-fit bg-white rounded-lg pb-6">
              <p className="font-sans text-sm font-medium mt-2 mb-5">{elements.cardName}</p>
              {elements.items.map((ele, indexKey) =>
                <motion.div
                  animate={{ y: 10 }}
                  transition={{
                    type: "spring", duration: 1, stiffness: 100,
                    damping: 10
                  }}
                  id="dragStarting"
                  draggable
                  onDragStart={() => handleDragStart(elements.cardName, indexKey, ele)}
                  key={indexKey}
                  onDrop={() => { rIndex = indexKey }}
                  className="flex justify-center cursor-pointer  items-center w-11/12 h-auto mb-2 bg-cyan-100 rounded-lg mt-2 font-sans text-sm font-normal break-words pl-4 pr-4 pt-2 pb-2">
                  {ele.title}
                </motion.div>
              )}
            </div>
          </>)
        }
      </div>
    </div>
  );
}

```

Your Drop and Drag functionality should like this:

[demo.mp4](https://drive.google.com/file/d/1f3qy9XFrml2p636ao0M5kfJU2n39hyLY/view?usp=sharing)

**Step 3 : Create a Board Logs section :**

This will display all the changes done by the user on the board. For this we have to store all the changes like from which board the title was taken and where it has been dropped!

1. Create a another div inside the main grid, this will act as our board logs. Add the following code in your return :

```jsx
return (
    <div className="bg-cyan-300 min-h-screen">
      <div className="grid grid-cols-4 gap-4 pt-4 pl-7 ">
        {boards?.map((elements, index) =>
          <>
            <div
              id="dropStarting"
              key={index}
              onDrop={() => handleDropped(elements, index)}
              onDragOver={(event) => allowDrag(event)}
              className="flex justify-center items-center flex-col w-5/6 h-fit bg-white rounded-lg pb-6">
              <p className="font-sans text-sm font-medium mt-2 mb-5">{elements.cardName}</p>
              {elements.items.map((ele, indexKey) =>
                <motion.div
                  animate={{ y: 10 }}
                  transition={{
                    type: "spring", duration: 1, stiffness: 100,
                    damping: 10
                  }}
                  id="dragStarting"
                  draggable
                  onDragStart={() => handleDragStart(elements.cardName, indexKey, ele)}
                  key={indexKey}
                  onDrop={() => { rIndex = indexKey }}
                  className="flex justify-center cursor-pointer  items-center w-11/12 h-auto mb-2 bg-cyan-100 rounded-lg mt-2 font-sans text-sm font-normal break-words pl-4 pr-4 pt-2 pb-2">
                  {ele.title}
                </motion.div>
              )}
            </div>
          </>)
        }
        <motion.div
          className="relative flex pb-6 items-center overflow-auto flex-col w-full h-fit bg-white  rounded-lg mx-auto"
          animate="open"
          variants={variants}
        >
          <div className="font-sans font-sans font-medium mb-1 mt-2 pb-4" 
                style={{ backgroundColor: '#ffffff' }}>
                 Board Logs
           </div>
        </motion.div>
      </div>
    </div>
  );
```

1. Create a state named ‘commits Logs’ and store the changes of the board in it. Also use this state to map inside the board logs div. Also, create a function to generate a random number and save the color alongside the changes inside the ‘commit Logs’ . Your code should look like this : 

```jsx
import { useState } from "react";
import { motion } from "framer-motion";
import cuid from 'cuid';
import { format } from 'date-fns';

export default function Home() {
  const [picked, setPicked] = useState({});
  const [selectedIndex, setSelectedIndex] = useState(0);
  const [selectedTitle, setSelectedTitle] = useState();
  const [commitLogs, setCommitLogs] = useState([]);
  const [boards, setBoards] = useState(
    [
      {
        cardName: "Stuff to Try (this is a list)",
        items: [
          { title: "Swipe left or right to see other lists on this board." }
        ],
      },
      {
        cardName: "Try it ( Another Board )",
        items: [
          { title: "Done with this board? Tap Archive board in the board settings menu to close it." },
          { title: "Tap and hold a card to pick it up and move it. Try it now!" },
          { title: "Create as many cards as you want, we've got an unlimited supply!" },
          { title: "Tap this card to open it and see more details." },
          { title: "Start using Trello!" }
        ],
      }]
  );

  let rIndex;

  const generateRandomColor = () => {
    const randomRed = Math.floor(Math.random() * 256);
    const randomGreen = Math.floor(Math.random() * 256);
    const randomBlue = Math.floor(Math.random() * 256);
    return `rgb(${randomRed}, ${randomGreen}, ${randomBlue})`;
  };

  const handleDropped = (recievedElements, index) => {
    //Make changes in the original 
    boards.map(item => {
      if (item.cardName === picked) {
        //Delete the element
        item.items.splice(selectedIndex, 1);
      }
      if (item.cardName === recievedElements.cardName) {
        //Adding the dropped element
        item.items.splice(rIndex, 0, selectedTitle);
        rIndex = 0;
      }
    });
    const color = generateRandomColor();
    const currentTime = new Date()
    const modifiedTimestamp = format(currentTime, "HH:mm a");
    const log = {
      commitId: cuid(),
      time: modifiedTimestamp,
      fromCard: picked,
      toCard: recievedElements.cardName,
      title: selectedTitle,
      color: color,
    }
    setCommitLogs((prev) => [...prev, log])
    //Save the changes back in state
    setBoards([...boards]);
  }

  const allowDrag = (event) => {
    event.preventDefault();
  }

  const handleDragStart = (element, index, ele) => {
    //Store the dragged item
    setPicked(element);
    setSelectedIndex(index);
    setSelectedTitle(ele);
  }

  const variants = {
    open: {
      height: "100%",
      transition: {
        type: "spring",
        duration: 1
      }
    },
    closed: {
      height: "1/6",
      transition: {
        type: "spring",
        stiffness: 500,
        damping: 30
      }
    }
  };

  return (
    <div className="bg-cyan-300 min-h-screen">
      <div className="grid grid-cols-4 gap-4 pt-4 pl-7 ">
        {boards?.map((elements, index) =>
          <>
            <div
              id="dropStarting"
              key={index}
              onDrop={() => handleDropped(elements, index)}
              onDragOver={(event) => allowDrag(event)}
              className="flex justify-center items-center flex-col w-5/6 h-fit bg-white rounded-lg pb-6">
              <p className="font-sans text-sm font-medium mt-2 mb-5">{elements.cardName}</p>
              {elements.items.map((ele, indexKey) =>
                <motion.div
                  animate={{ y: 10 }}
                  transition={{
                    type: "spring", duration: 1, stiffness: 100,
                    damping: 10
                  }}
                  id="dragStarting"
                  draggable
                  onDragStart={() => handleDragStart(elements.cardName, indexKey, ele)}
                  key={indexKey}
                  onDrop={() => { rIndex = indexKey }}
                  className="flex justify-center cursor-pointer  items-center w-11/12 h-auto mb-2 bg-cyan-100 rounded-lg mt-2 font-sans text-sm font-normal break-words pl-4 pr-4 pt-2 pb-2">
                  {ele.title}
                </motion.div>
              )}
            </div>
          </>)
        }
        <motion.div
          className="relative flex pb-6 items-center overflow-auto flex-col w-full h-fit bg-white  rounded-lg mx-auto"
          animate="open"
          variants={variants}
        >
          <div className="font-sans font-sans font-medium mb-1 mt-2 pb-4" style={{ backgroundColor: '#ffffff' }}>Board Logs</div>
          <div className="relative flex items-center overflow-auto flex-col w-full h-fit bg-white  rounded-lg">
            <div className="flex flex-cols">
              <div className="mt-4 force-overflow pb-10">
                {commitLogs?.slice().reverse().map((item, index) =>
                  <motion.div
                    key={index}
                    animate={{ y: 6 }}
                    transition={{ type: "spring" }}
                  >
                    <motion.div whileTap={{ scale: 0.8 }} whileHover={{ scale: 1.2 }}
                      onHoverStart={e => { }}
                      onHoverEnd={e => { }} className={`z-10 w-4 h-4 rounded-full `}
                      style={{ backgroundColor: item.color }}
                    >
                    </motion.div>
                    <motion.div
                      transition={{ type: "spring", duration: 5 }}
                      className="z-0 ml-1.5 border w-0 h-10 border-slate-300" >
                    </motion.div>
                  </motion.div>
                )}
              </div>
              <div className="mt-4">
                {commitLogs?.slice().reverse()?.map((item, index) => <motion.div animate={{ y: 6 }}
                  transition={{ type: "spring" }} key={index} className="text-xs ml-7 w-fit h-14 ">{item.time}</motion.div>)}
              </div>
            </div>
          </div>
        </motion.div>
      </div>
    </div>
  );
}

```