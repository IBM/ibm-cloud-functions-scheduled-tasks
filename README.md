# Triggering IBM Cloud Functions with scheduled tasks
Set up scheduled tasks, like a cron job or batch job, with IBM Cloud Functions powered by Apache OpenWhisk. This tutorial should take about 5 minutes to complete. After this, move on to more complex serverless applications such as those tagged [_openwhisk-hands-on-demo_](https://github.com/search?q=topic%3Aopenwhisk-hands-on-demo+org%3AIBM&type=Repositories).

![Sample Architecture](openwhisk-scheduled-tasks.png)

If you're not familiar with the OpenWhisk programming model [try the action, trigger, and rule sample first](https://github.com/IBM/openwhisk-action-trigger-rule). [You'll need a Bluemix account and the latest OpenWhisk command line tool](https://github.com/IBM/openwhisk-action-trigger-rule/blob/master/docs/OPENWHISK.md).

Short description.

1. [Section 1](#1-section-1)
2. [Section 2](#2-section-2)
4. [Section 3](#3-section-3)

# 1. Section 1
To come.

> **Note**: If you see any error messages, refer to the [Troubleshooting](#troubleshooting) section below.

# 2. Section 2
To come.

# 3. Section 3
To come.

# Troubleshooting
Check for errors first in the OpenWhisk activation log. Tail the log on the command line with `wsk activation poll` or drill into details visually with the [monitoring console on Bluemix](https://console.ng.bluemix.net/openwhisk/dashboard).

If the error is not immediately obvious, make sure you have the [latest version of the `wsk` CLI installed](https://console.ng.bluemix.net/openwhisk/learn/cli). If it's older than a few weeks, download an update.
```bash
wsk property get --cliversion
```

# License
[Apache 2.0](LICENSE.txt)
