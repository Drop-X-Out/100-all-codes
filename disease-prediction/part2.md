```

`Calendar.jsx`

```jsx
import React, { useState, useRef, useEffect } from "react";

const Calendar = () => {
  const currentDate = new Date();
  const [selectedDate, setSelectedDate] = useState(currentDate);
  const [selectedMonth, setSelectedMonth] = useState(currentDate.getMonth());
  const [selectedYear, setSelectedYear] = useState(currentDate.getFullYear());
  const [isMonthDropdownOpen, setIsMonthDropdownOpen] = useState(false);
  const [isYearDropdownOpen, setIsYearDropdownOpen] = useState(false);

  const monthDropdownRef = useRef(null);
  const yearDropdownRef = useRef(null);

  const getDaysInMonth = (year, month) => {
    return new Date(year, month + 1, 0).getDate();
  };

  const getMonthName = (month) => {
    const options = { month: "short" };
    return new Intl.DateTimeFormat("en-US", options).format(
      new Date(2000, month, 1)
    );
  };

  const getWeekdayName = (day) => {
    const options = { weekday: "short" };
    return new Intl.DateTimeFormat("en-US", options).format(
      new Date(2000, 0, day + 1)
    );
  };

  const handleMonthChange = (month) => {
    setSelectedMonth(month);
    setIsMonthDropdownOpen(false);
  };

  const handleYearChange = (year) => {
    setSelectedYear(year);
    setIsYearDropdownOpen(false);
  };

  const handleDateClick = (date) => {
    setSelectedDate(date);
  };

  const handleClickOutside = (event) => {
    if (
      monthDropdownRef.current &&
      !monthDropdownRef.current.contains(event.target)
    ) {
      setIsMonthDropdownOpen(false);
    }

    if (
      yearDropdownRef.current &&
      !yearDropdownRef.current.contains(event.target)
    ) {
      setIsYearDropdownOpen(false);
    }
  };

  useEffect(() => {
    document.addEventListener("click", handleClickOutside);
    return () => {
      document.removeEventListener("click", handleClickOutside);
    };
  }, []);

  const renderDaysList = () => {
    const daysList = [];

    for (let i = 0; i < 7; i++) {
      const dayName = getWeekdayName(i);
      daysList.push(
        <div
          key={`day-${i}`}
          className="flex-1 text-center w-8 text-gray-700 text-sm font-semibold"
        >
          {dayName}
        </div>
      );
    }

    return daysList;
  };

  const renderMonthDropdown = () => {
    const monthOptions = Array.from({ length: 12 }, (_, i) => {
      const month = new Date(2000, i, 1).toLocaleString("default", {
        month: "short",
      });
      return {
        value: i,
        label: month,
      };
    });

    return (
      <div className="relative inline-block" ref={monthDropdownRef}>
        <button
          className="bg-white text-teal-500  hover:scale-105 w-18 sm:mx-1  transition-all duration-300 text-3xl font-bold"
          onClick={() => setIsMonthDropdownOpen(!isMonthDropdownOpen)}
        >
          {getMonthName(selectedMonth)}
        </button>
        {isMonthDropdownOpen && (
          <div className="absolute mt-2 py-1 w-20 overflow-y-auto max-h-44 bg-white text-gray-600 rounded-lg z-10 font-medium">
            {monthOptions.map((option) => (
              <div
                key={option.value}
                className="px-2 py-1 hover:bg-gray-200 cursor-pointer text-sm"
                onClick={() => handleMonthChange(option.value)}
              >
                {option.label}
              </div>
            ))}
          </div>
        )}
      </div>
    );
  };

  const renderYearDropdown = () => {
    const yearOptions = Array.from({ length: 50 }, (_, i) => {
      const year = currentDate.getFullYear() - 25 + i;
      return {
        value: year,
        label: year.toString(),
      };
    });

    return (
      <div className="relative inline-block" ref={yearDropdownRef}>
        <button
          className="bg-white text-gray-700 hover:scale-105 w-16 sm:mx-1 transition-all duration-300 text-2xl font-bold"
          onClick={() => setIsYearDropdownOpen(!isYearDropdownOpen)}
        >
          {selectedYear}
        </button>
        {isYearDropdownOpen && (
          <div className="absolute mt-2 py-1 w-24 max-h-44 overflow-y-auto text-gray-600 bg-white  rounded-lg    z-10 font-medium">
            {yearOptions.map((option) => (
              <div
                key={option.value}
                className="px-2 py-1 hover:bg-gray-200 cursor-pointer text-sm"
                onClick={() => handleYearChange(option.value)}
              >
                {option.label}
              </div>
            ))}
          </div>
        )}
      </div>
    );
  };

  const renderCalendar = () => {
    const daysInMonth = getDaysInMonth(selectedYear, selectedMonth);
    const firstDay = new Date(selectedYear, selectedMonth, 1).getDay();

    const calendar = [];

    // Add empty cells for previous month
    for (let i = 0; i < firstDay; i++) {
      calendar.push(<div key={`empty-${i}`} className="w-8 h-8"></div>);
    }

    // Add cells for current month
    for (let i = 1; i <= daysInMonth; i++) {
      const date = new Date(selectedYear, selectedMonth, i);
      const isSelected = date.toDateString() === selectedDate.toDateString();
      const isCurrentDate = date.toDateString() === currentDate.toDateString();

      const cellClasses = `w-8 h-8 rounded-2xl text-sm text-center cursor-pointer ${
        isSelected
          ? "bg-teal-500 text-white transition-all duration-300 "
          : isCurrentDate
          ? "bg-sky-500 text-white"
          : "text-gray-600"
      }`;

      calendar.push(
        <div
          key={`date-${i}`}
          className={cellClasses}
          onClick={() => handleDateClick(date)}
        >
          <div className="flex items-center justify-center h-full">{i}</div>
        </div>
      );
    }

    return calendar;
  };

  return (
    <div className="w-full rounded-lg flex overflow-scroll flex-col items-center p-3 justify-center md:flex-row lg:flex-col bg-white">
      <div className="rounded-s-lg flex gap-1 items-center mb-2 justify-center sm:w-40">
        <div className="text-teal-500 font-bold text-xl">
          {renderMonthDropdown()}
        </div>
        <div className="text-2xl font-bold">{renderYearDropdown()}</div>
      </div>
      <div className="flex flex-col rounded-e-md justify-center p-1 ">
        <div className="flex justify-center sm:mb-">
          <div className="grid-container mx-auto">
            <div className="gridd gap-1">{renderDaysList()}</div>
          </div>
        </div>
        <div className="grid-container mx-auto">
          <div className="gridd gap-1">{renderCalendar()}</div>
        </div>
      </div>
    </div>
  );
};

