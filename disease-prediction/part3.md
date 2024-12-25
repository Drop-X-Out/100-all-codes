

`Hero.jsx`

```jsx
import hero_img from "../img/hero-img.svg";
import Button from "@mui/material/Button";
import { useGlobalContext } from "./context";

const Hero = () => {
  const { setLoginButtonClicked, setRegistrationToggle } = useGlobalContext();
  return (
    <div className="w-4/5 hero-container   pt-10 lg:pt-0 flex flex-col-reverse  md:flex-row justify-center gap-8 items-center h-3/4 ">
      <div className="hero flex flex-col  text-center md:text-left w-full md:w-2/5 lg:pl-8  lg:scale-105">
        <div className="hero-text text-4xl lg:text-5xl mb-4 md:mb-5 text-gray-800">
          Your Healthcare, Simplified
        </div>
        <div className="hero-stanza  text-lg lg:text-lg flex  text-center md:text-left items-center w-full md:w-4/5 mb-4 md:mb-7 text-gray-800 px-1 md:px-0">
          Experience optimal health with simplified solutions, just a click
          away!
        </div>
        <div className="hero-btn-container  flex justify-center md:justify-start gap-3 items-center">
          <Button
            variant="outlined"
            color="primary"
            className="hover:scale-105 w-28 h-12 hover:transition-all duration-200 "
            onClick={() => {
              setRegistrationToggle(true);
              setLoginButtonClicked(true);
            }}
          >
            Join Us!
          </Button>
          <Button
            variant="outlined"
            color="success"
            className="hover:scale-105 h-12 hover:transition-all duration-200 "
            onClick={() => {
              setLoginButtonClicked(true);
              setRegistrationToggle(false);
            }}
          >
            Already a member?
          </Button>
        </div>
      </div>
      <div className="img-wrapper w-80 sm:w-96 lg:w-1/2 flex">
        <img src={hero_img} alt="hero-image" className="block w-full -z-10" />
      </div>
    </div>
  );
};

export default Hero;
```

`LogModal.jsx`

```jsx
import React, { useEffect, useRef } from "react";
import crossIcon from "../img/cross icon.svg";
import { TextField, Button, Grid } from "@mui/material";
import axios from "axios";

import { useGlobalContext } from "./context";

const LogModal = ({ logModal, setLogModal }) => {
  const { formData, fetchData, setFormData, url, data, setData } =
    useGlobalContext();
  const afterRef = useRef("");
  const beforeRef = useRef("");
  const highRef = useRef("");
  const lowRef = useRef("");
  const dateRef = useRef("");

  useEffect(() => {
    const currentDate = new Date();
    const year = currentDate.getFullYear().toString().substr(-2);
    const month = currentDate.toLocaleString("default", { month: "short" });
    const day = currentDate.getDate();

    const dateString = `${day} ${month} '${year}`;

    dateRef.current = dateString;
  }, []);

  const closeLogModal = () => {
    setLogModal(false);
  };

  const handleSubmit = async (event) => {
    event.preventDefault();

    setData((data) => {
      const updatedFormData = { ...data };

      // Append form values and date to bp_log
      const highValue = highRef.current.value;
      const lowValue = lowRef.current.value;
      const dateValue = dateRef.current;

      if (highValue !== "" && lowValue !== "") {
        updatedFormData.bp_log.high.push(highValue);
        updatedFormData.bp_log.low.push(lowValue);
        updatedFormData.bp_log.date.push(dateValue);
      }

      // Append form values and date to blood_glucose
      const beforeValue = beforeRef.current.value;
      const afterValue = afterRef.current.value;

      if (beforeValue !== "" && afterValue !== "") {
        updatedFormData.blood_glucose.before.push(beforeValue);
        updatedFormData.blood_glucose.after.push(afterValue);
        updatedFormData.blood_glucose.date.push(dateValue);
      }

      return updatedFormData;
    });

    try {
      await axios.put(url, data, {
        withCredentials: true,
      });

      await fetchData();
    } catch (error) {
      console.log(error);
    }

    closeLogModal();
  };

  useEffect(() => {
    const handleClickOutside = (event) => {
      if (event.target.classList.contains("modal")) {
        closeLogModal();
      }
    };

    if (logModal) {
      document.addEventListener("click", handleClickOutside);
    }

    return () => {
      document.removeEventListener("click", handleClickOutside);
    };
  }, [logModal]);

  if (!logModal) {
    return null;
  }

  return (
    <div className="fixed top-0 left-0 w-screen h-screen flex justify-center items-center p-4 backdrop-blur-sm modal">
      <div className="flex flex-col gap-2 items-center w-96 sm:w-1/3  flex-wrap  bg-white rounded-lg shadow-lg p-8">
        <div className="w-full flex justify-end">
          <button onClick={closeLogModal} className="hover:scale-105">
            <img src={crossIcon} alt="cross-icon" loading="lazy" />
          </button>
        </div>
        <h1 className="text-3xl  mt-4 font-semibold text-gray-700">
          MEDICAL LOG
        </h1>
        <form
          className="w-full flex flex-col gap-4 items-center"
          onSubmit={handleSubmit}
        >
          <h2 className="p-1 text-lg text-teal-600 font-semibold">
            {dateRef.current}
          </h2>
          <h3 className="w-full text-xl font-semibold text-gray-600 px-1">
            Blood Pressure Level
          </h3>
          <Grid container spacing={1}>
            <Grid item xs={6}>
              <TextField
                name="high"
                label="High"
                inputRef={highRef}
                type="number"
              />
            </Grid>
            <Grid item xs={6}>
              <TextField
                name="low"
                label="Low"
                inputRef={lowRef}
                type="number"
              />
            </Grid>
          </Grid>
          <h3 className="w-full text-xl font-semibold text-gray-600 px-1">
            Glucose Level
          </h3>
          <Grid container spacing={1}>
            <Grid item xs={6}>
              <TextField
                name="before"
                label="Before Breakfast"
                inputRef={beforeRef}
                type="number"
              />
            </Grid>
            <Grid item xs={6}>
              <TextField
                name="after"
                label="After Breakfast"
                inputRef={afterRef}
                type="number"
              />
            </Grid>
          </Grid>
          <Button variant="outlined" color="success" type="submit">
            Add
          </Button>
        </form>
      </div>
    </div>
  );
};

