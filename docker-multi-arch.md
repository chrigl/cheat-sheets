# Building multi arch images with docker buildx

Docker does not support local multi-platform images, which means, we must tell `buildx` to push the results.
If we for whatever reason don't want to push to a real registry, we must setup a local one, but also use `network=host` for the builder.

Start the local registry:
```
docker run -d -p 5000:5000 --restart=always --name registry registry:2
```

Create a builder with `network=host`:
```
docker buildx create --use --buildkitd-flags '--allow-insecure-entitlement network.host' --driver-opt network=host
```

Create an example `Dockerfile`. This example uses `TARGETARCH` for the image `debian-base`, because there is no multi-arch option for this image.
`golang:latest` is already multi-arch enabled:
```
cat > Dockerfile.new <<EOF
FROM golang:latest AS builder

COPY main.go /go/foo/main.go
WORKDIR /go/foo/
RUN go build -o foo main.go

FROM k8s.gcr.io/build-image/debian-base-\${TARGETARCH}:v2.1.3
COPY --from=builder /go/foo/foo /foo

CMD ["/foo"]
EOF
```

Build and push to `localhost`:
```
docker buildx build --network host --platform linux/arm64/v8,linux/amd64 --tag localhost:5000/multi-arch-test:latest --push .
```
