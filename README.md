<h2>F5 Distributed Cloud WAAP Hands-On Lab</h2>

![image](images/001welcome.png)<br>
<br>
<h3>Table of Contents</h3>
PRE-REQUISITE<br>
SECTION 1: SETTING UP THE NEW ACCOUNT<br>
SECTION 2:  ONBOARDING A NEW APPLICATION<br>
SECTION 3:  ENABLE WAAP<br>
SECTION 4:  VISIBILITY/DAY2 OPERATIONS<br>
SECTION 5:  TURNING ON API DISCOVERY & SHADOW API<br>
ACKNOWLEDGEMENT<br>
<br>
<h4>PRE-REQUISITE</h4>
•	F5 Distributed Cloud credential<br>
•	Laptop/desktop browser with internet connectivity<br>
•	Target application: http://js.witcher.sg<br>
<br>
<h4>SECTION 1: SETTING UP THE NEW ACCOUNT</h4>
This section focusses on setting up your new account<br><br>
1.1)	Email with link to change password. Click “Update Password”<br>

![image](images/002updatepassword.png)<br>

1.2)	 Provide new credential and click “Submit”<br>
![image](images/003submitpassword.png)<br>

1.3)	Click “Log In”<br>
![image](images/004clicklogin.png)<br>

1.4)	Provide tenant name “f5-xctestdrive”. Click “Next”<br>
![image](images/005tenantlogin.png)<br>

1.5)	Provide credential to login<br>
![image](images/006credentiallogin.png)<br>

1.6)	Click “Check box” and “Accept and Agreement”<br>
![image](images/007acceptagreement.png)<br>

1.7)	Click “Super User” and “Next”<br>
![image](images/008superusernext.png)<br>

1.8)	Select Advance. Click “Get Started”<br>
![image](images/009getstarted.png)<br>
 
<h4>SECTION 2:  ONBOARDING A NEW APPLICATION</h4>
This section focusses on onboarding an application and accessing it via F5 XC<br><br> 
2.1)	On the home page click “Web App & API protection”, from Common Services<br>

![image](images/010waap.png)<br> 

2.2)	Click “Manage” on the Menu on left hand side<br>
![image](images/011managewaap.png)<br> 

2.3)	Click “Load Balancers” and the click “HTTP Load Balancers”<br>
![image](images/012managelb.png)<br> 
 
2.4)	Click “Add HTTP Load Balancer”<br>
![image](images/013addlb.png)<br>

2.5)	Provide the following Metadata details<br>
Name: yourname-juiceshop [e.g., steve-juiceshop as shown in screenshot]<br>
Domain and LB Type: yourname-juiceshop.hybridcloud.ml [e.g., steve-juiceshop.hybridcloud.ml]<br>
![image](images/014addlbmetadata.png)<br>

2.6)	Load Balancer Type: “HTTPS with Custom Certificate”<br>
HTTP Redirect to HTTPS:<br>
HTTPS Port: 443<br>
Click "Configure" under TLS Parameters<br>
![image](images/A001-customssl.png)<br>
 
2.7)	Under TLS Certificates, click “Add Item”<br>
![image](images/A002-addcustomssl.png)<br>
 
2.8)	Under Certificate URL, select “PEM” and paste full chain certificate public key value provided as part of the lab guide file (obtain public key from Lab Assistant)<br>
Under Private Key, click “Configure”<br>
![image](images/A003-addpublickey.png)<br>

2.9) Under Secret Info, click “Blindfold Secret”<br>
Under Text, paste private key value provided as part of the lab guide file (obtain public key from Lab Assistant)<br>
Click “Blindfold”<br>
Click “Apply"<br>
![image](images/A004-blindfoldkey.png)<br>

2.10) Click "Apply" at end of the page<br>
![image](images/A005-blindfoldapply.png)<br>

2.11) Under TLS Certificates, click "Apply"<br>
![image](images/A006-blindfoldendapply.png)<br>

2.12) Under Origin Pools, click "Add Item"<br>
![image](images/015addorigin.png)<br>

2.13) Click "Create new Origin Pool" or "Add Item" in Origin Pool<br>
![image](images/016createorigin.png)<br>

2.14)	Provide Metadata for new origin pool<br>
Name: yourname-juiceshop-orig, e.g., steve-juiceshop-orig<br>
![image](images/017originmetadata.png)<br>
 
2.15)	Click “Add Item” under Origin Servers<br>
![image](images/018addoriginservers.png)<br>

