# Plugin Cloudfare DNS Challenge for caddy

# Stage 1: Build caddy with the Cloudflare DNS plugin
FROM caddy:builder as builder
RUN xcaddy build --with github.com/caddy-dns/cloudflare

# Stage 2: Create the final image with the built caddy binary
FROM caddy:latest
COPY --from=builder /usr/bin/caddy /usr/bin/caddy