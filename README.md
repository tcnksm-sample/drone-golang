Sample releae golang project from [Drone.io](https://drone.io/)
====

This is sample of releasing golang project from [Drone.io](https://drone.io/).

1. Get source code
1. Run test
1. Send test coverage to [coveralls.io](https://coveralls.io/) by [mattn/goveralls](https://github.com/mattn/goveralls)
1. Cross-compile by [mitchellh/gox](https://github.com/mitchellh/gox)
1. Release binary to Github Release by [tcnksm/ghr](https://github.com/tcnksm/ghr)

## Configuration

Write belows on drone.io's settings tab

```bash
go get -t -d ./...

go get github.com/axw/gocov/gocov
go get github.com/mattn/goveralls

go test -v -covermode=count -coverprofile=coverage.out ./...
goveralls -coverprofile=coverage.out -service drone.io -repotoken $COVERALLS_TOKEN
```



