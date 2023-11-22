## docker keycloak
### using docker-compose instead of this long docker command:
```bash
docker run -p 8080:8080 -e KEYCLOAK_ADMIN=admin -e KEYCLOAK_ADMIN_PASSWORD=admin quay.io/keycloak/keycloak:22.0.5 start-dev
```

there are some other conifgs for production like using real DB can be found here :
- https://github.com/keycloak/keycloak-containers/tree/main/docker-compose-examples

there are also some other configs like this which I don't have a chance to look at them yet:
- https://medium.com/@ozbillwang/run-keycloak-locally-with-docker-compose-db9a9f2fb437

### the url for authetntication:

#### see the userDetails in keycloak :

- http://localhost:8080/realms/Alibou/.well-known/openid-configuration

#### here is the curl for authentication:

```curl
curl -X POST http://localhost:8080/realms/Alibou/protocol/openid-connect/token \
    -H 'Content-Type: application/x-www-form-urlencoded' \
    -d 'grant_type=password&client_id=alibou-rest-api&username=alibou&password=alibou'
```