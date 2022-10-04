
# Manage Chaos Teams

## Objective 

- Invite & manage users on the chaos project with dedicated roles to participate in the chaos experimentation efforts

## Before You Begin

- Ensure availability of admin project. Refer to the installation guide to [install HCE](https://ksatchit.github.io/hce-docs-staging/user-guide/installation/kubernetes/placeholder/) and create the default admin project

## Steps

#### Step-1: As Admin, Add User(s) From The Settings Page

- Click on the **Create New User** button on the **User Management** tab 

  <img width="1692" alt="Screenshot 2022-10-04 at 10 09 52 AM" src="https://user-images.githubusercontent.com/21166217/193735647-83032e2a-c95e-4e05-b3df-2fd2b34a3a41.png">

- Fill in the user's personal (name, email, username) and login (username, password) details 

  **Note**: The email info is essentially metadata at this point. Please note that no invite emails are sent from the platform upon addition of users. The username provisioned in this step needs to shared with the respective user persona. 

  <img width="1681" alt="Screenshot 2022-10-04 at 10 16 40 AM" src="https://user-images.githubusercontent.com/21166217/193736476-79d52d28-2076-465c-94cd-b791a4515a23.png">

- Click on the **Create** button to provision the user

  <img width="1384" alt="Screenshot 2022-10-04 at 10 19 35 AM" src="https://user-images.githubusercontent.com/21166217/193736793-49cacac4-f721-4629-9c39-10bef9475102.png">


#### Step-2: Invite User(s) To The Project To Create Your Chaos Team

- From the Teams tab, click on the Invite New Member to add the (newly created) user into the Admin project's team

  <img width="1658" alt="Screenshot 2022-10-04 at 10 23 29 AM" src="https://user-images.githubusercontent.com/21166217/193737303-9a2b3847-0818-4fa2-86bc-4b09086a7ea1.png">

- Invite the user(s) into the project by selecting the checkbox against the respective usernames. Use the drop-down against the usernames to select the appropriate **role** you would like to associate with the user. 

  **Note**: By default, the HCE platform provides two inbuilt roles for users invited into projects: **Editor** & **Viewer**. The editor will be capable
  of carrying out standard CRUD (create, read, update, delete) operations against project resources such as ChaosHubs, Chaos Delegates & Chaos Scenarios.     The viewer has read-only permissions for the said resources, apart from permissions to generate/download reports. 

  <img width="620" alt="Screenshot 2022-10-04 at 10 24 16 AM" src="https://user-images.githubusercontent.com/21166217/193737454-1abce185-08f4-4b79-b55b-2480416ee69e.png">
  
  <img width="501" alt="Screenshot 2022-10-04 at 10 33 37 AM" src="https://user-images.githubusercontent.com/21166217/193738474-170e4e7b-ded0-4740-8b78-d6aff4c49565.png">

  
#### Step-3: Acceptance of Invite By User(s) 

- The user is required to accept the invite to obtain access to the project (in this case, the Admin project). To achieve this, the user 
  is expected to first login to the HCE platform using the credentials shared by the admin.
  
- Scroll to the lower haf of the settings page and **accept** the invite

  <img width="1726" alt="Screenshot 2022-10-04 at 10 34 41 AM" src="https://user-images.githubusercontent.com/21166217/193738803-ab551d3e-bb94-46e3-9129-9d0adce0b199.png">
  

  
## Summary 



