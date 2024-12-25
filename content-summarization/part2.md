### **4\. Create the API for Summarization**

In the `pages/api/summarize.js` file, create the API route that will interact with the OpenAI GPT-3.5 API to summarize the provided text:  
javascript  
Copy code  
`import { OpenAI } from "openai";`

`export default async function handler(req, res) {`  
  `if (req.method === "POST") {`  
    `const { text, length, style } = req.body;`

    `const openai = new OpenAI({`  
      `apiKey: process.env.OPENAI_API_KEY,`  
    `});`

    `try {`  
      `// Call OpenAI to generate a summary`  
      `const completion = await openai.chat.completions.create({`  
        `messages: [`  
          `{`  
            `role: "user",`  
            ``content: `Summarize the following text in ${length} length and in a ${style} style: ${text}`,``  
          `},`  
        `],`  
        `model: "gpt-3.5-turbo",`  
      `});`

      `// Send the summary back to the client`  
      `res.status(200).json(completion.choices[0].message.content);`  
    `} catch (error) {`  
      `console.error('Error:', error.message);`  
      `res.status(400).json({ error: error.message });`  
    `}`  
  `} else {`  
    `res.status(405).json({ error: "Method not allowed" });`  
  `}`  
`}`

*   
* This API receives the user's input, communicates with OpenAI to generate the summary, and responds with the summarized content.

---

### **5\. Run the Application**

Start the development server:  
bash  
Copy code  
`npm run dev`

*   
* Navigate to http://localhost:3000 to view the text summarization app.

---

### **6\. Test the Application**

* Input a piece of text into the textarea.  
* Select a summary length (short, medium, or long) and style (concise, detailed, formal, or informal).  
* Submit the form to receive a summarized version of the input text.  
* The summarized content will appear below the form.

---

### **7\. Customize Further (Optional)**

* **Add More Styles/Lengths**: You can add more summarization styles or options by expanding the select elements and handling logic accordingly.  
* **Improve UI**: Use additional libraries like Tailwind CSS for a better user interface or animations.  
* **Validation**: Add form validation to ensure the input text is not empty before submitting the form.