2.16)	Select Type of Origin Server: “Public DNS Name of Origin Server”<br>
DNS Name: “js.witcher.sg”<br>
Click “Apply”<br>
![image](images/019addorigindns.png)<br>

2.17)	Enter Origin server Port value as “80”, and leave the rest default<br>
![image](images/020addoriginport.png)<br>

2.18)	Click “Continue” at the end of the page<br>
![image](images/021tlssave.png)<br>

2.19)	Click “Add Item” or “Apply”<br>
![image](images/022tlsapply.png)<br>
 
2.20)	Keep the rest as default and click “Save and Exit” at the end of the page<br>
![image](images/023tlsaveandexit.png)<br>
 
2.21)	System provisioned the load balancer with a valid custom wildcard SSL certificate. Inform Lab assistant to provision your CNAME and virtual host DNS info separately <b>BEFORE</b> proceeding to browse your domain e.g., steve-juiceshop.hybridcloud.ml<br>
![image](images/A009wildcardssl.png)<br>
 
2.22)	Application has been on boarded and can be browsed using the domain name with http to https redirection capable e.g., steve-juiceshop.hybridcloud.ml<br>
![image](images/024browsedomain.png)<br>
<br>
<h4>Section 3:  Enable WAAP</h4>
This section focusses on securing the application with WAAP<br><br>

3.1)	Test application for SQL injection vulnerability. Click “Account” and then “Login”<br>
![image](images/025logindomain.png)<br>

3.2)	Key in random Email and Password and click “Log in”, for login testing purpose<br>

3.3)	Key in the following payload for Email “admin' or 1=1 --” and dummy Password. Click “Log in”<br>
![image](images/026sqlinjectionlogin.png)<br>

3.4)	SQL injection succeeded and logged in as “admin” without any valid credential<br>
![image](images/027sqlinjectedloggedon.png)<br>

3.5)	Click “Logout”, Key in Email “' or email like "%bender%";--“ and dummy Password. Click “Log in”, check which account logged in without any valid credential?<br>
![image](images/028sqlinjectionusername.png)<br>

3.6)	To enable WAAP on the load balancer. Go to “Web Application and API Protection” -> Manage - > Load Balancers -> HTTP Load Balancer<br>
Click “…” under edited load balancer e.g., steve-juiceshop<br>
Click “Manage Configuration”<br>
![image](images/029managewaap.png)<br>

3.7)	Click “Edit Configuration”<br>
![image](images/030editlbconfiguration.png)<br>

3.8)	Click “Web Application Firewall” on the left menu<br>
![image](images/031waf.png)<br>

3.9)	Click the drop down under Web Application Firewall. Select “Enable”<br>
![image](images/032enablewaf.png)<br>

3.10)	Click “Create new App Firewall” or “Add Item”<br>
![image](images/033createwaf.png)<br>

3.11)	Provide metadata for new app firewall rule<br>
Name: yourname-blocking-waf [e.g., steve-blocking-waf]<br>
![image](images/034wafrulemetadata.png)<br>

3.12)	Select Enforcement Mode as “Blocking”<br>
![image](images/035wafblocking.png)<br>

3.13)	Leave the rest as default and click “Continue”<br>
![image](images/036wafcontinue.png)<br>

3.14)	Keep the rest as default and click “Save and Exit” at end of the page<br>
![image](images/037wafsaveandexit.png)<br>

3.15)	Run the same test as step 1 and this time, it should be blocked<br>
![image](images/038blockedsqlinjection.png)<br>
<br>
<h4>Section 4:  Visibility/Day2 Operations</h4>
This section focusses on traffic and security insights<br><br>

4.1)	Log on to the XC platform and click on “Web App and API Protection”<br>
![image](images/039waap.png)<br>

4.2)	Click on “Apps & API” on the left menu and click on “Performance”<br>
![image](images/040appsandapis.png)<br>

4.3)	The detail of traffic for all load balancers is available here. Click on the load balancer created in section 2 [E.g., steve-juiceshop]<br>
![image](images/041lbdetailoftraffic.png)<br>

4.4)	Analyze the detail on application telemetry<br>
![image](images/042analyzedetail.png)<br>

4.5)	Click on “Security” under Apps & APIs on the left side menu<br>
![image](images/043appsandapissecurity.png)<br>

4.6)	Click on the load balancer created in section 2 [E.g., steve-juiceshop]<br>
![image](images/044nagivatelb.png)<br>

