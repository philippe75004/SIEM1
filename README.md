# SIEM1

*See the below scenario and configuration made with SPLUNK Enterprise solution.
For this demonstartion, we used a Splunker Docker hoster in my Ubuntu 18.04 Virtual Machine.*

## Unit 18 Homework: Lets go Splunking!

### Scenario

You have just been hired as an SOC Analyst by Vandalay Industries, an importing and exporting company.
 
- Vandalay Industries uses Splunk for their security monitoring and have been experiencing a variety of security issues against their online systems over the past few months. 
 
- You are tasked with developing searches, custom reports and alerts to monitor Vandalay's security environment in order to protect them from future attacks.


### System Requirements 

You will be using the Splunk app located in the Ubuntu VM.


### Your Objective 

Utilize your Splunk skills to design a powerful monitoring solution to protect Vandaly from security attacks.

After you complete the assignment you are asked to provide the following:

- Screen shots where indicated.
- Custom report results where indicated.

### Topics Covered in This Assignment

- Researching and adding new apps
- Installing new apps
- Uploading files
- Splunk searching
- Using fields
- Custom reports
- Custom alerts

Let's get started!

---

## Vandalay Industries Monitoring Activity Instructions


### Step 1: The Need for Speed 

**Background**: As the worldwide leader of importing and exporting, Vandalay Industries has been the target of many adversaries attempting to disrupt their online business. Recently, Vandaly has been experiencing DDOS attacks against their web servers.

Not only were web servers taken offline by a DDOS attack, but upload and download speed were also significantly impacted after the outage. Your networking team provided results of a network speed run around the time of the latest DDOS attack.

**Task:** Create a report to determine the impact that the DDOS attack had on download and upload speed. Additionally, create an additional field to calculate the ratio of the upload speed to the download speed.


1.  Upload the following file of the system speeds around the time of the attack.

*As you can see below, we first uploaded the file then start searching:*
    
![Speed Test File uploaded](/Screenshots/Splunk-Homeworks-1.PNG "Speed Test File uploaded")
![search1](/Screenshots/Splunk-Homeworks-2.PNG "search1")
![search2](/Screenshots/Splunk-Homeworks-3.PNG "search2")

2. Using the `eval` command, create a field called `ratio` that shows the ratio between the upload and download speeds.

*Thanks to the Eval command we are able to see quickly the bandwith throuput used and the table command to format the result*
   
![eval1](/Screenshots/Splunk-Homeworks-44.PNG "eval1")

*We can save the same search as a dashboard in order to make it clearly visible for other team to see the result, see below Dashboard:*

![eval2](/Screenshots/Splunk-Homeworks-5.PNG "eval2")
      
3. Create a report using the Splunk's `table` command to display the following fields in a statistics report:
    - `_time`
    - `IP_ADDRESS`
    - `DOWNLOAD_MEGABITS`
    - `UPLOAD_MEGABITS`
    - `ratio`

*I used this SPL query to create my report:
sourcetype="csv" sourcetype=csv source="server_speedtest.csv" | table _time IP_ADDRESS DOWNLOAD_MEGABITS UPLOAD_MEGABITS | eval ratio = 'DOWNLOAD_MEGABITS'  / 'UPLOAD_MEGABITS' | sort - count*

![report](/Screenshots/Splunk-Homeworks-report1.PNG "report")

4. Answer the following questions:

    - Based on the report created, what is the approximate date and time of the attack?

*On February 24, 3 spikes starting at 16:30 then 18:30 and the last one at 12:30, for unusal high download and upload performed on the Internet to destination in Atlanta.*

    - How long did it take your systems to recover?
    
*The Systems took an hour for each 3 spikes to recover based on the splunk search.*

*The download/upload spikes could cause the bandwith to be saturated causing a not reliable Internet connection.*

*As a prevention we can throttle / limit the bandwith usage per device via our Next Generation Firewall and Secure Web Gateway.*
 
### Step 2: Are We Vulnerable? 

