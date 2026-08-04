---
title: "Week 1 Worklog"
date: "2026-05-22"
weight: 1
chapter: false
pre: " <b> 1.1. </b> "
---


### Week 1 Objectives:

* Connect and get acquainted with members of First Cloud Journey.
* Understand basic AWS services, how to use the console & CLI.

### Tasks to be carried out this week:
| Day | Task                                                                                                                                                                                                   | Start Date | Completion Date | Reference Material                        |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ---------- | --------------- | ----------------------------------------- |
| 1   | - Meeting AWS members and admins <br> - Joined an AWS event <br> - Found teammates and formed a group project | 22/05/2026 | 22/05/2026 | |
| 2   | - Module 01 <br>&emsp; + What is Cloud Computing, and why businesses are moving to the cloud <br>&emsp; + What makes AWS different from other providers <br>&emsp; + How to start your journey to the cloud the right way <br>&emsp; + AWS Global Infrastructure (Region, Availability Zone, Edge Location) <br>&emsp; + AWS service management tools (Console, CLI, SDK) <br>&emsp; + Getting familiar with the current AWS Free Tier program to optimize learning costs <br>&emsp; + Additional practice and research | 25/05/2026 | 25/05/2026 | <https://000001.awsstudygroup.com/> |
| 3   | - Created a new AWS account <br> - Enabled MFA on the Root account to reduce the risk of losing control <br> - Created a separate Admin Group and Admin User, avoiding daily use of the Root account <br> - Explored account authentication support channels <br> - Explored and configured the AWS Management Console <br> - Noted a few tips to avoid unexpected charges, and created a test Support Case to get familiar with the process | 26/05/2026 | 26/05/2026 | <https://000001.awsstudygroup.com/> |
| 4   | - Learned about AWS Budgets, the service for tracking and alerting on account costs <br>&emsp; + Cost Budget: alerts when total cost exceeds a threshold <br>&emsp; + Usage Budget: alerts based on usage of a selected service (e.g. EC2 running hours) <br>&emsp; + RI Budget: tracks Reserved Instance usage <br>&emsp; + Savings Plans Budget: tracks long-term usage commitments (more flexible than RI) <br>&emsp; + Practiced the 5-step process to set up a budget: define goals, analyze current costs, set alert thresholds, configure notifications, monitor and adjust periodically <br>&emsp; + Cleaned up test budgets | 27/05/2026 | 27/05/2026 | <https://000007.awsstudygroup.com/> |
| 5   | - Learned about AWS Support Plans and the differences between them <br> - Accessed AWS Support <br>&emsp; + Types of support requests <br>&emsp; + Changing the support plan as system needs change <br> - Practiced creating and managing a Support Request <br>&emsp; + Created a sample Support Case <br>&emsp; + Selected the right severity level for each type of issue | 28/05/2026 | 28/05/2026 | <https://000009.awsstudygroup.com/> |
| 6   | - Module 02: Amazon VPC and AWS Site-to-Site VPN <br>&emsp; + Covered the fundamentals: Subnet, Route Table, Internet Gateway, NAT Gateway <br>&emsp; + Compared Security Group vs. Network ACL, and got familiar with the VPC Resource Map <br>&emsp; + Built a VPC from scratch: created the VPC, Subnet, Internet Gateway, Route Table, Security Group, and enabled VPC Flow Logs <br>&emsp; + Deployed an EC2 instance inside the new VPC and tested connectivity <br>&emsp; + Set up a Site-to-Site VPN: created the Virtual Private Gateway, Customer Gateway, VPN Connection, and modified the VPN Tunnel <br>&emsp; + Explored alternative VPN configurations and basic troubleshooting steps | 29/05/2026 | 29/05/2026 | <https://000003.awsstudygroup.com/> |

### 🏆 **Week 1 Achievements**

**1. Networking & Collaboration**

* Met **AWS members and administrators**, and got familiar with the FCJ team's way of working
* Joined my first **AWS event** in the program
* Found teammates and formed a **group project**

**2. AWS Cloud Fundamentals**

* Understood the essence of **Cloud Computing** and AWS's **Global Infrastructure** (Region/AZ/Edge Location)
* Got familiar with the **AWS Free Tier** program to keep learning costs under control
* **Created and secured** a new AWS account with **MFA**, separating **Admin Group/User** from the Root account
* Practiced the workflow for **creating and managing a Support Case** on the AWS Console

**3. Cost Management & Technical Support**

* Distinguished and practiced creating all 4 types of **AWS Budgets** (Cost, Usage, RI, Savings Plans)
* Learned the 5-step process for setting up an effective budget
* Understood the **severity level** mechanism when creating an AWS Support Case

**4. Networking Foundation with Amazon VPC**

* Successfully built a complete **VPC** (Subnet, Route Table, Internet Gateway, Security Group) and enabled **VPC Flow Logs**
* Clearly distinguished between **Security Group** and **Network ACL** for traffic control
* Deployed an **EC2 instance** and set up a **Site-to-Site VPN** connecting on-premises to AWS

### Week 1 Conclusion

The first week was mainly about getting oriented: meeting the people, learning FCJ's way of working, and covering the most foundational AWS concepts. The most useful part was pairing Budgets and Support early on — it built the habit of tracking costs and knowing which support channel to use right as the hands-on work moved into heavier VPC labs. The VPC and Site-to-Site VPN section was still at an introductory level, but it was enough to picture the basic networking flow, which will be a required foundation for most of the labs ahead.