export default LogModal;
```

`Login-Button.jsx`

```jsx
import React from "react";
import { useGlobalContext } from "./context";
import { useNavigate } from "react-router-dom";

const LoginBtn = () => {
  const { submitLogout, update_form_btn, currentUser } = useGlobalContext();
  const navigate = useNavigate();

  const handleLogout = () => {
    navigate("/");
  };

  if (currentUser) {
    return (
      <button
        onClick={() => {
          submitLogout();
          handleLogout();
        }}
        className="px-1"
      >
        Log out
      </button>
    );
  }

  return (
    <>
      <button
        id="form_btn"
        onClick={update_form_btn}
        className="flex justify-center w-full md:w-fit md:justify-start  p-5 md:p-0 md:px-1"
      >
        Register/Login
      </button>
    </>
  );
};

export default LoginBtn;
```

`LoginForm.jsx`

```jsx
import { useGlobalContext } from "./context";
import { useState } from "react";
import TextField from "@mui/material/TextField";
import Button from "@mui/material/Button";
import cancelIcon from "../img/cross icon.svg";
import SignIn from "../components/GoogleSignIn";

const LoginForm = () => {
  const [errorDisplay, setErrorDisplay] = useState("");
  const { email, password, submitLogin, closeModal, error } =
    useGlobalContext();
  const [user_email, setUserEmail] = useState("");
  const [user_password, setUserPassword] = useState("");

  return (
    <div>
      <div className="flex justify-end mb-3 mr-2">
        <button onClick={closeModal}>
          <img src={cancelIcon} alt="cross" />
        </button>
      </div>
      <div className="flex flex-col gap-8">
        <div className="flex flex-col items-center gap-2">
          <h2 className="text-4xl modal-heading text-center w-full">
            Hello Again!
          </h2>
          <p className="text-center w-full">Welcome back you've been missed!</p>
        </div>
        <form
          onSubmit={(event) => {
            setErrorDisplay(error.current);
            submitLogin(event);
          }}
          className="flex flex-col justify-center gap-3"
        >
          <TextField
            id="FormBasicEmail"
            label="Email"
            variant="outlined"
            value={user_email}
            onChange={(event) => {
              setUserEmail(event.target.value);
              email.current = event.target.value;
            }}
            helperText="We'll never share your email"
            color="success"
            required
          />
          <TextField
            id="formBasicPassword"
            label="Password"
            type="password"
            variant="outlined"
            value={user_password}
            onChange={(event) => {
              setUserPassword(event.target.value);
              password.current = event.target.value;
            }}
            color="success"
            required
          />
          <Button variant="outlined" color="primary" type="submit">
            Login
          </Button>
          <p className="text-red-500" style={{ fontSize: "13px" }}>
            {errorDisplay}
          </p>
          <SignIn />
        </form>
      </div>
    </div>
  );
};

export default LoginForm;
```

`Main.jsx`

```jsx
import Modal from "./Modal";
import { useGlobalContext } from "./context";
import Hero from "./Hero";
import Services from "./Services";
import About from "./About";
import DpWindow from "./dpWindow";

const Main = () => {
  const { currentUser } = useGlobalContext();
  if (!currentUser) {
    return (
      <main className="flex items-center flex-col min-h-screen pt-20">
        <Hero />
        <Services />
        <About />
        <Modal />
      </main>
    );
  }
  return (
    <main className="flex items-center flex-col min-h-screen pt-20">
      <DpWindow />
    </main>
  );
};

export default Main;
```

![Untitled](https://talktohire.blr1.digitaloceanspaces.com/Untitled_2.png)

`Medical History.jsx`

```jsx
import React from "react";

const MedicalHistory = ({ data }) => {
  if (!data || data.length === 0) {
    return (
      <div className="h-full w-full flex flex-col items-center">
        <h2 className="text-xl p-2 w-full text-center text-gray-800 border-b font-semibold">
          Medical History
        </h2>
        <p className="w-full h-full bg-teal-50 grid place-content-center italic md:text-lg text-teal-900">
          None
        </p>
      </div>
    );
  }

  return (
    <div className="bg-white">
      <table className="w-full">
        <thead>
          <tr className="border-b text-gray-800 bg-gray-100">
            <th className="py-2 px-4 border-b font-semibold text-center text-xl">
              Medical History
            </th>
          </tr>
        </thead>
        <tbody>
          {data.map((item, index) => (
            <tr key={index} className="border">
              <td className="p-2 text-gray-800 px-5">
                <div className="flex items-center text-base md:text-lg text-gray-700">
                  {item}
                </div>
              </td>
            </tr>
          ))}
        </tbody>
      </table>
    </div>
  );
};

export default MedicalHistory;
```

`Modal.jsx`

```jsx
import { useGlobalContext } from "./context";
import RegisterForm from "./RegisterForm";
import LoginForm from "./LoginForm";
import ModalImg from "../img/modal.svg";

