# Steps to deploy this web API to a Podman container in WSL, and to debug it

### 1. Install Podman on WSL:
`sudo apt -y install podman`
### 2. Enable connection between Podman and Visual Studio 2022: 
`podman system service --time=0 tcp:127.0.0.1:2375 &`

### 3. From WSL go to the project directory (MyWebApp, located in `/mnt/C/path-to-your-project-on-disk`) - or simply open it in VS 2022 Terminal (using WSL profile)

### 4. Use dotnet CLI to publish the app.:
`dotnet publish -r linux-x64 -o publish`

### 5. Build the image:
`podman build -t mywebapp .`

### 6. Run the container: 
`podman run -d -p 5000:8080 --name mywebapp mywebapp`

### 7. Check that the container is running: 
`podman ps`

### 8. Attach to the container from Visual Studio / Debug /  Attach to Process: 
-  In `Connection Type`, choose `Docker (Linux Container)`  
-  Click the `Fin`d button and type `tcp://localhost:2375` in `Docker host`, then click `Refresh`.
- Select the container and click `OK`.
- Select the container process (dotnet) and click `Attach`.

### 9. Add some breakpoints in the code.

### 10. Access the web API: Open your browser and go to http://localhost:5000/weatherforecast


### 11. Run the following commands to cleanup the containers and stop podman listening on port 2375 the : 
- podman ps
- podman stop <container_id>
- podman rm <container_id>
- pkill -f "podman system service"

