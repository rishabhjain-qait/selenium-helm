# medium-blog-post
[Medium Blog Post](https://medium.com/@rishabhjain01/autoscalable-selenium-grid-4-86bde156947e)

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

#### Sample Graphql Query
```
curl -X POST -H "Content-Type: application/json" --data '{"query": "{ grid { maxSession, nodeCount } }"}' -s http://localhost:4444/graphql

```

#### to get all session info - status of grid4 
```
curl -X POST \
  -H "Content-Type: application/json" \
  -d '{"query":"{ sessionsInfo { sessionQueueRequests, sessions { capabilities } } }"}' \
  http://localhost:4444/graphql

```

#### to get information related to nodes - nodeInfo
```
curl -X POST \
  -H "Content-Type: application/json" \
  -d '{"query":"{ nodesInfo { nodes { maxSession, stereotypes } } }"}' \
  http://localhost:4444/graphql

```

#### to get node ids on which session is not running - node ids and pod ips map 
```
curl -X POST \
  -H "Content-Type: application/json" \
  -d '{"query":"{ nodesInfo { nodes { id uri sessionCount stereotypes } } }"}' \
  http://localhost:4444/graphql
```

[Selenium Grid Important Endpoints](https://www.selenium.dev/documentation/grid/advanced_features/endpoints/#:~:text=Distributor-,Remove%20Node,it%20is%20unless%20explicitly%20killed.)
