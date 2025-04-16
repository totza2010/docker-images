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
curl https://sh.rustup.rs -sSf | bash -s -- -y
. "$HOME/.cargo/env"

# ดาวน์โหลด pgx_ulid
git clone https://github.com/pksunkara/pgx_ulid.git /tmp/pgx_ulid
cd /tmp/pgx_ulid

# Build ด้วย cargo ตรง ๆ โดยใช้ feature pg17
PGX_PG_CONFIG=/usr/lib/postgresql/${PG_MAJOR}/bin/pg_config \
cargo build --release --no-default-features --features pg17

# สร้าง path ปลายทางของ extension
mkdir -p /usr/share/postgresql/${PG_MAJOR}/extension
mkdir -p /usr/lib/postgresql/${PG_MAJOR}/lib

# คัดลอกไฟล์ไปที่ Postgres
cp ./pgx_ulid.control /usr/share/postgresql/${PG_MAJOR}/extension/
cp ./target/release/libpgx_ulid.so /usr/lib/postgresql/${PG_MAJOR}/lib/

# cleanup
cd /
rm -rf /tmp/pgx_ulid
EOR

CMD ["postgres"]
