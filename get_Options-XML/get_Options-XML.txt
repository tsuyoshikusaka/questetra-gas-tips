//get Options-XML by Google Sheet data
//
// 1. input data to Google Sheet
//
//    Sheet column A:id
//    Sheet column B:label
//
// 2. deploy Web app
//
// 3. set URL to Questetra
//
//    ref. http://www.questetra.com/tour/system-engineer/master-data/


var SHEET_ID = "xxxxx"; //input Google Sheet id
var SHEET_NAME = "Sheet1"; //input Sheet name

function doGet(e) {
  return  receive_(e);
}

function doPost(e) {
  return  receive_(e);
}

function receive_(e) {

  var search_key = e.parameter['query'];
  if (search_key == undefined) {
    search_key = "";
  }

  var out = ContentService.createTextOutput(buildXml(search_key));
  Logger.log(out.getContent());

  return out;
}

function buildXml(search_key) {
  var sheet = SpreadsheetApp.openById(SHEET_ID).getSheetByName(SHEET_NAME)

  var startRow = 2;
  var numRows = sheet.getLastRow() - 1;
    
  var dataRange = sheet.getRange(startRow, 1, numRows, 2);
  var data = dataRange.getValues();
  var out_string = '';

  out_string += '<items>\n';
  for ( var i = 0; i < data.length; ++i) {
    var row = data[i];
    
    var select_value = row[0];
    var select_display = row[1];

    if ((search_key == "") || (select_display.indexOf(search_key) >= 0)) {
      out_string += '<item value="' + select_value + '" display="' + select_display + '" />\n';
    }
  }
  out_string += '</items>';
  Logger.log(out_string);

  return out_string;
}
