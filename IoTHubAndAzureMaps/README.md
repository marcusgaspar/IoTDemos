# IoTHub and Azure Maps Demos

## Overview
This Demo uses the web application of the Workplace Health and Safety Demo to show in Azure Maps the device location from a IoTHub Device telemety.

## Architecture
![Architecture](https://github.com/marcusgaspar/IoTDemos/blob/TemporaryChangesForPOC/IoTHubAndAzureMaps/images/IoTHubAndAzureMapsArchitecture.png)

## Deployment of resources

### Azure Maps account

Here we will setup an event subscription for the Azure Maps account in order to notify the geofence events to our Logic App.

1. In the [Azure portal](https://portal.azure.com/) select the `Resource Group` you created earlier.
1. Select the `Azure Maps Account` resource.
1. Click the `Events` option in the left menu.
1. Click the `+ Event Subscription` button in the top of the panel.
1. Enter `logicappalerts` to the `Name` input field.
1. Leave `Event Schema` as `Event Grid Schema` 
1. Uncheck the `Geofence Result` option in the `Filter to Event Types` dropdown. Ensure that only the following 2 events are selected:
    * Geofence Entered
    * Geofence Exited
1. For the `Endpoint Type` select the `Web Hook` option.
1. Click the `Select an endpoint` link.
1. In the new panel update the `Subscriber Endpoint` field with the value from the deployment output named `geofence Alerts Logic App Endpoint`.
1. Click the `Confirm Selection` button.
1. Click the `Create` button.

### Azure Maps Solution

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

### Azure App Service Web App 


### Azure Logic App - Send Device Telemetry  

### Azure Event Grid - Configure Telemetry on IoTHub  

### Azure Logic App - Create GeoFence Alerts

#### Create GeoFence Alert: Entering Geofence area

#### Create GeoFence Alert: Exiting Geofence area

### Azure Azure Maps - Configure GeoFence Event Grid on Azure Maps