export default Calendar;
```

`ConsumptionModal.jsx`

```jsx
import React, { useEffect } from "react";
import crossIcon from "../img/cross icon.svg";
import {
  TextField,
  Button,
  FormControl,
  Select,
  MenuItem,
  Grid,
  InputLabel,
} from "@mui/material";
import { useGlobalContext } from "./context";

const ConsumptionModal = ({ consumptionModal, setConsumptionModal }) => {
  const { handleDashboardChange, data, handleDashboardSubmit } =
    useGlobalContext();
  const closeConsumptionModal = () => {
    setConsumptionModal(false);
  };

  useEffect(() => {
    const handleClickOutside = (event) => {
      if (event.target.classList.contains("modal")) {
        closeConsumptionModal();
      }
    };

    if (consumptionModal) {
      document.addEventListener("click", handleClickOutside);
    }

    return () => {
      document.removeEventListener("click", handleClickOutside);
    };
  }, [consumptionModal]);

  if (!consumptionModal) {
    return null;
  }

  return (
    <div className="fixed top-0 left-0 w-screen h-screen flex justify-center items-center p-4 backdrop-blur-sm modal">
      <div className="flex flex-col justify-center items-center w-72 xl:w-1/4  flex-wrap  bg-white rounded-lg shadow-lg px-8 py-8">
        <div className="w-full flex justify-end">
          <button
            onClick={closeConsumptionModal}
            className="z-50 hover:scale-105"
          >
            <img src={crossIcon} alt="cross-icon" loading="lazy" />
          </button>
        </div>
        <form
          className="w-full flex flex-col gap-6"
          onSubmit={(e) => {
            e.preventDefault();
            closeConsumptionModal();
            handleDashboardSubmit(e);
          }}
        >
          <Grid container spacing={4}>
            <Grid item xs={12}>
              <h1 className="text-2xl p-1 font-semibold text-gray-700">
                Consumption Data
              </h1>
            </Grid>
            <Grid item xs={12}>
              <FormControl variant="outlined" fullWidth>
                <InputLabel id="demo-simple-select-label">
                  Smoking Consumption
                </InputLabel>
                <Select
                  labelId="dropdown-label"
                  value={data.smoke_cons}
                  onChange={handleDashboardChange}
                  name="smoke_cons"
                  label="Smoke Consumption"
                >
                  <MenuItem value={"No Consumption"}>Non Smoker</MenuItem>
                  <MenuItem value={"Mild Smoking"}>Mild Smoking</MenuItem>
                  <MenuItem value={"Oftenly Smokes/ Addiction"}>
                    Addiction
                  </MenuItem>
                </Select>
              </FormControl>
            </Grid>
            <Grid item xs={12}>
              <FormControl variant="outlined" fullWidth>
                <InputLabel> Alcohol Consumption</InputLabel>
                <Select
                  labelId="dropdown-label"
                  value={data.alcohol_cons}
                  onChange={handleDashboardChange}
                  name="alcohol_cons"
                  label="Alcohol Consumption"
                >
                  <MenuItem value={"No Consumption"}>No Consumption</MenuItem>
                  <MenuItem value={"Mild Consumption"}>Mild</MenuItem>
                  <MenuItem value={"High Consumption"}>
                    High Consumption
                  </MenuItem>
                </Select>
              </FormControl>
            </Grid>
          </Grid>
          <Button variant="outlined" color="success" type="submit">
            Submit
          </Button>
        </form>
      </div>
    </div>
  );
};

