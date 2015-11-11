<properties 
	pageTitle="Application Insights: Proactive detection" 
	description="Application Insights performs deep analysis of your app telemetry and warns you of potential problems." 
	services="application-insights" 
    documentationCenter="windows"
	authors="alancameronwills" 
	manager="douge"/>

<tags 
	ms.service="application-insights" 
	ms.workload="tbd" 
	ms.tgt_pltfrm="ibiza" 
	ms.devlang="na" 
	ms.topic="article" 
	ms.date="11/02/2015" 
	ms.author="awills"/>

#  Application Insights: Proactive Detection

*Application Insights is in preview.*

Application Insights performs deep analysis of your app telemetry, and can warn you about potential performance problems. You're probably reading this because you received one of our proactive alerts by email.

## What is Proactive detection 

Proactive detection using Machine Learning & Data Mining algorithms to detect abnormal patterns that impact application performance. Proactive detection automatically analyzes performance telemetry collected to Application Insights and if detected, notifies about abnormal performances in application, no thresholds or rule setting required. Proactive detection notifications are deeply integrated with Application Insights advanced analytics capabilities which enables a quick triage and diagnosis of the issues.  

## About the proactive alert

* *Why have I received this email?*
 * Proactive detection analyzed the telemetry your application sent to Application Insights and detected a performance issues in your application.
* *Does the notification mean I definitely have a problem?*
 * No. It's simply a suggestion about something you might want to look at more closely. 
* *What should I do?*
 * [Look at the data presented](#responding-to-an-alert) use ME to see the performance over time and drill in to additional metrics, use Search to filter out a specific events that will help you identify the root cause.
* *So, you guys look at my data?*
 * No. The service is entirely automatic. Only you get the notifications. Your data is [private](app-insights-data-retention-privacy.md).


## The detection process

* *What kinds of anomalies are detected?*
 * Patterns that you would find it time-consuming to check for yourself. For example, poor performance in a specific combination of location, time of day and platform.
 * *Are you analyze all the telemetry collected to Application Insights?*
 * No, today we analyze request response time and page load time. Analysis of additional metrics is coming soon. 
* *Can I create my own anomaly detection rules?*
 * Not yet. But you can:
 * [Set up alerts](app-insights-alerts.md) that tell you when a metric crosses a threshold.)
 * [Export telemetry](app-insights-export-telemetry.md) to a [database](app-insights-code-sample-export-sql-stream-analytics.md) or [to PowerBI](app-insights-export-power-bi.md) or [other](app-insights-code-sample-export-telemetry-sql-database.md) tools, where you can analyze it yourself.
* *How often is the analysis performed?*
 * We run the analysis daily on the telemetry from the previous day.
* *Does this replace alerts?*
 * No. We don't commit to detect every behaviour that you might consider abnormal.

## How to investigate issues detected by Proactive detection 

Click on the link in the email you recieved or click on.

![](./media/app-insights-anomaly/02.png)

Notice:

* When - the time frame when the issue was detected.
* What - description of the slow performing group, e.g. "POST /api/Contoso/health" servier requests.
* Table - Show the performance of the slow performing group vs performance of all other beside the group.

Click on Metric explorer below will open Metric explorer blade with relevant reports filtered on the time and the properties of the slow perfroming group.
Modify the time range and filters to explore the telemetry.

Click on Search will opne Search with time range and filters set on the slow performing group time and properties

## How can improve my application performance?

Slow and failed responses are one of the biggest frustrations for web site users, as you will know from your own experience. So it's important to address the issues.

### Triage

First, does it matter? If a page is always slow to load, but only 1% of your site's users ever have to look at it, maybe you have more important things to think about. On the other hand, if only 1% of users open it, but it throws exceptions every time, that might be worth investigating.

Use the impact statement in the email as a general guide, but be aware that it isn't the whole story. Gather other evidence to confirm.

Consider the parameters of the issue. If it's geography-dependent, set up [availability tests](app-insights-monitor-web-app-availability.md) including that region: there might simply be network issues in that area. 

### Diagnose slow page loads 

Where is the problem? Is the server slow to respond, is the page very long, or does the browser have to do a lot of work to display it?

Open the Browsers metric blade. The [segmented display of browser page load time](app-insights-javascript.md#explore-your-data) shows where the time is going. 

* If **Send Request Time** is high, either the server is responding slowly, or the request is a post with a lot of data. Look at the [performance metrics](app-insights-web-monitor-performance.md#metrics) to investigate response times. 
* Set up [dependency tracking](app-insights-dependencies.md) to see whether the slowness is due to external services or your database.
* If **Receiving Response** is predominant, your page and its dependent parts - JavaScript, CSS, images and so on (but not asynchronously loaded data) are long. Set up an [availability test](app-insights-monitor-web-app-availability.md), and be sure to set the option to load dependent parts. When you get some results, open the detail of a result and expand it to see the load times of different files.
* High **Client Processing time** suggests scripts are running slowly. If the reason isn't obvious, consider adding some timing code and send the times in trackMetric calls.

### Improve slow pages

There's a web full of advice on improving your server responses and page load times, so we won't try to repeat it all here. Here are a few tips that you probably already know about, just to get you thinking:

* Slow loading because of big files: Load the scripts and other parts asynchronously. Use script bundling. Break the main page into widgets that load their data separately. Don't send plain old HTML for long tables: use a script to request the data as JSON or other compact format, then fill the table in place. There are great frameworks to help with all this. (They also entail big scripts, of course.)
* Slow server dependencies: Consider the geographical locations of your components. For example, if you're using Azure, make sure the web server and the database are in the same region. Do queries retrieve more information than they need? Would caching or batching help?
* Capacity issues: Look at the server metrics of response times and request counts. If response times peak disproportionately with peaks in request counts, it's likely that your servers are stretched. 


## Notification emails

* *Do I have to subscribe to this service in order to receive notifications?*
 * No. Our bot periodically surveys the data from all Application Insights users, and sends notifications if it detects problems.
* *Can I unsubscribe or get the notifications sent to my colleagues instead?*
 * Click the link in the alert or email. Open anomaly settings.
 
    ![](./media/app-insights-anomaly/01.png)

    Currently they're sent to those who have [write access to the Application Insights resource](app-insights-resources-roles-access-control.md).
* *I don't want to be flooded with these messages.*
 * They are limited to one per day. You won't get repeats of any message.
* *If I don't do anything, will I get a reminder?*
 * No, you get a message about each issue only once.
* *I lost the email. Where can I find the notifications in the portal?*
 * In the Application Insights overview of your app, click the **Proactive Detection** tile. 






 
