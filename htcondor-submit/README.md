# Condor Scitokens Container

We assume that this container is running with the hostname `schedd.client.address`

## Register the Condor submit host

Edit `docker-compose.yml` to set the `hostname` and `domainname` to be the name of the machine on which this container will run so that it is visible from the outside world.

Register the client with a scitokens server. The callback URL should be

```
https://schedd.client.address:443/return/scitokens
```

Record the values of client ID and client secret provided by the server and use them to set the build arguments below.

Copy a `hostcert.pem` and `hostkey.pem` for the container's web server into this directory.

## Build docker image

```
docker build \
  --build-arg SCITOKENS_CLIENT_ID='myproxy:oa4mp,2012:/client_id/xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx' \
  --build-arg SCITOKENS_CLIENT_SECRET='xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx' \
  --build-arg SCITOKENS_AUTHORIZATION_URL=https://scitokens.server.address:443/scitokens-server/authorize \
  --build-arg SCITOKENS_TOKEN_URL=https://scitokens.server.address:443/scitokens-server/token \
  --build-arg SCITOKENS_USER_URL=https://scitokens.server.address:443/scitokens-server/userinfo \
  --rm -t scitokens/htcondor-submit .
```

## Run docker image

```
docker-compose up --detach
```
