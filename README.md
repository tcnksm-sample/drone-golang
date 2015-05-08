Drone.io for golang
====

[![Build Status](https://drone.io/github.com/tcnksm-sample/drone-golang/status.png)](https://drone.io/github.com/tcnksm-sample/drone-golang/latest)

This is sample of releasing golang project from [Drone.io](https://drone.io/).

1. Get source code
1. Run test
1. Send test coverage to [coveralls.io](https://coveralls.io/) by [mattn/goveralls](https://github.com/mattn/goveralls)
1. Cross-compile by [mitchellh/gox](https://github.com/mitchellh/gox)
1. Release binary to Github Release by [tcnksm/ghr](https://github.com/tcnksm/ghr)

## Configuration

Paste below codes on **commands** in drone.io from settings tab and set `GITHUB_TOKEN`, `COVERALLS_TOKEN` on **Environmental Variables**:

```bash
# Install go 1.3.1
pushd ~/
curl -s -o go.tar.gz https://storage.googleapis.com/golang/go1.3.1.linux-amd64.tar.gz
tar xzf go.tar.gz
export GOROOT=~/go
export PATH=$GOROOT/bin:$PATH
go version
popd

# Get source code
go get -t -d ./...

go get github.com/axw/gocov/gocov
go get github.com/mattn/goveralls

go test -v -covermode=count -coverprofile=coverage.out ./...
goveralls -coverprofile=coverage.out -service drone.io -repotoken $COVERALLS_TOKEN

# Install gox
go get github.com/mitchellh/gox
gox -build-toolchain
gox -output "pkg/{{.OS}}_{{.Arch}}_{{.Dir}}"

# Release by ghr
go get github.com/tcnksm/ghr
ghr --username tcnksm-sample \
       --token $GITHUB_TOKEN \
       --replace \
       --prerelease \
       --debug \
       pre-release pkg/                                   
```
