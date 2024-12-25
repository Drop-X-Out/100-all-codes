# How to create Web Text to PDF chrome extension in Node.js?

Chrome extensions are small programs to modify the experience or add functionality to the Chrome browser. We will be creating **Web Page Text to PDF** converter in Node which will be converting every text on the web page to PDF. This guide is divided into two parts, part one is  creating chrome extension and the part two is creating routes in Node and Express that will be handling all the text processing.

## **Part 1 : Creating Chrome Extension**

1. **Create the chrome extension directory:**

```jsx
mkdir chrome-extension
cd chrome-extension
```

1. **Create `manifest.json`:**

```jsx
{
    "manifest_version": 3,
    "name": "Text to PDF Converter",
    "version": "1.0",
    "description": "Scrape current page text and convert to PDF",
    "permissions": [
      "activeTab",
      "contextMenus",
      "scripting",
      "storage"
    ],
    "background": {
      "service_worker": "background.js"
    },
    "action": {
      "default_popup": "popup.html"
    }
  }
```

1. **Create `background.js`:**

```jsx
chrome.runtime.onInstalled.addListener(() => {
  chrome.contextMenus.create({
    id: "convertToPDF",
    title: "Convert page text to PDF",
    contexts: ["page", "selection"]
  });
});

chrome.contextMenus.onClicked.addListener((info, tab) => {
  if (info.menuItemId === "convertToPDF") {
    console.log("Getting convertToPDF");
    const selectedText = info.selectionText;
    console.log("Selected text:", selectedText);
    if (selectedText) {
      chrome.tabs.query({ active: true, currentWindow: true }, function (tabs) {
        const tab = tabs[0];
        chrome.scripting.executeScript({
          target: { tabId: tab.id },
          function: function (selectedText) {
            fetch('http://localhost:5000/api/generate-pdf', {
              method: 'POST',
              headers: {
                'Content-Type': 'application/json'
              },
              body: JSON.stringify({ text: selectedText })
            })
              .then(response => {
                if (!response.ok) {
                  throw new Error('Failed to generate PDF');
                }
                return response.arrayBuffer();
              })
              .then(arrayBuffer => {
                const blob = new Blob([arrayBuffer], { type: 'application/pdf' });
                const url = URL.createObjectURL(blob);
                const a = document.createElement('a');
                a.href = url;
                a.download = 'output.pdf';
                document.body.appendChild(a);
                a.click();
                document.body.removeChild(a);
                URL.revokeObjectURL(url);
              })
              .catch(error => console.error('Error:', error));
          },
          args: [selectedText]
        });
      });
    }
    else {
      console.log("Hit in all")
      chrome.tabs.query({ active: true, currentWindow: true }, function (tabs) {
        const tab = tabs[0];
        chrome.scripting.executeScript({
          target: { tabId: tab.id },
          function: function () {
          const allText = document.body.innerText;
            fetch('http://localhost:5000/api/generate-pdf', {
              method: 'POST',
              headers: {
                'Content-Type': 'application/json'
              },
              body: JSON.stringify({ text: allText })
            })
              .then(response => {
                if (!response.ok) {
                  throw new Error('Failed to generate PDF');
                }
                return response.arrayBuffer();
              })
              .then(arrayBuffer => {
                const blob = new Blob([arrayBuffer], { type: 'application/pdf' });
                const url = URL.createObjectURL(blob);
                const a = document.createElement('a');
                a.href = url;
                a.download = 'output.pdf';
                document.body.appendChild(a);
                a.click();
                document.body.removeChild(a);
                URL.revokeObjectURL(url);
              })
              .catch(error => console.error('Error:', error));
          },
          args: []
        });
      });
    }
  }
});
```

1. **Create popup.html**

```html
<!DOCTYPE html>
<html>
<head>
    <title>Text to PDF Converter</title>
    <!-- Include Tailwind CSS -->
    <link href="https://cdn.jsdelivr.net/npm/tailwindcss@2.2.19/dist/tailwind.min.css" rel="stylesheet">
</head>
<body class="bg-gray-100 w-64 h-screen flex flex-col justify-center items-center">
    <div class="text-xl text-slate-300 font-bold mb-3 mx-auto">Text to PDF Converter</div>
    <p class="text-sm mb-4 mr-4 ml-4">Right click and select "Convert into pdf" </p>
    <a href="http://localhost:5000/" target="_blank" class="bg-blue-500  hover:bg-blue-600 text-white font-bold py-2 px-4 rounded-full shadow-md focus:outline-none focus:ring-2 focus:ring-blue-500 focus:ring-opacity-50">
      View All PDFs
    </a>
</body>
</html>
```

