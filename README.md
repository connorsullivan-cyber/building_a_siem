<h1>Building a SIEM using Elastic</h1>

 

<h2>Description</h2>
In this project I will be creating a customized Security Information and Event Management (SIEM) platform using Elastic. A SIEM is a tool that helps security teams recognize potential security vulnerabilities and threats. There are many existing SIEM tools in existence, but with Elastic you can create and customize your own, creating flexibility with what data you would like to track. I will be connecting to the SIEM using my Linux virtual machine in VMWare Workstation. Before getting started, we will need to create an account with Elastic which can be done so <a href="https://cloud.elastic.co/home">here</a>.
<br />

<h2>Utilities Used</h2>

- <b>VMWare Workstation</b> 
- <b>Elastic</b>

<h2>Environments Used </h2>

- <b>Kali Linux</b>


<h2>Program walk-through:</h2>


<b>Configure the SIEM:</b> <br/>

<img src="https://i.imgur.com/qpaM84X.jpg" height="80%" width="80%" alt="install curl"/>
<br />
The first thing we need to do once we create the Elastic account is click "Create Deployment" and select "Elasticsearch" as the type. Now is a good time to launch the Linux machine while we wait for that to configure. Once it is completed, we will navigate to "Intergrations" by clicking on the menu bar in the top left of the screen and clicking "Add Intergrations" at the bottom. From there, we will select "Elastic Defend" and install using the provided curl command. </b><br>
<br />
<img src="https://i.imgur.com/zJHHoGb.jpg" height="80%" width="80%" alt="systemctl"/> <br/>
I verified that the agent was successfully installed by running the following command: <br/>
<b>sudo systemctl status elastic-agent.service</b> 
<br><br>
<b>Creating a security event:</b>  <br>
<img src="https://i.imgur.com/eH5pogy.jpg" height="80%" width="80%" alt="creating security event"/> <br/><br/>
Now that we have everything configured, it's time to start generating some events to test our SIEM tool. One of the asiest ways to do this is by running a nmap scan. Nmap is network scanning tool that will allow us to scan the virtual machine, creating an event in the SIEM tool. I did this by running a simple nmap command: <br/>
<b>nmap -p- localhost</b><br/>
This command scans all ports (-p-) on the local host ip address. Upon completion of the scan, I brought up the logs by going to "Observability" in the menu then "Logs". To view the events that are related to nmap, I typed in the search <b>process.args: "nmap"</b>. <br/>
<img src="https://i.imgur.com/HXAMlTX.jpg" height="80%" width="80%" alt="event"/> <br/><br/>
Clicking on the results brings up the details for the entry, where we can confirm that it is the result of the nmap scan that we ran on the VM.<br/><br/>
<b>Creating a visualization dashboard:</b> <br/>
<img src="https://i.imgur.com/jYsMKh9.jpg" height="80%" width="80%" alt="creating security event"/> <br/><br/>
Now that we know the SIEM is working properly, we can create a dashboard to visually display our results. We do so by navigating in the menu bar to "Analytics" then "Dashboard". Click "Create Visualization" and from there we can select what type of chart. I selected a vertical bar chart. There are a lot of different options here depending on what you want to display and track, but I selected "Count" as the vertical axis and "timestamp" as the horizontal axis to track activity over time. Since there wasn't a lot of activity coming from the machine, I ran a couple of simple commands to generate more data for the graph.<br/><br/>
<b>Creating an alert:</b>  <br/>
The final step of the process for the SIEM tool is setting up an alert. Alerts are important for incident response and can be fine tuned depending on the organization's needs. Usually alerts are a little bit more detailed so they don't overwhelm the system, but for the purpose of the lab, I will be creating an alert that triggers every time an nmap scan is run.<br/>
<img src="https://i.imgur.com/C5QIVjn.jpg" height="80%" width="80%" alt="rule"/> <br/><br/>
To create an alert, click on the menu bar then navigate to "Security" followed by "Alerts". From here click on "Manage rules" then "Create new rule". In the "Define rule" area, select "custom query". This is where we will set the conditions for the nmap scan rule. To do so we will type <b>event.action: "nmap_scan"</b>. This will trigger for all events with the action "nmap_scan". From here, all we have to do is name the rule, give it a description and select the method for notification. I selected email.<br/>
<img src="https://i.imgur.com/eV8AfYH.jpeg" height="80%" width="80%" alt="alert"/> <br/><br/><br/>
Now that there is a rule in place, we have made it easier to monitor the system for nmap scans. <br/>
In conclusion, this is a very basic setup but the Elastic tool provides excellent flexibility. We created a custom SIEM tool, sent the data from our virtual machine to our SIEM, generated events and analyzed the logs in the SIEM. This is a great starting off point for creating comprehensive and customizable SIEM tools that enhance security. 
