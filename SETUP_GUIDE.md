# Google Sheets Integration – Setup Guide

## Step 1: Create a Google Sheet

1. Go to [sheets.google.com](https://sheets.google.com) and create a new spreadsheet.
2. Name it **"Survey Responses"** (or anything you like).
3. In **Row 1**, add these headers (exactly):

| A | B | C | D | E | F | G | H | I | J | K | L | M | N | O | P | Q | R | S | T | U | V | W | X | Y | Z | AA | AB | AC | AD | AE |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| timestamp | name | email | phone | finalScore | totalRaw | persona | isUpdate | Q1_answer | Q1_pts | Q2_answer | Q2_pts | Q3_answer | Q3_pts | Q4_answer | Q4_pts | Q5_answer | Q5_pts | Q6_answer | Q6_pts | Q7_answer | Q7_pts | Q8_answer | Q8_pts | Q9_answer | Q9_pts | Q10_answer | Q10_pts | Q11_answer | Q11_pts | Q12_answer | Q12_pts |

## Step 2: Create the Apps Script

1. In your Google Sheet, go to **Extensions → Apps Script**.
2. Delete any code in the editor and paste the following:

```javascript
function doPost(e) {
  var sheet = SpreadsheetApp.getActiveSpreadsheet().getActiveSheet();
  var data = JSON.parse(e.postData.contents);

  sheet.appendRow([
    data.timestamp,
    data.name || '',
    data.email || '',
    data.phone || '',
    data.finalScore,
    data.totalRaw,
    data.persona,
    data.isUpdate ? 'UPDATE' : 'NEW',
    data.Q1_answer, data.Q1_pts,
    data.Q2_answer, data.Q2_pts,
    data.Q3_answer, data.Q3_pts,
    data.Q4_answer, data.Q4_pts,
    data.Q5_answer, data.Q5_pts,
    data.Q6_answer, data.Q6_pts,
    data.Q7_answer, data.Q7_pts,
    data.Q8_answer, data.Q8_pts,
    data.Q9_answer, data.Q9_pts,
    data.Q10_answer, data.Q10_pts,
    data.Q11_answer, data.Q11_pts,
    data.Q12_answer, data.Q12_pts
  ]);

  return ContentService
    .createTextOutput(JSON.stringify({ status: 'ok' }))
    .setMimeType(ContentService.MimeType.JSON);
}
```

3. Click **Save** (Ctrl+S), name the project (e.g., "Survey Backend").

## Step 3: Deploy as Web App

1. Click **Deploy → New deployment**.
2. Click the gear icon ⚙ next to "Select type" and choose **Web app**.
3. Set:
   - **Description**: Survey endpoint
   - **Execute as**: Me
   - **Who has access**: **Anyone**
4. Click **Deploy**.
5. Authorize the app when prompted (click through the "unsafe" warning—it's your own script).
6. **Copy the Web app URL** — it looks like:
   ```
   https://script.google.com/macros/s/AKfycb.../exec
   ```

## Step 4: Paste the URL in index.html

Open `index.html` and find this line:

```javascript
const GOOGLE_SCRIPT_URL = 'YOUR_GOOGLE_APPS_SCRIPT_URL_HERE';
```

Replace `YOUR_GOOGLE_APPS_SCRIPT_URL_HERE` with the URL you copied:

```javascript
const GOOGLE_SCRIPT_URL = 'https://script.google.com/macros/s/AKfycb.../exec';
```

## Step 5: Deploy the Survey

### Option A: GitHub Pages (free)
1. Create a GitHub repo and push `index.html`.
2. Go to **Settings → Pages → Source: main branch**.
3. Your survey will be live at `https://yourusername.github.io/repo-name/`.

### Option B: Netlify (free, drag & drop)
1. Go to [app.netlify.com](https://app.netlify.com).
2. Drag the folder containing `index.html` onto the page.
3. Done — you get a live URL instantly.

### Option C: Vercel (free)
1. Go to [vercel.com](https://vercel.com), import your repo or drag & drop.

## Viewing Responses

Open your **Google Sheet** — every survey submission will appear as a new row with:
- Timestamp
- Final score and persona
- Each question's selected answer and points

That's it! The survey sends data silently in the background after the user submits.