const Modal = () => {
  const { registrationToggle, loginButtonClicked, responseCall } =
    useGlobalContext();
  if (loginButtonClicked) {
    return (
      <>
        {responseCall && (
          <div className="fixed responseCall top-0 flex flex-col justify-center items-center  w-screen h-screen z-50">
            <div>
              <div className="rounded-full h-20 w-24 animate-bounce  bg-teal-700 flex items-center italic text-gray-100 font-semibold justify-center"></div>
            </div>
            <div className="w-28 h-2 bg-teal-700 rounded-lg"></div>
          </div>
        )}
        <div className=" fixed top-0 left-0 w-screen h-screen flex justify-center items-center backdrop-blur-sm">
          <div className="flex justify-center items-center w-80 xl:w-1/2 gap-3 h-68 flex-wrap  bg-white rounded-lg shadow-lg">
            <figure className="hidden xl:block w-80 z-20">
              <img src={ModalImg} alt="Modal" className="w-full rounded-lg" />
            </figure>
            {registrationToggle ? <RegisterForm /> : <LoginForm />}
          </div>
        </div>
      </>
    );
  }
  return null;
};

export default Modal;
```

`PatientForm.jsx`

```jsx
import React from "react";
import {
  TextField,
  Button,
  Container,
  Grid,
  InputLabel,
  Divider,
  MenuItem,
  Select,
  FormControl,
} from "@mui/material";

const PatientForm = ({
  profileData,
  handleInputChange,
  handleFormSubmit,
  patientData,
}) => {
  if (patientData.new_patient) {
    return (
      <div className="my-5 flex flex-col justify-center">
        <div className="flex justify-center">
          <h2 className="m-2 heading p-6 w-4/5 text-3xl text-gray-800">
            Empowering Your Health Journey
          </h2>
        </div>
        <Container>
          <form onSubmit={handleFormSubmit}>
            <Grid container spacing={2}>
              <Grid item xs={12} sm={6}>
                <TextField
                  required
                  name="age"
                  label="Age"
                  variant="outlined"
                  fullWidth
                  type="number"
                  value={profileData.age}
                  onChange={handleInputChange}
                />
              </Grid>

              <Grid item xs={12} sm={6}>
                <FormControl variant="outlined" fullWidth>
                  <InputLabel id="dropdown-label">Sex</InputLabel>
                  <Select
                    required
                    name="sex"
                    labelId="dropdown-label"
                    value={profileData.sex}
                    onChange={handleInputChange}
                    label="Sex"
                  >
                    <MenuItem value="Male">Male</MenuItem>
                    <MenuItem value="Female">Female</MenuItem>
                    <MenuItem value="Others">Others</MenuItem>
                  </Select>
                </FormControl>
              </Grid>

              <Grid item xs={12} sm={6}>
                <TextField
                  required
                  type="text"
                  name="first_name"
                  label="First Name"
                  variant="outlined"
                  fullWidth
                  value={profileData.first_name}
                  onChange={handleInputChange}
                />
              </Grid>

              <Grid item xs={12} sm={6}>
                <TextField
                  required
                  name="last_name"
                  label="Last Name"
                  variant="outlined"
                  fullWidth
                  type="text"
                  value={profileData.last_name}
                  onChange={handleInputChange}
                />
              </Grid>

              <Grid item xs={12}>
                <InputLabel sx={{ fontSize: "1.05rem", pl: "8px" }}>
                  Date of Birth
                </InputLabel>
                <Divider sx={{ my: 0.5 }} />
              </Grid>
              <Grid item xs={4}>
                <TextField
                  name="dob_day"
                  type="text"
                  label="Day"
                  variant="outlined"
                  fullWidth
                  value={profileData.dob_day}
                  onChange={handleInputChange}
                />
              </Grid>

              <Grid item xs={4}>
                <TextField
                  name="dob_month"
                  label="Month"
                  variant="outlined"
                  fullWidth
                  value={profileData.dob_month}
                  onChange={handleInputChange}
                />
              </Grid>

              <Grid item xs={4}>
                <TextField
                  name="dob_year"
                  label="Year"
                  variant="outlined"
                  fullWidth
                  value={profileData.dob_year}
                  onChange={handleInputChange}
                />
              </Grid>
              <Grid item xs={12} sm={6}>
                <TextField
                  name="height"
                  label="Height"
                  variant="outlined"
                  fullWidth
                  type="number"
                  value={profileData.height}
                  onChange={handleInputChange}
                />
              </Grid>

              <Grid item xs={12} sm={6}>
                <TextField
                  name="weight"
                  label="Weight"
                  variant="outlined"
                  fullWidth
                  type="number"
                  value={profileData.weight}
                  onChange={handleInputChange}
                />
              </Grid>

              <Grid item xs={12}>
                <TextField
                  name="current_med"
                  label="Current Medications (separated by commas)"
                  variant="outlined"
                  type="text"
                  fullWidth
                  multiline
                  rows={4}
                  value={profileData.current_med}
                  onChange={handleInputChange}
                />
              </Grid>
              <Grid item xs={12}>
                <TextField
                  name="medical_history"
                  label="Medical History (separated by commas)"
                  variant="outlined"
                  fullWidth
                  type="text"
                  multiline
                  rows={4}
                  value={profileData.medical_history}
                  onChange={handleInputChange}
                />
              </Grid>
              <Grid item xs={6}>
                <FormControl variant="outlined" fullWidth>
                  <InputLabel id="dropdown-label">Exercise</InputLabel>
                  <Select
                    labelId="dropdown-label"
                    value={profileData.exercise}
                    onChange={handleInputChange}
                    name="exercise"
                    label="Exercise"
                  >
                    <MenuItem value="Yoga">Yoga</MenuItem>
                    <MenuItem value="Mild">
                      Mild-Exercises - Walks, Jogs
                    </MenuItem>
                    <MenuItem value="Heavy">
                      Heavy-Exercises - Running, Lifting
                    </MenuItem>
                    <MenuItem value="No">No Exercise</MenuItem>
                  </Select>
                </FormControl>
              </Grid>

              <Grid item xs={6}>
                <FormControl variant="outlined" fullWidth>
                  <InputLabel id="dropdown-label">Diet</InputLabel>
                  <Select
                    labelId="dropdown-label"
                    value={profileData.diet}
                    onChange={handleInputChange}
                    name="diet"
                    label="Diet"
                  >
                    <MenuItem value="Vegan">Vegan</MenuItem>
                    <MenuItem value="Vegetarian">Vegetarian</MenuItem>
                    <MenuItem value="Non-Vegetarian">Non-Vegetarian</MenuItem>
                  </Select>
                </FormControl>
              </Grid>
              <Grid item xs={6}>
                <FormControl fullWidth>
                  <InputLabel id="demo-simple-select-label" required>
                    Alcohol Consumption
                  </InputLabel>
                  <Select
                    required
                    name="alcohol_cons"
                    labelId="demo-simple-select-label"
                    id="demo-simple-select"
                    value={profileData.alcohol_cons}
                    label="Alcoholic Consumption"
                    onChange={handleInputChange}
                  >
                    <MenuItem value={"No"}>No</MenuItem>
                    <MenuItem value={"Mild"}>Mild</MenuItem>
                    <MenuItem value={"high"}>High</MenuItem>
                  </Select>
                </FormControl>
              </Grid>
              <Grid item xs={6}>
                <FormControl fullWidth>
                  <InputLabel id="demo-simple-select-label" required>
                    Smoking Consumption
                  </InputLabel>
                  <Select
                    name="smoke_cons"
                    labelId="demo-simple-select-label"
                    id="demo-simple-select"
                    value={profileData.smoke_cons}
                    label="Smoking Consumption"
                    onChange={handleInputChange}
                  >
                    <MenuItem value={"No"}>No</MenuItem>
                    <MenuItem value={"Mild"}>Mild</MenuItem>
                    <MenuItem value={"high"}>High</MenuItem>
                  </Select>
                </FormControl>
              </Grid>
            </Grid>

            <div className="buttonContainer mt-5 w-full">
              <Button
                type="submit"
                variant="outlined"
                color="primary"
                className="w-1/6 h-12"
              >
                Submit
              </Button>
            </div>
          </form>
        </Container>
      </div>
    );
  }
  return null;
};

