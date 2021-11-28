---
tags: [docker, buildx, buildkit]
---
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

Inspect the manifest:
```
docker manifest inspect --insecure localhost:5000/multi-arch-test:latest

{
   "schemaVersion": 2,
   "mediaType": "application/vnd.docker.distribution.manifest.list.v2+json",
   "manifests": [
      {
         "mediaType": "application/vnd.docker.distribution.manifest.v2+json",
         "size": 739,
         "digest": "sha256:4139855abd22559eefa51893353d7d5b6c87336f0910fce5a2f852cd7616a1c4",
         "platform": {
            "architecture": "arm64",
            "os": "linux"
         }
      },
      {
         "mediaType": "application/vnd.docker.distribution.manifest.v2+json",
         "size": 739,
         "digest": "sha256:efee871e3283ce51480576c9d70f29d451a37661bf79028252a5d12f9fcc6621",
         "platform": {
            "architecture": "amd64",
            "os": "linux"
         }
      }
   ]
}
```
