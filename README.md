# Server Setup

This is a way to setup [Portainer](https://hub.docker.com/r/portainer/portainer-ce) &amp; [Nginx Proxy Manager](https://hub.docker.com/r/jc21/nginx-proxy-manager) to manage your server.

1. Create the volumes to persist data.

   ```bash
   docker volume create proxy_data
   docker volume create proxy_letsencrypt
   docker volume create portainer_data
   ```

2. Create the network for the proxied containers.

   ```bash
   docker network create proxy
   ```

   _Reuse this in portainer later if you need to expose containers._

3. Deploy the compose file & open port 81 for npm.

   ```bash
   # Adjust the time zone in the compose file.
   # Uncomment the port mapping for port 81 in the compose file.
   docker compose up -d
   ```

4. Point your domain for the proxy to the server.

5. Setup a proxy host npm for npm to proxy it's own admin interface.

   `npm.your-domain.tld` -> `http://localhost:81`

   Request a Let's Encrypt certificate and force ssl.

6. **Close the port 81 and change your npm password.**

   ```bash
   docker compose down
   # Comment/remove the port mapping for port 81 in the compose file and recreate the containers.
   docker compose up -d
   ```

7. Setup your portainer domain.

8. Setup the proxy host for portainer.

   `portainer.your-domain.tld` -> `https://portainer:9443`

   Request a Let's Encrypt certificate and force ssl.

9. Verify force ssl is enabled for both domains, since it sometimes switches of when requesting a certificate.

## Issues

_Make sure your using IPv4 so that npm can verify domain ownership for Let's Encrypt going through the docker network._
