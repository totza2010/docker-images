ARG TARGETARCH
ARG PG_MAJOR=17

FROM postgres:${PG_MAJOR}-bookworm

COPY --from=binary /pgvecto-rs-binary-release.deb /tmp/vectors.deb

RUN <<EOR
# อัปเดตและติดตั้งเครื่องมือที่จำเป็น
apt-get update
apt-get install -y --no-install-recommends \
    build-essential \
    postgresql-server-dev-${PG_MAJOR} \
    git \
    curl \
    ca-certificates \
    rustc \
    cargo

# ติดตั้ง pgx และ compile pgx_ulid
cargo install cargo-pgx
cargo pgx init --pg${PG_MAJOR}=/usr/lib/postgresql/${PG_MAJOR}/bin/postgres

git clone https://github.com/pksunkara/pgx_ulid.git /tmp/pgx_ulid
cd /tmp/pgx_ulid
cargo pgx install --pg-config /usr/lib/postgresql/${PG_MAJOR}/bin/pg_config

# ทำความสะอาด
cd /
rm -rf /tmp/pgx_ulid
apt-get remove -y build-essential postgresql-server-dev-${PG_MAJOR} rustc cargo git
apt-get autoremove -y
rm -rf /var/lib/apt/lists/*
EOR

# เปิดการใช้งาน extension pgx_ulid โดยอัตโนมัติ
CMD ["postgres"]