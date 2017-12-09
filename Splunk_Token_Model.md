# Splunk Token Model

**Splunk uses three different token models on its dashboards.**

`DefaultTokenModel`
* tokens are immediately propagated in case of a change

`SubmittedTokenModel`
* tokens are only propagated if a search is submitted
* the content of the default token model is then copied into the submit token model

`URLTokenModel`
* tokens are extracted from the URL

## How does it work?

Let's take the text input "myinput" to filter a search with the filled in keywords.
* the keywords in the textbox are saved in the token `form.myinput`
* the last token used is in the token `myinput`
