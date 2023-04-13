- 参考：
	- [xxjwxc/uber_go_guide_cn: Uber Go 语言编码规范中文版. The Uber Go Style Guide . (github.com)](https://github.com/xxjwxc/uber_go_guide_cn)
- Linting
	- Lint Runners
		- [golangci-lint](https://github.com/golangci/golangci-lint)
			- ```
			  # Refer to golangci-lint's example config file for more options and information:
			  # https://github.com/golangci/golangci-lint/blob/master/.golangci.reference.yml
			  
			  run:
			    timeout: 5m
			    modules-download-mode: readonly
			  
			  linters:
			    enable:
			      - errcheck
			      - goimports
			      - golint
			      - govet
			      - staticcheck
			  
			  issues:
			    exclude-use-default: false
			    max-issues-per-linter: 0
			    max-same-issues: 0
			  ```