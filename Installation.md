This guide is a quick run through on how to setup NexusForever.

### Prerequisites
* Visual Studio 2017 with .NET Core 2.1 and C# 7.3
* MySQL or MariaDB server
* WildStar 16042 client
* WildStar Studio

***

### Building
The build process for NexusForever is straightforward, with the solution loaded in Visual Studio you only need to restore the NuGet packages and build the solution.  

***

### Database Files
The database layout for NexusForever revolves around 3 separate databases, authentication, character and world.  
The authentication database includes account information for all realms, the character database includes realm dependent information such as characters and guilds for a single realm and finally the world database includes static information shared by all realms such as unit spawns.  

You want to execute the 3 base SQL files found in `...\Database\Base` of your NexusForever repository directory on 3 separate databases, by default these are `nexus_forever_auth`, `nexus_forever_character` and `nexus_forever_world`.  
With the base files executed you now need to execute all the SQL files found in the `...Database\Updates` on the respective database in order by date.  

The final step is to add the world server information to the `server` table found in `nexus_forever_auth`, by default this should be:  
```
INSERT INTO `server` VALUES ('NexusForever', '127.0.0.1', 24000, 0);
```

***

### Game Table Files
WildStar uses client side game tables (similar to dbc or db2 files from World of Warcraft) to supply the client with various bits of information about items, quest, spells, ect..., NexusForever utilises this information as well.  

With WildStar Studio loaded click the `Load` button, in the open dialog box find your installation path for WildStar, select `ClientData.index` inside the `Patch` folder.  
In the new tree view select the `DB` folder, the click the `Extract filtered` button, in the new window that opens make sure the `Export tables as .csv` box isn't ticked and click `Extract`.  

If done successfully you'll find a directory full of `.tbl` files found in `...\out\DB` of your WildStar Studio directory, copy these files to your NexusForever world server build folder in a directory called `tbl`.

_In the future a dedicated tool will be supplied with NexusForever that can do this process in a single action._

***

### Configuration Files
In the build folders for the 3 separate servers you'll find a configuration JSON file, `StsServer.example.json`, `AuthServer.example.json` and `WorldServer.example.json` respectively, copy or rename these files by removing the `.example` extension.  
Update the MySQL information in both these configuration files to match the 3 databases created earlier.   

With this information set correctly the STS, auth and world servers should start successfully.

***

### Account Creation
In order to login to the server you'll need to create an account, this can be achieved from the world server console.  
You can use the `accountcreate` command to achieve this, the command takes 2 parameters of a email/username and password.  
With an account created you can now use it to login to the server from the main menu of WildStar.

***

### Launching
Copy `NexusForever.ClientConnector.exe` and `Newtonsoft.Json.dll` from your `NexusForever.ClientConnector` build folder into the `WildStar\Client64` folder (at this time the connector only supports the x64 client).

Running the connector with administration privileges will open a console window allowing you to specify what host you would like your WildStar client to connect to, for a local server you need to enter either `127.0.0.1` or `localhost`.

Your entered host will be saved to a file called `config.json`, this allows you to skip entering the host for future launches of the connector.  
If you wish to change the saved host you can either modify this file or delete it which will prompt you for a new host on the next launch.
