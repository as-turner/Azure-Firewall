# Azure-Firewall

<img src="https://github.com/0xbythesecond/Azure-Firewall-/blob/main/Azure%20Firewall%20Lab%20Image%20Project.png?raw=true" height="100%" width="100%" alt="Azure Firewall Project Image"/>

## Introduction
This lab or exercise aims to deploy and configure an Azure firewall for a secure network environment. Key tasks include using a template to set up the lab environment, deploying the Azure firewall as a boundary between the network and the internet, creating a default route for outbound traffic, configuring application and network rules for traffic control, setting up DNS servers for proper name resolution, and testing the firewall's effectiveness. The objective is to establish a strong and secure Azure firewall configuration for network protection.

## Objective
The objective of this lab is to deploy and configure an Azure firewall, ensuring the secure operation of the network environment. To achieve this, the following key tasks will be performed. Firstly, a template will be utilized to efficiently deploy the lab environment, ensuring all necessary resources are provisioned. Subsequently, the Azure firewall will be deployed to establish a secure boundary between the network and the internet, allowing controlled traffic flow. A default route will be created to define the next hop for outbound traffic, ensuring proper routing through the firewall. Furthermore, application rules will be configured to grant or deny specific applications or services, providing granular control over network traffic. Additionally, network rules will be set up to allow or deny traffic based on source and destination IP addresses or ranges, thereby enhancing network security. DNS servers will be configured to specify the DNS resolution mechanism for the firewall, enabling proper name resolution for network traffic. Finally, the effectiveness of the firewall will be tested to validate its functionality in filtering and controlling network traffic. By completing these tasks, the objective is to establish a robust and secure Azure firewall configuration and ensure its effectiveness in safeguarding the network environment.

#

