# Installation

Install Visual Studio with the following workloads activated:
* ASP.NET and Web Development
* .NET-Desktop development
* Datastorage and management

(I translated the workload names from German to English, since the installer can't change language. Can someone please verify the workload names in an english installer?)

The first workload is needed because we are using the Microsoft web server IIS Express, and have a lot of html/typescript files in the project.  
The second workload is need for the map generator, a small C# project that uses the windows forms framework.  
The third workload installs a sql server, which will hold all game data. 


While you're runnig the installation (well - in case that this is needed), I'm explaining a bit of the project structure:

*Three-tier application image*

Client, server and database are distinct programs, communicating with each other. In web applications, the server is code run by the web server. Since the web server also hosts the websites themselves, client and server tier are a bit mixed up. When a browser requests a website, the server might e.g. add dynamic values to the otherwise static website. Or he even might create the whole page by code run on the server. In those cases, even though we have three distinct programs running (webbrowser with the website, server, database), the website and the server share source code and they can be developed in a single visual studio project.  
Empires in Space does in fact have a slightly different structure:

*Three-tier image, this time with a divider through the server block, and the dataConnector and gameConnector shown in the image*

The central part (EiS-server) is a .net class library (.dll) which contains all essential server sourcecode. It uses an interface class to communicate with the database, and it provides a gameConnector class for communication with the calling program - the web server.
So on startup, the web server instantiates an object of the EiS-server, which itself does create a connection to the database and fetches all game data. The Eis-server object is then kept alive for all further communications of users by the server. More to that in the chapter ***

So, in the GitHub repository, you'll find different folders:
* Database: A folder containing lots of sql scripts to create the database
* EmpiresInSpaceServer: A C# class library project which creates the EmpiresInSpace-server dll
* EmpiresInSpace: A C# web application which uses the EmpiresInSpace-server .dll -> running this runs the game
* EmpiresInSpaceIndex: A C# web application for the login page. This application contains a bit of user management stuff like salting and hashing the user password and verifying it on login
* MapGenerator: A C# windows forms app to write a new starmap and solar system maps to the database
* AndroidWebApp: An Android Studio project which encapsulates the website in an android app. This is in a very early state yet

In case that the installation is not complete yet, and in case that you don't know them yet, you might visit the following websites. They are an important source of informations:
* https://docs.microsoft.com/
* https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/
* http://www.typescriptlang.org/docs/home.html

Carry on from here when the installation is done.
These are the next steps:
* Create the database ond the server and execute all "Create database objects" scripts
* Run the map generator (use the new database as target)
* Run the game in development mode - automatic creation of a player account will take place and the game will run in with that users in game session

Then we will take a look at debugging and run the game line by line. I'll explain a bit along the way. To be skilled in using the debugger is very essential in programming, don't skip this chapter if you're new to visual studio development.

# Setting up the game

Open visual studio, and clone the GitHub repository  
https://github.com/Skratti/EmpiresInSpace

Best would be a target folder which is not under your home directory, else we might run into problems later on.

Open the visual studio solution file EmpiresInSpace.sln in the EmpiresInSpace root folder.  
You should see the solution explorer (a tree view of the solution showing several projects). Right click the project EmpiresInSpaceDatabase and select "Set as StartUp Project". 

## The database

We now need a database for the game. I normally give them the name of a galaxy, in the course of this book I'll assume that the database has the name Andromeda.
By choosing "Datastorage and management" during the VS installation, we have installed a sql server on your computer. 
* In the VS menu, choose view and click on "SQL Server Object Explorer"
* In the new SQL explorer, click the arrow left of "SQL Server"
* Then the arrow left of your local server instance
* Right click on "Databases" and add the Andromeda database.

We do now have to fill that database with tables, views, types, procedures and base data. All this can be done by executing a single script. Before we do that, some information about the content of the database:
* The database holds the game data. There are two types of data - base data like the galaxy map, what kind of researches exist, buildings, ship hulls etc.
* The second type is the player related stuff which might change constantly - colonies, messages, alliance, ships, trade offers and so on.
* In addition, the database contains views which are mostly used fetching the data, and procedures called to write the data.

In the solution explorer, under the EmpiresInSpaceDatabase project, open the file "00 CreateDameDB.sql". There are some instructions at the top. Since you're using the visual studio (another alternative would be the sql sever management studio), the name of your local sql server is shown in the sql explorer, it is probably "(localdb)\MSSQLLocalDB". Copy that into the sql script.  
The database name is Andromeda, and the file location is the one of your GitHub repository\sql\  
These information are needed when the script is running, but one also has to provide a database connection to give the context where the script is executed. That is done in the project properties of the EmpiresInSpaceDatabase project. Rightclick it, and select Properties and the Debug. Click "Edit" at "Target Connection String" and select your server and database.

In case that you're opening the script from outside the project:  use the toolbar just above the text. There is a small "connect" button - click it and connect to your server and the Andromeda database.

Now execute the script.  

Take a look at the Result and Message tab. The third result from the bottom might show an error in '06 fillLabels'. If that is so, open the file, correct the error and execute just that file by having the script open and using the execute command.  
Now we just need to create a galaxy map in that database, and the we can run empires in space on your machine.

## Map generation
In the solution explorer, right click the "MapGenerator" project and select "Set as Startup Project". Right click again and do a rebuild.  
If there is an error because of the missing Newtonsoft Library, rebuild it again. Visual studio will have downloaded the missing package and the second rebuild should show no more errors.

The map generator will need a database where he writes the generated data to. That will of course be our Andromeda database, but we have to provide a connection string to that database.  
Open the folder DBWriter and open the file DBWriter.cs. The connection string is hard coded in here - that's actually not best practice (App.config would be better), but it will do for now.
Replace the connection string with the correct one. You can get that by right clicking on your sql server in the sql server object explorer, choosing properties, and copying it from the properties window.
If there is a backslash in the datasource name, that will have to be escaped by adding a second backslash in front of it.
The Initial Catalog in the connection string should be Andromeda.  

You can now run the generator pressing F5, 	
* Click "Add Spreaded Stars" and generate and close that window
* Click "Add Nebula Fields" and generate and close that window

The generated objects are shown in the upper left corner. You can use left click to center the view on those, and the mouse wheel to zoom in.
* Click "Write to DB" to save the galaxy to the database
Wait till the message  "All data saved to the database." appeared.

That's it for the star map, the program can be closed now.

## Running the game
Right click the "EmpiresInSpace" project, set as startup project and rebuild, change the connection string in Web.config.  
Press F5 and that's it - your development environment is up and running.
