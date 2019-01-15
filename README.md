# Jira Sheet Tools

[Jira](https://www.atlassian.com/software/jira) is a powerful and well established project management tool among small to enterprise businesses. Still we often end up using Google Sheets for some overview roadmaps, project dashboards and other purposes.

With this Google Sheet Add-on the, called "[Jira Sheet Tools](https://chrome.google.com/webstore/detail/jira-sheet-tools/ncijnapilmmnebhbdanhkbbofofcniao)" available in the Google Add-On store from within Google Sheet, you can now take your sheet based reports with Jira information to the next level.

[Jira Sheet Tools](https://chrome.google.com/webstore/detail/jira-sheet-tools/ncijnapilmmnebhbdanhkbbofofcniao) allows you to visualize the status of any Jira ticket you mention in a sheet.
You can directly import entire issue lists with your Jira filters just from within Google sheet.
Or create time reports for any of your users based on the Jira worklogs.

Enter your Jira server domain and user details once, and be able to use these Jira features in any sheet at any time.
No manual status update copy&paste anymore.

> Tested with latest Jira Cloud (OnDemand) - compatible with latest Jira Server. Please provide feedback if you expirience any issue.

# Table of Content
[Install / Get started](#install--get-started)

[Features](#features)

[Custom Functions](#custom-functions)

[Known Limitations](#known-limitations)

[Known Issues](#known-issues)

[Development](#development)

# Install / Get started
* Open up your chrome browser
* Open or create a Google Sheet
* Find & Install the Add-on "[Jira Sheet Tools](https://chrome.google.com/webstore/detail/jira-sheet-tools/ncijnapilmmnebhbdanhkbbofofcniao)"
* Authorize the Add-On when asked for

### Authorizations
Explanation of the different privileges, this add-on will ask your permission for when installing.

`View and manage spreadsheets that this application has been installed in`
Required to change and add Jira issue information within the active sheet.

`Display and run third-party web content in prompts and sidebars inside Google applications`
Implied when using CSS style sheets from google (gstatic.com) within any dialog window and when executing Jira API requests.

`Allow this application to run when you are not present`
Fault. This permission is actually not used at all, but triggered by yet unknown word within the code. Will hopefully be not necessary in a sonner release.
In fact, this add-on does not execute anything while the sheet is closed or not implicitly requested by the user.

`Connect to an external service`
Required for establishing connection to the Jira RESTful API to fetch your Jira issue details when requested.

`Publish this application as a web app or a service that may share your data`
Implied when publishing this add-on as a google sheet add-on.
NO data is shared with any third-party other then your google sheet and the used Jira instance.


## Setup Connection to JIRA
Once installed:
Simply provide your individual Jira server settings before you use any feature.

In any Google sheet, go in the menu to “Add-ons" > "Jira Sheet Tools" > "Settings”.
Enter your "Jira Domain" and your log on credentials.
Either combination of your Atlassian account username/email + password or your Atlassian account email + API Token.
> Use API Token for improved security. [How to obtain API Token](https://confluence.atlassian.com/cloud/api-tokens-938839638.html)

> It is recommended to use this Add-on only with an Jira Cloud/Server instance which runs via SSL (https).
> This Add-on is using simple Basic Auth mechanism to authenticate with Jira, which means, user credentials are transmitted unencrypted when used without SSL.

**You're all set and ready to go**

# Features
### Update Ticket Key Status
“Add-ons" > “Jira Sheet Tools” > "Update Ticket Key Status "KEY-123 [Done]""

Any Jira ticket Id in the form of "KEY-123" will be updated on the current active google sheet and extended with the current status of matching Jira ticket.

Sample Data:
```markdown
| Before  | After
| KEY-123 | KEY-123 [Done]
| KEY-456 | KEY-456 [In Progress]
| KEY-789 | KEY-789 [Closed]
```
Even when used within text it will search for keys and add the status.
If a Jira issue key is found in a single cell, the value will be linked automatically to the Jira issue page.


### Re-Calculate all formulas in active sheet
“Add-ons" > “Jira Sheet Tools” > "Re-Calculate all formulas in active sheet"

When anu custom function or other formula is used, this simple 'click' will refresh / re-calculate all the formulas and custom functions used in the current active google sheet.
If a sheet is re-opened this will re-calculate all custom functions by default anyway, but usually not while editing or watching the current sheet.


### Show Jira Field Map
“Add-ons" > “Jira Sheet Tools” > "Show Jira Field Map"

Fetch and show all your Jira fields name and id in a sidebar. Very useful for our custom functions where you can make use of JQL queries.


### List Issues From Filter
“Add-ons" > “Jira Sheet Tools” > "List Issues from Filter"

Allows you to add a table/list of all found Jira issues based on your favorite Jira Filter.
The dialog will let you choose from all your Jira filters and then insert all results into the active Google sheet.
You can even decide which information to be shown in the resulting table.
Most common Jira fields / columns are available to select from.
Additionally you can configure many different types of custom Jira field, which then will be available for you in this dialog. (“Add-ons" > “Jira Sheet Tools” > "Configure Custom Fields")

> Note: This feature is currently limited to list a maximum of 1000 jira issues.
> It may even break earlier when the requests takes longer then Google's maximum execution timeout.
> Depending on Jira response time i had successfully listed 1000 issues but sometimes only about >850.

### Time Report
“Add-ons" > “Jira Sheet Tools” > "Create Time Report"

Lets you pick a user from Jira and a date period to filter for and generates a nice Time sheet report based on all worklogs for the filtered user and date period.
Supports two different time report formats; "1d 7h 59m" for better readibility or "7.5" (work hours as decimal number) for better calculations in the sheet.
Under “Settings” you can configure which time format you prefer to use.

> Careful when selecting to big date periods, can be slow and become a wide table. Start with 1 week and scale up.

### Configure Custom Fields
“Add-ons" > “Jira Sheet Tools” > "Configure Custom Fields"

If you wish to list issues in your sheet with the function "List Issues From Filter" you can specify which columns to insert.
By default only most common Jira default Fields (Columns) are available to choose from.
In case you use custom Jira fields you can now go to the settings section and select some of these customs fields as your favorites.
> Note: Not all custom field formats are supported, these are indicated in the list of fields.

Once you configured your custom fields, these fields are available to create column of in the "List Issues From Filter" dialog.

> Supported custom fields are of type: **string**, **number**, **date**, **datetime**
**option**, **array of options**, **array of strings**, **user**, **array of users**, **group**, **array of groups**, **version** and **array of versions**


# Custom Functions
Custom functions in Google sheet's are created using standard JavaScript.
(see https://developers.google.com/apps-script/guides/sheets/functions#using_a_custom_function)

### JST_EPICLABEL

Sample: `JST_EPICLABEL("JST-123")`

Description: `Fetch EPIC label from Jira instance for a given Jira Issue Key of type EPIC.`

TicketId: `A well-formed Jira EPIC Ticket Id / Key.`

Use this custom function whenever you like to automatically retrieve the Jira issue label for a given EPIC ticket Id / Key.


### JST_getTotalForSearchResult
Sample: `JST_getTotalForSearchResult("status = Done")`

Description: `Fetch the total count of results for given Jira JQL search query.`

JQL: `A well-formed Jira JQL query.`
(see [https://confluence.atlassian.com/jirasoftwarecloud/...](https://confluence.atlassian.com/jirasoftwarecloud/advanced-searching-764478330.html#Advancedsearching-ConstructingJQLqueries))

Use this custom function whenever you simply need the total count of Jira issues resulting from your JQL ([Jira Query Language](https://confluence.atlassian.com/jirasoftwarecloud/advanced-searching-764478330.html#Advancedsearching-ConstructingJQLqueries)) queries.


### JST_search
Sample: `JST_search("status = Done"; "summary,status")`

Description: `(Mini)Search for Jira issues using JQL.`

JQL: `A well-formed Jira JQL query.` _(*required)_
(see [https://confluence.atlassian.com/jirasoftwarecloud/...](https://confluence.atlassian.com/jirasoftwarecloud/advanced-searching-764478330.html#Advancedsearching-ConstructingJQLqueries))

Fields: `Jira issue field IDs. e.g.: "key,summary,status"` _(*required)_

Limit: `Number of results to return. 1 to 100. Default: 1` _(*optional)_

StartAt: `The index of the first result to return (0-based)` _(*optional)_

Little but quite powerful function to search for Jira issues and fill your sheet with the results.
Using JQL ([Jira Query Language](https://confluence.atlassian.com/jirasoftwarecloud/advanced-searching-764478330.html#Advancedsearching-ConstructingJQLqueries)) queries as you would inside Jira.
Can return just a single cell value or entire list of issues spanning over multiple columns.
Expecting a valid JQL query as 1st parameter and a comma-separated list of Jira field IDs as the 2nd.
If your dont know the exact names and syntax of Jira fields, then look at the Field Map (“Add-ons" > “Jira Sheet Tools” > "Show Jira Field Map").

**Limitation**: This custom function can return a maximum of **100** results/issues. Search and processing is limited to **30 seconds** per call (Google Limitation), if the Jira Server responds slow, it might not be able to provide full result to you.

> **Tip:** When using more than one field as the second function parameter, the result will use 2 columns in your sheet, starting from the cell you enter the function.
When you define a `Limit` greater than `1`, the results will fill multiple rows below starting from the cell you enter the function.
Give it a try, with a very basic JQL: `JST_search("status = Done"; "key,summary,status"; 5)`
This will search for any Jira issue with `status` equals `Done` and fill your cells with max 5 rows over 3 columns (3 fields = 3 columns).

**Sample Result:**
In cell `A1` put in `JST_search("status = Done"; "key,summary,status"; 5)`
```markdown
1 | A      | B                       | C
2 | KEY-11 | Summary of first issue  | Done
3 | KEY-12 | Summary of second issue | Pending
4 | KEY-13 | Summary of third issue  | Closed
5 | KEY-14 | Summary of fourth issue | Done
6 | KEY-15 | Summary of fifth issue  | ToDo
```


### JST_formatDuration

Sample: `JST_formatDuration(60*60)`

Description: `Format time difference in seconds into nice duration format.`

Seconds: `Duration in seconds`

Use this custom function whenever you like to format a duration time in seconds into JIRA common work duration format.

**Sample Result:**
In cell `A1` put in `JST_formatDuration(60*60)`
```markdown
1 | A
2 | 1h
```
357878 = `12d 3h 24m 38s`


# Known Limitations
With the features of this Add-On come a few hard limits implemented purposly.
Specifically related to the amount of records you can fetch from your Jira API due to Atlassians REST API policy and Google's execution timeouts.
It is described here on [Atlassian.com](https://confluence.atlassian.com/jirakb/changing-maxresults-parameter-for-jira-rest-api-779160706.html) that the limit of records per call can be changed without notice.
Therefore i do use already pagination where ever possible to fetch as many data as possible.

Current existing limitations by this Add-On:
* "List Issues from Filter" is limited to a total amount of **10.000** issues to be listed per request
  * To comply with Atlassians policy, it does internally fetch only *50* records per page which can result in quite some delay when dealing with too many issues.
* Listing of Jira users and groups (within dialogs) is limited to **100** user/group records
* "Time Report" is limited to report max **1.000** worklogs per Jira issue (max **1.000** issues) per Time sheet
* All data processing however is bound to run within Google's maximum execution time of **5 minutes**.


# Known Issues
`Could not connect to Jira Server![401]`
**1st: Make sure you use your Atlassian username and password, not an email or possibly Google password!**
In case someone comes across the same or similar issue, i could actually reproduce that error and identify one use case where this would happen.

### Solution
This applies to **JIRA Cloud** using **G-Suite synced** account. It might not apply to self hosted Jira instances.

Note that if you are logging in via a synced Google account, it is **NOT** the google password you are supposed to use. Instead you should go to your user profile and look up your username and set a password.

For site admin functions, RSS feeds, REST API access, or WebDAV uploads you'll need to have an Atlassian Cloud password (separate to your Google Apps password.) Which applies to this Add-On as well.

#### Instructions
Log out from your Jira portal.
Go to https://id.atlassian.com and click on "Can't log in?" - just below the log on form.
On the next page enter your email address (which would be your Google Email) and press "Send recovery link".

You will get an email from Atlassian where you please click the provided link at "Reset your password".
Now on the Atlassian page where you can set/change your Atlassian (and not Google) password, enter a new password for your Atlassian account, not to mix up with your Google account.

Of course it makes no sense that this information is not available on the REST API documentation page, since it is quite crucial to get it working.


# Development

## Pre-requisites
To enable Google-Apps-Script (GAS) build, deployment and running unit test on local environment you will need `node`, `gulp` and `clasp`.

* Install latest version of Node.js - https://nodejs.org/en/download/
* Gulp - https://gulpjs.com/
* Clasp - https://codelabs.developers.google.com/codelabs/clasp

Installing `node` will differ from your environment, see install instructions on https://nodejs.org/ .

Assuming `node` is already installed, first install `gulp`:

```sh
sudo npm install gulp-cli -g
sudo npm install gulp -D
```

Now you can install `clasp`:
```sh
sudo npm i @google/clasp -g
```

Then enable Apps Script API: https://script.google.com/home/usersettings

(If that fails, run this:)
```sh
sudo npm i -g grpc @google/clasp --unsafe-perm
```

## Checkout and Setup
Clone the code from Github onto your local machine

```sh
git clone https://github.com/ljay79/jira-tools.git jira-tools
cd jira-tools
```

Then install dependencies

```sh
npm install
```

> *Note:* It will most likely throw a warning about `gulp-util` which you can safly ignore for now.
> `npm WARN deprecated gulp-util@3.0.8: gulp-util is deprecated - replace it, following the guidelines at https://medium.com/gulpjs/gulp-util-ca3b1f9f9ac5` 


Check gulp runs ok and displays list of tasks
```sh
$ gulp --tasks
[10:49:24] Tasks for /jira-tools/gulpfile.js
[10:49:25] ├── clean
[10:49:25] ├── build
[10:49:25] ├── clasp-push
[10:49:25] ├── clasp-pull
[10:49:25] ├── un-google
[10:49:25] ├── diff-pulled-code
[10:49:25] ├── copy-changed-pulled-code
[10:49:25] ├─┬ deploy
[10:49:25] │ └─┬ <series>
[10:49:25] │   ├── clean
[10:49:25] │   ├── build
[10:49:25] │   └── clasp-push
[10:49:25] └─┬ pull-code
[10:49:25]   └─┬ <series>
[10:49:25]     ├── clean
[10:49:25]     ├── clasp-pull
[10:49:25]     └── un-google
```

Check unit tests are running
```sh
npm test
```

You should now be good for development.

## Developing locally
You will need to be able to test any development work using the code on the target deployment environment of the Google App Scripts (GAS) runtime with a connection to a JIRA instance.

To speed up development of features the code is set up to be able to run locally on your development machine and use TDD to test code before running against the deployment environment. The use of TDD also enables the reduced risk of regression bugs when building new features.

### Workarounds needed to run GAS files locally in Node
The Google App Scripts (GAS) runtime environment differs from Node.js. For example, in the GAS runtime, any function available in a .gs file in your project is automatically available to call from other files.
Node.js does not allow that.

In enable to allow the running of the unit tests locally using Node JS each .gs file requires the use of import and export statements to make the files available e.g. The following require statements imports the 'getCfg', 'setCfg' and 'hasSettings' functions defined in 'settings.gs' into in another JS file (when running in Node)

```markdown
// Require imports
const getCfg = require("./settings.gs").getCfg;
const setCfg = require("./settings.gs").setCfg;
const hasSettings = require("./settings.gs").hasSettings;
// End of Require imports
```

This exports statement in 'settings.gs' is also required
```markdown
module.exports = {getCfg: getCfg, setCfg: setCfg, hasSettings: hasSettings}
```

These statements are unnecessary in GAS and would cause an error since 'Require' is a Node.js feature.

The gulp scripts used to deploy the source code to GAS (via Clasp) include tasks to comment out these lines of code. The script looks for blocks starting and ending with the following lines and comments the whole block out. 

```markdown
// Require imports
THIS WHOLE BLOCK WILL BE COMMENTED WHEN DEPLOYED BY THE GULP SCRIPT
// End of Require imports
```

Another approach could be to use this code in the unit tests...
https://github.com/mzagorny/gas-local
This should be investigated.

## Testing and deploying to GAS using `clasp`
### Set up and linking to your project
Some notes on how to create or clone the Google Project you want to use for deploying and testing.
- if you already have a Project
- if you don't 

```markdown
clasp login (go through authorisation)
should see something like 
Authorization successful.

Default credentials saved to: ~/.clasprc.json (/Users/paullemon/.clasprc.json).

gulp clean
gulp build
cd dist/build/
```

1. You already have a project
use clasp list
Copy of Jira-tools   – https://script.google.com/d/17K2nCY_jOgxCfJycQLCDj5_-EUZmFUHxWUrpiuNpjhRysPidWGkeegKq/edit
clasp clone https://script.google.com/d/17K2nCY_jOgxCfJycQLCDj5_-EUZmFUHxWUrpiuNpjhRysPidWGkeegKq/edit
check you have a .clasp.json file


2. creating a new one
clasp create --type Spreadsheet --title Jira-Tools-2

Then cp .clasp.json ../../src
cd ../../
gulp deploy

### Testing unit tests
...

### Deploying using `gulp` task
Using the following gulp task will clean the export and require statements and push the code to the configured GAS project.

```markdown
gulp deploy
```

### Pulling changes back from your Google project

If you make changes to the code in the google project web interface  you can pull those changes down onto your local machine.

Step 1 - execute this gulp task

```markdown
gulp pull-code
```
This will pull the changes down from your GAS project, and uncomment the require and exports statments. The files will be pulled into a temporary folder 'dist/pull'

You can then see a diff of the code in your 'src' folder with that code in 'dist/pull' so you can visually verify that this is a change you want to have in your local copy and commit to GIT (just in case any temporary changes or debug code has been left)

```markdown
gulp diff-pulled-code
```
You can then use this gulp task to copy the changed files from 'dist/pull' to 'src/' so you can verify unit tests and commit back to git

```markdown
gulp copy-changed-pulled-code
```




