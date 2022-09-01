- test
- ```
  > Test run finished at 2022/7/14 21:00:34 <
  
  Running tool: /usr/local/go/bin/go test -timeout 30s -run ^TestCloneZfsSnapshotNonExist$ ardbagent/src/storage
  
  === RUN   TestCloneZfsSnapshotNonExist
  2022-07-14 21:02:38     INFO    storage/api.go:285      CloneZfsSnapshot opts: {Snapshot:zpool/site-061858a0-3b41-4e39-87cb-88bf6301bbed/0717c3ba-4ce9-40ca-9667-2ad2e2ae06a3/3306/1657783604@731f85ee-5bd3-48bf-acff-cc743f78629a DatasetDst:zpool/site-061858a0-3b41-4e39-87cb-88bf6301bbed/0717c3ba-4ce9-40ca-9667-2ad2e2ae06a3/3306/1657783604-clone}
  2022-07-14 21:02:38     INFO    storage/api.go:288      resp: true
      /root/zy/projects/mysql-demo-1.0/src/storage/api_test.go:34: err code:0, msg:ok
  --- PASS: TestCloneZfsSnapshotNonExist (0.12s)
  PASS
  ok      ardbagent/src/storage   0.136s
  
  
  > Test run finished at 2022/7/14 21:02:42 <
  ```