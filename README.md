# Docker Volume Demo 
# Objective

Demonstrate Docker volume creation, container deletion, and volume reuse using Windows Command Prompt.
# Project Overview:
This project demonstrates how to:
* Create a Docker volume
* Use the volume with an Apache container
* Persist website data even after deleting containers
* Reuse the same volume in another container
* Verify data persistence using a browser

# Project Directory Structure
docker-volume-demo/ \
│
├── website/ \
│   └── index.html \
└── README.md 
# Steps Performed
# Step 1: Create project folders
```bash
cd %USERPROFILE%\Documents 
mkdir docker-volume-demo 
cd docker-volume-demo 
mkdir website 
cd website 
```
# Step 2: Create a sample website file
```bash
echo Hello from Docker Volume > index.html 
dir
```
# Step 3: Create a Docker volume
```bash
docker volume create web_volume \
```
Verify volume: 
```bash
docker volume ls 
```
Inspect volume:
```bash
docker volume inspect web_volume
```
# Step 4: Copy website data into the volume
Run this command from docker-volume-demo folder: 
```bash
docker run --rm ^ 
-v web_volume:/usr/local/apache2/htdocs ^ 
-v "%cd%\website":/temp ^ 
busybox sh -c "cp -r /temp/* /usr/local/apache2/htdocs/"
```
This copies index.html into the Docker volume
# Step 5: Run Apache container using the volume
```bash
docker run -d ^ 
--name apache1 ^ 
-p 8080:80 ^ 
-v web_volume:/usr/local/apache2/htdocs ^ 
httpd:latest 
```
Verify container: 
```bash
docker ps
```
# Step 6: Verify in browser
```bash
http://localhost:8080
```
# Step 7: Stop and delete the container
```bash
docker stop apache1 
docker rm apache1
```
# Step 8: Reuse the SAME volume in another container
```bash
docker run -d ^ 
--name apache2 ^ 
-p 9090:80 ^ 
-v web_volume:/usr/local/apache2/htdocs ^ 
httpd:latest
```
Verify: 
```bash
docker ps
``` 
# Step 9: Final verification (data persistence)
```bash
http://localhost:9090
```

# cleanup
Remove containers: 
```bash
ocker stop apache2 
docker rm apache2
```
Remove volume: 
```bash
docker volume rm web_volume
```



















