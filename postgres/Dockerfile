ARG PG_MAJOR=17
FROM postgres:${PG_MAJOR}-bookworm

RUN <<EOR
apt-get update
apt-get install -y --no-install-recommends \
  build-essential \
  postgresql-server-dev-${PG_MAJOR} \
  git \
  curl \
  ca-certificates

# ติดตั้ง Rust (ใหม่สุด)
curl https://sh.rustup.rs -sSf | bash -s -- -y
export PATH="/root/.cargo/bin:$PATH"

# ติดตั้ง pgx
cargo install cargo-pgx --locked
cargo pgx init --pg${PG_MAJOR}=/usr/lib/postgresql/${PG_MAJOR}/bin/postgres

# ดาวน์โหลดและติดตั้ง pgx_ulid
git clone https://github.com/pksunkara/pgx_ulid.git /tmp/pgx_ulid
cd /tmp/pgx_ulid
cargo pgx install --pg-config /usr/lib/postgresql/${PG_MAJOR}/bin/pg_config

# คัดลอกไฟล์ extension ไป path ที่ PostgreSQL ใช้
cp /root/.pgx/pgx-install/pg${PG_MAJOR}/share/extension/pgx_ulid.control /usr/share/postgresql/${PG_MAJOR}/extension/
cp /root/.pgx/pgx-install/pg${PG_MAJOR}/lib/pgx_ulid.so /usr/lib/postgresql/${PG_MAJOR}/lib/

# ทำความสะอาด
cd /
rm -rf /tmp/pgx_ulid
apt-get remove -y build-essential postgresql-server-dev-${PG_MAJOR} git
apt-get autoremove -y
rm -rf /var/lib/apt/lists/*
EOR

CMD ["postgres"]