export default ConsumptionModal;
```

`ContactDoctor.jsx`

```jsx
import React, { useEffect, useRef, useState } from "react";
import axios from "axios";
import DoctorProfile from "./DoctorProfile";
import SkeletonLoader from "./SkeletonLoader";
import { Autocomplete, TextField } from "@mui/material";

const docOptions = [
  "Family Medicine",
  "Internal Medicine",
  "Pediatrician",
  "Gynecologist",
  "Cardiologist",
  "Oncologist",
  "Gastroenterologist",
  "Pulmonologist",
  "Infectious disease",
  "Nephrologist",
  "Endocrinologist",
  "Ophthalmologist",
  "Otolaryngologist",
  "Dermatologist",
  "Psychiatrist",
  "Neurologist",
  "Radiologist",
  "Anesthesiologist",
  "Surgeon",
  "Physician executive",
];

const ContactDoctor = () => {
  const [doctors, setDoctors] = useState([]);
  const [speciality, setSpeciality] = useState(null);
  const doctorType = useRef("All");

  const fetchData = async () => {
    try {
      const response = await axios.get(
        `http://127.0.0.1:8000/doctor/${doctorType.current}`
      );
      console.log(response.data);
      setDoctors(response.data);
    } catch (error) {
      console.error(error);
    }
  };

  useEffect(() => {
    const fetchData = async () => {
      try {
        const response = await axios.get(
          `http://127.0.0.1:8000/doctor/${doctorType.current}`
        );
        console.log(response.data);
        setDoctors(response.data);
      } catch (error) {
        console.error(error);
      }
    };

    fetchData();
  }, []);

  const handleDocChange = () => {
    if (speciality === "" || !docOptions.includes(speciality)) {
      alert("Please select a valid option");
    } else {
      doctorType.current = speciality;
      fetchData();
    }
  };

  return (
    <section className="w-screen flex flex-col items-center p-5 h-full pt-24">
      <div className="flex w-80 md:w-3/5   justify-center gap-3 items-center mt-4 mb-4">
        <Autocomplete
          options={docOptions}
          value={speciality}
          onChange={(e, newValue) => {
            setSpeciality(newValue);
          }}
          className="searchbox w-5/6  bg-white"
          renderInput={(params) => (
            <TextField
              variant="outlined"
              color="primary"
              {...params}
              label="Select a speciality.."
            />
          )}
        />
        <button
          onClick={handleDocChange}
          className="w-24 text-base font-semibold text-center hover:text-white hover:bg-emerald-500 rounded-lg  border-emerald-500 border-2 text-emerald-500 focus:ring-4 focus:outline-none focus:ring-blue-100 transition-all duration-300 py-2"
        >
          Search
        </button>
      </div>
      <div className="border-t border-gray-200 mb-8 w-4/5"></div>

      <div className="flex justify-center w-full mt-4">
        {doctors.length ? (
          <DoctorProfile doctors={doctors} />
        ) : (
          <SkeletonLoader />
        )}
      </div>
      <article id="info-contact doctor"></article>
    </section>
  );
};

