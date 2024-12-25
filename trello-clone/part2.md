
[demo 2.mp4](https://drive.google.com/file/d/1Gmsj3BeuRPrX2BdmgiErDVaAByy8vAhB/view?usp=drive_link)

1. Now we need to display the changes for each commits. To do so we will be using tooltip of material UI. So, that when we hover on the commit head it will display the changes for that head. To achieve this we will be creating a custom component named ‘Light Tool Tip’ and call this component on the hover of commit head. Your code should look like this :

 

```jsx
import { useState } from "react";
import { motion } from "framer-motion";
import Tooltip, { tooltipClasses } from '@mui/material/Tooltip';
import Typography from '@mui/material/Typography';
import cuid from 'cuid';
import PropTypes from 'prop-types';
import { format } from 'date-fns';
import { styled } from '@mui/material/styles';

const LightTooltip = styled(({ className, ...props }) => (
  <Tooltip
    {...props}
    classes={{ popper: className }}
    className="cursor-pointer"
    slotProps={{
      popper: {
        sx: {
          [`&.${tooltipClasses.popper}[data-popper-placement*="bottom"] .${tooltipClasses.tooltip}`]:
          {
            marginTop: '0px',
          },
          [`&.${tooltipClasses.popper}[data-popper-placement*="top"] .${tooltipClasses.tooltip}`]:
          {
            marginBottom: '0px',
          },
          [`&.${tooltipClasses.popper}[data-popper-placement*="right"] .${tooltipClasses.tooltip}`]:
          {
            marginLeft: '30px',
          },
          [`&.${tooltipClasses.popper}[data-popper-placement*="left"] .${tooltipClasses.tooltip}`]:
          {
            marginRight: '0px',
          },
        },
      },
    }} />
))(({ theme }) => ({
  [`& .${tooltipClasses.tooltip}`]: {
    backgroundColor: theme.palette.common.white,
    color: 'rgba(0, 0, 0, 0.87)',
    boxShadow: theme.shadows[1],
    fontSize: 11,
  },
}));

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
              <div className="relative flex items-center overflow-auto flex-col w-full h-fit bg-white  rounded-lg">
                <div className="flex flex-cols">
                  <div className="mt-4 force-overflow pb-10">
                    {commitLogs?.slice().reverse().map((item, index) =>
                      <motion.div
                        key={index}
                        animate={{ y: 6 }}
                        transition={{ type: "spring" }}
                      >
                        <LightTooltip title=
                          {<div>
                            <Typography className="font-sans text-sm" style={{ backgroundColor: '#ffffff', color: 'black' }}><span className="text-sm font-sans font-semibold mr-2" style={{ color: `${item.color}` }}>From Card:</span> {item.fromCard} </Typography>
                            <Typography className="font-sans font-sans text-sm" style={{ backgroundColor: '#ffffff' }}><span className="text-sm font-semibold  mr-2 font-sans" style={{ color: `${item.color}` }}>To Card:</span>  {item.toCard}</Typography>
                            <Typography className="font-sans font-sans text-sm" style={{ backgroundColor: '#ffffff' }}>
                              <span style={{ color: `${item.color}` }} className=" font-sans font-semibold mr-2 text-sm">Title:</span>  {item.title.title}</Typography>
                          </div>}
                          className="cursor-pointer" placement="right" arrow>
                          <motion.div whileTap={{ scale: 0.8 }} whileHover={{ scale: 1.2 }}
                            onHoverStart={e => { }}
                            onHoverEnd={e => { }} onClick={() => handleCommit(index, item)} key={index} className={`z-10 w-4 h-4 rounded-full `}
                            style={{ backgroundColor: item.color }}
                          >
                          </motion.div>
                        </LightTooltip>
                        <motion.div
                          transition={{ type: "spring", duration: 5 }} className="z-0 ml-1.5 border w-0 h-10 border-slate-300" ></motion.div>
                      </motion.div>
                    )}
                  </div>
                  <div className="mt-4">
                    {commitLogs?.slice().reverse()?.map((item, index) => <motion.div animate={{ y: 6 }}
                      transition={{ type: "spring" }} key={index} className="text-xs ml-7 w-fit h-14 ">{item.time}</motion.div>)}
                  </div>
                </div>
              </div>
            </div>
          </div>
        </motion.div>
      </div>
    </div>
  );
}
```

You should be able to see the changes like this :

[demo 3.mp4](https://drive.google.com/file/d/19yNeJWE8yklfKL-TlNNN-vd3Plsf_5-J/view?usp=sharing)

**Step 4 : Delete a Log / Commit feature:**

You can also delete a commit from the Board Logs. to achieve this we need to select the commits and we need delete and keep button so that we can also keep the selected commit if we don't want to delete it.

1. Create UI for delete and keep commit changes section. To achieve we will use Lucid icons also we will be needing another div inside the board log section which will contain both delete and keep icons. Add below code in your board logs div:

```jsx
<motion.div animate={{ y: 6 }} className="flex  mb-4" transition={{ type: "spring", duration: 1 }}>
<motion.div whileHover={{ scale: 1.1 }} className="rounded-lg text-xs flex cursor-pointer mr-10"><BookCheck color="#00a1ff" size={18} className="mr-2" /> <span className="pt-0.5">Keep</span> </motion.div>
<motion.div whileHover={{ scale: 1.1 }} className="text-xs flex cursor-pointer "><Trash color="#ef0b60" size={18} className="mr-2" /> <span className="pt-0.5">Remove</span> </motion.div></motion.div>
```

Your board Logs should look like this now : 

![Untitled](https://talktohire.blr1.digitaloceanspaces.com/Untitled.png)

1. Now we can select the commits by clicking on the commit heads. To select the commit we need to add a new state named ‘discard Logs’ to store the selected commits. This will ensure that we can delete the selected commits. We also need two function one for keep icon and one for delete icon. These functions will make operations on both the Board state and Discard Logs state. If the user selects a commit then the commit after that selected commit will also be selected. This ensures that the latest commit always the commit previous to that selected commit. Your complete code will look like this :

```jsx
import { useState } from "react";
import { motion } from "framer-motion";
import Tooltip, { tooltipClasses } from '@mui/material/Tooltip';
import Typography from '@mui/material/Typography';
import cuid from 'cuid';
import PropTypes from 'prop-types';
import { format } from 'date-fns';
import { styled } from '@mui/material/styles';
import { BookCheck } from 'lucide-react';
import { Trash } from 'lucide-react';

const LightTooltip = styled(({ className, ...props }) => (
  <Tooltip
    {...props}
    classes={{ popper: className }}
    className="cursor-pointer"
    slotProps={{
      popper: {
        sx: {
          [`&.${tooltipClasses.popper}[data-popper-placement*="bottom"] .${tooltipClasses.tooltip}`]:
          {
            marginTop: '0px',
          },
          [`&.${tooltipClasses.popper}[data-popper-placement*="top"] .${tooltipClasses.tooltip}`]:
          {
            marginBottom: '0px',
          },
          [`&.${tooltipClasses.popper}[data-popper-placement*="right"] .${tooltipClasses.tooltip}`]:
          {
            marginLeft: '30px',
          },
          [`&.${tooltipClasses.popper}[data-popper-placement*="left"] .${tooltipClasses.tooltip}`]:
          {
            marginRight: '0px',
          },
        },
      },
    }} />
))(({ theme }) => ({
  [`& .${tooltipClasses.tooltip}`]: {
    backgroundColor: theme.palette.common.white,
    color: 'rgba(0, 0, 0, 0.87)',
    boxShadow: theme.shadows[1],
    fontSize: 11,
  },
}));

export default function Home() {
  const [picked, setPicked] = useState({});
  const [selectedIndex, setSelectedIndex] = useState(0);
  const [selectedTitle, setSelectedTitle] = useState();
  const [commitLogs, setCommitLogs] = useState([]);
  const [discardingCommit, setDiscardingLogs] = useState([]);
  const [selectedLog, setSelectedLog] = useState([]);
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

  const deleteCommit = () => {
    const updatedCards = commitLogs.filter((itemCommit, index) => !discardingCommit.includes(itemCommit.commitId));
    setCommitLogs(updatedCards);
    const revertedCommits = boards.map((insertItem, indexItem) => {
      selectedLog.forEach((e, i) => {
        if (e.fromCard === insertItem.cardName) {
          insertItem.items.push({ title: e.title.title });
        }
        if (e.toCard === insertItem.cardName) {
          const indexfound = insertItem.items.findIndex((item) => item.title === e.title.title)
          insertItem.items.splice(indexfound, 1);
        }
      });
      return insertItem;
    });
    setDiscardingLogs([]);
  }

  const handleKeep = () => {
    setDiscardingLogs([]);
  }

  const handleCommit = (element, item) => {
    const selectedIndex = commitLogs.findIndex(ele => ele.commitId === item.commitId)
    const selectedCommit = commitLogs.filter((ele, ind) => ind >= selectedIndex);
    setSelectedLog(selectedCommit)
    const commitIdArray = commitLogs.map((commits, ind) => ind >= selectedIndex ? commits.commitId : null).filter(comm => comm !== null)
    setDiscardingLogs([...commitIdArray])
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
        <motion.div
          className="relative flex pb-6 items-center overflow-auto flex-col w-full h-fit bg-white  rounded-lg mx-auto"
          animate="open"
          variants={variants}
        >
          <div className="font-sans font-sans font-medium mb-1 mt-2 pb-4" style={{ backgroundColor: '#ffffff' }}>Board Logs</div>
          <div className="relative flex items-center overflow-auto flex-col w-full h-fit bg-white  rounded-lg">
            {discardingCommit.length > 0 ?
              <motion.div animate={{ y: 6 }} className="flex  mb-4" transition={{ type: "spring", duration: 1 }}>
                <motion.div onClick={() => handleKeep()} whileHover={{ scale: 1.1 }} className="rounded-lg text-xs flex cursor-pointer mr-10"><BookCheck color="#00a1ff" size={18} className="mr-2" /> <span className="pt-0.5">Keep</span> </motion.div>
                <motion.div onClick={() => deleteCommit()} whileHover={{ scale: 1.1 }} className="text-xs flex cursor-pointer "><Trash color="#ef0b60" size={18} className="mr-2" /> <span className="pt-0.5">Remove</span> </motion.div>
              </motion.div>
              :
              <div className=" mb-4"></div>}
            <div className="flex flex-cols">
              <div className="relative flex items-center overflow-auto flex-col w-full h-fit bg-white  rounded-lg">
                <div className="flex flex-cols">
                  <div className="mt-4 force-overflow pb-10">
                    {commitLogs?.slice().reverse().map((item, index) =>
                      <motion.div
                        key={index}
                        animate={{ y: 6 }}
                        transition={{ type: "spring" }}
                      >
                        <LightTooltip title=
                          {<div>
                            <Typography className="font-sans text-sm" style={{ backgroundColor: '#ffffff', color: 'black' }}><span className="text-sm font-sans font-semibold mr-2" style={{ color: `${item.color}` }}>From Card:</span> {item.fromCard} </Typography>
                            <Typography className="font-sans font-sans text-sm" style={{ backgroundColor: '#ffffff' }}><span className="text-sm font-semibold  mr-2 font-sans" style={{ color: `${item.color}` }}>To Card:</span>  {item.toCard}</Typography>
                            <Typography className="font-sans font-sans text-sm" style={{ backgroundColor: '#ffffff' }}>
                              <span style={{ color: `${item.color}` }} className=" font-sans font-semibold mr-2 text-sm">Title:</span>  {item.title.title}</Typography>
                          </div>}
                          className="cursor-pointer" placement="right" arrow>
                          <motion.div whileTap={{ scale: 0.8 }} whileHover={{ scale: 1.2 }}
                            onHoverStart={e => { }}
                            style={discardingCommit.includes(item.commitId) ? { backgroundColor: '#ef0b60' } : { backgroundColor: item.color }}
                            onHoverEnd={e => { }} onClick={() => handleCommit(index, item)} key={index} className={`z-10 w-4 h-4 rounded-full `}
                          >
                          </motion.div>
                        </LightTooltip>
                        <motion.div style={discardingCommit.includes(item.commitId) ? { borderColor: '#ef0b60' } : {}}
                          transition={{ type: "spring", duration: 5 }} className="z-0 ml-1.5 border w-0 h-10 border-slate-300" ></motion.div>
                      </motion.div>
                    )}
                  </div>
                  <div className="mt-4">
                    {commitLogs?.slice().reverse()?.map((item, index) => <motion.div animate={{ y: 6 }}
                      transition={{ type: "spring" }} style={discardingCommit.includes(item.commitId) ? { color: '#ef0b60' } : {}} key={index} className="text-xs ml-7 w-fit h-14 ">{item.time}</motion.div>)}
                  </div>
                </div>
              </div>
            </div>
          </div>
        </motion.div>
      </div>
    </div>
  );
}

```

You final application will look like this:

[demo 4.mp4](https://drive.google.com/file/d/1BfWC9C0AwoX4cXT7iAEVVFl-dEBf5n81/view?usp=sharing)