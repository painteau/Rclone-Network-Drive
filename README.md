# RcloneNetworkDrive

## Create an Unlimited Network Drive on Windows (15 minutes)

Summary :
1. Create an unlimited Drive on Gdrive (1 min)
2. Making your own client_id for a Google Drive "Rclone App" (3 min)
3. Install and config rclone on windows (3 min)
4. Install Chocolatey & Winfsp (3 min)
5. Setup your Cloud drive as a Network drive on explorer (3 min)
6.  Optionnal : Start your Network drive at Windows launch (1 min)


## Create an unlimited Drive on Gdrive (1 min)

1. First you need a gmail account. 
2. Go to [td.msgsuite.workers.dev](https://td.msgsuite.workers.dev/ "td.msgsuite.workers.dev")  or  [td.hackgence.com](https://td.hackgence.com/ "td.hackgence.com") and complete like this :

- _Shared Drive Name_: Name of your disk
- _Your Google Mail Adress_: Your Gmail adress
- Choose your server
- Fill the Turing test and click "Create"

3. You now have a Team Drive in your google account !


## Making your own client_id for a Google Drive "Rclone App" (3 min)

When you use rclone with Google drive in its default configuration you are using rclone's client_id. This is shared between all the rclone users. There is a global rate limit on the number of queries per second that each client_id can do set by Google. rclone already has a high quota and I will continue to make sure it is high enough by contacting Google.

It is **strongly recommended** to use your own client ID as the default rclone ID is heavily used. If you have multiple services running, it is recommended to use an API key for each service. The default Google quota is 10 transactions per second so it is recommended to stay under that number as if you use more than that, it will cause rclone to rate limit and make things slower.

Here is how to create your own Google Drive client ID for rclone:

1.  Log into the  [Google API Console](https://console.developers.google.com/)  with your Google account. It doesn't matter which Google account you use. (It doesn't need to be the same account as the Google Drive you want to access)
    
2.  Select a project or create a new project.
    
3.  Under "ENABLE APIS AND SERVICES" search for "Drive", and enable the "Google Drive API".
    
4.  Click "Credentials" in the left-side panel (not "Create credentials", which opens the wizard), then "Create credentials"
    
5.  If you already configured an "Oauth Consent Screen", then skip to the next step; if not, click on "CONFIGURE CONSENT SCREEN" button (near the top right corner of the right panel), then select "External" and click on "CREATE"; on the next screen, enter an "Application name" ("rclone" is OK) then click on "Save" (all other data is optional). Click again on "Credentials" on the left panel to go back to the "Credentials" screen.
    

(PS: if you are a GSuite user, you could also select "Internal" instead of "External" above, but this has not been tested/documented so far).

6.  Click on the "+ CREATE CREDENTIALS" button at the top of the screen, then select "OAuth client ID".
    
7.  Choose an application type of "Desktop app" if you using a Google account or "Other" if you using a GSuite account and click "Create". (the default name is fine)
    
8.  It will show you a client ID and client secret. *You will use these values in rclone config to add a new remote or edit an existing remote*.

9.  Now you need to activate the app, by publishing it !

Be aware that, due to the "enhanced security" recently introduced by Google, you are theoretically expected to "submit your app for verification" and then wait a few weeks(!) for their response; in practice, you can go right ahead and use the client ID and client secret with rclone, the only issue will be a very scary confirmation screen shown when you connect via your browser for rclone to be able to get its token-id (but as this only happens during the remote configuration, it's not such a big deal).


## Install and config rclone on Windows (3 min)

1. Download rclone for windows (https://rclone.org/downloads/)
2. Extract all your files to a directory named "c:\rclone"
3. Open Powershell with admin mode
4. Launch the rclone config :
>    cd c:\rclone
>     .\rclone.exe config
5. Now answer the questions :
6. Give a Simple and easy to remember Name to your Drive
> Name :
7. Select Google Drive for your provider (choice 15)
> Select Drive (15)
8. Choose "No" to advanced config but "Yes" to auto config
9. Connect to the Rclone app with your Google API Credentials 
10. Select Yes to "Team Drive"
11. Answer "Yes" to the last question if everything is correct
12. Type "q" to quit

## Install Chocolatey & Winfsp (3 min)
We will need chocolatey to install Winfsp the easiest way possible.

1. With PowerShell in Admin mode, you must ensure  [Get-ExecutionPolicy](https://go.microsoft.com/fwlink/?LinkID=135170)  is not Restricted. We suggest using  `Bypass`  to bypass the policy to get things installed or  `AllSigned`  for quite a bit more security.
    
 2.  Run  `Get-ExecutionPolicy`. 
 3. If it returns  `Restricted`, then run  `Set-ExecutionPolicy AllSigned`  or  `Set-ExecutionPolicy Bypass -Scope Process`. If not, everything is ok.
2. Now run the following command:

    Set-ExecutionPolicy Bypass -Scope Process -Force; [System.Net.ServicePointManager]::SecurityProtocol = [System.Net.ServicePointManager]::SecurityProtocol -bor 3072; iex ((New-Object System.Net.WebClient).DownloadString('https://chocolatey.org/install.ps1'))
    
3.  Wait a few seconds for the command to complete.
4. Now that Chocolatey is installed, run that command to install Winfsp

    choco install winfsp -y

## Setup your Cloud drive as a Network drive on explorer (3 min)

1. Create a CMD file named "rclone-GDrive.cmd" in "c:\rclone" with the following content : 

cd c:\rclone
rclone mount --vfs-cache-mode full THENAMEOFYOURDRIVE:/ S: --fuse-flag --VolumePrefix=\server\share

THENAMEOFYOURDRIVE is exactly own you named your Drive in step 6
Here I chose the letter S for the drive but you can change to any drive letter available.

2. Save the file and launch it WITHOUT admin rights. If you launch with admin rights, you will not be able to see the drive with other users :D
3. As long as the CMD window is open, the drive will stay availaible as a Network Drive

You can personalize your setup as you want from now...

## Start your Network drive at Windows launch  (1 min)

You can copy a link to the CMD file or copy the CMD file to 

    C:\ProgramData\Microsoft\Windows\Start Menu\Programs\StartUp

It will start automatically the program at launch