export default PatientForm;
```

`PatientProfile.jsx`

```jsx
import Calendar from "./Calendar";
import Sidebar from "./Sidebar";
import { useState, useEffect } from "react";
import Record from "./Record";
import BP_chart from "./BP_chart";
import LogModal from "./LogModal";
import BP_Log from "./BP_Log";
import ProfileModal from "./ProfileModal";
import GlucoseLevel from "./GlucoseLevel";
import Sugar_chart from "./Sugar_chart";
import Personal from "./Personal";
import MedicalHistory from "./Medical History";
import ConsumptionModal from "./ConsumptionModal";

const PatientProfile = ({ responseData }) => {
  const [record, setRecord] = useState(false);
  const [logModal, setLogModal] = useState(false);
  const [profileModal, setProfileModal] = useState(false);
  const [consumptionModal, setConsumptionModal] = useState(false);
  const { first_name, height, weight, last_name } = responseData;
  const [bmi, setBmi] = useState(0);
  const [bmiColor, setBmiColor] = useState("");

  useEffect(() => {
    // Calculate BMI
    const calculateBMI = () => {
      const heightInMeters = height / 100; // Convert height to meters
      const bmiValue = weight / (heightInMeters * heightInMeters);
      setBmi(bmiValue);

      // Set BMI color
      if (bmiValue < 18.5) {
        setBmiColor("bg-purple-400");
      } else if (bmiValue >= 18.5 && bmiValue < 24.9) {
        setBmiColor("bg-blue-400");
      } else if (bmiValue >= 24.9 && bmiValue < 29.9) {
        setBmiColor("bg-orange-400");
      } else {
        setBmiColor("bg-red-500");
      }
    };

    calculateBMI();
  }, [height, weight]);

  if (responseData.new_patient) {
    return null;
  }

  return (
    <div className="profile flex justify-center flex-col items-center pb-4">
      {Object.keys(responseData).length > 0 ? (
        <>
          <div className="w-full flex flex-wrap justify-center gap-2">
            <div className="bg-gray-800 w-5/6 md:p-2 sm:w-1/6 lg:w-1/12 h-full scale-90 sm:scale-100 sm:h-screen rounded-md">
              <Sidebar
                setRecord={setRecord}
                setLogModal={setLogModal}
                setProfileModal={setProfileModal}
                setConsumptionModal={setConsumptionModal}
              />
            </div>
            <div className="sm:h-screen md:fit-content w-5/6 sm:w-3/4 lg:w-2/5 bg-white  flex flex-col justify-start items-center">
              <div className="md:pt-6 h-40 w-full p-1 justify-between flex items-center greeting px-3 md:px-8">
                <div className="w-full md:w-1/2  md:block text-2xl md:text-3xl font-semibold text-gray-800 ">
                  <p>Hi, {first_name + " " + last_name}</p>
                  <p>Check your</p>
                  <p>Health!</p>
                </div>
                <div className="flex items-center justify-between h-14 md:h-fit xl:w-1/4 gap-3 bg-gray-100 rounded-2xl py-2 px-3 ">
                  <p className="text-lg md:text-xl font-semibold text-gray-900">
                    BMI
                  </p>
                  <div
                    className={`bmi w-12 h-10 md:w-16 md:h-12 rounded-full flex items-center justify-center text-white text-lg md:text-xl font-semibold ${bmiColor}`}
                  >
                    {bmi.toFixed(1)}
                  </div>
                </div>
              </div>
              <div className="charts-container w-full rounded-md flex justify-between sm:px-4 flex-wrap h-96 sm:h-1/3 py-2">
                <div className="w-full sm:w-47 rounded-md ">
                  <BP_chart chartData={responseData.bp_log} />
                </div>
                <div className="w-full  sm:w-47 rounded-md">
                  <Sugar_chart chartData={responseData.blood_glucose} />
                </div>
              </div>
              <div className=" my-2 w-full sm:h-96 md:h-1/2 rounded-md overflow-scroll bg-white border">
                <Personal responseData={responseData} />
              </div>
            </div>
            <div className="sm:w-full lg:px-0 lg:w-1/2 gap-2 p-1  flex flex-col items-center">
              <div className="w-full flex flex-wrap lg:flex-nowrap justify-center">
                <div className="flex w-5/6 sm:w-3/5 md:w-1/2 border  rounded-md">
                  <Calendar />
                </div>

                <div className="w-5/6 bg-gray-50 mt-2 sm:mt-0 sm:w-2/5 md:w-47 lg:mx-2 h-96 sm:h-full border  rounded-md">
                  <MedicalHistory data={responseData.medical_history} />
                </div>
              </div>
              <div className="lg:h-full justify-center w-full flex-wrap sm:flex-nowrap flex gap-2">
                <div className="w-5/6 h-96 md:w-1/2 lg:h-full rounded-md overflow-scroll m-1  border  p-1 flex flex-col">
                  <h2 className="font-semibold text-lg md:text-2xl py-1 text-teal-900 text-center">
                    Glucose
                  </h2>
                  <div className="flex-grow bg-gray-50 ">
                    <GlucoseLevel responseData={responseData} />
                  </div>
                </div>
                <div className="w-5/6  h-96 md:w-1/2 lg:h-full rounded-md overflow-scroll  border  m-1 p-1 flex flex-col">
                  <h2 className="font-semibold text-lg md:text-2xl text-gray-900 text-center p-2 ">
                    Blood Pressure
                  </h2>
                  <div className="flex-grow bg-gray-50">
                    <BP_Log responseData={responseData} />
                  </div>
                </div>
              </div>
            </div>
          </div>

          <Record setRecord={setRecord} record={record} />
          <LogModal setLogModal={setLogModal} logModal={logModal} />
          <ProfileModal
            setProfileModal={setProfileModal}
            profileModal={profileModal}
          />
          <ConsumptionModal
            consumptionModal={consumptionModal}
            setConsumptionModal={setConsumptionModal}
          />
        </>
      ) : (
        <p>Loading...</p>
      )}
    </div>
  );
};

