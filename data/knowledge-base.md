# Overview
This is our knowledge base it is where we store information like descriptions of our systems and also run books which tell us how to troubleshoot incidences.

# Software Architecture
Our rates flow into our systems and onto our customers UI's in the following way. 
1. There is a market update on the London Exchanges
1. Market Listener Service: We subscribe to this exchange withour Exchange Listener System in the LD4 Data Center
1. Raw Rate Listener Service: This service listens to rates that flow across the direct line.  Rates then flow from London down to across a direct line into our JHB1 Data Center. 
1. Pricing Service: The Pricing Service uses the Static Data Store Service to lookup all pricing data for our customers. It prices a pair for a subscribed consuer and pushes this pair onto the message bus.
1. FX UI: Listens on the bus via WebSockets for a a given rate. This rate then lands on their screen. 

## Our Services
### Market Listener Service
The Market Listener Service is hosted in London in the [EQUINIX, LD4 Data Center](https://www.equinix.com/data-centers/europe-colocation/united-kingdom-colocation/london-data-centers/ld4) and is owned by the FX Infrastructure Team. 

### Raw Rate Listener Service
The Raw Rate Listener Service is the [Reuters Market Data System (RMDS)](https://en.wikipedia.org/wiki/Reuters_Market_Data_System) built by the company Thomson Reuters. It is owned and managed by the FX Infrastructure Team.

### Pricing Service
The Pricing Service receives us a row right and then applies a pricing to it. It then pushes this margin rate onto the service bus so it can flow out to consumers. It is owned the FX Dev Team.

### FX UI
The FX UI is the User Inteface that our customers use to interact with our FX Systems. On the main flow here is 
1. User logs in, this requires an OTP
1. User subscribes to a rate, this goes to the FX Pricing Service
1. Pricing Service: Starts streaming margined rates to the UI via a message bus and WebSockets
1. UI: Recives a margined rate and displays it to the user

### Static Data Store Service 
The Static Data Store Service stores and services our foreign exchange and market customer pricing data. The type of data you can find in here is things like segment margin pricing, customer pricing for a given customer,  price group pricing for a group and on forward rate pricing for a given FX broken date or FX standard tenors. It is owned and managed by the FX Data Team.

# Teams and People
## FX Infrastructure Team
How to contact them? Send them an email to fxinfrateam@contoso.com
In the email you must put the folllowing information: How urgent this is, who to reach out to for more information

## FX Data Team
How to contact them? 
Cut a ServiceNow Incident against them with the following information: 
Service: "{name_of_service} {environment}
Configuration Item: {name_of_service} {environment}
Assignment Group: FX Data Team

## FX Dev Team 
How to contact them? 
Cut a ServiceNow Incident against them with the following information: 
Service: "{name_of_service} {environment}
Configuration Item: {name_of_service} {environment}
Assignment Group: FX Pricing Dev Team

## Communications Delivery Team
This team manages relationships with sending communications out acros various channels. Their main work is around interacting with Cellphone Network Providers.
How to contact them? 
Send them an email at: communicationsdeliveryteam@contoso.com

## Notification Service Team
This team owns a set of services that send communications to customers. For example: templated emails, service status updates, SMS's.
How to contact them? 
Reach out to them on their MS Teams Group "Notification Service Team Engagement"

# Runbooks

## Rates Error Mapping Runbook
This section describes how to troubleshoot issues with rates. If users are not getting ticking rates this is where you must come to troubleshoot.

### Mappings incorrect
If mapping are incorrect for a given pair/tenore then you will  not be able to subscribe to them. Users will not get ticking rates.

## FX UI Runbook
### User does not get OTP sent to them on login
We have seen this happen when the customer service provider does not forward the OTP onto the customer. If this happens it is out of our control as we are dependent on the network service providers for this to happen.new line

#### What should I do? ####
1. Reach out to the Communications Delivery Team to see if there are issues with this network provider in sending messages
    1. If there are issues then feed this information back to the customer and also try and give them a time as to when they will be able to receive these messages again
1. If there is nothing wrong that we know at the moment then To the Notification Service Team common provide them with the number that we sent used to send the message. And asked his team to do an investigation

### App crashed when users click detach
There is a bug on the UI where a user clicks on the detach button and then the application crashes. We have not fully identified what the cause of this issue is. 
However we have found this to occur in the following instances and suspect it may be related to this, these are: 
### What are some causes of this? 
1. there are windows updates pending that have not been run yet. 
1. The user has put their laptop or machine into sleep mode
1. The user has not logged out for a long time and has left the application running, this is usually over the weekend or for a few days. Enter
1. The user has installed the application twice and it has fallen over somewhere on the uninstall

#### What should I do? ####
1. First gather as much information as you can from the user so we can try and pinpoint where this issue is in the future. See the points above for the type of information we are looking for.
2. Ask them to remove and clear one browser history
3. Next ask the customer to uninstall the application.
4. Ask them to download and install the latest version.
