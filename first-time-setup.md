# First Time Setup

## Summary <a href="#bkmrk-page-title" id="bkmrk-page-title"></a>

Once you have installed Organizr, the first page that you are presented with is the Wizard page.  Here you will enter which License type you want to use along with creating the Admin's credentials.

![](<.gitbook/assets/image (53).png>)

### **Install Type**

This is where you would choose if this is for Personal Use of Business use,&#x20;

| License Type | Information                                |
| ------------ | ------------------------------------------ |
| Personal     | Everything is unlocked - nothing is hidden |
| Business     | All Media related items are hidden         |

![](<.gitbook/assets/image (54).png>)

### **Admin Info**

Here you will need to fill out the Admin's info.  If you choose the personal License and you are using Plex or Emby as a backend you should fill out the username/email/password all the same as that admin's information so you can use SSO.

![](<.gitbook/assets/image (55).png>)

### **Security**

This is where locking down the system comes into play.  There are 3 fields that are needed, 1 of them is auto-generated, which is the API field.

| Field                 | Information                                                                 |
| --------------------- | --------------------------------------------------------------------------- |
| Hash Key              | This is the salt used to hash all passwords that will be in the config file |
| Registration Password | This is the field that is needed for anyone to sign up for you Organizr     |
| API Key               | This is auto-generated.  Used to access Organizr's data                     |

![](<.gitbook/assets/image (56).png>)

### **Database**

Organizr uses SQLITE for its Database.  All that is needed is what you want to name the Database - for security reasons... and The location of where to store it - also for security reasons.  Once you have filled out the information, it's best to click the Test/Create Path button to see if everything is good to go.

| Path                | Information                                                                               |
| ------------------- | ----------------------------------------------------------------------------------------- |
| Suggested Directory | This is a new directory named `db` inside the parent directory of where Organizr located  |
| Current Directory   | This where Organizr is located                                                            |
| Parent Directory    |  This is the parent directory of where Organizr located                                   |

![](<.gitbook/assets/image (57).png>)

### **Verify**

Once you have completed all the steps, you will be taken to an overview page so you may see everything that was entered.  You may hover over the items that are sensitive so you may see that information.  Once you have verified all information, you may click Finish.  This will create the Database and import you as the Admin user.  Once completed it will refresh to the login screen.



![](<.gitbook/assets/image (58).png>)

## **Login Screen**

Now that the Database is created, you will now be automatically logged in.  If that doesn't happen, login with the credentials that you just finished creating.