export default ContactDoctor;
```

![Untitled](https://talktohire.blr1.digitaloceanspaces.com/Untitled.png)

`Dashboard.jsx`

```jsx
import React, { useState, useEffect } from "react";
import axios from "axios";
import PatientForm from "./PatientForm";
import PatientProfile from "./PatientProfile";
import { useGlobalContext } from "./context";

axios.defaults.xsrfCookieName = "csrftoken";
axios.defaults.xsrfHeaderName = "X-CSRFToken";

const Dashboard = () => {
  const { handleInputChange, formData, handleFormSubmit, data, fetchData } =
    useGlobalContext();

  useEffect(() => {
    fetchData();
  }, []);

  return (
    <div className="pt-24">
      <PatientForm
        profileData={formData}
        handleInputChange={handleInputChange}
        handleFormSubmit={handleFormSubmit}
        patientData={data}
      />
      <PatientProfile responseData={data} />
    </div>
  );
};

export default Dashboard;
```

![Untitled](https://talktohire.blr1.digitaloceanspaces.com/Untitled_1.png)

`DoctorProfile.jsx`

```jsx
const DoctorProfile = ({ doctors }) => {
  return (
    <article className="flex justify-center w-screen gap-10 flex-wrap">
      {doctors.map((doctor) => (
        <div
          key={doctor.id}
          className="w-72 h-96 max-w-sm bg-white rounded-lg shadow-md flex flex-col justify-center items-center mb-5 p-2"
        >
          <img
            className="w-24 h-24 mb-3 rounded-full shadow-lg"
            src={doctor.image_link}
            alt="Image"
          />
          <h5 className=" text-xl text-gray-900 font-semibold">
            {doctor.name}
          </h5>
          <span
            className=" font-normal
              text-gray-600"
          >
            {doctor.speciality}
          </span>
          <div className="flex mt-4 space-x-3 md:mt-6 ">
            <a
              href="tel:${doctor.mobile_no}"
              className="w-24 h-10 py-2 text-sm font-semibold text-center text-white bg-teal-500 rounded-lg hover:bg-transparent hover:border-teal-500 hover:border-2 hover:text-teal-500 focus:ring-4 focus:outline-none focus:ring-blue-100 transition-all duration-300 "
            >
              Contact
            </a>
            <a
              href="#"
              className="w-24 h-10 py-2 text-sm font-semibold text-center hover:text-white border-2 border-teal-500 hover:bg-teal-500 rounded-lg bg-transparent  text-teal-500 focus:ring-4 focus:outline-none focus:ring-blue-100 transition-all duration-300"
            >
              View Profile
            </a>
          </div>
          <div className="mt-1 italic text-gray-500 text-sm font-semibold">
            {doctor.experience} Years of Experience
          </div>
          <div className="px-1 mt-2 text-gray-700 pl-4 w-5/6">
            <h2 className=" text-base font-semibold font  w-full">Address:</h2>
            <p className="italic text-sm h-14 overflow-scroll">
              {doctor.work_address}
            </p>
          </div>
        </div>
      ))}
    </article>
  );
};

export default DoctorProfile;
```

`Footer.jsx`

```jsx
import footer_logo from "../img/footerimg.svg";

