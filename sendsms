// Spreadsheet column names mapped to 0-based index numbers.
var TIME_ENTERED = 0;
var NAME = 1;
var PHONE_NUMBER = 2;
var MESSAGE = 3;
var PAYMENT_DUE = 4;
var LAST_DATE = 5;
var PAYMENT_INFO = 6;
var PAYMENT_LINK = 7;
var STATUS = 8;


// Enter your Twilio account information here.
var TWILIO_ACCOUNT_SID = 'AC35747ac399b288723cbf7f52xxxxxx';
var TWILIO_SMS_NUMBER = '+12393xxxxx';
var TWILIO_AUTH_TOKEN = '169a0bf037c18ffc544xxxxx';

function onOpen() {
  // To learn about custom menus, please read:
  // https://developers.google.com/apps-script/guides/menus
  var ui = SpreadsheetApp.getUi();
  ui.createMenu('Send SMS')
      .addItem('Send to all', 'sendSmsToAll')
      .addItem('Send to Users(Having Dues Greater than 300$)', 'sendsmsto300')
      .addItem('Send to Users(Having Due Date Between 1-15th)', 'sendsmstoduedate')
      .addItem('Send to Users(Having Due Date Between 16-30th)', 'sendsmstoduedate30')
      .addToUi();
};  

function sendsmsto300() {
  var sheet = SpreadsheetApp.getActiveSheet();
  var rows = sheet.getDataRange().getValues();
  
  // The `shift` method removes the first row and saves it into `headers`.
  var headers = rows.shift();
  
  // Try to send an SMS to every row and save their status.
  rows.forEach(function(row) {
    if (row[PAYMENT_DUE] > 299) {

  row[STATUS] = sendSms(row[PHONE_NUMBER], row[MESSAGE], row[PAYMENT_DUE], row[PAYMENT_LINK], row[NAME],row[PAYMENT_INFO], row[LAST_DATE]);
 
}
     });
  
  // Write the entire data back into the sheet.
  sheet.getRange(2, 1, rows.length, headers.length).setValues(rows);
}

function sendSmsToAll() {
  var sheet = SpreadsheetApp.getActiveSheet();
  var rows = sheet.getDataRange().getValues();
  
  // The `shift` method removes the first row and saves it into `headers`.
  var headers = rows.shift();
  
  // Try to send an SMS to every row and save their status.
  rows.forEach(function(row) {
    row[STATUS] = sendSms(row[PHONE_NUMBER], row[MESSAGE], row[PAYMENT_DUE], row[PAYMENT_LINK], row[NAME],row[PAYMENT_INFO], row[LAST_DATE]);
  });
  
  // Write the entire data back into the sheet.
  sheet.getRange(2, 1, rows.length, headers.length).setValues(rows);
}


/**
 * Sends a message to a given phone number via SMS through Twilio.
 * To learn more about sending an SMS via Twilio and Sheets: 
 * https://www.twilio.com/blog/2016/02/send-sms-from-a-google-spreadsheet.html
 *
 * @param {number} phoneNumber - phone number to send SMS to.
 * @param {string} message - text to send via SMS.
 * @return {string} status of SMS sent (successful sent date or error encountered).
 */
function sendSms(phoneNumber, message,paymentdue, paymentlink, name, info, lastdate) {
  var twilioUrl = 'https://api.twilio.com/2010-04-01/Accounts/' + TWILIO_ACCOUNT_SID + '/Messages.json';

  try {
    UrlFetchApp.fetch(twilioUrl, {
      method: 'post',
      headers: {
        Authorization: 'Basic ' + Utilities.base64Encode("AC35747ac399b28872xxxxxxxxx:169a0bf037c18ffc5447xxxxxxxxx")
      },
      payload: {
        To: "+" + phoneNumber.toString(),
        Body: "Hello " + name + ", an amount of $" + paymentdue + " is outstanding" + " for " + info    +" It is due on " + lastdate +"."+ " Please Visit "+paymentlink + " to pay your dues. For any query, contact us at support@xenonstudio.in",
        From: TWILIO_SMS_NUMBER,
      },
    });
    return 'sent: ' + new Date();
  } catch (err) {
    return 'error: ' + err;
  }
}
