# Software Installation
Parts of our workshop will require setup and installation of the ATT&CK Workbench, a local copy of the ATT&CK Navigator, and a local copy of the ATT&CK website. 

**Please follow these steps carefully to ensure a successful install.**

## 1. Download and Install the Required Software
### Git
We recommend [GitHub Desktop](https://desktop.github.com/) for simplicity, but any git client will work
### NodeJS
You can download NodeJS [here](https://nodejs.org/en/download/)
### Docker Desktop
[Docker Desktop for Windows](https://docs.docker.com/desktop/windows/install/)  
[Docker Desktop for Mac](https://docs.docker.com/desktop/mac/install/)
### Python
Download Python [here](https://www.python.org/downloads/)
### Angular CLI (installed through command line)
`npm install -g @angular/cli`
### PIP (installed through command line)
Mac - `$ python -m ensurepip --upgrade`  
Windows - `py -m ensurepip --upgrade`

## 2. Required Git Repositories
All repos should be under the same parent folder. 
### ATT&CK Workbench
The ATT&CK Workbench is comprised of three Git repositories. 

#### ATT&CK Workbench Frontend
Through Github Desktop:  
```
https://github.com/center-for-threat-informed-defense/attack-workbench-frontend.git
```
Through git CLI:
```
git clone https://github.com/center-for-threat-informed-defense/attack-workbench-frontend.git
```
#### ATT&CK Workbench API
Through Github Desktop:
```
https://github.com/center-for-threat-informed-defense/attack-workbench-rest-api.git
```
Through git CLI:
```
git clone https://github.com/center-for-threat-informed-defense/attack-workbench-rest-api.git
```
#### ATT&CK Workbench Collection Manager
Through Github Desktop:
```
https://github.com/center-for-threat-informed-defense/attack-workbench-collection-manager.git
```
Through git CLI:
```
git clone https://github.com/center-for-threat-informed-defense/attack-workbench-collection-manager.git
```  
#### ATT&CK Workbench Configuration
To eliminate timeouts, replace `./attack-workbench-frontend/nginx/nginx.conf` with this updated [.conf file](nginx.conf)  
To make the Workbench database persistent, replace `./attack-workbench-frontend/docker-compose.yml` with this [updated file](docker-compose.yml)

## ATT&CK Navigator

### ATT&CK Navigator Repo
Through Github Desktop:
```
https://github.com/mitre-attack/attack-navigator.git
```
Through git CLI:
```
git clone https://github.com/mitre-attack/attack-navigator.git
```

### ATT&CK Navigator Configuration
To point Navigator at your workbench, replace `./attack-navigator/nav-app/src/assests/config.json` with this [updated file](config.json)  

## Local ATT&CK Website
### ATT&CK  Website Repo 
Through Github Desktop:
```
https://github.com/mitre-attack/attack-website.git
```
Through git CLI:
```
git clone https://github.com/mitre-attack/attack-website.git
```
#### ATT&CK  Website Configuration
To point Website at your local instance of Workbench, replace `./attack-website/modules/site_config.py` with this [updated file](site_config.py)  

# 3. Building ATT&CK Workbench
## Build docker images
**YOU MUST START THE DOCKER DAEMON BEFORE RUNNING DOCKER COMPOSE**  
You can do this by starting docker desktop.  

1. Navigate to the `./attack-workbench-frontend` directory (containing the `docker-compose.yml` file)
2. Run the command:
```
docker-compose up
```

This command will build all of the necessary Docker images and run the corresponding Docker containers.

## Access Workbench

With the docker-compose running you can access the ATT&CK Workbench application by visiting the URL `localhost` in your browser. To shut down Workbench, open a new terminal and run
```
docker-compose down
```

# 4. Building ATT&CK Navigator
## Build the server
1. Navigate to `./attack-navigator/nav-app`
2. Run `npm install`

## Serve application on local machine

1. Run `ng serve` within `./attack-navigator/nav-app`
2. Browse to `localhost:4200` in browser

# 5. Building the ATT&CK Website

## Install requirements

1. Create a virtual environment: 
    - macOS and Linux: `python3 -m venv env`
    - Windows: `py -m venv env`
2. Activate the virtual environment: 
    - macOS and Linux: `source env/bin/activate`
    - Windows: `env/Scripts/activate.bat`
3. Install requirement packages: `pip3 install -r requirements.txt`

## Build and serve the local site

1. Update the `navigator_link` field within `./attack-website/modules/site-config.py` to point to local navigator instance. This will link the website with your custom Navigator
```
navigator_link = ???http://localhost:4200/??? 
```
1. Update local ATT&CK data:   
   `python3 update-attack.py`  
   Note: `update-attack.py`, has many optional command line arguments which affect the behavior of the build. Run `python3 update-attack.py -h` for a list of arguments and an explanation of their functionality.  
2. Serve the html to `localhost:8000`: 
    1. `cd output`
    2. `python3 -m pelican.server`

**Refreshing website content** - to refresh the website based on modified ATT&CK Workbench data run the following commands from your ATT&CK Website directory:
1. Stop the pelican server by pressing `ctrl-c` in the terminal window for the running website. 
2. Update your web pages:
   `python3 update-attack.py`
3. Serve the html to `localhost:8000`: 
    1. `cd output`
    2. `python3 -m pelican.server`

