

`SkeletonLoader.jsx`

```jsx
import React from "react";

const SkeletonLoader = () => {
  return (
    <article className="flex flex-wrap gap-10 w-screen justify-center">
      <div
        role="status"
        className="space-y-8 animate-pulse items-center flex flex-col w-72 h-96 shadow-md p-4"
      >
        <div className="flex items-center justify-center w-20 h-20 bg-gray-300 rounded-full ">
          <svg
            className="w-12 h-12 text-gray-200"
            xmlns="http://www.w3.org/2000/svg"
            aria-hidden="true"
            fill="currentColor"
            viewBox="0 0 640 512"
          >
            <path d="M480 80C480 35.82 515.8 0 560 0C604.2 0 640 35.82 640 80C640 124.2 604.2 160 560 160C515.8 160 480 124.2 480 80zM0 456.1C0 445.6 2.964 435.3 8.551 426.4L225.3 81.01C231.9 70.42 243.5 64 256 64C268.5 64 280.1 70.42 286.8 81.01L412.7 281.7L460.9 202.7C464.1 196.1 472.2 192 480 192C487.8 192 495 196.1 499.1 202.7L631.1 419.1C636.9 428.6 640 439.7 640 450.9C640 484.6 612.6 512 578.9 512H55.91C25.03 512 .0006 486.1 .0006 456.1L0 456.1z" />
          </svg>
        </div>
        <div className="w-full h-90 flex flex-col items-center ">
          <div className="h-6 bg-gray-200 rounded-full w-48 mb-4"></div>
          <div className="flex w-full mb-2.5 justify-around">
            <div className="h-10 bg-gray-200 rounded-full w-1/2 mx-2"></div>
            <div className="h-10 bg-gray-200 rounded-full w-1/2 mx-2"></div>
          </div>
          <div className="h-2.5 w-full bg-gray-200 rounded-full m-2"></div>
          <div className="h-2.5 w-full bg-gray-200 rounded-full max-w-[440px] m-2"></div>
          <div className="h-2.5 w-full bg-gray-200 rounded-full  max-w-[460px] m-2"></div>
          <div className="h-2.5 bg-gray-200 rounded-full  max-w-[360px] w-full m-2"></div>
        </div>
        <span className="sr-only">Loading...</span>
      </div>
      <div
        role="status"
        className="space-y-8 animate-pulse items-center flex flex-col w-72 h-96 shadow-md p-4"
      >
        <div className="flex items-center justify-center w-20 h-20 bg-gray-300 rounded-full ">
          <svg
            className="w-12 h-12 text-gray-200"
            xmlns="http://www.w3.org/2000/svg"
            aria-hidden="true"
            fill="currentColor"
            viewBox="0 0 640 512"
          >
            <path d="M480 80C480 35.82 515.8 0 560 0C604.2 0 640 35.82 640 80C640 124.2 604.2 160 560 160C515.8 160 480 124.2 480 80zM0 456.1C0 445.6 2.964 435.3 8.551 426.4L225.3 81.01C231.9 70.42 243.5 64 256 64C268.5 64 280.1 70.42 286.8 81.01L412.7 281.7L460.9 202.7C464.1 196.1 472.2 192 480 192C487.8 192 495 196.1 499.1 202.7L631.1 419.1C636.9 428.6 640 439.7 640 450.9C640 484.6 612.6 512 578.9 512H55.91C25.03 512 .0006 486.1 .0006 456.1L0 456.1z" />
          </svg>
        </div>
        <div className="w-full h-90 flex flex-col items-center ">
          <div className="h-6 bg-gray-200 rounded-full w-48 mb-4"></div>
          <div className="flex w-full mb-2.5 justify-around">
            <div className="h-10 bg-gray-200 rounded-full w-1/2 mx-2"></div>
            <div className="h-10 bg-gray-200 rounded-full w-1/2 mx-2"></div>
          </div>
          <div className="h-2.5 w-full bg-gray-200 rounded-full m-2"></div>
          <div className="h-2.5 w-full bg-gray-200 rounded-full max-w-[440px] m-2"></div>
          <div className="h-2.5 w-full bg-gray-200 rounded-full  max-w-[460px] m-2"></div>
          <div className="h-2.5 bg-gray-200 rounded-full  max-w-[360px] w-full m-2"></div>
        </div>
        <span className="sr-only">Loading...</span>
      </div>
      <div
        role="status"
        className="space-y-8 animate-pulse items-center flex flex-col w-72 h-96 shadow-md p-4"
      >
        <div className="flex items-center justify-center w-20 h-20 bg-gray-300 rounded-full ">
          <svg
            className="w-12 h-12 text-gray-200"
            xmlns="http://www.w3.org/2000/svg"
            aria-hidden="true"
            fill="currentColor"
            viewBox="0 0 640 512"
          >
            <path d="M480 80C480 35.82 515.8 0 560 0C604.2 0 640 35.82 640 80C640 124.2 604.2 160 560 160C515.8 160 480 124.2 480 80zM0 456.1C0 445.6 2.964 435.3 8.551 426.4L225.3 81.01C231.9 70.42 243.5 64 256 64C268.5 64 280.1 70.42 286.8 81.01L412.7 281.7L460.9 202.7C464.1 196.1 472.2 192 480 192C487.8 192 495 196.1 499.1 202.7L631.1 419.1C636.9 428.6 640 439.7 640 450.9C640 484.6 612.6 512 578.9 512H55.91C25.03 512 .0006 486.1 .0006 456.1L0 456.1z" />
          </svg>
        </div>
        <div className="w-full h-90 flex flex-col items-center ">
          <div className="h-6 bg-gray-200 rounded-full w-48 mb-4"></div>
          <div className="flex w-full mb-2.5 justify-around">
            <div className="h-10 bg-gray-200 rounded-full w-1/2 mx-2"></div>
            <div className="h-10 bg-gray-200 rounded-full w-1/2 mx-2"></div>
          </div>
          <div className="h-2.5 w-full bg-gray-200 rounded-full m-2"></div>
          <div className="h-2.5 w-full bg-gray-200 rounded-full max-w-[440px] m-2"></div>
          <div className="h-2.5 w-full bg-gray-200 rounded-full  max-w-[460px] m-2"></div>
          <div className="h-2.5 bg-gray-200 rounded-full  max-w-[360px] w-full m-2"></div>
        </div>
        <span className="sr-only">Loading...</span>
      </div>
      <div
        role="status"
        className="space-y-8 animate-pulse items-center flex flex-col w-72 h-96 shadow-md p-4"
      >
        <div className="flex items-center justify-center w-20 h-20 bg-gray-300 rounded-full ">
          <svg
            className="w-12 h-12 text-gray-200"
            xmlns="http://www.w3.org/2000/svg"
            aria-hidden="true"
            fill="currentColor"
            viewBox="0 0 640 512"
          >
            <path d="M480 80C480 35.82 515.8 0 560 0C604.2 0 640 35.82 640 80C640 124.2 604.2 160 560 160C515.8 160 480 124.2 480 80zM0 456.1C0 445.6 2.964 435.3 8.551 426.4L225.3 81.01C231.9 70.42 243.5 64 256 64C268.5 64 280.1 70.42 286.8 81.01L412.7 281.7L460.9 202.7C464.1 196.1 472.2 192 480 192C487.8 192 495 196.1 499.1 202.7L631.1 419.1C636.9 428.6 640 439.7 640 450.9C640 484.6 612.6 512 578.9 512H55.91C25.03 512 .0006 486.1 .0006 456.1L0 456.1z" />
          </svg>
        </div>
        <div className="w-full h-90 flex flex-col items-center ">
          <div className="h-6 bg-gray-200 rounded-full w-48 mb-4"></div>
          <div className="flex w-full mb-2.5 justify-around">
            <div className="h-10 bg-gray-200 rounded-full w-1/2 mx-2"></div>
            <div className="h-10 bg-gray-200 rounded-full w-1/2 mx-2"></div>
          </div>
          <div className="h-2.5 w-full bg-gray-200 rounded-full m-2"></div>
          <div className="h-2.5 w-full bg-gray-200 rounded-full max-w-[440px] m-2"></div>
          <div className="h-2.5 w-full bg-gray-200 rounded-full  max-w-[460px] m-2"></div>
          <div className="h-2.5 bg-gray-200 rounded-full  max-w-[360px] w-full m-2"></div>
        </div>
        <span className="sr-only">Loading...</span>
      </div>
      <div
        role="status"
        className="space-y-8 animate-pulse items-center flex flex-col w-72 h-96 shadow-md p-4"
      >
        <div className="flex items-center justify-center w-20 h-20 bg-gray-300 rounded-full ">
          <svg
            className="w-12 h-12 text-gray-200"
            xmlns="http://www.w3.org/2000/svg"
            aria-hidden="true"
            fill="currentColor"
            viewBox="0 0 640 512"
          >
            <path d="M480 80C480 35.82 515.8 0 560 0C604.2 0 640 35.82 640 80C640 124.2 604.2 160 560 160C515.8 160 480 124.2 480 80zM0 456.1C0 445.6 2.964 435.3 8.551 426.4L225.3 81.01C231.9 70.42 243.5 64 256 64C268.5 64 280.1 70.42 286.8 81.01L412.7 281.7L460.9 202.7C464.1 196.1 472.2 192 480 192C487.8 192 495 196.1 499.1 202.7L631.1 419.1C636.9 428.6 640 439.7 640 450.9C640 484.6 612.6 512 578.9 512H55.91C25.03 512 .0006 486.1 .0006 456.1L0 456.1z" />
          </svg>
        </div>
        <div className="w-full h-90 flex flex-col items-center ">
          <div className="h-6 bg-gray-200 rounded-full w-48 mb-4"></div>
          <div className="flex w-full mb-2.5 justify-around">
            <div className="h-10 bg-gray-200 rounded-full w-1/2 mx-2"></div>
            <div className="h-10 bg-gray-200 rounded-full w-1/2 mx-2"></div>
          </div>
          <div className="h-2.5 w-full bg-gray-200 rounded-full m-2"></div>
          <div className="h-2.5 w-full bg-gray-200 rounded-full max-w-[440px] m-2"></div>
          <div className="h-2.5 w-full bg-gray-200 rounded-full  max-w-[460px] m-2"></div>
          <div className="h-2.5 bg-gray-200 rounded-full  max-w-[360px] w-full m-2"></div>
        </div>
        <span className="sr-only">Loading...</span>
      </div>
      <div
        role="status"
        className="space-y-8 animate-pulse items-center flex flex-col w-72 h-96 shadow-md p-4"
      >
        <div className="flex items-center justify-center w-20 h-20 bg-gray-300 rounded-full ">
          <svg
            className="w-12 h-12 text-gray-200"
            xmlns="http://www.w3.org/2000/svg"
            aria-hidden="true"
            fill="currentColor"
            viewBox="0 0 640 512"
          >
            <path d="M480 80C480 35.82 515.8 0 560 0C604.2 0 640 35.82 640 80C640 124.2 604.2 160 560 160C515.8 160 480 124.2 480 80zM0 456.1C0 445.6 2.964 435.3 8.551 426.4L225.3 81.01C231.9 70.42 243.5 64 256 64C268.5 64 280.1 70.42 286.8 81.01L412.7 281.7L460.9 202.7C464.1 196.1 472.2 192 480 192C487.8 192 495 196.1 499.1 202.7L631.1 419.1C636.9 428.6 640 439.7 640 450.9C640 484.6 612.6 512 578.9 512H55.91C25.03 512 .0006 486.1 .0006 456.1L0 456.1z" />
          </svg>
        </div>
        <div className="w-full h-90 flex flex-col items-center ">
          <div className="h-6 bg-gray-200 rounded-full w-48 mb-4"></div>
          <div className="flex w-full mb-2.5 justify-around">
            <div className="h-10 bg-gray-200 rounded-full w-1/2 mx-2"></div>
            <div className="h-10 bg-gray-200 rounded-full w-1/2 mx-2"></div>
          </div>
          <div className="h-2.5 w-full bg-gray-200 rounded-full m-2"></div>
          <div className="h-2.5 w-full bg-gray-200 rounded-full max-w-[440px] m-2"></div>
          <div className="h-2.5 w-full bg-gray-200 rounded-full  max-w-[460px] m-2"></div>
          <div className="h-2.5 bg-gray-200 rounded-full  max-w-[360px] w-full m-2"></div>
        </div>
        <span className="sr-only">Loading...</span>
      </div>
    </article>
  );
};

export default SkeletonLoader;
```

