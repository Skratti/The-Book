# Installation

Install Visual Studio with the following workloads activated:
* ASP.NET and Web Development
* .NET-Desktop development
* Datastorage and management

I translated the workload names from german to english, since the installer can't change language. Can someone please verify the workload names in an english installer?  

The first workload is needed because we are using the microsoft web server IIS Express, and have a lot of html/typescript files in the project.  
The second workload is need for the map generator, a small C# project that uses the windows forms framework.  
The third framework installs a sql server, which holds all game data. 


While you're runnig the installation (well - in case that this is needed), I'm explaining a bit of the project structure:

*Three-tier application image*

Client, server and database are distinct programs, communicating with each other. In Webapplications, the server is code run by the webserver. Since the webserver also hosts the websites themselves, client and server tier are a bit mixed up. When a browser requests a website, the server might e.g. add dynamic values to the otherwise static website. Or he even might create the whole page by code run on the server. In those cases, even though we have three distinct programs running (webbrowser with the website, server, database), the website and the server share sourcecode and they can be developed in a single visual studio project.  
Empires in Space does in fact have a slightly different structure:

*Three-tier image, this time with a divider through the server block, and the dataConnector and gameConnector shown in the image*

The central part (EiS-server) is a .net class library (.dll) which contains all essential sourcecode. It uses an interface class to communicate with the database, and it provides a gameConnector class for communication with the calling program.
So on startup, the webserver instanciates an object of the EiS-server, which itself does create a connection to the database and fetches all game data. The Eis-server object is then kept alive for all further communications of users by the webser. More to that in the chapter ***

So, in the github repository, you'll find:
* A folder containing lots of sql scripts to create the database
* A C# class library project which creates the eis-server dll
* A C# web application which uses the eis-server .dll -> running this runs the game
* A C# web application for the login page. This application contains a bit of user management stuff like slating and hashing the user password and verifying it on login
* A C# windows forms app - the map generator
* An Android Studio project which encapsulates the website in an android app. This is in a very early state yet
* Labelfiles containing translations of in-game text, and a research folder which contains a svg representation of hte in-Game research tree to be opened by inkscape or other svg tools

These are the next steps after installation of the Visual Studio:
* Create the database ond the server and execute all scripts
* Run the map generator (use the new database as target)
* Run the game in development mode (automatic creation of a player account and shorter turn durations) 

Then we will take a look at debugging and run the game line by line. I'll explain a bit along the way. To be skilled in using the debugger is very essential in programming, don't skip this chapter if you're new to visual studio development.

# Setting up the game

Open visual studio, and clone the github repository  
https://github.com/Skratti/EmpiresInSpace

Best would be a target folder which is not under your home directory, else we might run into problems later on.

Use "Open a local folder" and examine it in the solution explorer.  
We so now need a database for the game. I normaly give them the name of a galaxy, in the course of this book I'll assume that the database has the name andromeda.




* The database holds the game data. There are two types of data - base data like the galaxy map, what kind of researches exist, buildings, ship hulls etc.
* The second type is the player related stuff which might change constantly - colonies, messages, alliance, ships, trade offers and so on.