export default function Footer() {
  return (
    <footer className="bg-white dark:bg-gray-900">
      <div className="mx-auto w-full max-w-screen-xl p-4 py-6 lg:py-8">
        <div className="md:flex md:justify-between">
          <div className="mb-6 md:mb-0">
            <a href="" className="flex items-center">
              <img src={footer_logo} className="h-8 mr-3" alt="FlowBite Logo" />
              <span className="self-center text-2xl font-semibold whitespace-nowrap dark:text-white">
                Medware
              </span>
            </a>
          </div>
          <div className="grid grid-cols-2 gap-12 sm:gap-8 sm:grid-cols-3">
            <div>
              <h2 className="mb-6 text-sm font-semibold text-gray-900 uppercase dark:text-white">
                Follow us
              </h2>
              <ul className="text-gray-600 dark:text-gray-400 font-medium">
                <li className="mb-4">
                  <a href="" className="hover:underline ">
                    Github
                  </a>
                </li>
                <li>
                  <a href="" className="hover:underline">
                    Discord
                  </a>
                </li>
              </ul>
            </div>
            <div>
              <h2 className="mb-6 text-sm font-semibold text-gray-900 uppercase dark:text-white">
                Legal
              </h2>
              <ul className="text-gray-600 dark:text-gray-400 font-medium">
                <li className="mb-4">
                  <a href="#" className="hover:underline">
                    Privacy Policy
                  </a>
                </li>
                <li>
                  <a href="#" className="hover:underline">
                    Terms &amp; Conditions
                  </a>
                </li>
              </ul>
            </div>
          </div>
        </div>
        <hr className="my-6 border-gray-200 sm:mx-auto dark:border-gray-700 lg:my-8" />
        <div className="sm:flex sm:items-center sm:justify-between">
          <span className="text-sm text-gray-500 sm:text-center dark:text-gray-400">
            © 2023{" "}
            <a href="" className="hover:underline">
              Medware™
            </a>
            . All Rights Reserved.
          </span>
          <div className="flex mt-4 space-x-6 sm:justify-center sm:mt-0">
            <a
              href="#"
              className="text-gray-500 hover:text-gray-900 dark:hover:text-white"
            >
              <svg
                className="w-5 h-5"
                fill="currentColor"
                viewBox="0 0 24 24"
                aria-hidden="true"
              >
                <path
                  fillRule="evenodd"
                  d="M22 12c0-5.523-4.477-10-10-10S2 6.477 2 12c0 4.991 3.657 9.128 8.438 9.878v-6.987h-2.54V12h2.54V9.797c0-2.506 1.492-3.89 3.777-3.89 1.094 0 2.238.195 2.238.195v2.46h-1.26c-1.243 0-1.63.771-1.63 1.562V12h2.773l-.443 2.89h-2.33v6.988C18.343 21.128 22 16.991 22 12z"
                  clipRule="evenodd"
                />
              </svg>
              <span className="sr-only">Facebook page</span>
            </a>
            <a
              href="#"
              className="text-gray-500 hover:text-gray-900 dark:hover:text-white"
            >
              <svg
                className="w-5 h-5"
                fill="currentColor"
                viewBox="0 0 24 24"
                aria-hidden="true"
              >
                <path
                  fillRule="evenodd"
                  d="M12.315 2c2.43 0 2.784.013 3.808.06 1.064.049 1.791.218 2.427.465a4.902 4.902 0 011.772 1.153 4.902 4.902 0 011.153 1.772c.247.636.416 1.363.465 2.427.048 1.067.06 1.407.06 4.123v.08c0 2.643-.012 2.987-.06 4.043-.049 1.064-.218 1.791-.465 2.427a4.902 4.902 0 01-1.153 1.772 4.902 4.902 0 01-1.772 1.153c-.636.247-1.363.416-2.427.465-1.067.048-1.407.06-4.123.06h-.08c-2.643 0-2.987-.012-4.043-.06-1.064-.049-1.791-.218-2.427-.465a4.902 4.902 0 01-1.772-1.153 4.902 4.902 0 01-1.153-1.772c-.247-.636-.416-1.363-.465-2.427-.047-1.024-.06-1.379-.06-3.808v-.63c0-2.43.013-2.784.06-3.808.049-1.064.218-1.791.465-2.427a4.902 4.902 0 011.153-1.772A4.902 4.902 0 015.45 2.525c.636-.247 1.363-.416 2.427-.465C8.901 2.013 9.256 2 11.685 2h.63zm-.081 1.802h-.468c-2.456 0-2.784.011-3.807.058-.975.045-1.504.207-1.857.344-.467.182-.8.398-1.15.748-.35.35-.566.683-.748 1.15-.137.353-.3.882-.344 1.857-.047 1.023-.058 1.351-.058 3.807v.468c0 2.456.011 2.784.058 3.807.045.975.207 1.504.344 1.857.182.466.399.8.748 1.15.35.35.683.566 1.15.748.353.137.882.3 1.857.344 1.054.048 1.37.058 4.041.058h.08c2.597 0 2.917-.01 3.96-.058.976-.045 1.505-.207 1.858-.344.466-.182.8-.398 1.15-.748.35-.35.566-.683.748-1.15.137-.353.3-.882.344-1.857.048-1.055.058-1.37.058-4.041v-.08c0-2.597-.01-2.917-.058-3.96-.045-.976-.207-1.505-.344-1.858a3.097 3.097 0 00-.748-1.15 3.098 3.098 0 00-1.15-.748c-.353-.137-.882-.3-1.857-.344-1.023-.047-1.351-.058-3.807-.058zM12 6.865a5.135 5.135 0 110 10.27 5.135 5.135 0 010-10.27zm0 1.802a3.333 3.333 0 100 6.666 3.333 3.333 0 000-6.666zm5.338-3.205a1.2 1.2 0 110 2.4 1.2 1.2 0 010-2.4z"
                  clipRule="evenodd"
                />
              </svg>
              <span className="sr-only">Instagram page</span>
            </a>
            <a
              href="#"
              className="text-gray-500 hover:text-gray-900 dark:hover:text-white"
            >
              <svg
                className="w-5 h-5"
                fill="currentColor"
                viewBox="0 0 24 24"
                aria-hidden="true"
              >
                <path d="M8.29 20.251c7.547 0 11.675-6.253 11.675-11.675 0-.178 0-.355-.012-.53A8.348 8.348 0 0022 5.92a8.19 8.19 0 01-2.357.646 4.118 4.118 0 001.804-2.27 8.224 8.224 0 01-2.605.996 4.107 4.107 0 00-6.993 3.743 11.65 11.65 0 01-8.457-4.287 4.106 4.106 0 001.27 5.477A4.072 4.072 0 012.8 9.713v.052a4.105 4.105 0 003.292 4.022 4.095 4.095 0 01-1.853.07 4.108 4.108 0 003.834 2.85A8.233 8.233 0 012 18.407a11.616 11.616 0 006.29 1.84" />
              </svg>
              <span className="sr-only">Twitter page</span>
            </a>
            <a
              href="#"
              className="text-gray-500 hover:text-gray-900 dark:hover:text-white"
            >
              <svg
                className="w-5 h-5"
                fill="currentColor"
                viewBox="0 0 24 24"
                aria-hidden="true"
              >
                <path
                  fillRule="evenodd"
                  d="M12 2C6.477 2 2 6.484 2 12.017c0 4.425 2.865 8.18 6.839 9.504.5.092.682-.217.682-.483 0-.237-.008-.868-.013-1.703-2.782.605-3.369-1.343-3.369-1.343-.454-1.158-1.11-1.466-1.11-1.466-.908-.62.069-.608.069-.608 1.003.07 1.531 1.032 1.531 1.032.892 1.53 2.341 1.088 2.91.832.092-.647.35-1.088.636-1.338-2.22-.253-4.555-1.113-4.555-4.951 0-1.093.39-1.988 1.029-2.688-.103-.253-.446-1.272.098-2.65 0 0 .84-.27 2.75 1.026A9.564 9.564 0 0112 6.844c.85.004 1.705.115 2.504.337 1.909-1.296 2.747-1.027 2.747-1.027.546 1.379.202 2.398.1 2.651.64.7 1.028 1.595 1.028 2.688 0 3.848-2.339 4.695-4.566 4.943.359.309.678.92.678 1.855 0 1.338-.012 2.419-.012 2.747 0 .268.18.58.688.482A10.019 10.019 0 0022 12.017C22 6.484 17.522 2 12 2z"
                  clipRule="evenodd"
                />
              </svg>
              <span className="sr-only">GitHub account</span>
            </a>
            <a
              href="#"
              className="text-gray-500 hover:text-gray-900 dark:hover:text-white"
            >
              <svg
                className="w-5 h-5"
                fill="currentColor"
                viewBox="0 0 24 24"
                aria-hidden="true"
              >
                <path
                  fillRule="evenodd"
                  d="M12 2C6.48 2 2 6.48 2 12s4.48 10 10 10c5.51 0 10-4.48 10-10S17.51 2 12 2zm6.605 4.61a8.502 8.502 0 011.93 5.314c-.281-.054-3.101-.629-5.943-.271-.065-.141-.12-.293-.184-.445a25.416 25.416 0 00-.564-1.236c3.145-1.28 4.577-3.124 4.761-3.362zM12 3.475c2.17 0 4.154.813 5.662 2.148-.152.216-1.443 1.941-4.48 3.08-1.399-2.57-2.95-4.675-3.189-5A8.687 8.687 0 0112 3.475zm-3.633.803a53.896 53.896 0 013.167 4.935c-3.992 1.063-7.517 1.04-7.896 1.04a8.581 8.581 0 014.729-5.975zM3.453 12.01v-.26c.37.01 4.512.065 8.775-1.215.25.477.477.965.694 1.453-.109.033-.228.065-.336.098-4.404 1.42-6.747 5.303-6.942 5.629a8.522 8.522 0 01-2.19-5.705zM12 20.547a8.482 8.482 0 01-5.239-1.8c.152-.315 1.888-3.656 6.703-5.337.022-.01.033-.01.054-.022a35.318 35.318 0 011.823 6.475 8.4 8.4 0 01-3.341.684zm4.761-1.465c-.086-.52-.542-3.015-1.659-6.084 2.679-.423 5.022.271 5.314.369a8.468 8.468 0 01-3.655 5.715z"
                  clipRule="evenodd"
                />
              </svg>
              <span className="sr-only">Dribbble account</span>
            </a>
          </div>
        </div>
      </div>
    </footer>
  );
}
```

`GlucoseLevel.jsx`

```jsx
import React from "react";

