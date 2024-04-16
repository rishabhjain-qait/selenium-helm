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


# k8s api endpoint for autoscaling logic

#### to get pods list - pod ips and name mapping 
```
curl -k -X GET \
  -H "Authorization: Bearer eyJhbGciOiJSUzI1NiIsImtpZCI6IklOR1I0STV2aHU4enRmdkZZSW5xUmRhdHJhWGpYaGItWnVJZTg4VXo1NG8ifQ.eyJpc3MiOiJrdWJlcm5ldGVzL3NlcnZpY2VhY2NvdW50Iiwia3ViZXJuZXRlcy5pby9zZXJ2aWNlYWNjb3VudC9uYW1lc3BhY2UiOiJkZWZhdWx0Iiwia3ViZXJuZXRlcy5pby9zZXJ2aWNlYWNjb3VudC9zZWNyZXQubmFtZSI6ImF1dG8tc2NhbGUtcm9ib3Qtc2EiLCJrdWJlcm5ldGVzLmlvL3NlcnZpY2VhY2NvdW50L3NlcnZpY2UtYWNjb3VudC5uYW1lIjoiYXV0by1zY2FsZS1yb2JvdC1zYSIsImt1YmVybmV0ZXMuaW8vc2VydmljZWFjY291bnQvc2VydmljZS1hY2NvdW50LnVpZCI6IjNmNWIxYTk4LTRhNDYtNDIzNS05OWE2LWNjYjU5NzBmODhiMiIsInN1YiI6InN5c3RlbTpzZXJ2aWNlYWNjb3VudDpkZWZhdWx0OmF1dG8tc2NhbGUtcm9ib3Qtc2EifQ.G3xbgq7BTfyf1mogsQpmp0Amcywv5DdHm_FfyqFpB-kzkJq3lDsZHpVquHXi041ruAnf5mY0CuyFqo6SI-RqBCF5jT9d2K-vpCOtEXHqtJyev-E8dbyqYLiAoYNYmD5cnhgvkoF70cWzZiKW2u9ryPm28ELSZvEBME1vfs7RvgfLxm4REGGh8jb1NAHUNHbEoCm2nfGke0_78Vi54nNbggmSMqgTLfOYP6OByHTgrqyQXRu56y6Ir2qRvgflfhQmHqR7kOi7wH7XYkjroDmDfLmA2hNmO91-LiSavxucdr6Vj3rn1fZFGd0tZ5gDU__fpkuNSwhGNo0ByGeq0dNAlA" \
  https://kubernetes.docker.internal:6443/api/v1/namespaces/default/pods | jq .
```


#### to update cost of pod
```
curl -k -X PATCH \
  -H "Authorization: Bearer eyJhbGciOiJSUzI1NiIsImtpZCI6IklOR1I0STV2aHU4enRmdkZZSW5xUmRhdHJhWGpYaGItWnVJZTg4VXo1NG8ifQ.eyJpc3MiOiJrdWJlcm5ldGVzL3NlcnZpY2VhY2NvdW50Iiwia3ViZXJuZXRlcy5pby9zZXJ2aWNlYWNjb3VudC9uYW1lc3BhY2UiOiJkZWZhdWx0Iiwia3ViZXJuZXRlcy5pby9zZXJ2aWNlYWNjb3VudC9zZWNyZXQubmFtZSI6ImF1dG8tc2NhbGUtcm9ib3Qtc2EiLCJrdWJlcm5ldGVzLmlvL3NlcnZpY2VhY2NvdW50L3NlcnZpY2UtYWNjb3VudC5uYW1lIjoiYXV0by1zY2FsZS1yb2JvdC1zYSIsImt1YmVybmV0ZXMuaW8vc2VydmljZWFjY291bnQvc2VydmljZS1hY2NvdW50LnVpZCI6IjNmNWIxYTk4LTRhNDYtNDIzNS05OWE2LWNjYjU5NzBmODhiMiIsInN1YiI6InN5c3RlbTpzZXJ2aWNlYWNjb3VudDpkZWZhdWx0OmF1dG8tc2NhbGUtcm9ib3Qtc2EifQ.G3xbgq7BTfyf1mogsQpmp0Amcywv5DdHm_FfyqFpB-kzkJq3lDsZHpVquHXi041ruAnf5mY0CuyFqo6SI-RqBCF5jT9d2K-vpCOtEXHqtJyev-E8dbyqYLiAoYNYmD5cnhgvkoF70cWzZiKW2u9ryPm28ELSZvEBME1vfs7RvgfLxm4REGGh8jb1NAHUNHbEoCm2nfGke0_78Vi54nNbggmSMqgTLfOYP6OByHTgrqyQXRu56y6Ir2qRvgflfhQmHqR7kOi7wH7XYkjroDmDfLmA2hNmO91-LiSavxucdr6Vj3rn1fZFGd0tZ5gDU__fpkuNSwhGNo0ByGeq0dNAlA" \
  -H "Accept: application/json" \
  -H "Content-Type: application/strategic-merge-patch+json" \
  -d '{ "metadata": { "annotations": { "controller.kubernetes.io/pod-deletion-cost": "-1" } } }' \
  https://kubernetes.docker.internal:6443/api/v1/namespaces/default/pods/selenium-node-chrome-8457f569c8-nhgct

```


