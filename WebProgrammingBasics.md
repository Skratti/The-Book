# Web Development Basics

This chapter will explain what happens when you open websites, and how JavaScript is executed.  

Open a web browser which has developer tools. Firefox, IE, Edge and chrome all do have those.  
Visit the website www.EmpiresInSpace.com.  Open the developer tools, they can be found in the browser settings. Alternatively, press shift F8 or F12, depending on your browser. The following describes what chrome and edge do show, but Firefox and IE are not that different.  

The developer tools are a very important tool, used for analyzing websites, debugging, trying out css and much more.  

With the dev tools open, reload the page by pressing F5. Go to the network tab and select the top entry, which will show that you requested data from www.EmpiresInSpace.com.  

The request tab page shows additional information of the web request. There is data that was sent to the web server (like language preferences, used web browser, cookies), along with data that was returned like the status code 200 (this would e.g. be the well known 404 if the server wouldn't find the requested page).  
But the main data returned by server is shown in the Response tab. You will see the source code of the current website here.  

When the web browser opens an URl, it makes a get request, gets the response and begins to evaluate the response line by line. HTML elements are put into the DOM (Document Object Model), images are loaded, scripts are loaded the moment the line is evaluated by the server, and they are instantly executed. All along, the site is also being drawn on the screen.
We can now compare the source code of the site with that of n our Empires In Space index project. Open it in the VS and open file index aspx.  

The only difference between the index.aspx and the response shown in the web browser are the uppermost line, and the version-ids.
 



So the web browser requests data, gets the data
The browser is sending a get request to the 
- browser
- open url

request is send to web server, web server returns websites
developer tools
open Network tab, reload
click first entry in list, analyse Headers and Response

Response is executed line by line, DOm is created, scripts (javascript) is executed


Before response is send by the webserver, that response might be modified.
Open EmpiresInSpace, project EmpiresInSpaceIndex, file ...