const GlucoseLevel = ({ responseData }) => {
  if (
    !responseData ||
    !responseData.blood_glucose ||
    !responseData.blood_glucose.date ||
    responseData.blood_glucose.date.length === 0
  ) {
    return (
      <p className="w-full h-full grid place-content-center italic  md:text-lg text-gray-500 bg-sky-50">
        No values in log
      </p>
    );
  }

  const getCellStyles = (value, isAfterColumn) => {
    if (isAfterColumn) {
      if (value > 180) {
        return "bg-red-100";
      }
    } else {
      if (value > 120) {
        return "bg-red-100";
      }
    }
    return "";
  };

  let prevDate = null; // Track previous date

  return (
    <div className="px-1 bg-white">
      <table className="w-full border-collapse">
        <thead>
          <tr className="bg-gray-200">
            <th className="py-2 px-4 border-b font-semibold text-left">Date</th>
            <th className="py-2 px-4 border-b font-semibold text-left">
              Before
            </th>
            <th className="py-2 px-4 border-b font-semibold text-left">
              After
            </th>
          </tr>
        </thead>
        <tbody>
          {responseData.blood_glucose.date.map((date, index) => {
            const currentBefore = responseData.blood_glucose.before[index];
            const currentAfter = responseData.blood_glucose.after[index];

            // Skip the date if both before and after values are empty
            if (!currentBefore && !currentAfter) {
              return null;
            }

            const isFirstDate = prevDate !== date;
            prevDate = date;

            return (
              <tr key={index} className="border-b">
                <td
                  className={`py-2 px-1 border-r text-gray-800 ${
                    isFirstDate ? "bg-sky-100" : "px-5"
                  }`}
                >
                  <div className="flex items-center">
                    {isFirstDate && (
                      <div className="w-2 h-2 bg-gray-500 rounded-full mr-2"></div>
                    )}
                    <div>{date}</div>
                  </div>
                </td>
                <td
                  className={`py-2 px-4 border-r ${getCellStyles(
                    currentBefore,
                    false
                  )}`}
                >
                  {currentBefore}
                </td>
                <td
                  className={`py-2 px-4 ${getCellStyles(currentAfter, true)}`}
                >
                  {currentAfter}
                </td>
              </tr>
            );
          })}
        </tbody>
      </table>
    </div>
  );
};