`SugarChart.jsx`

```jsx
import React from "react";
import {
  Chart as ChartJS,
  CategoryScale,
  LinearScale,
  PointElement,
  LineElement,
  Title,
  Tooltip,
  Legend,
} from "chart.js";
import { Line } from "react-chartjs-2";

export default function SugarChart({ chartData }) {
  const { after, before, date } = chartData;

  ChartJS.register(
    CategoryScale,
    LinearScale,
    PointElement,
    LineElement,
    Title,
    Tooltip,
    Legend
  );

  // Modify the date array to cut the strings to the first 10 characters

  const options = {
    responsive: true,
    maintainAspectRatio: false,
    interaction: {
      mode: "index",
      intersect: false,
    },
    stacked: false,
    plugins: {
      title: {
        display: true,
        text: "Glucose Breakfast",
      },
    },
    scales: {
      y: {
        type: "linear",
        display: true,
        position: "left",
        grid: {
          display: false,
        },
      },
    },
    elements: {
      line: {
        tension: 0.4, // Adjust the tension to control the curvature of the line
      },
    },
  };

  const data = {
    labels: date,
    datasets: [
      {
        label: "Before",
        data: after,
        borderColor: "rgba(0, 205, 145, 0.61)",
        backgroundColor: "white",
        yAxisID: "y",
      },
      {
        label: "After",
        data: before,
        borderColor: "rgba(84, 18, 255, 0.68)",
        backgroundColor: "white",
        yAxisID: "y",
      },
    ],
  };

  return <Line options={options} data={data} />;
}
```

