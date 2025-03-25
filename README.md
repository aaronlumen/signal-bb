### signal-bb
> [!IMPORTANT]
> Open Whisper systems signal bulletin board based off of the signal-cli package and open whisper systems secure messaging platform.

> [!TIP]
> 📦 What's Included:
> Dockerfile to build the environment with PHP, Apache, and Signal-CLI.
>
> Basic index.php placeholder (you can replace this with your full script).
>
> Configures Apache + allows www-data to run signal-cli via sudo.
>

> [!TIP]
> ✅ Signal-CLI Prerequisites Checklist
> Installed Signal-CLI:
> Make sure signal-cli is installed and the path ```/usr/bin/signal-cli``` is correct.
> You can check this by running which ```signal-cli``` or ```signal-cli --version```

> [!IMPORTANT]
> Signal Number Registered:
>
> Ensure ```$signalNumber``` is registered via ```signal-cli -u +12085551212 register``` and _verified_.
>
> You can send test messages manually via CLI to validate it's working:


```signal-cli -u +12085551212 send -m "Hello" +17075551212```

> [!WARNING]
> Permissions:
>
> Apache/PHP (www-data user) needs permission to run Signal-CLI.
>
> One option is to allow specific signal-cli execution via sudo without a password:

```sudo visudo```
## Add:

```www-data ALL=(ALL) NOPASSWD: /usr/bin/signal-cli```

> [!TIP]
> Then in your PHP, call:

``` $command = "sudo $signalCliPath -u $signalNumber send -m $message $recipient_number";```

> [!IMPORTANT]
> 🔧 Recommended Code Adjustments
> 1. Improve Signal Send Function
> 2. Update send_signal_message() to log output and errors for better diagnostics:

## PHP CODE FOR LOGGING
``` function send_signal_message($recipient_number, $message, $signalCliPath, $signalNumber) {```
```    $recipient_number = escapeshellarg($recipient_number); ```
```    $message = escapeshellarg($message);```
```    $command = "sudo $signalCliPath -u $signalNumber send -m $message $recipient_number 2>&1";```
```    exec($command, $output, $return_var); ```
    
```    // Log for debugging ```
```    file_put_contents('/tmp/signal_debug.log', implode("\n", $output), FILE_APPEND);```
    
```    return $return_var === 0;```
``` } ```
> [!TIP] 2. Add Input Validation
> To prevent blank or malformed messages:

## PHP CODE

``` if (isset($_SESSION['authenticated']) && $_SESSION['authenticated'] === true && isset($_POST['send_signal'])) { ```
```    $recipient_number = trim($_POST['recipient_number'] ?? $default_recipient_number); ```
```    $message = trim($_POST['signal_message'] ?? ''); ```
    
```    if (!empty($recipient_number) && !empty($message)) { ```
```        if (send_signal_message($recipient_number, $message, $signalCliPath, $signalNumber)) { ```
```            $signal_message_status = "Message sent successfully to $recipient_number.";  ```
```        } else { ```
```            $signal_message_status = "Failed to send the message. Check logs."; ```
```        } ```
```    } else { ```
```        $signal_message_status = "Recipient and message cannot be empty."; ```
```    } ```
``` } ```
```

 > [!INFO]
> 🛡️ Security Best Practices
> Consider IP whitelisting or rate limiting if exposed on a public server.
>
> Use CSRF protection on forms if expanded into a larger app.
>
> Avoid storing raw numbers or sensitive data directly in HTML without proper escaping.

[!IMPORTANT]
> 🧪 Final Test Steps
>
> Once the above updates are in place:
>
> Restart your web server:
>
> ``` sudo systemctl restart apache2 (or nginx) ```

> Log in via your browser with the passcode.

> Enter a test message and send it to a valid Signal number.

> Check:

> ``` tail -F -n 100 /tmp/signal_debug.log ``` for any error output.

> The recipient’s device for delivery.


> [!NOTE]
> 🧰 To Build & Run:
> Unzip and run the following commands in the project directory:
>
``` docker build -t surina-whisper-web ```
``` docker run -d -p 8080:80 --name whisper surina-whisper-web ```
 
> [!IMPORTANT]
> 
> Then visit: http://localhost:8080
