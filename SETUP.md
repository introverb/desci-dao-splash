# DeSci DAO splash page — sign-up setup

Sign-ups go into a Google Sheet on your own Google account. This takes about 2 minutes.

## 1. Make the Sheet
1. Create a new Google Sheet named **DeSci DAO sign-ups**.
2. In row 1, type: `timestamp` | `email` | `source`

## 2. Add the script
1. In the Sheet: **Extensions → Apps Script**.
2. Delete what's there, paste the code below, and click **Save**.

```javascript
function doPost(e) {
  var p = e.parameter || {};
  var email = String(p.email || "").trim();
  if (p.website || !/^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(email)) {
    return ContentService.createTextOutput("rejected");
  }
  var sheet = SpreadsheetApp.getActiveSpreadsheet().getSheets()[0];
  sheet.appendRow([new Date(), email, String(p.src || "web")]);
  // Optional: get an email for each sign-up. Uncomment the next line.
  // MailApp.sendEmail(Session.getActiveUser().getEmail(), "New DeSci DAO sign-up", email);
  return ContentService.createTextOutput("ok");
}
```

## 3. Deploy it
1. Click **Deploy → New deployment** → gear icon → **Web app**.
2. Execute as: **Me**. Who has access: **Anyone**.
3. Click **Deploy**, approve the permissions, and copy the **Web app URL**.
4. Send that URL to Claude, which will paste it into `index.html` (the `ENDPOINT` line).

## Tracking card sign-ups
The QR code on the cards links to the page with `?src=card`, so sign-ups from the cards show up as `card` in the source column.
