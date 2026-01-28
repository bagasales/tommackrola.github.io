# Google Sheets Email Capture Setup

## Step 1: Create a Google Sheet

1. Go to https://sheets.google.com
2. Create a new spreadsheet
3. Name it "Book Launch Email List"
4. In the first row, create these column headers:
   - A1: Email
   - B1: Timestamp
   - C1: Source

## Step 2: Create a Google Apps Script

1. In your Google Sheet, click **Extensions > Apps Script**
2. Delete any code in the editor
3. Paste this code:

```javascript
function doPost(e) {
  try {
    var sheet = SpreadsheetApp.getActiveSpreadsheet().getActiveSheet();
    var data = JSON.parse(e.postData.contents);
    
    sheet.appendRow([
      data.email,
      data.timestamp,
      data.source
    ]);
    
    return ContentService.createTextOutput(JSON.stringify({
      'status': 'success'
    })).setMimeType(ContentService.MimeType.JSON);
    
  } catch(error) {
    return ContentService.createTextOutput(JSON.stringify({
      'status': 'error',
      'message': error.toString()
    })).setMimeType(ContentService.MimeType.JSON);
  }
}
```

4. Click **Save** (disk icon)
5. Name your project "Book Launch Email Capture"

## Step 3: Deploy the Script

1. Click **Deploy > New deployment**
2. Click the gear icon next to "Select type"
3. Choose **Web app**
4. Configure:
   - Description: "Email capture for book launch"
   - Execute as: **Me**
   - Who has access: **Anyone**
5. Click **Deploy**
6. Click **Authorize access**
7. Choose your Google account
8. Click **Advanced** then **Go to [Project Name] (unsafe)**
9. Click **Allow**
10. **COPY THE WEB APP URL** - it looks like:
    `https://script.google.com/macros/s/XXXXX/exec`

## Step 4: Update the Landing Page

1. Open `when-its-time-to-sell-landing.html`
2. Find this line (around line 401):
   ```javascript
   const SHEET_URL = 'YOUR_GOOGLE_SHEETS_WEB_APP_URL_HERE';
   ```
3. Replace `YOUR_GOOGLE_SHEETS_WEB_APP_URL_HERE` with your actual Web App URL
4. Save the file

## Step 5: Upload to tommackrola.com

Upload these files to your web host:
- `when-its-time-to-sell-landing.html` (rename to `index.html` or keep as is)
- `when_its_time_to_sell_cover.jpg`

## Testing

1. After uploading, visit your page
2. Enter a test email
3. Check your Google Sheet - the email should appear

## Notes

- Emails will be captured in real-time
- You can export the sheet to CSV anytime
- You can share the sheet with Tom or others
- Set up notifications in Google Sheets to get alerted on new entries:
  - Tools > Notification rules > "A user submits a form"
