### [Supported Go versions](https://github.com/golang/tools/tree/master/gopls#supported-go-versions)
- `gopls` follows the [Go Release Policy](https://golang.org/doc/devel/release.html#policy), meaning that it officially supports the last 2 major Go releases. Per [issue #39146](https://go.dev/issues/39146), we attempt to maintain best-effort support for the last 4 major Go releases, but this support extends only to not breaking the build and avoiding easily fixable regressions.
- In the context of this discussion, gopls "supports" a Go version if it supports being built with that Go version as well as integrating with the `go` command of that Go version.
- The following table shows the final gopls version that supports a given Go version. Go releases more recent than any in the table can be used with any version of gopls.
  | Go Version | Final gopls version with support (without warnings) |
  | ---- | ---- | ---- |
  | Go 1.12 | [gopls@v0.7.5](https://github.com/golang/tools/releases/tag/gopls%2Fv0.7.5) |
  | Go 1.15 | [gopls@v0.9.5](https://github.com/golang/tools/releases/tag/gopls%2Fv0.9.5) |
  | Go 1.17 | [gopls@v0.11.0](https://github.com/golang/tools/releases/tag/gopls%2Fv0.11.0) |