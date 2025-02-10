# Builder
FROM byte443/materials-ludo:v1 AS base
FROM rust:bullseye AS builder

RUN update-ca-certificates
ENV CARGO_NET_GIT_FETCH_WITH_CLI=true

WORKDIR /prover
COPY --from=base /data/prover_key.bin ./materials/
COPY --from=base /data/ludo ./materials/ludo

RUN git clone https://github.com/zypher-network/pozk-ludo.git && cd pozk-ludo/prover && cargo update && cargo build --release && mv /prover/pozk-ludo/prover/target/release/prover /prover/

# Final image
FROM debian:bullseye-slim

RUN apt-get update
RUN apt-get install -y openssl libssl-dev
RUN apt-get clean && rm -rf /var/lib/apt/lists/*

WORKDIR /prover

# Copy our build
COPY --from=builder /prover/prover .

ENTRYPOINT ["./prover"]