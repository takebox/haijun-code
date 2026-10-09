# syntax=docker/dockerfile:1

# Runtime image for `haijun gateway`.
#
# This image does NOT build the binary. It expects a prebuilt native
# linux-x64 `haijun` executable in the build context — the public Haijun Code
# release binary, which includes the `gateway` subcommand. setup.sh places it
# at ./haijun (downloading it from the public release endpoint and verifying
# it against the release manifest if missing). Override HAIJUN_BINARY to
# point at a different path.
#
# Build (with the binary at ./haijun; otherwise add --build-arg HAIJUN_BINARY=<path>):
#   docker build --platform=linux/amd64 --provenance=false -t haijun-gateway .
#
# Run:
#   docker run --rm -p 8080:8080 \
#     -v "$PWD/gateway.yaml:/etc/haijun/gateway.yaml:ro" \
#     -e OIDC_CLIENT_SECRET -e GATEWAY_JWT_SECRET -e GATEWAY_POSTGRES_URL \
#     haijun-gateway

ARG HAIJUN_BINARY=./haijun

# distroless/cc provides glibc + libstdc++ (required by the Bun-compiled
# native binary). The :nonroot tag runs as uid/gid 65532.
FROM gcr.io/distroless/cc-debian12:nonroot

ARG HAIJUN_BINARY
COPY --chmod=0755 ${HAIJUN_BINARY} /usr/local/bin/haijun

ENV HAIJUN_CONFIG_DIR=/tmp/.haijun

EXPOSE 8080
USER nonroot

ENTRYPOINT ["/usr/local/bin/haijun", "gateway", "--config", "/etc/haijun/gateway.yaml"]
