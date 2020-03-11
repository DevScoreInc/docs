# DevScore Documentation

DevScore is an Automatio PAAS for developers. DevScore makes it super easy to automate SDLC processes, Service integrations, Webhook receiever and broadcaster, workflow automation, CI/CD automation and many more use cases.


DevScore provides Javascript runtime for business logic, database for storing your data and state management, POST and GET webhooks, and cronjob. 



Inside your function you have access `_context` object which has following methods.
```
'_context' : it is an object with following attributes 
    'dbLib.createVariable': Async Function to create variable in the database 
    'dbLib.deleteVariable': Async Function to delete variable in the database 
    'dbLib.deleteTableOrDailyCounterVariable' : Async Function to delete a row from a table ('table' or 'daily-counter') variable types
    'dbLib.getDocument' : Async Function to retrieve document from database
    'dbLib.getEncryptedVariable': Async Function to get and decrypt encrypted variables
    'dbLib.getTableRowsForTimePeriod': Async Function to get table rows for 'table' variable type for time span
    'dbLib.getDailyCounterVariableForTimePeriod': Async Function get rows for 'daily-counter' variable type
    'dbLib.getTableRowsWithFilterAndSort': Async Function to get table rows for 'table' variable type with filter and sort
    'authLib.refreshGoogleAccessToken(refresh_token)': Async Function to refresh your Google access_token. You need to pass refresh_token to this function and it returns the access_token
    'gitLib.getStatsReport(reportType, fromDate, toDate)': Async Function to get your Git Statistics which you can setup through onboarding. reportType is one of ["all", "issues", "collab-scores", "collabs", "commits", "pulls", "releases"]
    'emailLib.sendMail(messageData)': Async Function to send email via SendGrid. 
    'emailLib.compileHandleBarTemplate(template_string, data)': Function to compile Handlebar templates to create dynamic emails.
    'fileLib.storeAsset(asset_name, contentType, data)': Async Function to store Data(string) to the cloud base file storage
    'fileLib.getListOfAssets()': Async function to get list of your assets stored in the storage
    'fileLib.downloadAsset(asset_name)': Async function to get content of your asset from the sotrage
    'logger(logLevel, message)': Async Function to log your messages for debugging purposes. logLevel is one of ['error', 'debug', 'info']. message is either string or plain JSON
    'requestAgent': Module to create HTTP request. please visit https://github.com/request/request#readme for the documentation
    'momentLib': Module to work with dates. It is a wrapper over popular Moment module. Learn more at https://momentjs.com/docs/
    'parameters': Parameters that is passed to your script
    'eventContext': Event (JSON) object that is passed by the event which triggers this function
    'eventContext.devscore_function_exec_result': Object (JSON) which will be present if this function runs as a result of previous function execution - the value of this attribute is the return value of previous function

```

Available Functions on '_context.dbLib' object: 

