### **Step-by-Step Guide: Building a Content Summarization Application Using AI (GPT-3.5)**

This guide will walk you through building a simple content summarization web application using Next.js and the OpenAI GPT-3.5 API.

---

### **1\. Set Up a Next.js Project**

First, create a new Next.js project:  
bash  
Copy code  
`npx create-next-app@latest summarize-app`  
`cd summarize-app`

* 

Install necessary dependencies, including `axios` and `openai`:  
bash  
Copy code  
`npm install axios openai`

* 

---

### **2\. Set Up OpenAI API Key**

Create a `.env.local` file in the root directory of your project to securely store your OpenAI API key:  
makefile  
Copy code  
`OPENAI_API_KEY=your_openai_api_key_here`

*   
* Ensure the `.env.local` file is included in `.gitignore` to prevent sensitive information from being committed to version control.

---

### **3\. Create the Frontend (Text Input Form)**

In the `pages/index.js` file, add the following code to create the form where users can input text, select the summary length, style, and submit the request:  
javascript  
Copy code  
`import { useState } from 'react';`  
`import axios from 'axios';`

`export default function Home() {`  
  `const [text, setText] = useState('');`  
  `const [length, setLength] = useState('short');`  
  `const [style, setStyle] = useState('concise');`  
  `const [summary, setSummary] = useState('');`

  `const handleSubmit = async (e) => {`  
    `e.preventDefault();`

    `try {`  
      `const response = await axios.post('/api/summarize', { text, length, style });`  
      `setSummary(response.data);`  
    `} catch (error) {`  
      `console.error('Error summarizing text:', error);`  
    `}`  
  `};`

  `return (`  
    `<div className="container mx-auto p-4">`  
      `<h1 className="text-2xl mb-4">Text Summarization Application</h1>`  
      `<form onSubmit={handleSubmit}>`  
        `<div className="mb-4">`  
          `<label className="block text-gray-700">Text:</label>`  
          `<textarea`  
            `rows="6"`  
            `value={text}`  
            `onChange={(e) => setText(e.target.value)}`  
            `className="w-full border p-2"`  
          `/>`  
        `</div>`  
        `<div className="mb-4">`  
          `<label className="block text-gray-700">Length:</label>`  
          `<select value={length} onChange={(e) => setLength(e.target.value)} className="w-full border p-2">`  
            `<option value="short">Short</option>`  
            `<option value="medium">Medium</option>`  
            `<option value="long">Long</option>`  
          `</select>`  
        `</div>`  
        `<div className="mb-4">`  
          `<label className="block text-gray-700">Style:</label>`  
          `<select value={style} onChange={(e) => setStyle(e.target.value)} className="w-full border p-2">`  
            `<option value="concise">Concise</option>`  
            `<option value="detailed">Detailed</option>`  
            `<option value="formal">Formal</option>`  
            `<option value="informal">Informal</option>`  
          `</select>`  
        `</div>`  
        `<button type="submit" className="bg-blue-500 text-white px-4 py-2 rounded">Summarize</button>`  
      `</form>`  
      `{summary && (`  
        `<div className="mt-4 p-4 border border-gray-200">`  
          `<h2 className="text-xl mb-2">Summary:</h2>`  
          `<p>{summary}</p>`  
        `</div>`  
      `)}`  
    `</div>`  
  `);`  
`}`

*   
* This form allows users to input text, choose the desired summary length and style, and displays the generated summary.

---
