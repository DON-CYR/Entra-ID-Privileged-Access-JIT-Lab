
## Overview 

In this lab I will be demonstrating how to properly assign PIM users, resolve Privilege Access Management misconfigurations and configure Justin in Time access in Entra ID.

Today's subject: Marcus Reed

Marcus is a Tier 2 Help desk technician that handles tasks such as password resets, MFA issues, account lockouts, onboarding support and anything else related to user accounts.

One day Marcus is granted access to the User Administrator role to resolve an escalation from the Tier 1 team, but it was accidentally a permanent assignment. 

After a privileged access review, this was brought to the IAM team's attention. Marcus' account isnt compromised but if it was having permanent access can be extremely risky. 


## Objectives

- Replace the active permanent assignment with a controlled PIM eligible assignment instead.
- Reconfigure Just-In-Time controls for proper assignment
- Give Marcus temp access for another ticket incident but with proper privileged access management
- Verify configurations and removal of PAM access for Marcus


### Step 1 - Creating the over-privileged user account\

A new user is created with the following details: 

- Name: Marcus Reed 
- Job Title: Tier 2 Help Desk Technician
- Department: IT 

Marcus is then assigned with the User Administrator role and it is set as an active PERMANENT assignment to demonstrate the "error".

![1]


### Step 2 - Demonstrating the risk of incorrect role assignment

We are now going to login as Marcus in Entra ID using his newly created credentials. Due to our previous labs where we configured an MFA policy for all employees and groups, we will need to set it up using Microsoft authenticator. 

Once done we can then proceed to login and view that Marcus does indeed have his elevated permissions PERMANENTLY (not good, even for an IT admin).

![2]


### Step 3 - Remediating the access 

We are now going to go back to our admin panel and remove that elevated access that he has. 

1. Navigate to Users > Marcus Reed > Assigned roles 
2. Select Remove to remove the permanent assignment

His standing admin access is now removed as seen here: 

![3]


### Step 4 - Redistributing PIM eligibility

Since Marcus needs access to privileged rights every now and then, he will be set as Eligible instead of permanent to give him more controlled access. 

1. Navigate to ID Governance > Privileged Identity Management > Microsoft Entra Roles > Assignments > Add assignments 
2. Search for User administrator and select
3. Select Marcus Reed as the member > Next
4. Set Assignment Type to Eligible (NOT Active)
5. Select Assign

You should get confirmation from Entra ID that the role assignment was successful. 

![4]


### Step 5 - Configuring the Just-In-Time controls

As the admin Navigate to Privileged Identity Management > Microsoft Entra roles > Settings > Select User Administrator. 

Make sure that the following controls are as follows to allow for the best balance of least privilege and strict conditions: 

|PIM control|Lab configuration|
|---|---|
|Maximum activation|**1 hour**|
|Require MFA|**Yes**|
|Require justification|**Yes**|
|Require ticket information|**Yes**|
|Require approval|**Yes**|
|Assignment|**Eligible**|
Set yourself as the approver or create a new user as the IT ops manager and set them as the approver to make it more realistic but for demonstration purposes, I am setting myself (Global Admin) as the approver. 

![5]


### Step 6 - Simulating a real escalation scenario

We are now going to give Marcus a reason to need his elevated privileges to show what its like to manage PAM/PIM requests and approvals for access. 

We are going to use this example ticket (drafted with AI of course): 

**INC-2026-1048**
A high level employee is unable to complete account recovery after replacing their phone. Standard Help Desk troubleshooting has failed and the ticket has been escalated to Tier 2.

We are now going to Sign in as Marcus to act as him requesting elevated access to resolve this support ticket. 

1. As Marcus Navigate to Privileged Identity Management > My Roles > Select Activate. 
2. The Maximum request time is 1 hour and Enter the following information to submit the request: 
- Ticket number
- Reason



### Step 7 - PAM Approval

We are now going to approve Marcus's request for elevated privileges. 

Navigate to the PIM request and select "Approve"

![7]

Now return to Marcus's session and check PIM > My roles > Active Assignments. 

You should now see that he now has elevated privileges that are only active for 1 hour. 

![8]



### Step 8 - Resolve & Remove access

Once Marcus has finished completing his work. He no longer needs access anymore so its time to deactive. 

While logged in he can deactivate manually in "My roles"

OR 

We can allow the timer to expire which works either way.

![9]


### Step 9 - Review Audit Trail

Lastly, lets revisit the admin account again and investigate the audit trail to get a clear look of everything that was done and the changes that were made to the user accounts. 

This is evidence of the entire PIM/PAM lifecycle for the user account: Marcus Reed. 

Reviewing audit trails are just as important as system and sign in logs to ensure that the right access is granted and to see the key points of failure if something were to go wrong. 

![10]



## Summary 

We went through the entire lifecycle of assigning and granting privileged access to certain key users temporarily and fixed an error with providing permanent access to a user to reduce risk. 


BEFORE

Help Desk User
      ↓
Permanent User Administrator
      ↓
24/7 Standing Privilege


AFTER

Help Desk User
      ↓
PIM Eligible
      ↓
Escalated Ticket
      ↓
MFA
      ↓
Ticket + Justification
      ↓
Manager Approval
      ↓
IT User Administrator
      ↓
1 Hour
      ↓
Access Removed


[1]:https://github.com/DON-CYR/Entra-ID-Privileged-Access-JIT-Lab/blob/main/images/sc_1.png
[2]:https://github.com/DON-CYR/Entra-ID-Privileged-Access-JIT-Lab/blob/main/images/sc_2.png
[3]:https://github.com/DON-CYR/Entra-ID-Privileged-Access-JIT-Lab/blob/main/images/sc_3.png
[4]:https://github.com/DON-CYR/Entra-ID-Privileged-Access-JIT-Lab/blob/main/images/sc_4.png
[5]:https://github.com/DON-CYR/Entra-ID-Privileged-Access-JIT-Lab/blob/main/images/sc_5.png
[6]:https://github.com/DON-CYR/Entra-ID-Privileged-Access-JIT-Lab/blob/main/images/sc_6.png
[7]:https://github.com/DON-CYR/Entra-ID-Privileged-Access-JIT-Lab/blob/main/images/sc_7.png
[8]:https://github.com/DON-CYR/Entra-ID-Privileged-Access-JIT-Lab/blob/main/images/sc_7.2.png
[9]:https://github.com/DON-CYR/Entra-ID-Privileged-Access-JIT-Lab/blob/main/images/sc_8.png
[10]:https://github.com/DON-CYR/Entra-ID-Privileged-Access-JIT-Lab/blob/main/images/sc_9.png
