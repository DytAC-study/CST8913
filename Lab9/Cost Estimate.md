# Cost Estimation Using Azure Pricing Calculator

simple 2-tier application:
Frontend: 1 VM (Standard B2s), 50 GB storage.
Backend: 1 VM (Standard D2s_v3), 100 GB storage, SQL Database (Basic Tier).
Networking: 200 GB outbound data transfer.

| Service Category | Service Type     | Custom Name | Region            | Description                                                  | Estimated Monthly Cost | Estimated Upfront Cost |
| :--------------- | :--------------- | ----------: | :---------------- | :----------------------------------------------------------- | ---------------------: | ---------------------: |
| Compute          | Virtual Machines |             | Canada Central    | 1 B2s (2 Cores, 4 GB RAM) x 730 Hours (Pay as you go), Linux,  (Pay as you go); 0 managed disks – S6; Internet egress, 200 GB outbound data transfer from Canada Central routed via Public Internet |                 41.872 |                      0 |
| Compute          | Virtual Machines |             | Canada Central    | 1 D2s v3 (2 vCPUs, 8 GB RAM) x 730 Hours (Pay as you go), Windows (License included), SQL Enterprise (Pay as you go); 0 managed disks – S10; Inter Region transfer type, 5 GB outbound data transfer from Canada Central to East Asia |                1243.19 |                      0 |
| Support          |                  |             | Support           | nan                                                          |                      0 |                      0 |
|                  |                  |             | Licensing Program | Microsoft Customer Agreement (MCA)                           |                        |                        |
|                  |                  |             | Billing Account   |                                                              |                        |                        |
|                  |                  |             | Billing Profile   |                                                              |                        |                        |
|                  |                  |             | Total             |                                                              |                1285.06 |                      0 |

![1-1](/screenshots/1-1.png)
