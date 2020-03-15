# Errors Report

Screen_Shot_2018_08_19_at_15_44_14.png

The Errors tab, as it's name suggests, contains the errors that were received by the web-server under the test as a result of HTTP request. We can see all errors received during the test run, categorized by:

Labels (pages)
Respondes codes
Assertions


We report on these types of errors:

-Top requests
-Assertions
- Failed embedded resources
- For each error we will display the:

- Response code
- Response message
- Number of failed requests
- Execution__17_.png

# Engine Health Report

The Engine Health Report displays performance indicators received from the test engines (i.e. the infrastructure delivering the test traffic, not your system under test). EngineHleathReport00.png

The Engine Health indicates whether the test infrastructure itself could be the cause of bottlenecks or errors which are appearing in other reports. 

The Engine Health is also a great resource when deciding how many virtual users (VUs) each engine can support. The ideal ratio depends on the complexity and memory footprint of your script(s). You can read about the process for planning and calibration of test execution to optimally utilize available resources in the help topic, Calibrating a BlazeMeter test.

While running a test, make sure the CPU values are lower then 80% and memory levels are less then 70%.

Here are some actions to take when levels of CPU and/or Memory are too high:

1. Make sure that your script is resource effective, e.g with no enabled Listeners, redundant requests, or heavy samplers like WebDriver sampler that can be avoided.

2. Reduce the number of users per engine.

3. Add more engines to the test.

EngineHleathReport01.png

and here’s what each of these KPIs mean, 

CPU: Represents the Percentage usage of CPU in instance

Memory: Represents the Percentage usage of the Virtual Memory in instance

Network I/O: Represents the amount of data transferred in I/O operations (KB/s)

Connections: Represents the number of persistent connections established for each transaction throughout the test

# Error checking and dynamic requests

in this JMeter video tutorial we will learn how to use assertions to verify the responses on the fly.
 
1. Add error checking to your script - assertions that the responses are valid for a given request.
2. Use Firebug to view network traffic when you need to debug your test script.
3. Use a regular expression extractor to retrieve a dynamic value from a response and re-use it in a later request.

# JMeter Error Checking (Assertions) and Dynamic Requests (Extraction)

https://www.youtube.com/watch?v=SVxB3Tk4O4A&feature=emb_logo


# Test Works Locally but not on BlazeMeter

There are various reasons why a test script that works on your local machine will fail to run through BlazeMeter.

The first and the foremost thing you should do is check the errors found in your test's Errors Report, then download and review the logs from your test's Logs Report.

The most common causes for this problem fall under these categories:

- Missing File(s)
- Network Issues
- Load/Performance Issues
- Missing Files

You may not have uploaded all files required to run your test script in BlazeMeter.  If your test script requires any additional files (CSVs, JARs, etc.), then refer to our guide on Uploading Files & Shared Folders.
If you submit your test from CLI (e.g. using the -cloud option), make sure your yml includes a files: section and list all files that are referenced in your script so they will be uploaded to the test. See section "Specifying Additional Resource Files" in this article for details on how to do this.

You may have uploaded a required file, but your test script still refers to it using a local file path.  (For example, a CSV Data Set Config element may still refer to a CSV file using a local path.) This is likewise explained in our guide on Uploading Files & Shared Folders.

You may have uploaded a required file, but the filename referenced in your script is in a different case than the filename of the one uploaded.  Blazemeter engines run on Linux, which treats filenames as case-sensitive, so Your_File_Name.csv is a different file from your_file_name.csv or Your_File_name.csv.

You may have used a JMeter plugin but did not upload the plugin's JAR files.  Blazemeter automatically includes most standard JMeter plugins, but some plugins must be uploaded with the test.  Check the logs for any references to missing plugins.

# Network Issues

Your application server may only be accessible inside your local network.  If you see errors in your Errors Report such as "connection refused", "socket closed", "connection timed out", "unknown  host", 404 error codes, or similar errors, this means that even though your local machine (behind your internal network's firewall) can reach your application server, Blazemeter's engines (outside of your firewall) cannot. Review our options for Load Testing Behind Your Firewall.

If your application server returns connection errors as mentioned in the above bullet, but Blazemeter was able to establish some connections prior to the errors, then this is also indicative of a problem with either your application server or network.  Unlike the above scenario, it may not necessarily be a firewall issue, but an issue with your server or network struggling with the load or frequency of connections, or some other internal server/network issue that results in only partial connectivity during the test.

You may be trying to test one of the Websites Forbidden to Test Using Blazemeter.
Load/Performance Issues
You may have been running your local test as a small-load test (executing only a few threads/users), but the test you configured in Blazemeter is a significantly higher-load test.  If this is the case, depending on the type of test you're attempting to run, review our guide on Calibrating a JMeter Test or Calibrating a Taurus Test.

Please be aware that a local test run cannot be compared to a cloud run because your local machine will have a very different allocation of CPU/memory/etc. than a cloud VPC.

If you're running a Selenium test, be aware of our limitations on using Selenium for load testing.
