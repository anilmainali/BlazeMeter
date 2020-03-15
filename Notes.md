Errors report - provides a list of all errors received during the test. You will get a very good sense of what went wrong, e.g if you'll get a lot of 403 Forbidden errors, chances are that your firewall is blocking the traffic from our load generators and you will have to whitelist our IP ranges according to this list.

Engine Health Report - displays the performance indicators received during the test while monitoring the load generators. High CPU or Memory values will indicate the scenario is overwhelming the load generators and adjustments have to be made.

Logs - these mostly refer to the console's log and the load generators (engines) logs. You might be able to notice errors printed to the logs, e.g CSV file not found, Java errors, beanshell errors etc. The logs also present more details on the health of the load generators so the Monitoring Report's originated suspicions can be validated.


Aggregate report - enables you to view the statistics according to labels. You may notice that some labels generate a lot of errors, or get high response times than the rest of them.





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

# Load/Performance Issues
You may have been running your local test as a small-load test (executing only a few threads/users), but the test you configured in 

Blazemeter is a significantly higher-load test.  If this is the case, depending on the type of test you're attempting to run, review our guide on Calibrating a JMeter Test or Calibrating a Taurus Test.

Please be aware that a local test run cannot be compared to a cloud run because your local machine will have a very different allocation of CPU/memory/etc. than a cloud VPC.

If you're running a Selenium test, be aware of our limitations on using Selenium for load testing.

Calibrating a JMeter Test

You may find that when you run a JMeter test on Blazemeter with a low number of threads (users), the test will execute successfully, but 

once you begin scaling up to a higher load, the test may fail or return unexpected errors. This is often a sign that your test is in need of calibration, which must be performed to ensure a test will reliably run at higher loads.

With the exception of small, low-scale tests, all tests should be calibrated properly so as to prevent overloading engine CPU or memory.

This article will show you the steps you should take to properly run the test calibration process.

Using a Taurus YAML with your test?  If so, then please follow our Calibrating a Taurus Test guide instead.

Quick Overview of the Steps

- Create Your Script
- Test Locally with JMeter
- Run a Debug Test
- Determine Users per Engine
- Configure Your Full Load Test
- Use a Multi-Test for Multiple Scenarios

Step 1: Create Your Script

There are two ways to create your script:

Use the Blazemeter Proxy Recorder or Blazemeter Chrome Extension to record your script.

Go manually all-the-way and construct everything from scratch. This is more common for functional/QA tests.

If you generate a JMeter script from a recording, keep in mind that:

You'll need to change certain parameters, such as Username & Password. You can also set a CSV file with those values so each user can be unique.

You might need to extract elements such as Token-String, Form-Build-Id and others, by using Regular Expressions, the JSON Path Extractor, or the XPath Extractor. This will enable you to complete requests like "AddToCart", "Login" and more.

You should keep your script parameterized and use configuration elements like HTTP Requests Defaults to make your life easier when switching between environments.
 

Step 2: Test Locally with JMeter

Start debugging your script with one thread, one iteration, and using the View Results Tree element, Debug Sampler, and Dummy Sampler. 

Keep the Log Viewer open in case any JMeter errors are reported.

Go over the True and False responses of all the scenarios to make sure the script is performing as you expected.

After the script has run successfully using one thread, raise it to 0-20 threads for ten minutes and check:

Are the users coming up as unique (if this was your intention)?

Are you getting any errors?

If you're running a registration process, take a look at your backend. Are the accounts created according to your template? Are they unique?

Check test statistics under "Cumulative Stats". Do they make sense (in terms of average times, hits)?

Once your script is ready:

- Clean it up by removing any Debug/Dummy Samplers and deleting your script listeners
- If you use Listeners (such as "Save Responses to a file") or a CSV Data Set Config, make sure you don't use any paths; use only the filename (as if it was in the same folder as your script).
- If you're using your own proprietary JAR file(s), upload them.
- If your script uses more than one thread group, be aware of how Blazemeter divides users among multiple thread groups, as detailed in our explanation of Total Users.
- If your script uses special thread groups, be aware of how Blazemeter handles such thread groups.  Our guide of how Ultimate Thread Groups are handled provides these details.
 

Step 3: Run a Debug Test

Start with a Debug Test, which makes a logical copy of your test and runs it at a lower scale. The test will run with 10 threads and for a maximum of 5 minutes or 100 iterations, whichever occurs first.

This Debug configuration allows you to test your script and backend and ensure everything works well.

Here are some common issues you might come across:

