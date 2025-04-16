ARG PG_MAJOR=17
FROM postgres:${PG_MAJOR}-bookworm

RUN <<EOR
set -eux

apt-get update
apt-get install -y --no-install-recommends \
  build-essential \
  postgresql-server-dev-${PG_MAJOR} \
  git \
  curl \
  ca-certificates \
  pkg-config \
  libssl-dev

# ติดตั้ง Rust
RUN curl https://sh.rustup.rs -sSf | bash -s -- -y
ENV PATH="/root/.cargo/bin:${PATH}"

# Clone pgx_ulid
RUN git clone https://github.com/pksunkara/pgx_ulid.git /tmp/pgx_ulid

# Build pgx_ulid
WORKDIR /tmp/pgx_ulid
RUN PGX_PG_CONFIG=/usr/lib/postgresql/17/bin/pg_config \
    cargo build --release --no-default-features --features pg17

# Copy .so and .control
RUN mkdir -p /usr/share/postgresql/17/extension \
 && mkdir -p /usr/lib/postgresql/17/lib \
 && cp ./pgx_ulid.control /usr/share/postgresql/17/extension/ \
 && cp ./target/release/libpgx_ulid.so /usr/lib/postgresql/17/lib/
# cleanup
cd /
rm -rf /tmp/pgx_ulid
EOR

CMD ["postgres"]