export default PatientProfile;
```

`Personal.jsx`

```jsx
import React from "react";

const Personal = ({ responseData }) => {
  const {
    age,
    sex,
    height,
    weight,
    diet,
    exercise,
    dob_day,
    dob_month,
    dob_year,
    smoke_cons,
    alcohol_cons,
    current_med,
  } = responseData;

  return (
    <div className="px-4 py-6 bg-white rounded-lg ">
      <h3 className="text-2xl md:text-3xl text-gray-800 font-semibold mb-4">
        Personal Info
      </h3>
      <div className="grid grid-cols-2 gap-6 text-sm md:text-base">
        <div className="border-b border-r  p-2 rounded-lg">
          <div className="text-gray-700 font-semibold">Age:</div>
          <div>{age}</div>
        </div>
        <div className="border-b border-r p-2 rounded-lg">
          <div className="text-gray-700 font-semibold">Sex:</div>
          <div>{sex}</div>
        </div>
        {height && (
          <div className="border-b border-r p-2 rounded-lg">
            <div className="text-gray-700 font-semibold">Height:</div>
            <div>{height}</div>
          </div>
        )}
        {weight && (
          <div className="border-b border-r p-2 rounded-lg">
            <div className="text-gray-700 font-semibold">Weight:</div>
            <div>{weight}</div>
          </div>
        )}
        <div className="border-b border-r p-2 rounded-lg">
          <div className="text-gray-700 font-semibold">Date of Birth:</div>
          <div>{`${dob_day}/${dob_month}/${dob_year}`}</div>
        </div>
        <div className="border-b border-r p-2 rounded-lg">
          <div className="text-gray-700 font-semibold">Exercise:</div>
          <div>{exercise}</div>
        </div>
        <div className="border-b border-r-200 p-2 rounded-lg">
          <div className="text-gray-700 font-semibold">Diet:</div>
          <div>{diet}</div>
        </div>
        <div className="border-b border-r p-2 rounded-lg">
          <div className="text-gray-700 font-semibold">
            Alcohol Consumption:
          </div>
          <div>{alcohol_cons}</div>
        </div>
        <div className="border-b border-rp-2 rounded-lg">
          <div className="text-gray-700 font-semibold">
            Smoking Consumption:
          </div>
          <div>{smoke_cons}</div>
        </div>
        <div className="border-b border-rp-2 rounded-lg">
          <div className="text-gray-700 font-semibold">
            Current Medications:
          </div>
          <div>{current_med.join(", ")}</div>
        </div>
      </div>
    </div>
  );
};

