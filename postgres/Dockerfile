ARG PG_MAJOR=17
FROM postgres:${PG_MAJOR}-bookworm

# ติดตั้ง curl + .deb ของ pgx_ulid
RUN apt-get update && apt-get install -y curl \
 && curl -L -o /tmp/pgx_ulid.deb \
    https://github.com/pksunkara/pgx_ulid/releases/download/v0.2.0/pgx_ulid-v0.2.0-pg17-amd64-linux-gnu.deb \
 && apt-get install -y /tmp/pgx_ulid.deb \
 && rm /tmp/pgx_ulid.deb

CMD ["postgres"]
