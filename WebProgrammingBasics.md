# Web Development Basics

This chapter will explain what happens when you open websites, and how JavaScript is executed.  

Open a web browser which has developer tools. Firefox, IE, Edge and chrome all do have those.  
Visit the website www.EmpiresInSpace.com.  Open the developer tools, they can be found in the browser settings. Alternatively, press shift F8 or F12, depending on your browser. The following describes what chrome and edge do show, but Firefox and IE are not that different.  

With the dev tools open, reload the page by pressing F5. Go to the network tab and select the top entry, which will show that you requested data from www.EmpiresInSpace.com.  

The request tab page shows additional information of the web request. There is data that was sent to the web server, along with data that was returned like the status code 200.  
But the main data returned by server is shown in the Response tab. You will see the source code if the current website here.  
When the web browser opens an URl, it makes a get request, gets the response and begins to evaluate the response line by line. HTML elements are put into the DOM, images are loaded, scripts are loaded the moment the line is evaluated by the server, and they are instantly executed.  



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






