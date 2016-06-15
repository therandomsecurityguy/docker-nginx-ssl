# Secured Dockerized NGINX image

Based off the [NGINX](https://hub.docker.com/_/nginx/) Official Image.

## Running the container

This Dockerfile is meant to be a base config for NGINX. 

You should overwrite the /etc/nginx/external/ with a folder, containing your nginx \*.conf files, certs, and PEM key. The DH size of 2048 can take a long time to create FYI:   

    docker run -d \
    -p 80:80 -p 443:443 \
    -e 'DH_SIZE=2048’ \
    -v $EXT_DIR:/etc/nginx/external/ \
    therandomsecurityguy/docker-nginx-ssl

### Creating the PEM with openssl

To create a Diffie-Hellman cert, you can use the following command

    openssl dhparam -out dh4096.pem 2048

### Creating CSR with OpenSSL

Run the following command:

    openssl req -nodes -new -newkey rsa:2048 -out csr.pem -sha256

### Creating an SSL cert with OpenSSL

    openssl req -x509 -newkey rsa:2048 \
    -keyout key.pem -out cert.pem \
    -days 3650 -nodes -sha256

