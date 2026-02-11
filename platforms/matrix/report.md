# Matrix Assessment Report

> [!TIP]
> Use this document to record your progress, the problems you faced and how you solved (or avoided) them.
> You can include images or add files in this directory if you want.

## Project Overview
* **Domain:** `davidchoulex.me`
* **Infrastructure:** Google Cloud Platform (GCP) Compute Engine
* **IP Address:** `35.240.202.111`

<!-- TODO: Write about what you tried and your process here -->

## Implementation Steps
* **Domain Setup**: Created a free domain from namecheap (davidchoulex.me).
* **VPS Setup**: Setup a free VM in GCP, including firewall and external IP address. Changed dynamic IP to static IP address since the DNS records of the domain need to point to a fixed IP address.
* **DNS Configuration**: Updated Namecheap records to point @ and matrix A-records to the GCP IP and configured SRV records for federation.
* **Firewall Provisioning**: Added google cloud firewall rule to accept incoming traffics from ports 80, 443, 8008, and 8448. Note that, by default GCP deny all traffic.
* **Environment Setup**: Installed all dependencies such as docker, nginx, certbot on the VM in GCP.
* **Configuration Generation**: Ran the Synapse generator to create the homeserver.yaml. Then, create docker-compose.yml file to manage the server and a database and integrated a PostgreSQL container for the database backend. 
* **Reverse Proxy & SSL**: Use Nginx as a reverse proxy and Certbot to obtain Let's Encrypt certificates, enabling HTTPS. This step is required to connect to app.element.io since it doesn't allow http.
* **Send Message**: Created an account in app.element.io and sent message to @chino:oxn.sh .

## Challenges
* **Cloudflare DNS Record Doesn't Propagate**. The namecheap domain pointed to github IP addresses on initial setup. After setting up the GCP external IP address, I tried to change the DNS record to point to that address but it keeps failing. After going back and forth checking the GCP network setting and the namecheap domain DNS record, I realized that I need to delete all the A-records pointing to the github and add the A record pointing to the VM IP address.
* **Port Mapping**. The port for docker image is not visible due to container boot loop issue. My first try is to add enable_registration to true and restart the container. But the port still doesn't show up in the `docker ps` output. I identified that the error was caused by verification issue by checking the container logs. To solve this, I added set enable_registration_without_verification to true. 
* **Cannot Register Matrix Home Server to Element Client**. The "Home server URL invalid" error persisted when I tried to create a new account with my domain in the client UI. First issue caused by the matrix home server isn't deployed properly. My domain returned 404 when I tried to access it from browser. I found out that the SYNAPSE_SERVER_NAME was wrong and fixed the issue. The second issue caused by CORS from app.element.io, which doesn't allow http IP address. This issue was solved by obtaining TLS/SSL certificate from certbot.


## Matrix Handle

<!-- TODO: Replace with your matrix handle -->
`@davidchoulex:davidchoulex.me`

Overall the project is challenging but I find it enjoyable and engaging :)