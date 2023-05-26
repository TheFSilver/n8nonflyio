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

### Step 5: N8N_HOST Variable
After selecting an app name, you'll receive a *Hostname*.  
Keep a note of this as this will be your *N8N_HOST* variable at step 9.  

*Example:*  
```
Hostname: iloven8n.fly.dev
```

### Step 6: Set Up a Postgresql Database
When asked if you'd like to set up a Postgresql database, select Y, press Enter and choose the following option:  
```Development - Single node, 1x shared CPU, 256MB RAM, 1GB disk.```  

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
DATABASE_URL: postgres://n8nflies:qUKs6I933fQ8ORD@n8nflies-db.flycast:5432/n8nflies?sslmode=disable
```
Keep a note of the summary you'll receive as this will be used at step 9.


### Step 7: Skip Upstash Redis Database Setup
When asked if you'd like to set up an Upstash Redis database, select N and press Enter.

### Step 8: Skip Immediate Deployment
You will then be asked if you'd like to deploy now. Select N and press Enter.

### Step 9: Set Up Secrets
Set up your secrets using the `flyctl secrets set` command.  
You will need to replace each placeholders with your specific information in the following command:
```
flyctl secrets set \
  DATABASE_URL="postgres://iloven8n:4RZXzhZ9jaHGS5m@iloven8n-db.flycast:5432/iloven8n?sslmode=disable" \
  DB_POSTGRESDB_DATABASE="iloven8n" \
  DB_POSTGRESDB_HOST="iloven8n-db.internal" \
  DB_POSTGRESDB_PASSWORD="owliSiATKsIszoF" \
  N8N_ENCRYPTION_KEY="SomeRandomAndSecureEncryptionKeyOfYourChoice" \
  N8N_HOST="iloven8n.fly.dev" \
  WEBHOOK_TUNNEL_URL="https://iloven8n.fly.dev" \
  WEBHOOK_URL="https://iloven8n.fly.dev" \
  DB_SOURCE="vol_kgj5450ynn9ry2wz"
```
You can find your *DB_SOURCE* value at https://fly.io/apps/iloven8n/volumes (replace iloven8n with your actual app name).  
It's located under the name column and usually starts with "vol_".

### Step 10: Deploy Application
Finally, deploy your application by running the `flyctl deploy` command:
```
flyctl deploy
```

Now wait for the installation to complete, then navigate to your N8N_HOST URL to set your email and password.

Congrats and welcome on N8N!
