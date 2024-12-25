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