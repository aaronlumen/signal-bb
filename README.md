# signal-bb
> [!CAUTION]
> Open Whisper systems signal bulletin board based off of the signal-cli package and open whisper systems secure messaging platform.  
> [!TIP]
> 📦 What's Included:
> Dockerfile to build the environment with PHP, Apache, and Signal-CLI.
>
> Basic index.php placeholder (you can replace this with your full script).
>
> Configures Apache + allows www-data to run signal-cli via sudo.

> [!WARNING]
> 🧰 To Build & Run:
> Unzip and run the following commands in the project directory:
>
> docker build -t surina-whisper-web .
> docker run -d -p 8080:80 --name whisper surina-whisper-web
> 
> [!IMPORTANT]
> 
> Then visit: http://localhost:8080
