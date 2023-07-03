# Telemetry Tracking Using Customer Usage Attribution (PID)

Microsoft can identify the deployments of the Azure Resource Manager templates with the deployed Azure resources. Microsoft can correlate these resources used to support the deployments. Microsoft collects this information to provide the best experiences with their products and to operate their business. The telemetry is collected through [customer usage attribution](https://docs.microsoft.com/azure/marketplace/azure-partner-customer-usage-attribution). The data is collected and governed by Microsoft's privacy policies, located at the [trust center](https://www.microsoft.com/trustcenter).

To enable or disable the telemetry via the portal experience (recommended), use the radio toggle to specify your preference.

Customer Usage Attribution Disabled:
![ESLZ ARM Template Telemetry Opt Out Toggle Control Disabled](./media/cua-portal-experience-disabled.jpg)
Customer Usage Attribution Enabled:
![ESLZ ARM Template Telemetry Opt Out Toggle Control Enabled](./media/cua-portal-experience-enabled.jpg)

Alternatively, to enable or disable this tracking via the ARM template experience, we have included a parameter called `telemetryOptOut` in order to opt out of telemetry tracking to the ESLZ/ALZ ARM Template in this repo with a simple Boolean flag. The default value `false` which **enables** the telemetry. If you would like to disable this tracking, then simply set this value to `true` and this module will not be included in deployments and **therefore disables** the telemetry tracking.

In the `eslzARM.json` file, you will see the following:

![ESLZ ARM Template parameter example](./media/cua-parameter.png)
![ESLZ ARM Template variable example](./media/cua-variable.png)
![ESLZ ARM Template resource example](./media/cua-resource.png)

If you are happy with leaving telemetry tracking enabled, no changes are required. Please do not edit the module name or value of the variable `cuaID` in any module.

## Module PID Value Mapping

The following are the unique ID's (also known as PIDs) used in each of the modules.

| Telemetry                                                                 | PID                                  |
| ------------------------------------------------------------------------- | ------------------------------------ |
| ALZ Accelerator/ESLZ ARM Deployment                                       | 35c42e79-00b3-42eb-a9ac-e542953efb3c |
| ALZ Accelerator/ESLZ ARM Deployment - Zero Trust Networking - Phase 1 | f09f64b8-5cb3-4b16-900d-6ba1df8a597e |

### What is Zero Trust Network Telemetry

In an aligned effort with the Azure Networking Product Group, we have created an additional telemetry collection point to help us see customer choosing to adopt Zero Trust Networking best practices from ALZ.

> There will be multiple phases for Zero Trust Networking in ALZ that will mean additional telemetry collection points being added over time. This will only be captured when you run a portal deployment and select to leave telemetry collection enabled.

#### ALZ Accelerator/ESLZ ARM Deployment - Zero Trust Networking - Phase 1 | Definition

The following conditions and their values must be met for the telemetry point to be triggered for collection:

- Enable DDOS Protection is `true` in Hub and Spoke or Virtual WAN topologies
- Deploy Azure Firewall is `True` in Hub and Spoke or Virtual WAN topologies
- Azure Firewall Tier is `Premium` in Hub and Spoke or Virtual WAN topologies
- Ensure subnets are associated with NSG is `true` in both Landing Zones & Identity Management Groups
- Ensure secure connections to storage accounts (https) is `true` in the Landing Zones Management Group