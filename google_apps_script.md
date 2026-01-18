Google Apps Script for RiderCam Form Submission
================================================

Setup Instructions:

1. Create a new Google Sheet at https://sheets.google.com
2. Label the first row headers in this order:
   A1: Timestamp
   B1: Email
   C1: Updates
   D1: Vehicle
   E1: Vehicle Other
   F1: Complaint
   G1: Beta

3. In the Google Sheet:
   - Click "Extensions" > "Apps Script"
   - Delete any existing code
   - Copy and paste the script below
   - Click "Save" (give it a name like "Form Handler")

4. Deploy the script:
   - Click "Deploy" > "New deployment"
   - Select "Web app" as the deployment type
   - Description: "RiderCam Waitlist Form"
   - Execute as: "Me"
   - Who has access: "Anyone"
   - Click "Deploy"
   - Copy the Web App URL (it will look like: https://script.google.com/macros/s/.../exec)

5. Update index.html:
   - Find the line: const GOOGLE_SCRIPT_URL = 'YOUR_GOOGLE_SCRIPT_URL_HERE';
   - Replace 'YOUR_GOOGLE_SCRIPT_URL_HERE' with the URL you copied in step 4

6. Test the form on your webpage!

---

COPY THE CODE BELOW INTO GOOGLE APPS SCRIPT:

function doPost(e) {
  try {
    var sheet = SpreadsheetApp.getActiveSpreadsheet().getActiveSheet();
    
    // Parse the incoming data
    var data = JSON.parse(e.postData.contents);
    
    // Add new row with form data
    sheet.appendRow([
      new Date(),  // Timestamp
      data.email,
      data.updates,
      data.vehicle,
      data.vehicleOther,
      data.complaint,
      data.beta
    ]);
    
    return ContentService.createTextOutput(JSON.stringify({"result":"success"}))
      .setMimeType(ContentService.MimeType.JSON);
      
  } catch (error) {
    return ContentService.createTextOutput(JSON.stringify({"result":"error", "error": error.toString()}))
      .setMimeType(ContentService.MimeType.JSON);
  }
}

function doGet(e) {
  return ContentService.createTextOutput("RiderCam Form Handler - POST only")
    .setMimeType(ContentService.MimeType.TEXT);
}
