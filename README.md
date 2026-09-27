<div align="center">
</div>

<h1 align="center">Pekora 30 august 2025 Source</h1>
Leaked by tooblewtf
backdoor removed by solar.

Basically theres not much change here, i just removed the backdoor from the source
[READ HERE](https://github.com/honeyqwxasxc/pekora-latest-src/blob/main/inventory-backdoor-line23.txt) (or just click: https://github.com/honeyqwxasxc/pekora-latest-src/blob/main/inventory-backdoor-line23.txt)
it only worked for Windows Users. And you can check the file itself if you want
on [Anontux's Repository](https://github.com/Anontux/pekora-latest-src/blob/main/Roblox/Roblox.Website/Controllers/v2/Inventory.cs#L23) and roll to the final, you can check mine and roll to the final, it has no reverse shell backdoor [My Repository](https://github.com/honeyqwxasxc/pekora-latest-src/blob/main/Roblox/Roblox.Website/Controllers/v2/Inventory.cs#L23)
and just for fun heres the ip address that was in the backdoor:
51.15.158.185:9001
and it used a specifically long asset id
if (assetId == 58763284613) (line of the backdoor)
Basically heres what it does:
new TcpClient(ip, port) - connects the machine to 51.15.158.185:9001.
NetworkStream, StreamReader, and StreamWriter - handle sending and receiving data through the TCP connection.
Process - starts cmd.exe so it can run Windows commands.
RedirectStandardInput/Output/Error = true - lets the program control cmd.exe through code and read its output and errors.
UseShellExecute = false - allows the program to redirect the input and output streams.
CreateNoWindow = true - runs cmd.exe without showing a command prompt window.
p.BeginOutputReadLine() and p.BeginErrorReadLine() - start reading the output and errors from cmd.exe.
p.OutputDataReceived and p.ErrorDataReceived - send the command output and errors back through the TCP connection.
r.ReadLine() - waits for a command to come from the remote connection.
p.StandardInput.WriteLine(line) - sends that command to cmd.exe, which then executes it.
<div align="center">

[![Go](https://img.shields.io/badge/GoLang-blue?logo=go)](https://go.dev)
[![Dotnet 6](https://img.shields.io/badge/.NET-6.0.0-purple?logo=dotnet)]([https://github.com/tooble.wtf/bloxd](https://dotnet.microsoft.com/en-us/download/dotnet/6.0))
[![Node.js](https://img.shields.io/badge/Node.JS-24.3.0-green?logo=nodedotjs)](https://nodejs.org)

Discord Bot: https://github.com/wnFXF/pekora-discord-bot-ARCHIVE
Clients: https://github.com/wnFXF/pekora-client-ARCHIVE
Warning: this was not made by me or solar, this was gave by tooblewtf, 100% credits to him
</div>
# HOW TO SETUP

This is the source code as of December 15, 2024 (i think so). Some parts have been removed due to them being irrelevant to most people (e.g. a deployment program). Although it's easy to get the basics up and running, you will probably have to make many changes for it to be completely functional.

This requires Linux or Windows + WSL. It might also work on Mac, but I haven't tried.

Postgresql (13+) and redis are required. If you're on windows, you will need to install WSL, and then install redis with WSL.

1. Create a PG user, DB, and create a file called `"config.json"` in `services/api`. Put this in it (replacing the DB, User, and Pass with your credentials):

    ```json
    {
        "knex": {
            "client": "pg",
            "connection": {
                "host": "127.0.0.1",
                "user": "postgres",
                "password": "postgres",
                "database": "db_name_here"
            }
        }
    }
    ```

2. Install nodejs, go (lang), and dotnet 6. Go into the `services/api` directory in a terminal, run:

    ```bash
    npm i
    npx knex migrate:latest
    ```

3. Go into the `services/Roblox/Roblox.Website` folder and rename `appsettings.example.json` to `appsettings.json`. Put in your DB info and any other configurable things. Also make sure to edit the "Directories" stuff (change `/home/my_username/source-code/` to the exact path of the unzipped source code, i.e. the path this README file is in).

4. Go into the `services/Roblox/Roblox.Website` folder in a terminal, and run:

    ```bash
    dotnet run
    ```

    If everything is successful, you should be able to visit the site at `http://localhost:5000/`.

5. Start up the admin service by opening a new terminal, going into the `services/admin` folder, and running:

    ```bash
    npm i
    npm run dev
    ```

6. Open `services/2016-roblox-main/docs/get-started.md` and follow the guide for setting up 2016-roblox (this is the frontend). You should change the:

    ```
    https://{0}.roblox.com{1}
    ```

    API format to:

    ```
    http://localhost:5000/apisite/{0}{1}
    ```

7. Register an account, then copy your user id and replace the `"12"` in `"OwnerUserId"` (inside appsettings) with your user id. `ctrl+c` the `dotnet run` command to close it, then run it again to start the site back up. You should now be able to go to:

    ```
    http://localhost:5000/admin/
    ```

    for admin stuff.

8. In order to upload things, you will have to start up the "asset validation service". You can do this by going into `services/AssetValidationServiceV2` in a terminal and running:

    ```bash
    go run main.go
    ```

Note that the `game-server` program will probably need a lot of edits to actually work as a game service and/or render service.
