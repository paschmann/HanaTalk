HanaTalk
========

A small Javascript class to make SAP HANA XS Engine development simpler.

OK - so what does it do?

By adding a HANA Talk js and xsjs file to your project, you can simply write SQL statements in your HTML file and have the results returned synchronously.

e.g.  Index.html

var resp = hana.executeScalar('SELECT 1 FROM DUMMY');

console.log(resp); //Outputs 1

Super simple and easy. See below for further details.

<b>Prerequisites</b>

- Download/fork these 2 files from Github and add them to your package. The filenames/structure can be changed if you are feeling adventurous. In your HTML file, you will need to reference client.js, this is as simple as adding a tag to you header. For reference, if you are not using SAP UI5 or jQuery - you will need to add this to your HTML header as well.

<b>A Basic Example</b>

- In your javascript code, instantiate a new HanaTalk object. We will use this to "pass" our SQL commands to our HANA DB.

var hana = new HanaTalk('SYS'); //The 'SYS' reference is in relation to the Schema. It can be specified here or within your TSQL Statement

- Call your HanaTalk object with the operation type and SQL you would like execute (see below for additional operations).

var result = hana.executeRecordSet("SELECT 1 FROM DUMMY");

- We can populate that response into our html (DOM)

document.getElementById("SomeElementID").innerHTML = result;

<b>A few more examples</b> 

- Insert/Update/Delete a record - use .executeChange, this will execute your code and respond with the records which have been updated

document.getElementById("resp4").innerHTML = /*hana.executeChange("UPDATE/INSERT/DELETE .... ") + */ ' Record Changed';

- Return a Table - using .executeRecordSet will return a html formatted table, displaying the select's record set

document.getElementById("resp2").innerHTML = hana.executeRecordSet("SELECT TOP 5 * FROM M_HOST_INFORMATION");

- Return a Object - .executeRecordSetObj allows us to loop through records, and have quite a lot of control of the display of each record and its column name.

document.getElementById("resp3").innerHTML = hana.executeRecordSetObj("SELECT TOP 5 * FROM M_HOST_INFORMATION");

Here are the results of the calls above:

<img src='http://scn.sap.com/servlet/JiveServlet/downloadImage/38-93027-282032/620-112/pastedImage_2.png' />

As you can see, making these types of calls is fairly simple and since your files reside directly on the DB app server, responsiveness is not too bad. So what's so bad about this method?

<b>Disclaimers</b>

<b>Security:</b> Because you are passing your SQL text directly from your browser to the backend server, you open yourself up to SQL Injection hacks. Technically, we are not parsing URL arguments/parameters into the server side DB request but rather entire statements. In order for us to hack this, you could simply change the URL request to include some devious SQL.

<b>Speed:</b> For simplicity and to avoid AJAX Callbacks, the execution is performed Synchronously (i.e. We wait for the server to completely respond before we continue processing). The good: Code is "inline" and easy to manipulate the response/results from the server. The Bad: if we do a Select * on a massive (read billion) row table, the entire window is locked/hung until it either times out or completes. Not a particularly nice behaving UI.
