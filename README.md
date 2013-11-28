webdriver-accessibility
=======================

webdriver-accessibility is a Java tool that helps you run accessibility audits directly using [selenium webdriver][1]. It relies on [GoogleChrome accessibility-developer-tools][2] to run the audit. 
Once the audit is run, the tool returns a meaningful Java object which can then later used for reporting. The tool also takes a screenshot of your
webpage and marks errors & warnings. Currently errors are bordered with red and warnings with yellow. 
This project is decoupled from Webdriver project in the sense that user would need to pass along
a WebDriver object. It also assumes user is already on a page which he/she intends to run accessibility audits using this tool
All you need to do in your project is

```
   AccessibilityScanner scanner = new AccessibilityScanner(driver);
   Map<String, Object> audit_report = scanner.runAccessibilityAudit();
```
You could do this in your tests,

```
if (audit_report.containsKey("error")) {
 List<Result> errors = (List<Result>) audit_report.get("error");
 assertThat("No accessibility errors expected", errors.size(),equalTo(0));
}
```
If you want specific details of the error, you could scan through the List of errors like below. Similarly you could scan all warnings for details.

```
List<Result> errors = (List<Result>) audit_report.get("error"); 
for (Result error : errors) {
 log.info(error.getRule());//e.g. AX_TEXT_01
 log.info(error.getUrl());//e.g. [GoogleChrome accessibility-developer-tools][3] audit rules URL
 for (String element : error.getElements()) //violated elements
  log.info(element);//e.g. #myForm > P > INPUT
}
```

Interpreting audit report
===========================
Once you run ```runAccessibilityAudit()``` method it returns a ```Map<String, Object> audit_report``` and it contains following keys,
```
  /** @type {List<Result>} */
  error, //contains all errors
  /** @type {List<Result>} */
  warning, //contains all warnings
  /** @type {String} */
  plain_report, //contains plain report
  /** @type {byte[]} */
  screenshot, //contains screenshot with bordered elements (red:errors, yellow:warnings)
```
```Result``` object is made of following,
```
 /** @type {String} */
  rule, //contains specific rule information
  /** @type {List<String>} */
  elements, //contains all element locators with errors/warnings
  /** @type {String} */
  information_link, //link to [GoogleChrome accessibility-developer-tools][3] audit rules wiki for more details
```
[1]: https://code.google.com/p/selenium/wiki/GettingStarted "selenium webdriver"
[2]: https://github.com/GoogleChrome/accessibility-developer-tools "GoogleChrome accessibility-developer-tools"
[3]: https://github.com/GoogleChrome/accessibility-developer-tools/wiki/Audit-Rules "GoogleChrome accessibility-developer-tools audit rules"

Reporting in your Tests
=======================
Please take a look at a sample cucumber test I wrote and demonstrated how to imbed details of the output of webdriver-accessibility tool in test reports.
A sample cucumber test report can be found below. Here you can notice that I embedded plain audit report and screenshot. In the screenshot, you can notice
that the warnings are bordered yellow. There were no errors found on this site at the time.
 ![test report](/src/test/resources/cucumber-report.png?raw=true)

Contributing: 
=======================
Fork the project and submit pull request if you like to add a feature/fix bugs etc.
Disclaimer: I am no accessibility expert. I am open for suggestions.

Contact me directly
=======================
Twitter: @nileshdk 

Email: nilesh.cric@gmail.com

