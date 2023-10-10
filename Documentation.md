# Purpose:

This test demonstrated how to resolve a situation where a server is overwhelmed with requests, resulting in users being unable to access the application. The increased resources provided by vertically scaling ensures that our application can handle thousands of requests while also keeping our infrastructure simple. 

# Goal:

Due to a spike in demand, the URL Shortener now needs to be able to handle a minimum of 14,000 users sending requests to the application concurrently.

 # Testing:

To ensure that the application is able to manage increased traffic, the QA engineer sent 14,000 requests to the server. Unfortunately, the server was overloaded resulting in 3 users not being able to access the application. 
<img width="1199" alt="Screen Shot 2023-10-09 at 8 09 21 PM" src="https://github.com/nalDaniels/Blitz2/assets/135375665/5b78449d-004e-4727-b41b-93bed3ae494d">


3 people may seem insignificant, but it is important to ensure our application is available to all users. Otherwise, we face losing more users and business. Luckily, this is the test environment, so we are able to fix things before it goes to production. 

Here you can see the error log describing the incidents where 3 users were unable to access the app. 
<img width="1010" alt="Screen Shot 2023-10-09 at 6 21 18 PM" src="https://github.com/nalDaniels/Blitz2/assets/135375665/4349d463-54ec-4614-8119-c18c427d2bc9">

Error message is ‘Too Many Files,’ meaning the server is unable to fulfill all the requests coming across the open connections. 

# Resolution:

Since the t2.medium was unable to handle thousands of requests, we can choose to scale it vertically. Scaling up guarantees our application has enough resources to fulfill thousands of requests at the same time. Though, since the CPU was over 90%, we should change the instance type to a t2.xlarge rather than a t2.large. The xlarge type has double the amount of CPU than our current t2.medium and four times as much memory. 

I also considered increasing the worker processes and connection of the nginx server; however, those values can only be increased so much before having to increase the resources of the EC2. 

However, it is important to note that increasing the size of the server still creates a single point of failure. Therefore, when the CPU alarm is engaged on CloudWatch, it can spin up an additional server - likely with more resources.

# Steps

1. Go to EC2 > Elastic Block Store > Volumes
2. Detach the volume associated with my in-use EC2
3. Create new EC2 called blitz2
4. Attach volume to blitz2 EC2 as root volume or /dev/sda1
5. Rerun Jenkins build
6. Reinstall CloudWatch