export default Personal;
```

`Prediction.jsx`

```jsx
const Prediction = ({ prediction }) => {
  const handleProbability = (probability) => {
    const percentage = (probability * 100).toFixed(2);
    return `${percentage}%`;
  };

  const firstDiseaseProbability =
    prediction.length > 0 ? prediction[0].diseases_prob[0] : null;
  const showNotice =
    firstDiseaseProbability !== null && firstDiseaseProbability > 0.3;

  return (
    <>
      <div className="bg-teal-50 h-full mt-1 flex flex-col justify-evenly rounded-lg p-1 ">
        {prediction.length > 0 &&
          prediction[0].diseases.map((disease, index) => (
            <div
              key={index}
              className="rounded-md m-1 py-1 px-2 bg-sky-100 text-base md:text-lg text-indigo-950 flex justify-between"
            >
              <div>{disease}</div>
              <div>{handleProbability(prediction[0].diseases_prob[index])}</div>
            </div>
          ))}
      </div>
      <div className=" p-2 bg-violet-100 mt-1 rounded-md uppercase text-center">
        {showNotice ? "consult a doctor" : "No immediate concern"}
      </div>
    </>
  );
};

export default Prediction;
```

`ProfileModal.jsx`

```jsx
import React, { useEffect } from "react";
import crossIcon from "../img/cross icon.svg";
import {
  TextField,
  Button,
  Grid,
  Select,
  MenuItem,
  InputLabel,
} from "@mui/material";
import { useGlobalContext } from "./context";

const ProfileModal = ({ profileModal, setProfileModal }) => {
  const { handleDashboardChange, data, handleDashboardSubmit } =
    useGlobalContext();
  const closeModal = () => {
    setProfileModal(false);
  };

  useEffect(() => {
    const handleClickOutside = (event) => {
      if (event.target.classList.contains("modal")) {
        closeModal();
      }
    };

    if (profileModal) {
      document.addEventListener("click", handleClickOutside);
    }

    return () => {
      document.removeEventListener("click", handleClickOutside);
    };
  }, [profileModal]);

  if (!profileModal) {
    return null;
  }

  return (
    <div className="fixed top-0 left-0 w-screen h-screen  flex justify-center items-center p-4 backdrop-blur-sm modal">
      <div className="flex flex-col justify-center items-center w-96 md:w-1/2 lg:w-2/5  flex-wrap  bg-white rounded-lg shadow-lg px-8 py-8">
        <div className="w-full flex justify-end">
          <button onClick={closeModal} className="hover:scale-105">
            <img src={crossIcon} alt="cross-icon" loading="lazy" />
          </button>
        </div>
        <h1 className="text-3xl pb-6 font-semibold text-gray-700 w-full">
          Edit Profile
        </h1>
        <form
          onSubmit={(e) => {
            e.preventDefault();
            handleDashboardSubmit(e);
            closeModal();
          }}
          className="w-full flex flex-col gap-4 items-center"
        >
          <Grid container spacing={2}>
            <Grid item xs={6} className="w-full">
              <TextField
                name="first_name"
                label="First Name"
                fullWidth
                value={data.first_name}
                onChange={handleDashboardChange}
              />
            </Grid>
            <Grid item xs={6}>
              <TextField
                name="last_name"
                label="Last Name"
                fullWidth
                value={data.last_name}
                onChange={handleDashboardChange}
              />
            </Grid>
            <Grid item xs={6} className="w-full">
              <TextField
                name="age"
                label="Age"
                fullWidth
                type="number"
                value={data.age}
                onChange={handleDashboardChange}
              />
            </Grid>
            <Grid item xs={6}>
              <Select
                name="sex"
                value={data.sex}
                onChange={handleDashboardChange}
                fullWidth
              >
                <MenuItem value="Male">Male</MenuItem>

                <MenuItem value="Female">Female</MenuItem>

                <MenuItem value="Others">Others</MenuItem>
              </Select>
            </Grid>
            <Grid item xs={6}>
              <TextField
                name="height"
                label="Height (cm)"
                fullWidth
                type="number"
                value={data.height}
                onChange={handleDashboardChange}
              />
            </Grid>
            <Grid item xs={6}>
              <TextField
                name="weight"
                label="Weight (Kg)"
                fullWidth
                type="number"
                value={data.weight}
                onChange={handleDashboardChange}
              />
            </Grid>
            <Grid item xs={4}>
              <TextField
                name="dob_day"
                label="Day"
                variant="outlined"
                fullWidth
                helperText="Date of Birth"
                type="number"
                value={data.dob_day}
                onChange={handleDashboardChange}
              />
            </Grid>

            <Grid item xs={4}>
              <TextField
                name="dob_month"
                label="Month"
                variant="outlined"
                fullWidth
                helperText="Month of Birth"
                type="number"
                value={data.dob_month}
                onChange={handleDashboardChange}
              />
            </Grid>

            <Grid item xs={4}>
              <TextField
                name="dob_year"
                label="Year"
                variant="outlined"
                fullWidth
                helperText="Year of Birth"
                type="number"
                value={data.dob_year}
                onChange={handleDashboardChange}
              />
            </Grid>

            <Grid item xs={6}>
              <InputLabel>Diet Type</InputLabel>
              <Select
                name="diet"
                value={data.diet}
                onChange={handleDashboardChange}
                fullWidth
              >
                <MenuItem value="Vegan">Vegan</MenuItem>
                <MenuItem value="Vegetarian">Vegetarian</MenuItem>
                <MenuItem value="Non-Vegetarian">Non-Vegetarian</MenuItem>
              </Select>
            </Grid>
            <Grid item xs={6}>
              <InputLabel>Excercise</InputLabel>
              <Select
                name="exercise"
                value={data.exercise}
                onChange={handleDashboardChange}
                fullWidth
              >
                <MenuItem value="Yoga">Yoga</MenuItem>

                <MenuItem value="Mild Exercises">
                  Mild Excercises - Walks, Jogs
                </MenuItem>

                <MenuItem value="Heavy Exercises">
                  Heavy Excercises - Running, Lifting
                </MenuItem>

                <MenuItem value="No">No Excercise</MenuItem>
              </Select>
            </Grid>
          </Grid>

          <Button
            variant="outlined"
            color="primary"
            type="submit"
            className="w-48"
          >
            Submit
          </Button>
        </form>
      </div>
    </div>
  );
};

