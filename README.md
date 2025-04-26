<<<<<<< HEAD
# tourplaces-cluster
=======
# Tour Places Cluster Deployment

This project uses **Vagrant** and **VirtualBox** to deploy a cluster for the Tour Places React application.

## Steps:
1. Clone the frontend app into `frontend` folder. Make sure the folder contains your React app's `build` folder (the production build).
2. In the `frontend` folder, run the following commands to install dependencies and build the app:
   ```bash
   npm install
   npm run build
   ```
3. Make sure `Vagrantfile` and `lb-setup.sh` are present.
4. Verify that everything is running correctly:
   - Check the status of your VMs:
   ```bash
   vagrant status
   ```
   - Make sure the load balancer is working:
   ```bash
   curl -I http://192.168.70.100
   ```
5. Run:
   ```bash
   vagrant up
   ```

## Simulate Failure

You can simulate a failure to check the load balancing by halting one of the web servers and sending requests to the load balancer:
```bash
vagrant halt web2
for i in {1..10}; do curl http://192.168.70.100 && echo ""; sleep 1; done
```

## Add 4th Server

To add a 4th server (`web4`), follow these steps:

1. Update the `Vagrantfile` to include `web4`. Then, run:
   ```bash
   vagrant up web4
   ```

2. SSH into the load balancer:
   ```bash
   vagrant ssh loadbalancer
   ```

3. Edit the `tour-places.conf` file to include the new server:
   ```bash
   sudo sed -i '/upstream react_servers {/a server 192.168.70.104;' /etc/nginx/conf.d/tour-places.conf
   ```

4. Test the nginx configuration and reload the service:
   ```bash
   sudo nginx -t && sudo systemctl reload nginx
   ```

Enjoy!
>>>>>>> 7120c8a (Initial commit for tourplaces-cluster)
