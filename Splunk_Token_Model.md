# Splunk Token Model

**Splunk uses three different token models on its dashboards.**

`DefaultTokenModel`
* Tokens are immediately propagated in case of a change

`SubmittedTokenModel`
* Tokens are only propagated if a search is submitted (auto or by button)
* The content of the default token model is then copied into the submit token model

`URLTokenModel`
* Tokens are extracted from the url

## How does it work?

Let's take the text input "myinput" to filter a search with the filled in keywords.
* The keywords in the textbox are saved in the token `form.myinput`
* The last token used is in the token `myinput`
* A search is only started if "myinput" has changed in the SubmittedTokenModel

```
 -+-+-+-+-+-+-+-+-  
| Hello World!    |  ----> form.myinput = "Hello World!"  
 -+-+-+-+-+-+-+-+-
```
```
Submit: form.myinput ----> myinput = "Hello World!"  
```
```
Usage: $myinput$     ----> index=myindex Hello World!
```

## Dashboard without Submit Button (autosubmit)

Dashboard is loaded initially...

 <empty> | URLTokenModel | DefaultTokenModel | SubmittedTokenModel
--- | :---: | :---: | :---:
**form.myinput** | null | default | -
**myinput** | - | default | default
 
Dashboard is loaded with "?form.myinput=hello" in the url...

 <empty> | URLTokenModel | DefaultTokenModel | SubmittedTokenModel
--- | :---: | :---: | :---:
**form.myinput** | ?form.myinput=hello &rarr; | hello &darr; | -
**myinput** | - | hello &rarr; | hello
 
User types "world" into the textbox...

 <empty> | URLTokenModel | DefaultTokenModel | SubmittedTokenModel
--- | :---: | :---: | :---:
**form.myinput** | ?form.myinput=world | &larr; world &darr; | -
**myinput** | - | &nbsp;&nbsp;&nbsp; world &rarr; | world
 
## Dashboard with submit button (form)

Dashboard is loaded initially...

 <empty> | URLTokenModel | DefaultTokenModel | SubmittedTokenModel
--- | :---: | :---: | :---:
**form.myinput** | null | default | -
**myinput** | - | default | default
 
Dashboard is loaded with "?form.myinput=hello" in the url...

 <empty> | URLTokenModel | DefaultTokenModel | SubmittedTokenModel
--- | :---: | :---: | :---:
**form.myinput** | ?form.myinput=hello &rarr; | hello &darr; | -
**myinput** | - | hello &rarr; | hello
 
User types "world" into the textbox...

 <empty> | URLTokenModel | DefaultTokenModel | SubmittedTokenModel
--- | :---: | :---: | :---:
**form.myinput** | ?form.myinput=hello | world &darr; | -
**myinput** | - | world &nbsp;&nbsp;&nbsp; | hello
 
 User clicks on the submit button...
 
  <empty> | URLTokenModel | DefaultTokenModel | SubmittedTokenModel
--- | :---: | :---: | :---:
**form.myinput** | ?form.myinput=world | &larr; world &nbsp;&nbsp;&nbsp; | -
**myinput** | - | &nbsp;&nbsp;&nbsp; world &rarr; | world
 
## How does a multiselect input work then?

Dashboard is loaded with "?form.myinput=hello&form.myinput=world" in the url...

 <empty> | URLTokenModel | DefaultTokenModel | SubmittedTokenModel
--- | :---: | :---: | :---:
**form.myinput** | ?form.myinput=hello&  form.myinput=world &rarr; | [hello, world] &darr; | -
**myinput** | - | field="hello" OR field="world" &rarr; | field="hello" OR field="world"
 
 * form.myinput is always an array
 * Token Pre- and Suffix, token value Pre- and Suffix must be defined in the input so that splunk knows how to convert the input into a valid string.