export default ProfileModal;
```

`Record.jsx`

```jsx
import React, { useEffect } from "react";
import crossIcon from "../img/cross icon.svg";
import { TextField, Button } from "@mui/material";
import { useGlobalContext } from "./context";

const Record = ({ record, setRecord }) => {
  const { handleDashboardChange, data, handleDashboardSubmit, setData } =
    useGlobalContext();
  const closeRecord = () => {
    setRecord(false);
  };

  useEffect(() => {
    const handleClickOutside = (event) => {
      if (event.target.classList.contains("modal")) {
        closeRecord();
      }
    };

    if (record) {
      document.addEventListener("click", handleClickOutside);
    }

    return () => {
      document.removeEventListener("click", handleClickOutside);
    };
  }, [record]);

  if (!record) {
    return null;
  }

  return (
    <div className="fixed top-0 left-0 w-screen h-screen flex justify-center items-center p-4 backdrop-blur-sm modal">
      <div className="flex flex-col justify-center items-center w-72 sm:w-1/3  flex-wrap  bg-white rounded-lg shadow-lg px-8 py-8">
        <div className="w-full flex justify-end">
          <button onClick={closeRecord} className="hover:scale-105">
            <img src={crossIcon} alt="cross-icon" loading="lazy" />
          </button>
        </div>
        <form
          className="w-full"
          onSubmit={(e) => {
            e.preventDefault();
            closeRecord();
            handleDashboardSubmit(e);
          }}
        >
          <h1 className="text-2xl p-1 font-semibold text-gray-700">
            Update your Medical Info
          </h1>
          <TextField
            name="current_med"
            label="Current Medications"
            fullWidth
            margin="normal"
            multiline
            rows={3}
            helperText="Separate values by commas"
            onChange={handleDashboardChange}
            value={data.current_med}
          />
          <TextField
            name="medical_history"
            label="Medical History"
            fullWidth
            margin="normal"
            multiline
            rows={3}
            helperText="Separate values by commas"
            onChange={handleDashboardChange}
            value={data.medical_history}
          />
          <Button variant="outlined" color="success" type="submit">
            Submit
          </Button>
        </form>
      </div>
    </div>
  );
};

export default Record;
```

`RegisterForm.jsx`

```jsx
import { useGlobalContext } from "./context";
import { TextField, Button } from "@mui/material";

import cancelIcon from "../img/cross icon.svg";
import { useState } from "react";
import SignIn from "./GoogleSignIn";

