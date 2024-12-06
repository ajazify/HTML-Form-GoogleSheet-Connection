# HTML Form to Google Sheet Connection
To send data from an HTML page to an existing Google Sheet, you can use Google Apps Script to create a web app that acts as a backend endpoint. The process involves the following steps:

## 1. Prepare the Google Sheet

- Open your existing Google Sheet.
- Ensure the first row contains the column headers:
  - 1a - Name
  - 1b - Task
---
## 2. Create a Google Apps Script Web App

1) Go to Extensions > Apps Script in the Google Sheet.
2) Delete the default code and paste the following script:
```
function doPost(e) {
  // Parse the incoming data
  var params = JSON.parse(e.postData.contents);

  // Open the active spreadsheet
  var sheet = SpreadsheetApp.getActiveSpreadsheet().getActiveSheet();

  // Append the new data to the sheet
  sheet.appendRow([params.name, params.task]);

  return ContentService.createTextOutput(
    JSON.stringify({ status: "success", message: "Data added to Google Sheet" })
  ).setMimeType(ContentService.MimeType.JSON);
}
```

3)  Save the project with a name like `SubmitToGoogleSheet`.
4) Click **Deploy > New deployment**.
	- Select **Web app**.
	-   Choose an appropriate description.
	-   Under **Execute as**, select **Me**.
	-   Under **Who has access**, choose **Anyone** (if you want it publicly accessible).
5)  Click **Deploy** and authorize the script.
6) Copy the generated **Web App URL**.
---
## 3. Create the HTML Form
Here’s an example of a simple HTML form to collect and send data:
```
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Send Data to Google Sheet</title>
</head>
<body>
  <form id="dataForm">
    <label for="name">Name:</label>
    <input type="text" id="name" name="name" required><br><br>

    <label for="task">Task:</label>
    <input type="text" id="task" name="task" required><br><br>

    <button type="submit">Submit</button>
  </form>

  <script>
    document.getElementById('dataForm').addEventListener('submit', async function (e) {
      e.preventDefault();

      const name = document.getElementById('name').value;
      const task = document.getElementById('task').value;

      const response = await fetch('YOUR_WEB_APP_URL', {
        method: 'POST',
        headers: {
          'Content-Type': 'application/json',
        },
        body: JSON.stringify({ name, task }),
      });

      const result = await response.json();
      alert(result.message);
    });
  </script>
</body>
</html>
```
---
##  4. Replace `YOUR_WEB_APP_URL`
- Replace `YOUR_WEB_APP_URL` in the HTML file with the Web App URL you copied earlier.
---
##  5. Host the HTML File
- You can test this HTML file locally or host it online using platforms like GitHub Pages or Netlify. If you test locally, open the file in a browser and ensure it sends data to the Google Sheet.

---

##  6. Host the HTML File

-   Open the HTML form in your browser.
-   Fill in the fields and submit the form.
-   Check your Google Sheet to confirm the data has been added.

---

### **Tips**

-   **Validation**: Add client-side or server-side validation to ensure valid data is sent to the sheet.
-   **Authentication**: Restrict access to the Google Apps Script Web App to authenticated users if necessary.
-   **Error Handling**: Enhance the script to handle errors like invalid data or failed requests.

Let me know if you need further clarification or enhancements!
