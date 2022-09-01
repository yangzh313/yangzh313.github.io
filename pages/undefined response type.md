- | Content-Type | text/plain; charset=utf-8 |
- reason：context-type为text/plain未被反序列化
- solution：json-check中匹配text/plain，
  ```
  jsonCheck = regexp.MustCompile(`(?i:(?:application|text)/(?:vnd\.[^;]+\+)?(json|plain))`)
  ```
-