`context.jsx`

```jsx
import React, { useState, useContext, useEffect, useRef } from "react";
import axios from "axios";

const AppContext = React.createContext();

axios.defaults.xsrfCookieName = "csrftoken";
axios.defaults.xsrfHeaderName = "X-CSRFToken";
axios.defaults.withCredentials = true;

const client = axios.create({
  baseURL: "http://127.0.0.1:8000",
});

const AppProvider = ({ children }) => {
  const options = [
    "Itching",
    "Skin rash",
    "Shivering",
    "Chills",
    "Joint pain",
    "Stomach pain",
    "Acidity",
    "Ulcers on tongue",
    "Muscle wasting",
    "Vomiting",
    "Burning micturition",
    "Spotting urination",
    "Fatigue",
    "Weight gain",
    "Anxiety",
    "Cold hands and feets",
    "Mood swings",
    "Weight loss",
    "Restlessness",
    "Lethargy",
    "Patches in throat",
    "Irregular sugar level",
    "Cough",
    "High fever",
    "Sunken eyes",
    "Breathlessness",
    "Sweating",
    "Dehydration",
    "Indigestion",
    "Headache",
    "Yellowish skin",
    "Dark urine",
    "Nausea",
    "Loss of appetite",
    "Pain behind the eyes",
    "Back pain",
    "Constipation",
    "Abdominal pain",
    "Diarrhea",
    "Mild fever",
    "Yellow urine",
    "Yellowing of eyes",
    "Acute liver failure",
    "Fluid overload",
    "Swelling of stomach",
    "Swelled lymph nodes",
    "Malaise",
    "Blurred and distorted vision",
    "Phlegm",
    "Throat irritation",
    "Redness of eyes",
    "Sinus pressure",
    "Runny nose",
    "Congestion",
    "Chest pain",
    "Weakness in limbs",
    "Fast heart rate",
    "Pain during bowel movements",
    "Pain in anal region",
    "Bloody stool",
    "Irritation in anus",
    "Neck pain",
    "Dizziness",
    "Cramps",
    "Bruising",
    "Obesity",
    "Swollen legs",
    "Swollen blood vessels",
    "Puffy face and eyes",
    "Enlarged thyroid",
    "Brittle nails",
    "Swollen extremeties",
    "Excessive hunger",
    "Extra-marital contacts",
    "Drying and tingling lips",
    "Slurred speech",
    "Knee pain",
    "Hip joint pain",
    "Muscle weakness",
    "Stiff neck",
    "Swelling joints",
    "Movement stiffness",
    "Spinning movements",
    "Loss of balance",
    "Unsteadiness",
    "Weakness of one body side",
    "Loss of smell",
    "Bladder discomfort",
    "Foul smell of urine",
    "Continuous feel of urine",
    "Passage of gases",
    "Internal itching",
    "Toxic look",
    "Depression",
    "Irritability",
    "Muscle pain",
    "Altered sensorium",
    "Red spots over body",
    "Belly pain",
    "Abnormal menstruation",
    "Dischromic patches",
    "Watering from eyes",
    "Increased appetite",
    "Polyuria",
    "Family history",
    "Mucoid sputum",
    "Rusty sputum",
    "Lack of concentration",
    "Visual disturbances",
    "Receiving blood transfusion",
    "Receiving unsterile injections",
    "Coma",
    "Stomach bleeding",
    "Distention of abdomen",
    "History of alcohol consumption",
    "Blood in sputum",
    "Prominent veins on calf",
    "Palpitations",
    "Painful walking",
    "Pus-filled pimples",
    "Blackheads",
    "Scarring",
    "Skin peeling",
    "Silver-like dusting",
    "Small dents in nails",
    "Inflammatory nails",
    "Blister",
    "Red sore around nose",
    "Yellow crust oozing",
  ];

  const [currentUser, setCurrentUser] = useState();
  const [responseCall, setResponseCall] = useState(false);
  const [registrationToggle, setRegistrationToggle] = useState(false);
  const email = useRef("");
  const username = useRef("");
  const password = useRef("");
  const error = useRef("");
  const [age, setAge] = useState("");
  const [medicalhistory, setMedicalHistory] = useState([]);
  const [sex, setSex] = useState("");
  const [loginButtonClicked, setLoginButtonClicked] = useState(false);
  const url = "http://127.0.0.1:8000/patient";

  const [data, setData] = useState({});
  const [formData, setFormData] = useState({
    bp_log: { date: [], high: [], low: [] },
    blood_glucose: { date: [], before: [], after: [] },
  });

  useEffect(() => {
    client
      .get("/user")
      .then(function (res) {
        setCurrentUser(true);
      })
      .catch(function (error) {
        setCurrentUser(false);
      });
  }, []);

  function update_form_btn() {
    if (registrationToggle) {
      document.getElementById("form_btn").innerHTML = "Register";
      setRegistrationToggle(false);
      setLoginButtonClicked(true);
    } else {
      document.getElementById("form_btn").innerHTML = "Log in";
      setRegistrationToggle(true);
      setLoginButtonClicked(true);
    }
  }

  function closeModal() {
    setLoginButtonClicked(false);
  }

  function submitRegistration(e) {
    if (e) {
      e.preventDefault();
    }
    setResponseCall(true);
    client
      .post("/register", {
        email: email.current,
        username: username.current,
        password: password.current,
      })
      .then(function (res) {
        client
          .post("/login", {
            email: email.current,
            password: password.current,
          })
          .then(function (res) {
            setCurrentUser(true);
            setResponseCall(false);
          });
      });
  }

  function submitLogin(e) {
    if (e) {
      e.preventDefault();
    }
    setResponseCall(true);
    client
      .post("/login", {
        email: email.current,
        password: password.current,
      })
      .then(function (res) {
        setResponseCall(false);
        setCurrentUser(true);
        error.current = "";
      })
      .catch(function (error_) {
        error.current = "Wrong email or password. Please try again.";
        setResponseCall(false);
      });
  }

  function submitLogout() {
    client.post("/logout", { withCredentials: true }).then(function (res) {
      setCurrentUser(false);
    });
    // document.getElementById("signIndiv").hidden = false;
  }

  const handleInputChange = (event) => {
    const { name, value } = event.target;

    if (name === "medical_history" || name === "current_med") {
      const arrValue = value.split(","); // Split the string value into an array
      setFormData((prevData) => ({
        ...prevData,
        [name]: arrValue,
      }));
    } else {
      setFormData((prevData) => ({
        ...prevData,
        [name]: value,
      }));
    }
  };
  const handleDashboardChange = (event) => {
    const { name, value } = event.target;

    if (name === "medical_history" || name === "current_med") {
      const arrValue = value.split(","); // Split the string value into an array
      setData((prevData) => ({
        ...prevData,
        [name]: arrValue,
      }));
    } else {
      setData((prevData) => ({
        ...prevData,
        [name]: value,
      }));
    }
  };

  const handleFormSubmit = async (event) => {
    event.preventDefault();

    try {
      setFormData((prevData) => ({
        ...prevData,
        new_patient: false,
      }));

      await axios.put(url, formData, {
        withCredentials: true,
      });

      await fetchData();
    } catch (error) {
      console.log(error);
    }
  };
  const handleDashboardSubmit = async (event) => {
    event.preventDefault();

    try {
      await axios.put(url, data, {
        withCredentials: true,
      });

      await fetchData();
    } catch (error) {
      console.log(error);
    }
  };

  const fetchData = async () => {
    try {
      const response = await axios.get(url);
      setData(response.data);
      console.log(response.data);
    } catch (error) {
      console.log(error);
    }
  };

  return (
    <AppContext.Provider
      value={{
        update_form_btn,
        submitRegistration,
        submitLogin,
        submitLogout,
        currentUser,
        setCurrentUser,
        registrationToggle,
        setRegistrationToggle,
        email,
        username,
        password,
        age,
        setAge,
        medicalhistory,
        setMedicalHistory,
        sex,
        setSex,
        loginButtonClicked,
        setLoginButtonClicked,
        closeModal,
        options,
        handleInputChange,
        formData,
        setFormData,
        handleFormSubmit,
        url,
        data,
        setData,
        fetchData,
        handleDashboardSubmit,
        handleDashboardChange,
        error,
        responseCall,
        setResponseCall,
      }}
    >
      {children}
    </AppContext.Provider>
  );
};

export const useGlobalContext = () => {
  // console.log(useContext(AppContext));
  return useContext(AppContext);
};

export { AppContext, AppProvider };
```