const RegisterForm = () => {
  const { submitRegistration, email, username, password, closeModal } =
    useGlobalContext();

  const [user_email, setUserEmail] = useState();
  const [user_username, setUserUsername] = useState();
  const [user_password, setUserPassword] = useState();
  const [emailExists, setEmailExists] = useState();

  // Regular expression for password check
  const passwordRegex =
    /^(?=.*[A-Za-z])(?=.*\d)(?=.*[@$!%*#?&])[A-Za-z\d@$!%*#?&]{8,}$/;

  const isPasswordValid = (password) => {
    return passwordRegex.test(password);
  };

  const handleSubmit = async (event) => {
    event.preventDefault(); 
    if (isPasswordValid(user_password)) {
      try {
        const fetchResponse = await fetch(
          `http://127.0.0.1:8000/check_email?email=${user_email}`
        );
        const data = await fetchResponse.json();
        console.log(data.email_exists);
  
        if (data.email_exists) {
          setEmailExists("Email already exists");
        } else {
          submitRegistration(event); 
        }
      } catch (error) {
        console.error("Error checking email:", error);
      }
    } else {
      console.log("Invalid password"); 
    }
  };
  

  return (
    <div>
      <div className="flex justify-end mb-2 mr-2 ">
        <button onClick={closeModal}>
          <img src={cancelIcon} alt="cross" />
        </button>
      </div>
      <div className="flex flex-col gap-2">
        <div className="flex flex-col items-center gap-2">
          <h2 className="text-4xl modal-heading text-center full">
            Sign Up Now!
          </h2>
          <p className="text-center full">
            Access personalized healthcare services
          </p>
        </div>
        <form onSubmit={handleSubmit} className="flex flex-col gap-2 j">
          <TextField
            id="FormBasicEmail"
            label="Email"
            variant="outlined"
            value={user_email}
            onChange={(event) => {
              setUserEmail(event.target.value);
              setEmailExists("");
              email.current = event.target.value;
            }}
            color="success"
    
            required
          />
          <p
  className="text-gray-500 font-medium text-red-500"
  style={{ fontSize: "12px", width: "280px", textAlign: "center" }}
>
  {emailExists}
</p>

          <TextField
            id="formBasicUsername"
            label="Username"
            variant="outlined"
            value={user_username}
            onChange={(event) => {
              setUserUsername(event.target.value);
              username.current = event.target.value;
            }}
            color="success"
            required
          />
          <TextField
            id="formBasicPassword"
            label="Password"
            variant="outlined"
            value={user_password}
            onChange={(event) => {
              setUserPassword(event.target.value);
              password.current = event.target.value;
            }}
            color={isPasswordValid(user_password) ? "success" : "error"}
            type="password"
            required
          />
          {!isPasswordValid(user_password) && (
            <p
              className="text-gray-500 font-medium"
              style={{ fontSize: "12px", width: "280px", textAlign: "center" }}
            >
              Password must be at least 8 characters long, contain at least 1
              letter, 1 digit, and 1 special character.
            </p>
          )}
          <Button variant="outlined" color="primary" type="submit">
            Submit
          </Button>
          <SignIn />
        </form>
      </div>
    </div>
  );
};

export default RegisterForm;
```

`Services.jsx`

```jsx
import { Button } from "@mui/material";
import servicesImg from "../img/services-img.svg";
import diseasePredImg from "../img/diseasepredictor.svg";
import { useGlobalContext } from "./context";
const Services = () => {
  const { setLoginButtonClicked } = useGlobalContext();
  return (
    <>
      <div id="services" className="w-full flex flex-col items-center">
        <div className="services-container pt-20 mf:pt-0 flex flex-col md:flex-row items-center md:justify-center lg:w-5/6 flex-wrap">
          <div className="img-wrapper w-96 lg:w-1/2 flex pt-2 ">
            <img src={servicesImg} alt="hero-image" className="block w-full" />
          </div>
          <div className="hero flex flex-col justify-center w-5/6 md:w-1/2 pl-5 md:pl-8 items-center md:items-start md:p-5">
            <div className="hero-text px-1.5 sm:px-10 md:px-0 text-3xl lg:text-4xl  mb-2 sm:text-center md:text-left md:mb-5">
              Access Quality Healthcare Assistance Anytime, Anywhere
            </div>
            <div className="hero-stanza lg:text-lg flex items-center pl-2 pr-4 md:pr-0 md:w-4/5 mb-7">
              Medware provides you with your go to Healthcare Services at the
              ease of your device from any location!
            </div>
          </div>
        </div>
        <div className="disease-predictor flex  flex-col md:flex-row items-center">
          <div className="img-wrapper-predicto w-screen sm:w-4/5 p-1 sm:p-0 md:w-3/5 flex">
            <img
              src={diseasePredImg}
              alt="hero-image"
              className="block w-full"
            />
          </div>
          <div className=" w-4/5 md:w-1/2">
            <div className=" flex flex-col justify-center md:pl-16 p-2 pt-8 md:p-5">
              <div className="hero-text text-3xl lg:text-6xl mb-3 md:mb-5">
                Feeling low?
              </div>
              <div className="hero-stanza lg:text-xl flex items-center md:w-4/5 mb-3 md:mb-7">
                Use our built in Disease Predictor and get recommendations and
                medical assistance based on that
              </div>
              <div className="hero-btn-container flex gap-3 items-center">
                <Button
                  variant="outlined"
                  color="secondary"
                  className="hover:scale-105 md:w-60 md:h-16 hover:transition-all duration-200"
                  onClick={() => {
                    setLoginButtonClicked(true);
                  }}
                >
                  Disease Predictor
                </Button>
                <Button
                  variant="outlined"
                  color="primary"
                  className="hover:scale-105 md:w-60 md:h-16 hover:transition-all duration-200"
                  onClick={() => {
                    setLoginButtonClicked(true);
                  }}
                >
                  Contact Doctor
                </Button>
              </div>
            </div>
          </div>
        </div>
      </div>
    </>
  );
};

export default Services;
```

`Sidebar.jsx`

```jsx
import React from "react";
import record from "../img/record.svg";
import profile from "../img/profile.svg";
import settings from "../img/settings.svg";
import consumption from "../img/cons.svg";

const Sidebar = ({
  setRecord,
  setLogModal,
  setProfileModal,
  setConsumptionModal,
}) => {
  const handleRecord = () => {
    setRecord(true);
  };
  const handleLogModal = () => {
    setLogModal(true);
  };
  const handleProfileModal = () => {
    setProfileModal(true);
  };

  const handleConsumptionModal = () => {
    setConsumptionModal(true);
  };

  return (
    <div className="flex sm:flex-col justify-center items-center gap-5 m-1 sm:mt-10">
      <button
        onClick={handleProfileModal}
        className="w-10 h-10 p-1 hover:scale-90 hover:cursor-pointer hover:bg-teal-600 rounded-full duration-200 transition-all"
      >
        <img src={profile} alt="" className="w-full" />
      </button>
      <button
        onClick={handleRecord}
        className="w-10 h-10 p-1 hover:scale-90 hover:cursor-pointer hover:bg-teal-600 rounded-full duration-200 transition-all"
      >
        <img src={record} alt="" className="w-full" />
      </button>
      <button
        onClick={handleLogModal}
        className="w-10 h-10 p-1.5 hover:scale-90 hover:cursor-pointer hover:bg-teal-600 rounded-full duration-200 transition-all"
      >
        <img src={settings} alt="" className="w-full" />
      </button>
      <button
        onClick={handleConsumptionModal}
        className="w-9 h-9 p-1 hover:scale-90 hover:cursor-pointer hover:bg-teal-600 rounded-full duration-200 transition-all"
      >
        <img src={consumption} alt="" className="w-full" />
      </button>
    </div>
  );
};

export default Sidebar;