export default GlucoseLevel;
```

`GoogleSignIn.jsx`

```jsx
import { useEffect, useRef } from "react";
import jwt_decode from "jwt-decode";
import { useGlobalContext } from "./context";

const SignIn = () => {
  const {
    email,
    submitLogin,
    username,
    password,
    submitRegistration,
  } = useGlobalContext();

  const userObject = useRef({});

  const handleCallback = async (response, event) => {
    console.log(response.credential);
    userObject.current = jwt_decode(response.credential);
    console.log(userObject.current);

    username.current = userObject.current.name;
    email.current = userObject.current.email;
    password.current = response.credential.slice(0, 8);

    console.log(username.current);
    console.log(email.current);
    console.log(password.current);

    try {
      const fetchResponse = await fetch(
        "http://127.0.0.01:8000/check_email?email=" + email.current
      );
      const data = await fetchResponse.json();
      console.log(data.email_exists);

      if (data.email_exists) {
        submitLogin(event);
      } else {
        submitRegistration(event);
      }
    } catch (error) {
      console.error("Error:", error);
    }
  };

  useEffect(() => {
    const initializeGoogleSignIn = () => {
      window.google.accounts.id.initialize({
        client_id:
          "your-client-id-here-from-google-developer-account",
        callback: (response) => handleCallback(response, null),
      });

      window.google.accounts.id.renderButton(
        document.getElementById("signInDiv"),
        {
          theme: "outline",
          size: "large",
        }
      );
    };

    initializeGoogleSignIn();
  }, []);

  return (
    <div className="App">
      <div id="signInDiv"></div>
    </div>
  );
};