`dpWindow.jsx`

```jsx
import { useRef, useState, useEffect } from "react";
import Button from "@mui/material/Button";
import SymptomSearch from "./searchSymptoms";
import { useGlobalContext } from "./context";
import cancelIcon from "../img/cross icon.svg";
import axios from "axios";
import Prediction from "./Prediction";
import dpImg from "../img/dp-image.svg";

const DpWindow = () => {
  let { options } = useGlobalContext();
  let index = useRef(null);
  let allSymptomsString = useRef(null);
  const [symptoms, setSymptoms] = useState([]);
  const [prediction, setPrediction] = useState(null);
  const [copySymptoms, setCopySymptoms] = useState([]);
  const [allSymptoms, setAllSymptoms] = useState(
    Array(options.length + 1).fill("0")
  );

  const [selectedSymptom, setSelectedSymptom] = useState(null);
  const isDuplicate = (symptom) => symptoms.includes(symptom);

  const handleAddSymptom = (event) => {
    if (selectedSymptom && !isDuplicate(selectedSymptom)) {
      index.current = options.indexOf(selectedSymptom) + 1;
      setSelectedSymptom(null);
      addSymptom(selectedSymptom);
    } else if (isDuplicate(selectedSymptom)) {
      alert("This symptom has already been added!");
    } else {
      alert("Choose a valid  symptom");
    }
  };

  const handleClick = () => {
    if (symptoms.length != 0) {
      setCopySymptoms(allSymptoms);
    }
  };

  const clearSymptoms = () => {
    setSymptoms([]);
    setAllSymptoms(Array(options.length + 1).fill("0"));
    setPrediction(false);
  };

  const addSymptom = (symptom) => {
    if (!symptom) return;

    if (!isDuplicate(symptom)) {
      setSymptoms((prevSymptoms) => [...prevSymptoms, symptom]);
    }
  };

  const removeSymptom = (symptom) => {
    setSymptoms((prevSymptoms) => prevSymptoms.filter((s) => s !== symptom));
    const symptomIndex = options.indexOf(symptom) + 1;
    setAllSymptoms((prevAllSymptoms) => {
      const newAllSymptoms = [...prevAllSymptoms];
      newAllSymptoms[symptomIndex] = "0";
      return newAllSymptoms;
    });

    // Check if symptoms will become empty after removing
    if (symptoms.length === 1) {
      setPrediction(false);
    }
  };

  useEffect(() => {
    const newSymptomsArray = [...allSymptoms];
    newSymptomsArray[index.current] = "1";
    setAllSymptoms(newSymptomsArray); // Log the value of index whenever it changes
  }, [index.current]);

  useEffect(() => {
    allSymptomsString.current = allSymptoms.join(""); // Convert allSymptoms array to string
    axios
      .get(`http://127.0.0.1:8000/prediction/${allSymptomsString.current}`)
      .then((response) => {
        if (symptoms.length != 0) {
          setPrediction(response.data);
        }
      })
      .catch((error) => {
        console.log(error);
      });
  }, [copySymptoms]); // axios useEffect

  return (
    <div className="dpWindow w-full flex items-center flex-col justify-evenly ">
      <div className="bttns-container flex w-2/3  xl:w-1/2 justify-center items-center">
        <SymptomSearch
          handleAddSymptom={handleAddSymptom}
          selectedSymptom={selectedSymptom}
          setSelectedSymptom={setSelectedSymptom}
        />
      </div>
      <div className="symptoms w-5/6 flex justify-center gap-10 flex-wrap">
        <div className="w-full md:w-4/5 lg:w-1/2 overflow-y-scroll">
          <div className="w-full h-full flex flex-col justify-between items-center p-1 pb-2">
            <h2 className="w-full text-xl lg:text-2xl xl:text-3xl p-1">
              Your Symptoms
            </h2>
            <div className="flex flex-wrap bg-green-50 w-full m-1 p-1 h-full rounded-lg content-start">
              {symptoms.length === 0 ? (
                <div className="text-gray-500 italic flex justify-center items-center text-lg w-full h-full ">
                  Add your first symptom
                </div>
              ) : (
                symptoms.map((symptom) => (
                  <div
                    key={symptom}
                    className="added-symptom p-2 m-1.5 flex rounded-md gap-2 text-center bg-green-200 text-green-950 h-11"
                  >
                    <div className="mb-1">{symptom}</div>
                    <button onClick={() => removeSymptom(symptom)}>
                      <img src={cancelIcon} alt="" className="h-5" />
                    </button>
                  </div>
                ))
              )}
            </div>
            <div className="btn-container w-full flex gap-2 px-2 mt-2 justify-center">
              <Button
                variant="outlined"
                color="primary"
                onClick={handleClick}
                className="w-1/2 md:w-1/3 h-11 "
              >
                Predict
              </Button>
              <Button
                variant="outlined"
                color="error"
                className="w-1/2 md:w-1/3 h-11 "
                onClick={clearSymptoms}
              >
                Clear Symptoms
              </Button>
            </div>
          </div>
        </div>
        <div className="w-full md:w-4/5 lg:w-1/3 p-2 flex flex-col">
          <h2 className="text-xl lg:text-2xl xl:text-3xl">Predicted Result</h2>
          {prediction ? (
            <Prediction prediction={prediction} />
          ) : (
            <div className="w-full h-full bg-sky-50 mt-2 rounded-lg p-2 flex justify-center items-center text-xl italic text-gray-500">
              No prediction
            </div>
          )}
        </div>
      </div>
      <section className="w-full flex justify-center sm:px-8 md:px-10 lg:px-12 xl:px-14 flex-wrap">
        <div className="hero flex flex-col justify-center sm:w-5/6 md:w-4/5 lg:w-3/5 xl:w-2/5  px-5 md:px-6 lg:pd-8">
          <div className="hero-text text-3xl lg:text-4xl mb-5">
            About our Disease Predictor
          </div>
          <div className="text-base xl:text-lg flex items-center w-full text-gray-800">
            Introducing our advanced disease predictor, a powerful tool designed
            to simplify healthcare for you. Built upon cutting-edge machine
            learning technology and trained on extensive medical data, our
            predictor provides accurate predictions and analysis for various
            diseases. With a user-friendly interface and easy-to-understand
            results, you can gain valuable insights into potential health risks
            and take proactive measures to safeguard your well-being.
          </div>
        </div>
        <div className="img-wrapper w-1/2 flex justify-center">
          <img src={dpImg} alt="hero-image" className="block w-4/5 z-10" />
        </div>
      </section>
    </div>
  );
};