4.7)	Analyze the security dashboard<br>
![image](images/045analysesecurity.png)<br>
<br>
<h4>Section 5:  Turning on API Discovery & Shadow API</h4>
This section focusses on turning on API Discovery feature<br><br>

5.1)	Log on to the XC platform and click on “Web App and API Protection”<br>
![image](images/046waap.png)<br>

5.2)	Click on Manage -> Load Balancers -> HTTP Load Balancers<br>
![image](images/047managelb.png)<br>

5.3)	For the load balancer created in section 2 e.g., steve-juiceshop, click the three dots to open menu and click “Manage Configuration”<br>
![image](images/048managelbconfiguration.png)<br>

5.4)	Click “Edit Configuration” on the top right-hand corner<br>
![image](images/049editlbconfiguration.png)<br>

5.5)	Click “API Protection” from the left menu<br>
![image](images/050enableapidiscovery.png)<br>

5.6)	Enable “API Discovery” and “Enable Learning From Redirect Traffic“<br>
![image](images/051enablelearning.png)<br>

5.7)	Keep rest as default and click “Save and Exit” at the end of the page<br>
![image](images/052wafsaveandexit.png)<br>

5.8)	Navigate the application and generate traffic (you can also use an automated scanner like ZAP to Spider the application)<br>

5.9)	Click on “APPs & APIs” and then click on “Security”<br>
![image](images/053appsandapissecurity.png)<br>

5.10)	Select the load balancer, edited in this exercise [E.g., steve-juiceshop]<br>
![image](images/054lbsecurity.png)<br>

5.11)	Select “API Endpoints” on the top menu<br>
![image](images/055apiendpoints.png)<br>

5.12)	Based on traffic, the discovered APIs will be displayed.<br>
This might take up to 2 hours to populate<br>
![image](images/056discoveredapis.png)<br>

5.13)	Click on Manage -> Load Balancers -> HTTP Load Balancers<br>
![image](images/057managelb.png)<br>

5.14)	For the load balancer created in section 2 e.g., steve-juiceshop, click the three dots to open menu and click “Manage Configuration”<br>
![image](images/058managelbconfiguration.png)<br>

5.15)	Click “Edit Configuration” on the top right-hand corner<br>
![image](images/059editlbconfiguration.png)<br>

5.16)	Click “API Protection” from the left menu<br>
![image](images/060apiprotection.png)<br>

5.17)	Select “Enable” from API Definition, click “Add Item” from Enable<br>
![image](images/061addapiprotection.png)<br>

5.18)	Enter Metadata “Name” as yourname-juiceshop-swagger e.g., steve-juiceshop-swagger, click “Upload Swagger file” from Swagger Specs<br>
![image](images/062addswaggerfile.png)<br>

5.19)	Enter Metadata “Name” as yourname-juiceshop-swagger-va e.g., steve-juiceshop-swagger-v1, click “Upload File” from Upload Swagger File<br>
![image](images/063uploadswaggerfile.png)<br>

5.20)	Select “F5_Lab_Guide_v1_juice-shop_rest-v1-swagger.json” file saved in your local drive and click “Open”, which provided as part of the Lab exercise file or can be viewed and copied [here](https://github.com/f5xc/waap/blob/main/F5_Lab_Guide_v1_juice-shop_rest-v1-swagger.json)<br>
![image](images/064swaggerfileselect.png)<br>

5.21)	Click “Continue”<br>
![image](images/065swaggerfilecontinue.png)<br>

5.22)	Click “Continue”<br>
![image](images/065swaggerfilecontinue2.png)<br>

5.23)	Keep the rest as default and click “Save and Exit” at end of the page<br>
![image](images/066swaggersaveandexit.png)<br>

5.24)	Click on “Apps & APIs” and then click on “Security”<br>
![image](images/067appsandapissecurity.png)<br>

5.25)	Select the load balancer, edited in this exercise [E.g., steve-juiceshop]<br>
![image](images/068lbsecurity.png)<br>

5.26)	Click “API Endpoints” to view detected “Shadow APIs”<br>
![image](images/069shadowapis.png)<br>
<br>
<h4>Acknowledgement</h4>
1)	Shahnawaz Backer, Senior Solutions Architect, F5, Lab Author<br>
<br>
Please provide your feedback about this Lab session at following URL, thank you!<br>
<br>
https://forms.office.com/r/tVhhNSS3e2<br>
<br>
Next labs, Bot Protection, automated (scripted) provision, and more<br>
Look forward to seeing you again at next lab.<br><br>
