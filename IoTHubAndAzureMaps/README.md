# IoT Hub and Azure Maps Demo

## Overview
This demo uses the web application of the Workplace Health and Safety Demo to show in Azure Maps the device location from a IoTHub Device telemety.
In this demo you will be able to:
- Show in the map the position of a device based on its telemetry information
- Create GeoFence drawing it in the map
- Receive an email notification when the device enters in the geofence area
- Receive an email notification when the device exits the geofence area

### Device position in the map
![Maps Device_Position](https://github.com/marcusgaspar/IoTDemos/blob/master/IoTHubAndAzureMaps/images/WebApp-Map-Screen.png)

### Device entered in the geofence area
![Maps Device_Entered_GeoFence](https://github.com/marcusgaspar/IoTDemos/blob/master/IoTHubAndAzureMaps/images/WebApp-Map-Screen-Entered-Alert.png)

### Device exited the geofence area
![Maps Device_Exited_GeoFence](https://github.com/marcusgaspar/IoTDemos/blob/master/IoTHubAndAzureMaps/images/WebApp-Map-Screen-Exited-Alert.png)

## Architecture
![Architecture](https://github.com/marcusgaspar/IoTDemos/blob/master/IoTHubAndAzureMaps/images/IoTHubAndAzureMapsArchitecture.png)

## Deployment of resources
   
### Create Azure Maps account
1. Create Azure Maps Account using Tier S1

### Create Azure IoT Hub
1. Create IoT Hub using `Free` or `Standard` Tier 
1. Use `Public endpoint (all network)` setting
1. After provisioning IoT Hub, create a IoT Hub Device from the IoT Hub menu (`Explorer` > `IoT devices` > `New`).
1. Enter the IoT Device ID (`MyIoTDevice`) to the `Device ID` input field and keep all the default settings.
1. Click the `Save` button.

### Setup Azure Maps Web App Solution
#### Requirements
Download and Install 
   - NETCore 2.2
      - https://dotnet.microsoft.com/download/dotnet-core/2.2
   - Node.js
      - https://nodejs.org/
      
#### Open Solution in Visual Studio
1. Open Visual Studio.
1. From the top menu click the `File | Open | Project/Solution`.
1. Open `azuremaps\src\AzureMapsDemo.sln`.
1. Open appsettings.json and edit the Azure Maps Key
1. Press F5 in the Visual Studio project to run the web application locally.

### Create Azure App Service Web App 
#### Create a Web App - Azure App Service
1. Use F1 Tier
1. Get Publish Profile
1. Get Web App URL

#### Deploy Web App to Azure App Service
If you would like to customize the Azure Maps solution, the source code is available under `azuremaps\src`.

To deploy your updated solution to the existing resource via Visual Studio, complete the following steps:
1. In the [Azure portal](https://portal.azure.com/) select the `Resource Group` you created earlier.
1. Select the `App Service` resource.
1. Select `Get Publish Profile` from the top navigation.
1. Open Visual Studio.
1. From the top menu click the `File | Open | Project/Solution`.
1. Open `azuremaps\src\AzureMapsDemo.sln`.
1. From the left navigation, right click on `AzureMapsDemo.Web` and click `Publish`.
1. Click `Import Profile` from the bottom left.
1. Select the publish profile you downloaded in the earlier step.
1. Wait for the deployment to be completed. 

### Create Azure Logic App - Send Device Telemetry  
1. Use the Azure Resource Manager (ARM) template to deploy the Logic App called `SendLocationToMap`. Click on the link below to start the deployment.<br>
1. After deploy it, get the Trigger endpoint URL
<a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fmarcusgaspar%2FIoTDemos%2Fmaster%2FIoTHubAndAzureMaps%2Fdeployment%2FSendLocationToMap-ARM-Template.json" target="_blank">
<img src="https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/1-CONTRIBUTION-GUIDE/images/deploytoazure.png"/>
</a><br/>


### Create Azure Event Grid - Configure Telemetry on IoTHub  
Here we will setup an event subscription for the Azure IoT Hub in order to send Device Telemety to Logic App:

1. In the [Azure portal](https://portal.azure.com/) select the `Resource Group` you created earlier.
1. Select the `IoT Hub` resource.
1. Click the `Events` option in the left menu.
1. Click the `+ Event Subscription` button in the top of the panel.
1. Enter `SendLocationToMapEvent` to the `Name` input field.
1. Leave `Event Schema` as `Event Grid Schema` 
1. Check only the `Device Telemetry` option in the `Filter to Event Types` dropdown. 
1. For the `Endpoint Type` select the `Web Hook` option.
1. Click the `Select an endpoint` link.
1. In the new panel update the `Subscriber Endpoint` field with the value URL from the deployment output of Logic App: `SendLocationToMap - Trigger endpoint URL`.
1. Click the `Confirm Selection` button.
1. Click the `Create` button.

### Create Azure Logic App - Create GeoFence Alerts
1. Use the Azure Resource Manager (ARM) template to deploy 2 Logic Apps called: `GeoFenceEnterAlert` and `GeoFenceExitAlert`. Click on the link below to start the deployment.<br>
1. After deploy it, get the Trigger endpoint URL of each one.
<a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fmarcusgaspar%2FIoTDemos%2Fmaster%2FIoTHubAndAzureMaps%2Fdeployment%2FGeoFenceEvents-ARM.json" target="_blank">
<img src="https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/1-CONTRIBUTION-GUIDE/images/deploytoazure.png"/>
</a><br/>

### Setup Azure Maps Account - Configure GeoFence Event Grid on Azure Maps
Here we will setup 2 event subscriptions for the Azure Maps account in order to notify the geofence events to our Logic App.

#### Geofence Entered - Event Subscription
1. In the [Azure portal](https://portal.azure.com/) select the `Resource Group` you created earlier.
1. Select the `Azure Maps Account` resource.
1. Click the `Events` option in the left menu.
1. Click the `+ Event Subscription` button in the top of the panel.
1. Enter `GeoFenceEnterEvent` to the `Name` input field.
1. Leave `Event Schema` as `Event Grid Schema` 
1. Check only the `Geofence Entered` option in the `Filter to Event Types` dropdown. 
1. For the `Endpoint Type` select the `Web Hook` option.
1. Click the `Select an endpoint` link.
1. In the new panel update the `Subscriber Endpoint` field with the value URL from the deployment output of Logic App: `GeoFenceEnterAlert - Trigger endpoint URL`.
1. Click the `Confirm Selection` button.
1. Click the `Create` button.

#### Geofence Exited - Event Subscription
1. In the [Azure portal](https://portal.azure.com/) select the `Resource Group` you created earlier.
1. Select the `Azure Maps Account` resource.
1. Click the `Events` option in the left menu.
1. Click the `+ Event Subscription` button in the top of the panel.
1. Enter `GeoFenceExitEvent` to the `Name` input field.
1. Leave `Event Schema` as `Event Grid Schema` 
1. Check only the `Geofence Exited` option in the `Filter to Event Types` dropdown. 
1. For the `Endpoint Type` select the `Web Hook` option.
1. Click the `Select an endpoint` link.
1. In the new panel update the `Subscriber Endpoint` field with the value URL from the deployment output of Logic App: `GeoFenceExitAlert - Trigger endpoint URL`.
1. Click the `Confirm Selection` button.
1. Click the `Create` button.

## Testing the Solution

#### Simulating the Device Telemetry
To simulate the Device Telemetry, please use the `QuickStarts` project from the link:
- https://github.com/marcusgaspar/azure-iot-samples-csharp

##### Open Solution in Visual Studio
1. Open Visual Studio.
1. From the top menu click the `File | Open | Project/Solution`.
1. Open `azure-iot-samples-csharp\iot-hub\Quickstarts\Quickstarts.sln`.
1. Open `SimulatedDevice.cs` from the `simulated-device` project.
1. Configure the `s_connectionString` variable, following the instructions on the comments.
1. Press F5 in the Visual Studio project to run the console application locally.
1. Once you run it, you should start to see the device in the Map of the Azure Maps Web App created previously. 

