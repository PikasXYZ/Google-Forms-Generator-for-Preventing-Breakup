# Google-Forms-Generator-for-Preventing-Breakup
This Google Form Generator is designed to facilitate the rapid creation of Google Forms, which can save your friendship before traveling by allowing you to swiftly convert some fantastic questions in [this Google Sheet](https://docs.google.com/spreadsheets/d/1MgWS0RFdOLshZZiJTdjAvHDp7Xj8YXzqo2zoqaXyxt4/edit?usp=sharing) into a structured Google Form. The following are three fantastic questions in it:

> _Can you accept taking photos of the food before starting to eat?_
>
> _Does it need to be completely dark when sleeping?_
> 
> _Are you willing to take a taxi if the distance to the destination exceeds one kilometer?_

## Preparation
By clicking "File">>"Make a copy" in [this Google Sheet](https://docs.google.com/spreadsheets/d/1MgWS0RFdOLshZZiJTdjAvHDp7Xj8YXzqo2zoqaXyxt4/edit?usp=sharing), a  new one will be duplicated to your Google Drive, which means you have the permission to edit it.

 ㅤ
<p align="center">
  <img src="pic/copy.jpg" width=300>
  <img src="pic/copy2.jpg" width=300>
  <br>
  <em>▲ How to make a copy ▲</em>
  <br>
</p>

 ㅤ
Then you have to give me permission to run the main program by clicking the  `Generation Button` on the left bottom.

 ㅤ
<p align="center">
  <img src="pic/button.png" width=300>
  <br>
  <em>▲ Generation Button ▲</em>
  <br>
</p>


 ㅤ
<p align="center">
  <img src="pic/required.jpg" width=500>
  <br>
  <em>▲ Click "Continue" to give permission ▲</em>
  <br>
</p>

 ㅤ



 ㅤ
<p align="center">
  <img src="pic/click_advance_.jpg" width=500>
  <img src="pic/go_to_.jpg" width=500>
  <br>
  <em>▲ After choosing your account, click "Advance" and "Go to Pikas Saves Ur Friendship (unsafe)"  ▲</em>
  <br>
</p>

 ㅤ


 ㅤ
<p align="center">
  <img src="pic/allow.jpg" width=500>
  <br>
  <em>▲ Finally, click "Allow" to finish authorization ▲</em>
  <br>
</p>

 ㅤ
If you're worried about the safety of my code, click "Extensions" >> "Apps Script" and you'll see the following codes of Apps Script that perform the conversion:

```javascript
  // Get the current Google Sheets file
  var sheet = SpreadsheetApp.getActiveSpreadsheet().getActiveSheet();

  // Get values and font weights of all cells
  var data = sheet.getDataRange().getValues();
  var fontWeights = sheet.getDataRange().getFontWeights();
  var fontStyles = sheet.getDataRange().getFontStyles();

  // Get the name of the current Google Sheets and use it as the form name
  var formName = SpreadsheetApp.getActiveSpreadsheet().getName();
  var form = FormApp.create(formName);
  
  //
  var nameItem = form.addTextItem();
  nameItem.setTitle('What\'s your name?').setRequired(true);

  // Get options from B3
  var options = data.slice(2).map(function(row) {
    return row[1];
  }).filter(function(option) {
    return option; // Remove undefined or empty strings
  });

  // Loop from column C (index 2) to create titles and questions
  for (var i = 2; i < data[1].length; i++) {

    // Add a new page if A1 is not "MERGE" and set its title
    if (data[0][0].toUpperCase() !== "MERGE") {
      var page = form.addPageBreakItem();
      page.setTitle(data[1][i]);
    } else {
      var sectionHeader = form.addSectionHeaderItem();
      sectionHeader.setTitle(data[1][i]);
    }

    // Loop from the third row to create questions
    for (var j = 2; j < data.length; j++) {
      var questionTitle = data[j][i];
      if (questionTitle) {
        if (fontStyles[j][i] === "italic") {
          // if italic then addTextItem
          var item = form.addTextItem();
          item.setTitle(questionTitle);
        } else {
        // Add a multiple-choice item and set its title and options
          var item = form.addMultipleChoiceItem();
          item.setTitle(questionTitle)
              .setChoiceValues(options);
        }
        if (fontWeights[j][i] === "bold") {
          item.setRequired(true);
        }
      }
    }
  }

  // Display a dialog with the form URL
  var formUrl = form.getPublishedUrl();
  var htmlOutput = HtmlService.createHtmlOutput('You can access it <a href="' + formUrl + '" target="_blank">here</a>.')
                             .setWidth(300)
                             .setHeight(80);
  SpreadsheetApp.getUi().showModalDialog(htmlOutput, 'Form Created!');
}
```

## Usage
After preparation, you can try to click the `Generation Button` first to check the appearance of the default Google Form. And you'll find the rule below:

- From column C and so on, values in row 2 became titles, and those below became questions.
- Values below cell B2 (Options) became options in every multiple-choice question.
- Every title was separated into different pages. 
- Questions that were **bold** in the Google Sheet became required questions in the Google Form.
- Questions that were _italic_ in the Google Sheet became required questions in the Google Form.

Feel free to edit according to them :smiley:!

## FAQ
:question: What if I don't want every title to be separated into different pages?

:bulb: Every title will be merged as one page when the word "MERGE" is keyed into cell A1. 
 ㅤ
 
:question: Something different from the above.

:bulb: Send mail to pikasxyz@gmail.com and explain in detail.

## Reference
[にかいどう ゆき's Facebook post](https://www.facebook.com/lakuyuki/posts/pfbid0ikdESEiz3Gs8i1giEmfSjzrF9e9ZZS4UooSqtKBd2e27u1KRYTq5VTm4Gb8DbMGcl)