export default DpWindow;
```

![Untitled](https://talktohire.blr1.digitaloceanspaces.com/Untitled_3.png)

`searchSymptoms.jsx`

```jsx
import React from "react";
import { Autocomplete, Button } from "@mui/material";
import TextField from "@mui/material/TextField";
import { useGlobalContext } from "./context";

const SymptomSearch = ({
  selectedSymptom,
  setSelectedSymptom,
  handleAddSymptom,
}) => {
  const { options } = useGlobalContext();

  return (
    <div className="flex w-4/5 lg:w-full justify-center gap-3 items-center   ">
      <Autocomplete
        options={options}
        value={selectedSymptom}
        onChange={(e, newValue) => {
          setSelectedSymptom(newValue);
        }}
        className="searchbox w-full bg-white"
        renderInput={(params) => (
          <TextField
            variant="outlined"
            color="primary"
            {...params}
            label="Enter your symptoms.."
          />
        )}
      />
      <Button
        variant="outlined"
        color="info"
        onClick={handleAddSymptom}
        size="large"
      >
        Add
      </Button>
    </div>
  );
};

export default SymptomSearch;
```

### Root Files

`App.jsx`

```jsx
import "./App.css";
import React from "react";
import Header from "./assets/components/Header";
import Main from "./assets/components/Main";
import Footer from "./assets/components/Footer";
import { BrowserRouter, Route, Routes } from "react-router-dom";
import Dashboard from "./assets/components/Dashboard";
import ContactDoctor from "./assets/components/ContactDoctor";

