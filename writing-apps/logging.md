# Logging

There are various logging methods to use and here are some sample references for you.

## Debug

Debug logs usually give detailed information on the flow through the system and are not printed to Terminal or logs by default.

```vala
public static int main (string[] args) {
    // Use "G_MESSAGES_DEBUG=all ./test" to print debug messages.

    // Output: `** (process:<PID>): DEBUG: <FILENAME>:<LINE>: my 10. debug message`
    debug ("my %d. %s", 10, "debug message");
    return 0;
}
```

## Info

Use info log level to log informational messages as well as interesting runtime events. These logs are also immediately visible on a status console, and should be kept to a minimum.

```vala
public static int main (string[] args) {
    // Output: `** (process:<PID>): INFO: <FILENAME>:<LINE>: my 10. info message`
    info ("my %d. %s", 10, "info message");
}
```

## Message

Use the message log level to output a message.

```vala
public static int main (string[] args) {
    // Output: `** Message: <FILENAME>:<LINE>: my 10. message`
    message ("my %d. %s", 10, "message");
    return 0;
}
```

## Warning

The warning log level outputs messages that warn of, for example, use of deprecated APIs, 'almost' errors, or runtime situations that are undesirable or unexpected, but not necessarily "wrong". These logs are immediately visible on a status console.

```vala
public static int main (string[] args) {
    // Output: `** (process:<PID>): WARNING **: <FILENAME>:<LINE>: my 10. warning`
    warning ("my %d. %s", 10, "warning");
    return 0;
}
```

{% hint style="info" %} Start your app with `G_DEBUG=fatal-warnings` to exit the program at the first call to `warning ()` or `critical ()` {% endhint %}

## Critical

Critical log level is used when there is a severe application failure that should be investigated immediately.

```vala
public static int main (string[] args) {
    // Output: `** (process:<PID>): CRITICAL **: <FILENAME>:<LINE>: my 10. critical`
    critical ("my %d. %s", 10, "critical");
    return 0;
}
```

{% hint style="info" %} Start your app with `G_DEBUG=fatal-criticals` to exit the program at the first call to `critical ()` {% endhint %}

## Error

Error log level includes logs for runtime errors or unexpected conditions. These errors are immediately visible on a status console and cause your app to exit.

```vala
public static int main (string[] args) {
    // Output:
    //   `** (process:<PID>): ERROR **: <FILENAME>:<LINE>: my 10. error`
    //   `Trace/breakpoint trap`

    // Terminate calling process & log an error:
    error ("my %d. %s", 10, "error");
}
```