**Background:**  Due to the frequency of attacks, your manager needs to be sure that sensitive customer data on their servers is not vulnerable. Since Vandalay uses Nessus vulnerability scanners, you have pulled the last 24 hours of scans to see if there are any critical vulnerabilities.

  - For more information on Nessus, read the following link: https://www.tenable.com/products/nessus

**Task:** Create a report determining how many critical vulnerabilities exist on the customer data server. Then, build an alert to notify your team if a critical vulnerability reappears on this server.

1. Upload the following file from the Nessus vulnerability scan.
   - [Nessus Scan Results](resources/nessus_logs.csv)

*One way to retrieve the files uploaded is to search for * then on the Left Menu, under SELECTED FIELDS, the Source show all the files uploaded for our demonstration.*

![upload](/Screenshots/Splunk-Homeworks-Nessus_File.PNG "upload")

2. Create a report that shows the `count` of critical vulnerabilities from the customer database server.
   - The database server IP is `10.11.36.23`.
   - The field that identifies the level of vulnerabilities is `severity`.

*We used in our search the stats command in order to count the number of events for each severity then the sort - count command to oder the result from higher to lower*

![report](/Screenshots/Splunk-Homeworks-Nessus_Search.PNG "report")
      
3. Build an alert that monitors every day to see if this server has any critical vulnerabilities. If a vulnerability exists, have an alert emailed to `soc@vandalay.com`.

![Alert1](/Screenshots/Splunk-Homeworks-Nessus_Alert2.PNG "Alert1")
![Alert2](/Screenshots/Splunk-Homeworks-Nessus_Alert3.PNG "Alert2")
![Alert3](/Screenshots/Splunk-Homeworks-Nessus_Alert4.PNG "Alert3")
![Alert4](/Screenshots/Splunk-Homeworks-Nessus_Alert5.PNG "Alert4")

*As a prevention we can Install an Host IPS on the databse server in order to protect from critical vulnerabilities.*

### Step 3: Drawing the (base)line

**Background:**  A Vandaly server is also experiencing brute force attacks into their administrator account. Management would like you to set up monitoring to notify the SOC team if a brute force attack occurs again.

**Task:** Analyze administrator logs that document a brute force attack. Then, create a baseline of the ordinary amount of administrator bad logins and determine a threshold to indicate if a brute force attack is occurring.

1. Upload the administrator login logs.

![File](/Screenshots/Splunk-Homeworks-Baseline0.PNG "File")

2. When did the brute force attack occur?

*The brute force attack occured several times On Thursday February 20, 2020 and Friday February 21, 2020 at mutiples times where we saw between 32 and 48 failed login / password attempted.*

*We used in our search the stats count command in order to calculate the number of events by date, time:*

![HighFiled1](/Screenshots/Splunk-Homeworks-Baseline2.PNG "HighFiled1")
![HighFiled2](/Screenshots/Splunk-Homeworks-Baseline2-2.PNG "HighFiled2")

      
3. Determine a baseline of normal activity and a threshold that would alert if a brute force attack is occurring.

*The baseline of normal activity will be to set a maximum of 16 failed login / password attempt and a threshold of 32 failed login / password attempt would alert of a possible brute force attack occurring.*

4. Design an alert to check the threshold every hour and email the SOC team at SOC@vandalay.com if triggered. 

![Alert1](/Screenshots/Splunk-Homeworks-Baseline-Alert-BF-1.PNG "Alert1")

*We used the Trigger Alert when the number of results is greater than 31 therefore 32 will indicate a possbile Brute Force Attack*

![Alert2](/Screenshots/Splunk-Homeworks-Baseline-Alert-BF-2.PNG "Alert2")
![Alert3](/Screenshots/Splunk-Homeworks-Baseline-Alert-BF-3.PNG "Alert3")

*As a prevention we can Install a SIEM like Splunk to detect abnormal login activities and get alerts and actions based on the results.*

*As a baseline every users accounts must be configured with a lockout account policy which if in a period of 1 minute, 3 authentifications attemps failed = account locked for 15 minutes...*

**Vive Splunk ;)**
 