- Your firewall may block Blazemeter's engines from reaching your application server.  Review our guide on Load Testing Behind Your - - - Corporate Firewall for some solutions.
- Make sure all of your test files (CSVs, JARs, JSON, user.properties, etc.) are uploaded to the test.  (Refer to our guide on Uploading - Files & Shared Folders for more details.)
- Make sure you didn't use any paths with your filenames.

WARNING!  If you do not upload all files used by your test script, or if you do not remove local paths from your file references, the test may fail to start, and may hang indefinitely while Blazemeter searches for a file location that doesn't exist.

If you're still having trouble, look at the Errors Report and Logs Report for errors.

This will allow you to get enough data to analyze the results and ensure the script was executed as you expected.

You should also check the Engine Health Report to see how much memory & CPU was used, which is key to the next step.

Lastly, keep an eye out for common issues that may result in your test running locally but not on Blazemeter or your test not starting at all.

 

Step 4: Determine Users per Engine

Now that we're sure the script runs flawlessly in BlazeMeter, we need to figure out how many users we can apply to one engine.

Set your test configuration to:


Number of threads: 500

Ramp-up: 40 minutes

Duration: 50 minutes

Use 1 engine.

Run the test and monitor your test's engine via the Engine Health Report.

If your engine didn't reach either a 75% CPU utilization or 85% memory usage (one time peaks can be ignored):

- Change the number of threads to 700 and run the test again.
- Raise the number of threads until you get to 1,000 threads or 60% CPU.
- If your engine passed the 75% CPU utilization or 85% Memory usage (one time peaks can be ignored):

- Look at when your test first got to 75% and see how many users you had at this point.
- Run the test again. This time, decrease the threads per engine by 10%.
- Set the ramp-up time you want for the real test (5-15 minutes is a great start) and set the duration to 50 minutes.
- Make sure you don't go over 75% CPU or 85% memory usage throughout the test

Note: An important detail to be aware of is that the user limit set for your subscription plan is not the same as the user limit one engine can handle, which will vary from one test script to the next.  For example, if your plan allows for 1,000 users, that does not guarantee that all 1,000 users can execute on one engine.

 

Step 5: Configure Your Full Load Test

Once we know the script is working and we how many users each engine can sustain, we can finally configure the test to achieve our load testing goal.

Let’s assume these values (as an example):

One engine can have 500 users

We aim to test for 10K users

This means to achieve our goal, our test needs 20 engines (10,000 \ 500). Those 20 engines can either be all in one geographic location, or spread across multiple locations.  (Refer to our Load Distribution guide for more details.)

Note: BlazeMeter uses a variety of cloud providers (AWS, Google Cloud, and Azure) which have different types of machines and network infrastructures, so if your test runs engines from more than one cloud provider, it's recommended to ensure that each provider's engine can sustain the load and bring about the desired outcome.

 

Step 6: Use a Multi-Test for Multiple Scenarios

This step is optional and only applies if your test includes multiple scenarios, in which case you should setup your test as a Multi-Test. (For a detailed walkthrough, refer to our guide on Multi-Tests.)

# Create a new Multi-Test.

Screen_Shot_2019-04-24_at_5.47.23_PM.png

- Add each scenario (single test).
- You can change the configuration of each scenario (as detailed in the Modify the Scenarios section of our Multi-Test guide).
- Click "Run Test" to launch all of your scenarios (additional options are covered in the Run the Multi-Test section of our Multi-Test guide).

Screen_Shot_2019-04-24_at_5.50.19_PM.png

The aggregated report of your Multi Test will start generating results in a few minutes, which also can be filtered for each individual scenario. (For details on how to use these filters, refer to our Reporting Selectors for Scenario and Location guide.)

# My Blazemeter Test Returned a HTTP 500 Code
When executing your JMeter test script via Blazemeter, you may find that the test encounters a number of HTTP 500 errors in response to some of your samplers, which will appear under the Errors tab of your report.

Screen_Shot_2019-01-21_at_11.33.24_AM.png

HTTP 500 literally means "internal server error". This is a generic code that requires investigating in your application server itself to troubleshoot. On the Blazemeter side, your server does not send us any additional information by which to know why it sent a 500 code.

This error code usually does not point to a problem with Blazemeter, but to a scripting problem. We recommend investigating this problem two ways:

Ensure your script can run in your local JMeter environment prior to running through Blazemeter.
Investigate your application server, starting with your application server's logs, to see why your server chose to return 500 codes during the timeframe of the test.