<details> 
  
  <summary>  
    
   ### Task 1: Deploy the Lab Environment
    
  </summary>

  [Lab File JSON Template](https://github.com/MicrosoftLearning/AZ500-AzureSecurityTechnologies/blob/master/Allfiles/Labs/08/template.json)
  
  [Sign-in to the Azure portal](https://portal.azure.com/)
  
  >**Note**: Sign in to the Azure portal using an account with an Owner or Contributor role in the Azure subscription.
  
- Search for "Deploy a custom template" in the Azure portal search bar and select the option. <br />
- On the Custom deployment blade, choose "Build your own template in the editor."<br />
  <img src="https://github.com/0xbythesecond/Azure-Firewall-/blob/main/Deploy%20From%20Custom%20Template.png?raw=true" height="60%" width="60%" alt="deploy custom template"/>
- Load the template file by clicking "Load file" and selecting the [\Allfiles\Labs\08\template.json](https://github.com/MicrosoftLearning/AZ500-AzureSecurityTechnologies/blob/master/Allfiles/Labs/08/template.json) file.<br />
- Save the template and ensure the desired settings, such as subscription, resource group, and location, are configured.<br />
  <img src="https://github.com/0xbythesecond/Azure-Firewall-/blob/main/Customer%20Template%20Settings.png?raw=true" height="60%" width="60%" alt="deploy custom template setting"/>
- Review the settings and click "Review + create," then click "Create" to deploy the lab environment.<br />
  
  |Setting|Value|
   |---|---|
   |Subscription|the name of the Azure subscription you will be using in this lab|
   |Resource group|click **Create new** and type the name **AZ500LAB08**|
   |Location|**South Central US**|
  
    >**Note**: I was having trouble loading the resources for the Firewall and public IP Address and adjusted the regions for South Central US in the JSON file to resolve the error. You may have to make the adjustment for a location(region) near you. 
  
  </details>
  
  #
  
  <details>
  <summary>
    
    ### Task 2: Deploy the Azure Firewall
  
  </summary>
  
 In the Azure portal, search for "Firewalls" and select the option.
  <br />
- Click "+ Create" on the Firewalls blade. 
  <br />
- On the Create a firewall blade, specify the necessary settings, such as resource group, name, region, firewall SKU, firewall management, and choose the virtual network.
  <br />
  <img src="https://github.com/0xbythesecond/Azure-Firewall/blob/main/Create%20Firewall%20Settings.png?raw=true" height="60%" width="60%" alt="Create Firewall"/>
- Add a new public IP address (TEST-FW-PIP)  for the firewall.
  <br />
- Review the settings and click "Review + create," then click "Create" to deploy the Azure firewall.
  
   |Setting|Value|
   |---|---|
   |Resource group|**AZ500LAB08**|
   |Name|**Test-FW01**|
   |Region|**(US) South Central US**|
   |Firewall SKU|**Standard**|
   |Firewall management|**Use Firewall rules (classic) to manage this firewall**|
   |Choose a virtual network|click the **Use existing** option and, in the drop-down list, select **Test-FW-VN**|
   |Public IP address|clck **Add new** and type the name **TEST-FW-PIP** and click **OK**|
  
  </details>
  
  #
  
  <details>
  
  <summary>
   
  ### Task 3: Create a Default Route
  
  </summary>
  
Search for "Route tables" in the Azure portal and select the option.
  
- Click "+ Create" on the Route tables blade.<br />
- Specify the resource group, region, and name for the route table. <br />
  
   |Setting|Value|
   |---|---|
   |Resource group|**AZ500LAB08**|
   |Region| **South Central US**|
   |Name|**Firewall-route**|
  
  <img src="https://github.com/0xbythesecond/Azure-Firewall/blob/main/Create%20Route%20Table.png?raw=true" height="60%" width="60%" alt="create route table"/>
  
- After the route table is created, associate it with the Workload-SN subnet of the virtual network where the firewall is deployed. <br />
  
   |Setting|Value|
   |---|---|
   |Virtual network|**Test-FW-VN**|
   |Subnet|**Workload-SN**|
  
  <img src="https://github.com/0xbythesecond/Azure-Firewall/blob/main/Associate%20Firewall%20Route%20Table%20to%20Virtual%20Networ-Subnet.png?raw=true" height="90%" width="90%" alt="Associate Route Table to Subnet"/>
  
- Add a default route with the destination IP addresses set as "0.0.0.0/0" and the next hop type as "Virtual appliance" using the private IP address of the firewall. <br />
  
   |Setting|Value|
   |---|---|
   |Route name|**FW-DG**|
   |Address prefix destination|**IP Address**|
   |Destination IP addresses/CIDR ranges|**0.0.0.0/0**
   |Next hop type|**Virtual appliance**|
   |Next hop address|the private IP address of the firewall that you identified in the previous task|
  
  <img src="https://github.com/0xbythesecond/Azure-Firewall/blob/main/Configure%20Route%20Settings%20for%20Route%20Table.png?raw=true" height="90%" width="90%" alt="Add Route to Route Table"/>
  
- Save the route and proceed to the next task.
  
  </details>
  
  #
  
  <details>
  
  <summary> 
    
  ### Task 4: Configure an Application Rule
  
  </summary>
  
Navigate to the Test-FW01 firewall in the Azure portal.
  
- Access the Rules (classic) section of the firewall settings.
  <br />
- Click the Application rule collection tab and select "+ Add application rule collection."
  <br />
  <img src="https://github.com/0xbythesecond/Azure-Firewall/blob/main/Select%20to%20Add%20Application%20Rule%20Collection.png?raw=true" height="90%" width="90%" alt="Add Application Rule Collection"/>
- Specify the name, priority, and action for the application rule collection.
  <br />
  
  |Setting|Value|
   |---|---|
   |Name|**App-Coll01**|
   |Priority|**200**|
   |Action|**Allow**|
  
- Create an application rule that allows outbound access to "www.bing.com" using the specified source type, source IP, protocol ports, and target FQDNs.
  <br />
  
  |Setting|Value|
   |---|---|
   |name|**AllowGH**|
   |Source type|**IP Address**|
   |Source|**10.0.2.0/24**|
   |Protocol port|**http:80, https:443**|
   |Target FQDNS|**www.bing.com**|
  
  <img src="https://github.com/0xbythesecond/Azure-Firewall/blob/main/Add%20Application%20Rule%20Collection%20Settings.png?raw=true" height="90%" width="90%" alt="Application Rule Settings"/>
  
- Save the application rule and proceed to the next task.
  
  </details>
  
  #
  
  <details>
  
  <summary>
    
  ### Task 5: Configure a Network Rule
  
  </summary>
  
In the Test-FW01 firewall settings, go to the Network rule collection tab.
 
- Click "+ Add network rule collection" and provide the necessary details such as name, priority, and action.
  <br />
- Create a network rule that allows outbound access to two IP addresses on port 53 (DNS) using the specified source and destination types, addresses, and ports.<br />
  
  1st of Network Collection Rules
  
   |Setting|Value|
   |---|---|
   |Name|**Net-Coll01**|
   |Priority|**200**|
   |Action|**Allow**|
  
  2nd of Network Collection Rules
  
   |Setting|Value|
   |---|---|
   |Name|**AllowDNS**|
   |Protocol|**UDP**|
   |Source type|**IP address**|
   |Source Addresses|**10.0.2.0/24**|
   |Destination type|**IP address**|
   |Destination Address|**209.244.0.3,209.244.0.4**|
   |Destination Ports|**53**|
  <img src="https://github.com/0xbythesecond/Azure-Firewall/blob/main/Add%20Network%20Rule%20Collection%20Settings.png?raw=true" height="90%" width="90%" alt="Create Network Rule Collection Settings"/>
- Save the network rule and continue to the next task.
  
  >**Note**: Azure Firewall includes a built-in rule collection for infrastructure FQDNs that are allowed by default. These FQDNs are specific to the platform and can't be used for other purposes. 
  
     </details>
  
  #
  
  <details>
  
  <summary>
    
  ### Task 6: Configure Virtual Machine DNS Servers  
  
  </summary>
  
Navigate to the AZ500LAB08 resource group in the Azure portal.
  
- Select the Srv-Work virtual machine and access its Networking settings.
  <br />
- Go to the DNS servers section and select the Custom option.
  <br />
- Add the two DNS servers referenced in the network rule: 209.244.0.3 and 209.244.0.4.
  <br />
  <img src="https://github.com/0xbythesecond/Azure-Firewall/blob/main/Add%20DNS%20Server%20IP%20Address.png?raw=true" height="90%" width="90%" alt="Add Customer DNS Server IP Address"/>
- Save the changes and wait for the update to complete.
  
  >**Note**: Wait for the update to complete.

  >**Note**: Updating the DNS servers for a network interface will automatically restart the virtual machine to which that interface is attached, and if applicable, any other virtual machines in the same availability set.
  
     </details>
  
  #
  
  <details>
  
  <summary>
    
  ### Task 7: Test the Firewall
  
  </summary>
  
Access the Srv-Jump virtual machine in the AZ500LAB08 resource group.  <br />
- Connect to the Srv-Jump Azure VM via Remote Desktop using the provided credentials.  <br />
  
  <img src="https://github.com/0xbythesecond/Azure-Firewall/blob/main/Connect%20with%20RDP%20on%20Srv-Jump.png?raw=true" height="80%" width="80%" alt="RDP Connection File Download"/>
  
   |Setting|Value|
   |---|---|
   |User name|**localadmin**|
   |Password|**Pa55w.rd1234**|
  
  <img src="https://github.com/0xbythesecond/Azure-Firewall/blob/main/RDP%20Login%20Credentials.png?raw=true" height="40%" width="40%" alt="RDP Login Credentials"/>
  
  >**Note**: The following steps are performed in the Remote Desktop session to the Srv-Jump Azure VM.

  >**Note**: You will connect to the Srv-Work virtual machine. This is being done so we can test the ability to access the Bing.com website. 
  
- Within the Remote Desktop session, establish a connection to the Srv-Work virtual machine via Remote Desktop Connection. <br />
  
   ```elm
  mstsc /v:Srv-Work
  ```
  <img src="https://github.com/0xbythesecond/Azure-Firewall/blob/main/Connect%20with%20RDP%20to%20Srv-Work.png?raw=true" height="80%" width="80%" alt="RDP login from within existing VM"/>
-  When prompted to authenticate again, provide the following credentials:
  
   |Setting|Value|
   |---|---|
   |User name|**localadmin**|
   |Password|**Pa55w.rd1234**|
  
- Disable Internet Explorer Enhanced Security Configuration on Srv-Work. <br />
- Test the firewall by browsing to "https://www.bing.com" and verifying that the website loads successfully. <br />
- Also, attempt to browse "http://www.microsoft.com/" and observe the expected firewall block message. <br />
  <img src="https://github.com/0xbythesecond/Azure-Firewall/blob/main/Successful%20Connection%20to%20Bing%20and%20Failed%20to%20Microsoft%20site.png?raw=true" height="80%" width="80%" alt="Success Connection to Bing and Failed Connection to Microsoft.com"/>
  
  >**Note**: Within the browser page, you should receive a message with text resembling the following: `HTTP request from 10.0.2.4:xxxxx to microsoft.com:80. Action: Deny. No rule matched. Proceeding with default action.` This is expected, since the firewall blocks access to this website.
  
- Terminate both Remote Desktop sessions.  <br />

  >**Note**: You have successfully deployed and configured Azure Firewall, created a default route, set up the application and network rules, and tested the firewall's effectiveness. Remember to clean up any unused resources to avoid unexpected costs.

Clean up resources:
Remove the resource group "AZ500LAB08" using PowerShell in the Azure Cloud Shell by running the following command:

```powershell
Remove-AzResourceGroup -Name "AZ500LAB08" -Force -AsJob
```
<img src="https://github.com/0xbythesecond/Azure-Firewall/blob/main/Delete%20Resource%20Group.png?raw=true" height="70%" width="70%" alt="Delete Resource Group in PowerShell"/>
  
</details>

## Reflection
With a few clicks, I've created a default route and configured the application and network rules, granting VIP access to specific websites and secret doors to certain IP addresses. I've also made sure our virtual machine knows its way around the internet by setting up DNS servers. Once everything was in place, I put our firewall to the test, letting it decide which websites to allow and which ones to block. It's like having a fun but strict internet chaperone. It's been a fun and great learning experience. 