export default SignIn;
```

`Header.jsx`

```jsx
import React, { useRef, useEffect } from "react";
import LoginBtn from "./Login-Button";
import LogoHorizontal from "../img/logo.svg";
import { useGlobalContext } from "./context";
import { NavLink } from "react-router-dom";
import { useState } from "react";
import cancelIcon from "../img/cross icon.svg";

const Header = () => {
  const { currentUser } = useGlobalContext();
  const [dropdown, setDropdown] = useState(false);
  const dropdownRef = useRef(null);

  const toggleDropdownMenu = () => {
    setDropdown(!dropdown);
  };

  const handleClickOutsideDropdown = (event) => {
    if (dropdownRef.current && !dropdownRef.current.contains(event.target)) {
      setDropdown(false);
    }
  };

  useEffect(() => {
    document.addEventListener("mousedown", handleClickOutsideDropdown);
    return () => {
      document.removeEventListener("mousedown", handleClickOutsideDropdown);
    };
  }, []);

  return (
    <div className="fixed z-40 bg-white">
      <div className="shadow-md flex justify-around gap-24 items-center w-screen py-3">
        <figure className="h-16 w-auto z-20">
          <img
            src={LogoHorizontal}
            alt="logo-header"
            className="h-full w-full object-cover"
          />
        </figure>
        <nav className="hidden lg:flex lg:gap-3 text-lg z-20 justify-center text-gray-600">
          {currentUser ? (
            <div className="flex lg:gap-4">
              <NavLink to="/" className="px-1">
                Predictor
              </NavLink>
              <NavLink to="dashboard" className="px-1">
                Dashboard
              </NavLink>
              <NavLink to="contactdoctor" className="px-1">
                Consult
              </NavLink>
            </div>
          ) : (
            <>
              <a href="#services" className="px-1">
                Services
              </a>
              <a href="#about" className="px-1">
                About Us
              </a>
            </>
          )}
          <LoginBtn />
        </nav>
        <button className="block lg:hidden" onClick={toggleDropdownMenu}>
          <svg
            xmlns="http://www.w3.org/2000/svg"
            fill="none"
            viewBox="0 0 24 24"
            strokeWidth={1.5}
            stroke="currentColor"
            className="w-8 h-8 hover:rotate-180 transition all duration-200"
          >
            <path
              strokeLinecap="round"
              strokeLinejoin="round"
              d="M3.75 6.75h16.5M3.75 12h16.5m-16.5 5.25h16.5"
            />
          </svg>
        </button>
      </div>

      {dropdown ? (
        <div
          ref={dropdownRef}
          className="dropdown fixed top-30 mt-2 lg:hidden right-2 w-52 h-60 shadow-md rounded-md bg-slate-50 p-2 px-8 mr-3"
        >
          <nav className="gap-1 flex-col  text-xl w-full text-gray-900">
            <button
              className="w-full flex justify-end mb-5  hover:bg-transparent"
              onClick={() => {
                setDropdown(false);
              }}
            >
              <img src={cancelIcon} alt="" className="w-7 hover:scale-95" />
            </button>
            {currentUser ? (
              <>
                <NavLink
                  to="/"
                  onClick={() => {
                    setDropdown(false);
                  }}
                >
                  Predictor
                </NavLink>
                <div className="border-t border-gray-300 my-1.5" />
                <NavLink
                  to="dashboard"
                  onClick={() => {
                    setDropdown(false);
                  }}
                >
                  Dashboard
                </NavLink>
                <div className="border-t border-gray-300 my-1.5" />
                <NavLink
                  to="contactdoctor"
                  onClick={() => {
                    setDropdown(false);
                  }}
                >
                  Consult
                </NavLink>
              </>
            ) : (
              <>
                <a
                  href="#services"
                  className="p-5 flex justify-center"
                  onClick={() => {
                    setDropdown(false);
                  }}
                >
                  Services
                </a>
                <div className="border-t border-gray-300 my-1.5" />
                <a
                  href="#about"
                  className="p-5 flex justify-center"
                  onClick={() => {
                    setDropdown(false);
                  }}
                >
                  About Us
                </a>
              </>
            )}
            <div className="border-t border-gray-300 my-1.5" />
            <LoginBtn />
          </nav>
        </div>
      ) : null}
    </div>
  );
};

export default Header;
```