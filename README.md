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


### for test the get Controller

- first use the above curl to get the token
- then use Bearer and token 
 ```curl
curl http://localhost:8081/api/v1/demo \
    -H 'Content-Type: application/x-www-form-urlencoded' \
    -H 'Authorization: Bearer eyJhbGciOiJSUzI1NiIsInR5cCIgOiAiSldUIiwia2lkIiA6ICJsQ0V2NU42RE5QV01JWXFrQXJvek1mNXA3UHJESjhGUlVuLWt6d29DalMwIn0.eyJleHAiOjE3MDA2NTE4MjUsImlhdCI6MTcwMDY1MTUyNSwianRpIjoiYjQ2YWRjZGUtZWFkYy00ZTU5LWE5OTItMWRmYjM0ZjczM2Y2IiwiaXNzIjoiaHR0cDovL2xvY2FsaG9zdDo4MDgwL3JlYWxtcy9BbGlib3UiLCJhdWQiOiJhY2NvdW50Iiwic3ViIjoiZjhmYTU5MDAtNWM3OC00YTI0LTk5YmYtMGI0MjcxMjk1ZDU4IiwidHlwIjoiQmVhcmVyIiwiYXpwIjoiYWxpYm91LXJlc3QtYXBpIiwic2Vzc2lvbl9zdGF0ZSI6ImI4NDc5ZDMyLTAxYjctNDczZC04NjliLTUzNjVmOTA3YjIxOSIsImFjciI6IjEiLCJhbGxvd2VkLW9yaWdpbnMiOlsiKiJdLCJyZWFsbV9hY2Nlc3MiOnsicm9sZXMiOlsib2ZmbGluZV9hY2Nlc3MiLCJhZG1pbiIsInVtYV9hdXRob3JpemF0aW9uIiwiZGVmYXVsdC1yb2xlcy1hbGlib3UiXX0sInJlc291cmNlX2FjY2VzcyI6eyJhbGlib3UtcmVzdC1hcGkiOnsicm9sZXMiOlsiY2xpZW50X2FkbWluIl19LCJhY2NvdW50Ijp7InJvbGVzIjpbIm1hbmFnZS1hY2NvdW50IiwibWFuYWdlLWFjY291bnQtbGlua3MiLCJ2aWV3LXByb2ZpbGUiXX19LCJzY29wZSI6InByb2ZpbGUgZW1haWwiLCJzaWQiOiJiODQ3OWQzMi0wMWI3LTQ3M2QtODY5Yi01MzY1ZjkwN2IyMTkiLCJlbWFpbF92ZXJpZmllZCI6ZmFsc2UsInByZWZlcnJlZF91c2VybmFtZSI6ImFsaWJvdSJ9.hTogQ2YsQrkojoQMDCAf7BHkfEulgbG7Uk6ZR-eDDwFVGxjLuAXFiJUIjFKkpskNOYcimDfrq9HCgB48Pc5C9ew1NgwA6OOXhwgfXA-XZgbVz-Vb4M6xLaInh8QeQeIJuXrgsq8xrYBBm7dTJ5bLUmXFr8bIu_E52pNhKiMHmz7FS6KmYmx3V2uc1Y1X3CWmqXxCYa4-yB4Cj-ehEu71PqbYU5BtEGdkUEzmwX7-ohK23AWYz7Y5AczQhJAoTjZUEzHZeDYsdxmryzzvp7gzwXE5Z2ztFHLMnZnPLXIslgOfOxFzeLJ8yACAF2cx11uLRaxNaSxJBNSZrBA5icEGHg' \
    -d 'grant_type=password&client_id=alibou-rest-api&username=alibou&password=alibou'
```