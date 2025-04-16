ARG PG_MAJOR=17
FROM postgres:${PG_MAJOR}-bookworm

RUN <<EOR
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
curl https://sh.rustup.rs -sSf | bash -s -- -y
. "$HOME/.cargo/env"

# ติดตั้ง pgx CLI
cargo install cargo-pgx

# ดาวน์โหลด pgx_ulid และ build + ติดตั้ง
git clone https://github.com/pksunkara/pgx_ulid.git /tmp/pgx_ulid
cd /tmp/pgx_ulid

cargo pgx install --pg-config /usr/lib/postgresql/${PG_MAJOR}/bin/pg_config

# คัดลอกไฟล์ extension ที่ build เสร็จแล้วไปยัง PostgreSQL
cp ~/.pgx/pgx-install/pg${PG_MAJOR}/share/extension/pgx_ulid.control /usr/share/postgresql/${PG_MAJOR}/extension/
cp ~/.pgx/pgx-install/pg${PG_MAJOR}/lib/pgx_ulid.so /usr/lib/postgresql/${PG_MAJOR}/lib/

cd /
rm -rf /tmp/pgx_ulid
EOR

CMD ["postgres"]
