# **Step by step guide to create Voice to Text guide** 

### **1\. Set Up a Next.js Project**

Create a new Next.js project if you don’t have one:  
bash  
Copy code  
`npx create-next-app@latest voice-to-text-app`  
`cd voice-to-text-app`

* 

Install necessary dependencies:  
bash  
Copy code  
`npm install framer-motion axios`

![][image1]

### **2\. Create the Home Component**

* In the `pages/index.js` file, set up the main component for the app. Your component will handle voice-to-text input, translation, and task management.  
* Copy the code you provided into the `index.js` file.

### **3\. Setting Up Speech Recognition**

* Use the `SpeechRecognition` API for converting speech into text. The `useEffect` hook in the code ensures the speech recognition starts when the user clicks the button.  
* When the user speaks, the API captures chunks of speech and adds them to the `transcript`.  
* By default, the language is set to Hindi (`hi-IN`), but you can modify it for other languages.

### **4\. Translation Using OpenAI API (Backend API)**

Create a simple translation API in your `pages/api/translate.js` to convert the Hindi transcript into English:  
javascript  
Copy code  
`import { Configuration, OpenAIApi } from "openai";`

`const configuration = new Configuration({`  
  `apiKey: process.env.OPENAI_API_KEY,`  
`});`  
`const openai = new OpenAIApi(configuration);`

`export default async function handler(req, res) {`  
  `const { text } = req.body;`  
  `try {`  
    `const response = await openai.createCompletion({`  
      `model: "text-davinci-003",`  
      ``prompt: `Translate this Hindi text to English: ${text}`,``  
      `max_tokens: 100,`  
    `});`  
    `const translatedText = response.data.choices[0].text.trim();`  
    `res.status(200).json(translatedText);`  
  `} catch (error) {`  
    `console.error("Error with OpenAI translation:", error);`  
    `res.status(500).json({ error: "Translation failed" });`  
  `}`  
`}`

* 

Ensure you have your OpenAI API key set up in an `.env` file:  
makefile  
Copy code  
`OPENAI_API_KEY=your_openai_api_key_here`

* 

### **5\. Voice-to-Text Functionality**

* The `useEffect` hook contains the logic for the speech recognition process. When the user clicks the "Start Listening" button, it triggers the recognition of speech.  
* If the recognized language is Hindi (`hi-IN`), the app sends the transcript to the `/api/translate` endpoint to translate it into English using OpenAI.  
* If the language is already in English, the app directly adds the task to the list.

### **6\. Task Management**

* The tasks are displayed on the right side of the screen in a scrollable list.  
* You are using the `motion` component from `framer-motion` to add a smooth animation effect when new tasks are added.  
* ![][image2]

### **7\. Handle Listening State**

* The `isListening` state controls whether the app is actively listening for user input.  
* The app starts or stops listening based on the button click, toggling the state.

### **8\. Styling the Application**

You’ve added basic styling with Tailwind CSS for the layout and the components (buttons, tasks, etc.). Ensure Tailwind CSS is installed and configured:  
bash  
Copy code  
`npm install -D tailwindcss postcss autoprefixer`  
`npx tailwindcss init`

* 

In `tailwind.config.js`, add:  
javascript  
Copy code  
`module.exports = {`  
  `content: ["./pages/**/*.{js,ts,jsx,tsx}", "./components/**/*.{js,ts,jsx,tsx}"],`  
  `theme: {`  
    `extend: {},`  
  `},`  
  `plugins: [],`  
`};`

* 

In `styles/globals.css`, import Tailwind CSS:  
css  
Copy code  
`@tailwind base;`  
`@tailwind components;`  
`@tailwind utilities;`

### **9\. Create the API Endpoint for Translation**

##### In the `pages/api/translate.js` file, set up the OpenAI translation API: javascript Copy code `import { OpenAI } from "openai";`

##### 

##### export default async function handler(req, res) {

#####   if (req.method \=== "POST") {

#####     const { text } \= req.body;

##### 

#####     const openai \= new OpenAI({

#####       apiKey: process.env.OPENAI\_API\_KEY,

#####     });

##### 

#####     try {

#####       // Send the text to GPT-3.5 Turbo for translation

#####       const completion \= await openai.chat.completions.create({

#####         messages: \[

#####           {

#####             role: "user",

#####             content: \`Convert ${text} into English\`,

#####           },

#####         \],

#####         model: "gpt-3.5-turbo",

#####       });

##### 

#####       // Respond with the translated text

#####       res.status(200).json(completion.choices\[0\].message.content);

#####     } catch (error) {

#####       console.error(error.message);

#####       res.status(400).json({ error: error.message });

#####     }

#####   } else {

#####     res.status(405).json({ error: "Method not allowed" });

#####   }

##### }

* ##### 

* ##### This API receives the transcript text (e.g., in Hindi), sends it to OpenAI for translation, and responds with the translated English text.

### 

### **10 . Running the Application**

To run the application, use:  
bash  
Copy code  
`npm run dev`

* Open the application at http://localhost:3000. You should now be able to click the "Start Listening" button to begin recording tasks via voice input.