#### to get scale
```
curl -k -X GET \
  -H "Authorization: Bearer eyJhbGciOiJSUzI1NiIsImtpZCI6IklOR1I0STV2aHU4enRmdkZZSW5xUmRhdHJhWGpYaGItWnVJZTg4VXo1NG8ifQ.eyJpc3MiOiJrdWJlcm5ldGVzL3NlcnZpY2VhY2NvdW50Iiwia3ViZXJuZXRlcy5pby9zZXJ2aWNlYWNjb3VudC9uYW1lc3BhY2UiOiJkZWZhdWx0Iiwia3ViZXJuZXRlcy5pby9zZXJ2aWNlYWNjb3VudC9zZWNyZXQubmFtZSI6ImF1dG8tc2NhbGUtcm9ib3Qtc2EiLCJrdWJlcm5ldGVzLmlvL3NlcnZpY2VhY2NvdW50L3NlcnZpY2UtYWNjb3VudC5uYW1lIjoiYXV0by1zY2FsZS1yb2JvdC1zYSIsImt1YmVybmV0ZXMuaW8vc2VydmljZWFjY291bnQvc2VydmljZS1hY2NvdW50LnVpZCI6IjNmNWIxYTk4LTRhNDYtNDIzNS05OWE2LWNjYjU5NzBmODhiMiIsInN1YiI6InN5c3RlbTpzZXJ2aWNlYWNjb3VudDpkZWZhdWx0OmF1dG8tc2NhbGUtcm9ib3Qtc2EifQ.G3xbgq7BTfyf1mogsQpmp0Amcywv5DdHm_FfyqFpB-kzkJq3lDsZHpVquHXi041ruAnf5mY0CuyFqo6SI-RqBCF5jT9d2K-vpCOtEXHqtJyev-E8dbyqYLiAoYNYmD5cnhgvkoF70cWzZiKW2u9ryPm28ELSZvEBME1vfs7RvgfLxm4REGGh8jb1NAHUNHbEoCm2nfGke0_78Vi54nNbggmSMqgTLfOYP6OByHTgrqyQXRu56y6Ir2qRvgflfhQmHqR7kOi7wH7XYkjroDmDfLmA2hNmO91-LiSavxucdr6Vj3rn1fZFGd0tZ5gDU__fpkuNSwhGNo0ByGeq0dNAlA" \
  https://kubernetes.docker.internal:6443/apis/apps/v1/namespaces/default/deployments/selenium-node-chrome/scale | jq .  

```

#### to update scale
```
curl -k -X PATCH \
  -H "Authorization: Bearer eyJhbGciOiJSUzI1NiIsImtpZCI6IklOR1I0STV2aHU4enRmdkZZSW5xUmRhdHJhWGpYaGItWnVJZTg4VXo1NG8ifQ.eyJpc3MiOiJrdWJlcm5ldGVzL3NlcnZpY2VhY2NvdW50Iiwia3ViZXJuZXRlcy5pby9zZXJ2aWNlYWNjb3VudC9uYW1lc3BhY2UiOiJkZWZhdWx0Iiwia3ViZXJuZXRlcy5pby9zZXJ2aWNlYWNjb3VudC9zZWNyZXQubmFtZSI6ImF1dG8tc2NhbGUtcm9ib3Qtc2EiLCJrdWJlcm5ldGVzLmlvL3NlcnZpY2VhY2NvdW50L3NlcnZpY2UtYWNjb3VudC5uYW1lIjoiYXV0by1zY2FsZS1yb2JvdC1zYSIsImt1YmVybmV0ZXMuaW8vc2VydmljZWFjY291bnQvc2VydmljZS1hY2NvdW50LnVpZCI6IjNmNWIxYTk4LTRhNDYtNDIzNS05OWE2LWNjYjU5NzBmODhiMiIsInN1YiI6InN5c3RlbTpzZXJ2aWNlYWNjb3VudDpkZWZhdWx0OmF1dG8tc2NhbGUtcm9ib3Qtc2EifQ.G3xbgq7BTfyf1mogsQpmp0Amcywv5DdHm_FfyqFpB-kzkJq3lDsZHpVquHXi041ruAnf5mY0CuyFqo6SI-RqBCF5jT9d2K-vpCOtEXHqtJyev-E8dbyqYLiAoYNYmD5cnhgvkoF70cWzZiKW2u9ryPm28ELSZvEBME1vfs7RvgfLxm4REGGh8jb1NAHUNHbEoCm2nfGke0_78Vi54nNbggmSMqgTLfOYP6OByHTgrqyQXRu56y6Ir2qRvgflfhQmHqR7kOi7wH7XYkjroDmDfLmA2hNmO91-LiSavxucdr6Vj3rn1fZFGd0tZ5gDU__fpkuNSwhGNo0ByGeq0dNAlA" \
  -H "Accept: application/json" \
  -H "Content-Type: application/strategic-merge-patch+json" \
  -d '{ "spec": { "replicas": 1 } }' \  
  https://kubernetes.docker.internal:6443/apis/apps/v1/namespaces/default/deployments/selenium-node-chrome/scale
```
