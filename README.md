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

### so the result is : 
you first create two users 
- alibou (credential::password alibou )(ADMIN)
- hasan(credential::password hasan )(USER)

## Admin
```curl
curl -X POST http://localhost:8080/realms/Alibou/protocol/openid-connect/token \
    -H 'Content-Type: application/x-www-form-urlencoded' \
    -d 'grant_type=password&client_id=alibou-rest-api&username=alibou&password=alibou'
```

then you use access token to do (just put the token in Bearer and request to /hello-2":
```curl
curl http://localhost:8081/api/v1/demo/hello-2 \
    -H 'Authorization: Bearer eyJhbGciOiJSUzI1NiIsInR5cCIgOiAiSldUIiwia2lkIiA6ICJsQ0V2NU42RE5QV01JWXFrQXJvek1mNXA3UHJESjhGUlVuLWt6d29DalMwIn0.eyJleHAiOjE3MDA4NTc1OTksImlhdCI6MTcwMDg1NzI5OSwianRpIjoiMGUxMTBlMDQtMmZhOS00M2ZlLTg3NGUtN2QyYTBhMmZiNTkwIiwiaXNzIjoiaHR0cDovL2xvY2FsaG9zdDo4MDgwL3JlYWxtcy9BbGlib3UiLCJhdWQiOiJhY2NvdW50Iiwic3ViIjoiZjhmYTU5MDAtNWM3OC00YTI0LTk5YmYtMGI0MjcxMjk1ZDU4IiwidHlwIjoiQmVhcmVyIiwiYXpwIjoiYWxpYm91LXJlc3QtYXBpIiwic2Vzc2lvbl9zdGF0ZSI6ImY4ZjcwNjYyLTRkMzItNDBmNC1hNGNiLWJmYmE4NTY5M2IzYyIsImFjciI6IjEiLCJhbGxvd2VkLW9yaWdpbnMiOlsiKiJdLCJyZWFsbV9hY2Nlc3MiOnsicm9sZXMiOlsib2ZmbGluZV9hY2Nlc3MiLCJhZG1pbiIsInVtYV9hdXRob3JpemF0aW9uIiwiZGVmYXVsdC1yb2xlcy1hbGlib3UiXX0sInJlc291cmNlX2FjY2VzcyI6eyJhbGlib3UtcmVzdC1hcGkiOnsicm9sZXMiOlsiY2xpZW50X2FkbWluIl19LCJhY2NvdW50Ijp7InJvbGVzIjpbIm1hbmFnZS1hY2NvdW50IiwibWFuYWdlLWFjY291bnQtbGlua3MiLCJ2aWV3LXByb2ZpbGUiXX19LCJzY29wZSI6InByb2ZpbGUgZW1haWwiLCJzaWQiOiJmOGY3MDY2Mi00ZDMyLTQwZjQtYTRjYi1iZmJhODU2OTNiM2MiLCJlbWFpbF92ZXJpZmllZCI6ZmFsc2UsInByZWZlcnJlZF91c2VybmFtZSI6ImFsaWJvdSJ9.duW-M4VdVwaYjkVbY2yGnjgqUd3Im9HMTpg6B1t7PMd1jziJ_EnC5eJXtA9nsKSm3QJR6oi6QhpFEtV1kh-xL1FDARrOUlfYhTzYfjj-o_fApLDxnWKKHscRY5Hc-1hxNoI8hdWS32I9GhFmmmMANEREcPl2jD9rc-OW1IIMxzmpPTvpbaEfiU2PfBohZqwnYpk55woK_3tiJgRn2uT9VjL9opY0rEkeh9mY6V7bkPX5YzxdVlaNlvYox3jF-jP6Z5_MvMaYEmf3nVqwzCFE60NISjIyMLlPy9vibQT1o_EF-0KxnDK0N19b7xHSm2jeHvAI10IyTEsMfJQ-fQaDog'
```

## User
you first do this and get access token
```curl
curl -X POST http://localhost:8080/realms/Alibou/protocol/openid-connect/token \
    -H 'Content-Type: application/x-www-form-urlencoded' \
    -d 'grant_type=password&client_id=alibou-rest-api&username=hasan&password=hasan'
```
then you do this :
```curl
curl http://localhost:8081/api/v1/demo \
    -H 'Authorization: Bearer eyJhbGciOiJSUzI1NiIsInR5cCIgOiAiSldUIiwia2lkIiA6ICJsQ0V2NU42RE5QV01JWXFrQXJvek1mNXA3UHJESjhGUlVuLWt6d29DalMwIn0.eyJleHAiOjE3MDA4NTc3NTMsImlhdCI6MTcwMDg1NzQ1MywianRpIjoiOThhYzNlZGQtY2NlYS00MmIxLWIyYWMtNjZmNzZmMDYxNDRjIiwiaXNzIjoiaHR0cDovL2xvY2FsaG9zdDo4MDgwL3JlYWxtcy9BbGlib3UiLCJhdWQiOiJhY2NvdW50Iiwic3ViIjoiMDNhMTNmOTItNjQwMi00OTE5LTg3NzktZThiMzFjNzMzMzRmIiwidHlwIjoiQmVhcmVyIiwiYXpwIjoiYWxpYm91LXJlc3QtYXBpIiwic2Vzc2lvbl9zdGF0ZSI6IjA3OTlkYzM3LWI0M2YtNDAyMi05YzBjLTBjNzFhNWU3Y2I3YyIsImFjciI6IjEiLCJhbGxvd2VkLW9yaWdpbnMiOlsiKiJdLCJyZWFsbV9hY2Nlc3MiOnsicm9sZXMiOlsib2ZmbGluZV9hY2Nlc3MiLCJ1bWFfYXV0aG9yaXphdGlvbiIsInVzZXIiLCJkZWZhdWx0LXJvbGVzLWFsaWJvdSJdfSwicmVzb3VyY2VfYWNjZXNzIjp7ImFsaWJvdS1yZXN0LWFwaSI6eyJyb2xlcyI6WyJjbGllbnRfdXNlciJdfSwiYWNjb3VudCI6eyJyb2xlcyI6WyJtYW5hZ2UtYWNjb3VudCIsIm1hbmFnZS1hY2NvdW50LWxpbmtzIiwidmlldy1wcm9maWxlIl19fSwic2NvcGUiOiJwcm9maWxlIGVtYWlsIiwic2lkIjoiMDc5OWRjMzctYjQzZi00MDIyLTljMGMtMGM3MWE1ZTdjYjdjIiwiZW1haWxfdmVyaWZpZWQiOmZhbHNlLCJwcmVmZXJyZWRfdXNlcm5hbWUiOiJoYXNhbiJ9.WnnKDXsWxKotAElit0PkZndXBktUnTeyf-wa6Oz6XjgpZg87tWiGMoqaEs5B7eOiueVODPM7YuSp6ldjb8Cv9hzqLe8kBDxzRxkGsoD9Gvjd2KLycGAgOGWGE8aaKATLew_Uldjns1l0Z9cN9JWvh98Hjxe5VBHSSY0uLKd7s367dKrlGBNq2n8lB1MaRtYq_fsOkQTKyy2HW4P62BLDKseag4RfN5pV67CayenJQiD9eOn4tzyLuXFwqvPp-yzpwTxUyfLaIe0XFJGao1wOQehRfAiW32Z3RWw8dEP5n4LARwq_Xn37syDdWygKhxzUztQlnySK1uG5SxuFdhaEkw'
```

# the Security Config
based on latest update https://docs.spring.io/spring-security/reference/servlet/oauth2/resource-server/jwt.html