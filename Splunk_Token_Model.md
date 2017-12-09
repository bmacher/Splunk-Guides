# Splunk Token Model

**Splunk uses three different token models on its dashboards.**

`DefaultTokenModel`
* Tokens are immediately propagated in case of a change

`SubmittedTokenModel`
* Tokens are only propagated if a search is submitted (auto or by button)
* The content of the default token model is then copied into the submit token model

`URLTokenModel`
* Tokens are extracted from the URL

## How does it work?

Let's take the text input "myinput" to filter a search with the filled in keywords.
* The keywords in the textbox are saved in the token `form.myinput`
* The last token used is in the token `myinput`
* A search is only started if "myinput" has changed in the SubmittedTokenModel


 -+-+-+-+-+-+-+-+-  
| Hello World!&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;| --------> `form.myinput` = "Hello World!"  
 -+-+-+-+-+-+-+-+-
 
 Submit: `form.myinput` ----> `myinput` = "Hello World!"  
 
 Usage: `myinput` -----> <data> $myinput$