```
async function createVariable(name, type, value, documentId) {
    // name: variable name
    // type: variable type which can be one of these ['string', 'encrypted-string', 'number', 'counter', 'table', 'daily-counter', 'bool', 'map', 'array']
    // value: variable value
    // (optional) documentId: ID (name) of document which you want to store this 
    // Output: 'success'
}

async function deleteVariable(name, type, userDocumentId) {
    // name: variable name
    // type: variable type which can be one of these ['string', 'encrypted-string', 'number', 'counter', 'bool', 'map', 'array']
    // (optional) documentId: ID (name) of document which your variable is stored at  
    // Output: 'success'
}

async function deleteTableOrDailyCounterVariable(name, type, doc_id, userDocumentId) {
    // name: variable name
    // type: variable type which can be one of these ['table', 'daily-counter']
    // doc_id: it is the row ID in the table which you want to delete it. You can get doc_id when you call getTableRowsForTimePeriod or getDailyCounterVariableForTimePeriod
    // (optional) documentId: ID (name) of document which your variable is stored at
    // Output: 'success'
}

async function getDocument(documentId) {
    // (optional) documentId: ID (name) of document which you want to retrieve
    // Output: {'data': {val...}, 'status': 'success'}
}

async function getEncryptedVariable(variable_name, documentId) {
    // variable_name: variable name
    // (optional) documentId: ID (name) of document which your variable is in
    // Output: {'data': {'decrypted_value': val}, 'status': 'success'} 
}

async function getTableRowsForTimePeriod(variable_name, fromDate, toDate, documentId) {
    // variable_name: variable name
    // fromDate: start date (it can be any acceptable javascript Date format (string, UTC timestamp number, or Date object))
    // toDate: end date (it can be any acceptable javascript Date format (string, UTC timestamp number, or Date object))
    // (optional) documentId: ID (name) of document which your variable is in
    // Output: {'data': [data], 'status': 'success'}
}

async function getDailyCounterVariableForTimePeriod(variable_name, fromDate, toDate, documentId) {
    // variable_name: variable name
    // fromDate: start date (it can be any acceptable javascript Date format (string, UTC timestamp number, or Date object))
    // toDate: end date (it can be any acceptable javascript Date format (string, UTC timestamp number, or Date object))
    // (optional) documentId: ID (name) of document which your variable is in
    // Output: {'data': [data], 'status': 'success'}
}

async function getTableRowsWithFilterAndSort(variable_name, filter_variable_name, filter_operator, filter_value, sort, limit, documentId) {
    // variable_name: variable name
    // filter_variable_name: variable name that you want to create filter for
    // filter_operator: it can be one of following ['>','<', '<=', '>=', '==', 'array-contains', 'in', 'array-contains-any']
    // filter_value: filter value
    // sort: Boolean (true, or false) to sort the result based on variable_name
    // limit: Number of rows to return (it cannot be greater than 1000)
    // fromDate: start date (it can be any acceptable javascript Date format (string, UTC timestamp number, or Date object))
    // toDate: end date (it can be any acceptable javascript Date format (string, UTC timestamp number, or Date object))
    // (optional) documentId: ID (name) of document which your variable is in
    // Output: {'data': [data], 'status': 'success'}
}
```

Available Functions on '_context' object:
```    
async function logger(logLevel, message) {
    // logLevel is a string indicating log level such as 'error', 'debug', 'info'
    // message is plain JSON with your logging data
}
```
Available Functions on '_context.emailLib' object: 
```    
async function sendMail(messageData) {
    messageData = { 
        'api_key': 'SENDGRID_API_KEY', 
        'to': 'TO_EMAIL_ADDRESS',
        'cc': 'CC_EMAIL_ADDRESS',
        'bcc': 'BCC_EMAIL_ADDRESS',
        'reply_to': 'REPLY_TO_EMAIL_ADDRESS',
        'from': 'FROM_EMAIL_ADDRESS',
        'subject': 'EMAIL_SUBJECT',
        'html': 'EMAIL_BODY_IN_HTML_FORMAT',
        'text': 'EMAIL_BODY_IN_TEXT_FORMAT',
        'isMultiple': 'FLAG_TO_SEND_MULTIPLE_INDIVIDUAL_EMAILS_VS_ONE_EMAIL_WITH_MULT_RECIPIENTS'
    };
   // Output: messageId in string type
}

function compileHandleBarTemplate(template_string, data) {
    // template_string is a your Handlbar template in String format
    // date is JSON object that will be passed to Handlbar to compile email template
    // Output: compiled template in string type
}
```
Available Functions on '_context.fileLib' object: 
```    
async function storeAsset(asset_name, contentType, data) {
    // asset_name: name your file
    // contentType: type of your content - use HTTP content-type format https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Content-Type
    // data: your string data
    // Output: {'status': 'success', 'name': name};
}

async function getListOfAssets() {
    // Output: [ { asset_name: 'my_template.txt' } ]
}

async function downloadAsset(asset_name) {
    // asset_name: name your file
    // Output: the content of stored file in string format
}

```
