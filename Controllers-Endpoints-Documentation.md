End Points Documentation

/**************** AccountController.js ****************/

a. status
Returns : Json object

/**************** BrowserDataController.js ***********/


/**************** FacebookLoginController.js *********/


/**************** MessageWorkerController.js *********/

a. read
Returns : Json object

b. relatedSnippets
Returns : Json object

c. relatedMessages
Returns : Json object

d. search
Returns : Json object

/*************** ProxyController.js ***************/

a. get
Returns : send - String

b. log
Returns : send - Integer - 200, 429

/************** UserController.js ****************/

a. account
Renders

/************** AccountsController.js ***********/

a. add
Renders

b. remove
Returns : send - String

c. refresh
Returns : send - String

d. tags 
Returns : send - String

e. scanProgress
Returns : JSON object

/************** ContentController.js ***********/

a. set
Returns  ; send - Integer - 401, 500, 200

b. remove
Returns : send - Integer - 401, 500, 200

Returns : JSON object

/*********** GmailLoginController.js **********/


/*********** MetaController.js ****************/


/*********** ScannerController.js ************/ 


/*********** AdminController.js **************/

a. unauthorized
Renders

b. bios
Renders or Redirects ('/admin/unauthorized')

c. content
Renders or redirect ('/admin/unauthorized')

d. progress
redirects ('/') or send string or '{}'

e. dbmetrics
redirects ('/') or send string or '{}'

f. scan 
redirects ('/', '/admin/unauthorized') or send string or '{}'

g. stats
redirects ('/', '/admin/unauthorized') or renders

h. users
redirects ('/', '/admin/unauthorized') or renders

i. email
redirects ('/', '/admin/unauthorized') or renders

j. blacklist
redirects ('/', '/admin/unauthorized') or send string


/*********** DataWorkerController.js *********/


/*********** HomeController.js **************/


/*********** OpenIdLoginController.js *******/


/********** SearchWebappController.js *******/

a. read
Renders

/********** BioController.js ****************/

a. set
Returns - send integer - 401 500, 200

b. remove
Returns - send integer - 401 500, 200

c. getAll
Returns : send integer 500 or JSON object

/********** EmailController.js *************/

a. contact
Returns : send string or boolean

b. confirm
Renders

c. reconfirm
Redirects to '/email/reconfirmed'

d. reconfirmed
Renders

e. optout
Renders

f. confirmedOptOut
Renders

g. optin
Renders

/************* MessageController.js *********/

a. read
returns JSON object

b. relatedSnippets
returns JSON object

c. relatedMessages
returns JSON object

d. search
returns JSON object

/************* PaymentsController.js ********/

a. gopro
Redirects

b. downgrade
Redirects or Renders

c. downgradeconfirmed
Redirects or Renders

d. coinbase
Returns JSON or send

e. coinbasesuccess
Redirects

/************ SystemHealthController.js *********/

a. read
returns JSON