function App() {
  return (
    <BrowserRouter>
      <Header />
      <Routes>
        <Route path="dashboard" element={<Dashboard />} />
        <Route path="contactdoctor" element={<ContactDoctor />} />
        <Route path="/">
          <Route index element={<Main />} />
        </Route>
      </Routes>
      <Footer />
    </BrowserRouter>
  );
}

export default App;
```

`App.css`

```css
/* CSS Styles */
@import url("https://fonts.googleapis.com/css2?family=Comme:wght@500&family=Open+Sans:wght@500&family=Roboto:wght@400&display=swap");
* {
  padding: 0;
  margin: 0;
  box-sizing: border-box;
}

:root {
  --primary-color-1: #76c893;
  --primary-color-2: #52b69a;
  --secondary-color-1: #34a0a4;
  --secondary-color-2: #168aa6;
  --secondary-color-3: #093e1a;
  --accent-color-1: #c5f9c9;
  --background-color-1: #f5f5f5;
  --background-color-2: #152b2c;
  --font-family-1: "Roboto", sans-serif;
  --font-family-heading: "Comme", sans-serif;
  --font-family-2: "Open Sans", sans-serif;
}

html {
  scroll-behavior: smooth;
  font-smooth: auto;
  overflow-x: hidden;
}
nav {
  width: 40%;
}

