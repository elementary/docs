# Creating Logs

One way to debug apps is by logging information in the code. This enables seeing which code was run when a problem occurred and what the value of variables were. You can directly call [`GLib.Log ()`](https://valadoc.org/glib-2.0/GLib.log.html) or use the convenience functions listed below.

{% hint style="info" %}
To view logs from all your applications you can use [`journalctl`](https://www.digitalocean.com/community/tutorials/how-to-use-journalctl-to-view-and-manipulate-systemd-logs).
{% endhint %}

## Debug

Debug logs usually give detailed information on the flow through the system and are not printed to Terminal or logs by default.

Log functions, like `debug` use [`printf`](http://www.cplusplus.com/reference/cstdio/printf/) style formatting and can be called like this:

```text
string name = "Bob";
int age = 30;
debug ("Person: %s %i", name, age);
```

{% hint style="info" %}
Start your app with `G_MESSAGES_DEBUG=all` to print debug messages
{% endhint %}

## Info

Use info log level to log informational messages as well as interesting runtime events. These logs are also immediately visible on a status console, and should be kept to a minimum.

```text
info ("An event occured");
```

## Message

Use the message log level to output a message.

```text
message ("An event occured");
```

## Warning

The warning log level outputs messages that warn of, for example, use of deprecated APIs, 'almost' errors, or runtime situations that are undesirable or unexpected, but not necessarily "wrong". These logs are immediately visible on a status console.

```text
warning ("Something potentially problematic happened!");
```

{% hint style="info" %}
Start your app with `G_DEBUG=fatal-warnings` to exit the program at the first call to `warning ()` or `critical ()`
{% endhint %}

## Critical

Critical log level is used when there is a severe application failure that should be investigated immediately.

```text
critical ("A major issue occured! Uh oh!");
```

{% hint style="info" %}
Start your app with `G_DEBUG=fatal-criticals` to exit the program at the first call to `critical ()`
{% endhint %}

## Error

Error log level includes logs for runtime errors or unexpected conditions. These errors are immediately visible on a status console and cause your app to exit.

```text
error ("Some catastrophic happened and the app had to exit!");
```

