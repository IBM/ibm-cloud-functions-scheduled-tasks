# Triggering IBM Cloud Functions with scheduled tasks
Set up scheduled tasks, like a cron job or batch job, with IBM Cloud Functions powered by Apache OpenWhisk. This tutorial should take about 5 minutes to complete. After this, move on to more complex serverless applications such as those tagged [_openwhisk-hands-on-demo_](https://github.com/search?q=topic%3Aopenwhisk-hands-on-demo+org%3AIBM&type=Repositories).

![Sample Architecture](openwhisk-scheduled-tasks.png)

If you're not familiar with the OpenWhisk programming model [try the action, trigger, and rule sample first](https://github.com/IBM/openwhisk-action-trigger-rule). [You'll need a Bluemix account and the latest OpenWhisk command line tool](https://github.com/IBM/openwhisk-action-trigger-rule/blob/master/docs/OPENWHISK.md).

This example shows how to create an action that runs when invoked by a cron-style trigger.

1. [Define and test the action](#1-define-and-test-the-action)
2. [Create create the cron-style trigger](#2-create-create-the-cron-style-trigger)
3. [Map the trigger to the action](#2-map-the-trigger-to-the-action)
4. [Clean up](#4-clean-up)

# 1. Define and test the action

This simple JavaScript function (called an _action_ in OpenWhisk) accepts a `params` argument and writes information that can be retrieved from the Cloud Functions activation log and/or the IBM Cloud Functions monitoring console. It also returns a JSON object with the current date.

First, open a terminal window to start polling the activation log. The `console.log` statements in the action will be logged here, which you can stream with the following command:

```bash
wsk activation poll
```

Next, create a file named `handler.js`. This file will define an action written as a JavaScript function. This function will print out the date to the logs and return the same data from the function.

```javascript
function main(params) {

  var date = new Date();

  console.log("Invoked at: " + date.toLocaleString());

  return { 
    message: "Invoked at: " + date.toLocaleString() 
  };

}
```

In another terminal window, upload the action file as a Cloud Function using the following command:

```bash
wsk action create handler handler.js
```

Then invoke it manually. This will echo the resulting JSON message to the current window and log the activation in the other window.

```bash
wsk action invoke --blocking handler
```

# 2. Create create the cron-style trigger
Triggers can be explicitly fired by a user or fired on behalf of a user by an external event source, such as an alarm. This trigger uses the built-in alarm package feed to fire events every 20 seconds. This is specified through cron syntax in the `cron` parameter. The `maxTriggers` parameter ensures that it only fires for five minutes (15 times), rather than indefinitely.

Create an `every-20-seconds` with the following command:

```bash
wsk trigger create every-20-seconds \
  --feed  /whisk.system/alarms/alarm \
  --param cron '*/20 * * * * *' \
  --param maxTriggers 15
```

Once it's created, you will see activations firing in the log, but it's not yet mapped to the `handler` action so it's not very exciting.

# 3. Map the trigger to the action
Rules provide a mechanism for mapping triggers to actions in a many-to-many relationship. That is, one trigger can fire many independent actions and a single action can be triggered by many different triggers.

This rule shows how the `every-20-seconds` trigger can be declaratively mapped to the `handler.js` action. Notice that it's named somewhat abstractly so that if we wanted to use a different trigger - perhaps something that fires every minute instead - we could still keep the same logical name.

Create the rule with the following command:

```bash
wsk rule create \
  invoke-periodically \
  every-20-seconds \
  handler
```

At this point you can check the activation log that you are tailing in the other window to confirm that the action is invoked by the trigger.

# 4. Clean up
## Remove the rule, trigger, action, and package

```bash
# Remove rule
wsk rule disable invoke-periodically
wsk rule delete invoke-periodically

# Remove trigger
wsk trigger delete every-20-seconds

# Remove actions
wsk action delete handler
```

# Troubleshooting
Check for errors first in the Cloud Functions activation log. Tail the log on the command line with `wsk activation poll` or drill into details visually with the [Cloud Functions monitoring console](https://console.ng.bluemix.net/openwhisk/dashboard).

If the error is not immediately obvious, make sure you have the [latest version of the `wsk` CLI installed](https://console.ng.bluemix.net/openwhisk/learn/cli). If it's older than a few weeks, download an update.
```bash
wsk property get --cliversion
```

# License
[Apache 2.0](LICENSE.txt)

