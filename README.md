# n8n on Fly.io
This repository contains a Dockerfile and a fly.toml configuration file to deploy an [n8n](https://github.com/n8n-io) server on [Fly.io](https://fly.io).

## Prerequisites
- [Fly.io account](https://fly.io/)  
- [Fly command-line tool aka Flyctl](https://fly.io/docs/getting-started/installing-flyctl/): install and set up Fly.io on your local machine as per the instructions there.  
- [Docker](https://www.docker.com/products/docker-desktop)

## Deployment Process  
### Step 1: Clone Repository  
Clone this repository to your local machine with the following command:
```
git clone https://github.com/TheFSilver/n8nonflyio.git
``` 

### Step 2: Navigate to the Repository Directory  
Move into the newly cloned repository's directory with this command:
```
cd n8nonflyio
```

### Step 3: Launch Flyctl
You can then launch Flyctl using:
```
flyctl launch
```

### Step 4: Configuration
Upon launch, you'll be prompted to copy the configuration to the new app, select Y and press Enter.  
Then, choose your desired app name and press Enter.  
Default app name is *ìloven8n*.

### Step 5: Choose your server region
After selecting an app name, you'll be asked to choose a region for deployment.  
Use the Up & Down arrows to select a region and press Enter.  

You'll then receive a *Hostname*.  
Keep a note of this as this will be your *N8N_HOST* variable at step 10.  

*Example:*  
```
Hostname: iloven8n.fly.dev
```

### Step 6: Set Up a Postgresql Database
When asked if you'd like to set up a Postgresql database, select Y, press Enter and choose the following option:  
```Development - Single node, 1x shared CPU, 256MB RAM, 1GB disk.```  

When asked if you'd like to scale single node pg to zero after one hour, select N and press Enter.  

You should see a summary similar to this:
```
Postgres cluster iloven8n-db created
Username:    postgres
Password:    owliSiATKsIszoF
Hostname:    iloven8n-db.internal
Flycast:     fdaa:2:1472:0:1::5
Proxy port:  5432
Postgres port:  5433
Connection string: postgres://postgres:owliSiATKsIszoF@iloven8n-db.flycast:5432    
```
Keep a note of the summary you'll receive as this will be used at step 10.

### Step 7: Skip Upstash Redis Database Setup
When asked if you'd like to set up an Upstash Redis database, select N and press Enter.

### Step 8: Skip Immediate Deployment
You will then be asked if you'd like to deploy now. Select N and press Enter.

### Step 9: Set Up Secrets
Set up your secrets using the `flyctl secrets set` command.  
You will need to replace each placeholders with your specific information in the following command:
```
flyctl secrets set \
  DB_POSTGRESDB_DATABASE="iloven8n" \
  DB_POSTGRESDB_HOST="iloven8n-db.internal" \
  DB_POSTGRESDB_PASSWORD="owliSiATKsIszoF" \
  N8N_ENCRYPTION_KEY="SomeRandomAndSecureEncryptionKeyOfYourChoice" \
  N8N_HOST="iloven8n.fly.dev" \
  WEBHOOK_URL="https://iloven8n.fly.dev"
```
*DB_POSTGRESDB_DATABASE* is your app name.  
*DB_POSTGRESDB_HOST* is your Database Hostname from step 6.  
*DB_POSTGRESDB_PASSWORD* is your Database Password from step 6.  
*N8N_ENCRYPTION_KEY* is up to you. ;)  
*N8N_HOST* is your app Hostname from step 5.  
*WEBHOOK_URL* is your app Hostname from step 5 preceded by "*https://*".  

### Step 10: Update the fly.toml file
Go to your [Fly.io Dashboard](https://fly.io/dashboard/)  
Click on the app which name starts by the app name you chose and ends by "*-db*".  
Go to the Volumes section on the left menu.  
Copy the value located under the id column of the Volumes table. It usually starts with "*vol_*".  

Open the fly.toml file, paste the below code at the end of the file and replace the mounts source value by the one you just copied.  

Example:  
```
[mounts]
  source = "vol_id" # Replace vol_id by your database volumn id
  destination = "/home/node/.n8n"
```

### Step 11: Deploy Application
Finally, deploy your application by running the following command:
```
flyctl deploy
```

### Step 12: Add 2GB of RAM to your server
Once the installation is complete, I highly recommend you to scale the VM memory to 2GB of RAM to avoid n8n crashing due to lack of memory:  
```
fly scale memory 2048
```
Your server will be updated accordingly.  
Then navigate to your *N8N_HOST* URL to set up the owner account.

Congrats and welcome on N8N!