1. **Upload your extension :** 

```
1. Open Google Chrome.
2. Open chrome://extensions/ in your tab.
3. Make sure your developer mode is on
4. Select 'Load unpacked' button.
5. Select your 'chrome-extension' folder.

// If the extension shows error it is because we have not set up the backend. 
// Kindly ignore 
```

1. **Test your extension** by right clicking on any web page, you will be able to see :

![ext2.png](https://talktohire.blr1.digitaloceanspaces.com/ext2.png)

![Untitled](https://talktohire.blr1.digitaloceanspaces.com/Untitled.png)

## **Part 2 : Create Back-end using Node and Express**

Here we will be creating a server using express on port 5000 with cross origin resource sharing **CORS** so that our extension can easy communicate with the node server. Also we are **body parser** so that we can easily send JSON formatted text from frontend to backend. We are also using **fs** in build module so that we can create files and manipulate them as we like. To create PDF file we are using **pdf kit** library here. 

1.  **Set Up Node Backend**

```jsx
mkdir text-to-pdf-backend
cd text-to-pdf-backend
npm init -y
```

1. **Install dependencies**:

```jsx
npm install express cors body-parser pdfkit ejs
```

1. **Create server.js** file inside the root folder
2. **Create Two folders :**

```jsx
mkdir views  //To make a html page that lists all the pdfs uploaded by the user
mkdir pdfs  //To store all the pdfs
//Create allpdfs.ejs inside the views folders
```

1. **Create the back-end server route to handle pdfs**:

```jsx
const express = require("express");
const cors = require("cors");
const bodyParser = require("body-parser");
const PDFDocument = require("pdfkit");
const fs = require("fs");

const app = express();
const PORT = 5000;

app.use(cors());
app.use(bodyParser.json());   //To parse JSON formatted data

app.post("/api/generate-pdf", async (req, res) => {
  const { text } = req.body;
  try {
    let uuid = crypto.randomUUID();
    const filePath = `${__dirname}\\pdfs\\${uuid}.pdf`;
    const writeStream = fs.createWriteStream(filePath);

    res.setHeader("Content-Type", "application/pdf");
    res.setHeader("Content-Disposition", `attachment; filename=${uuid}.pdf`);
    const doc = new PDFDocument();
    doc.pipe(writeStream);
    doc.text(text);
    doc.end();

    writeStream.on("close", async () => {
      try {
        const fileContent = await fs.promises.readFile(filePath);
        res.setHeader("Content-Type", "application/pdf");
        res.setHeader("Content-Disposition", `attachment; filename=${uuid}.pdf`);
        res.send(fileContent);
      } catch (err) {
        console.error("Error reading PDF file:", err);
        res.status(500).send("Error generating PDF");
      }
    });
  } catch (err) {
    console.error(err);
    res.status(500).send("Error generating PDF");
  }
});

app.listen(PORT, () =>
  console.log(`Server running on http://localhost:${PORT}`)
);
```

1. **Test out the Application:**

```
1. Go to any web page.
2. Right click on the web page and select "convert text to pdf".
3. You will be able to download the pdf of the text.
4. You can also select and then convert it into the pdf.
```

1. Now we will be creating routes to see all the **pdf links:**
Add the following routes to the server file.

```jsx
app.get('/', (req, res) => {
  fs.readdir(path.join(__dirname, 'pdfs'), (err, files) => {
    if (err) {
      console.error('Error reading PDF directory:', err);
      return res.status(500).send('Error reading PDF directory');
    }
    res.render('allpdfs', { files });
  });
});

app.get('/pdfs/:filename', (req, res) => {
  const { filename } = req.params;
  const filePath = path.join(__dirname, 'pdfs', filename);
  fs.access(filePath, fs.constants.R_OK, (err) => {
    if (err) {
      console.error('Error accessing PDF file:', err);
      return res.status(404).send('PDF not found');
    }
    res.sendFile(filePath);
  });
});
```

1. **Go on** the  [http://localhost:5000/](http://localhost:5000/) route you will be able to see all the uploaded pdf’s:

![Untitled](https://talktohire.blr1.digitaloceanspaces.com/Untitled_1.png)

1. Clicking on the **pdf file link** will open :

![Untitled](https://talktohire.blr1.digitaloceanspaces.com/Untitled_2.png)