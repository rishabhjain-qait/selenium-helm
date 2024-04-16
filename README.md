# selenium-grid
[Selenium Grid Official Documentation](https://www.selenium.dev/documentation/grid/)

#### Grid status
```
curl http://localhost:4444/status

```

#### Delete a particular session 
```
curl -X DELETE http://localhost:4444/wd/hub/session/8dad3b8dee9be007a3d10be38cc2dfd2

```

#### To Delete all sessions running on grid
```
curl -X POST -H "Content-Type: application/json" --data '{"query":"{ sessionsInfo { sessions { id, capabilities, startTime, uri, nodeId, nodeId, sessionDurationMillis } } }"}' -s 'http://localhost:4444/graphql' | jq '.data.sessionsInfo.sessions[].id' | xargs -I {} curl -XDELETE http://localhost:4444/session/{}

```

#### Start a session
```
curl -X POST \
  http://localhost:4444/session \
  -H 'Content-Type: application/json' \
  -d '{
    "capabilities": {
        "alwaysMatch": {
            "browserName": "chrome",
            "platformName": "any"
        }
    }
}'
```

#### Open google.com using curl
```
curl -X POST \
http://localhost:4444/session/<session_id>/url \
  -H 'Content-Type: application/json' \
  -d '{
    "url": "https://www.google.com"
}'
```

[Graphql Support](https://www.selenium.dev/documentation/grid/advanced_features/graphql_support/)

[Selenium Grid Important Endpoints](https://www.selenium.dev/documentation/grid/advanced_features/endpoints/#:~:text=Distributor-,Remove%20Node,it%20is%20unless%20explicitly%20killed.)