nav a,
nav button {
  display: flex;
  align-items: center;
  height: 2rem;
  font-family: var(--font-family-2);
  font-weight: lighter;
}

nav a:hover,
nav button:hover {
  background-color: var(--primary-color-2);
  color: var(--background-color-1);
  border-radius: 8px;
  transition: all 0.2s ease-in-out;
}

.modal-heading {
  font-family: var(--font-family-heading);
}

/* hero section */

.hero-text {
  font-family: var(--font-family-heading);
}

.hero-stanza {
  font-family: var(--font-family-1);
}

.img-wrapper {
  min-width: 300px;
}

.img-wrapper-predictor {
  background-color: var(--accent-color-1);
}

.dpWindow {
  min-height: 165vh;
}
.symptoms {
  min-height: 60vh;
}
.added-symptoms {
  overflow: scroll;
}

.bttns-container:nth-child(n) {
  min-width: 425px;
}

/* footer */
footer {
  min-height: 25vh;
}

.heading {
  max-width: 1200px;
}

.grid-container {
  max-width: 100%;
}

/* Create a 7-column grid */
.gridd {
  display: grid;
  grid-template-columns: repeat(
    7,
    1fr
  ); /* Adjust the gap between columns if desired */
}

.dropdown-option {
  font-size: 12px;
  color: gray;
}

.sidebar {
  height: 90vh;
}
.current-medications {
  background-color: #ffffff;
  color: #4a5568;
  border-radius: 0.5rem;
  box-shadow: 0 1px 3px 0 rgba(0, 0, 0, 0.1), 0 1px 2px 0 rgba(0, 0, 0, 0.06);
  padding: 1.5rem;
}

.responseCall {
  background-color: rgba(153, 236, 182, 0.289);
}
```

### Image Assets

Inside your `assets` folder create an `img` directory with the following tree, you can use your own images or checkout the attached file below

```bash
img/
├── 1.svg
├── 5.svg
├── Gradient Modern Technology Company Developers Logo.zip
├── cons.svg
├── cross icon.svg
├── dashboard-hero.svg
├── diseasepredictor.svg
├── dp-image.svg
├── footerimg.svg
├── hero-arrow.svg
├── hero-img.svg
├── logo.svg
├── modal.svg
├── pattern.svg
├── profile.svg
├── record.svg
├── services-img.svg
├── settings.svg

```

[img.zip](img.zip)

The Disease Prediction App is now ready, you can start the server by the following command

```bash
python manage.py runserve
```

Run the Frontend by the following command

```bash
cd Frontend && npm run dev
```

Your Disease Prediction App is now ready

