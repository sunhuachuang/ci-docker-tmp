# Builder
FROM zyphernetwork/competition:v1 AS base
FROM rust:bullseye AS builder

RUN update-ca-certificates
ENV CARGO_NET_GIT_FETCH_WITH_CLI=true

WORKDIR /prover
COPY --from=base /data/* ./materials/

RUN git clone https://github.com/zypher-network/pozk-competition.git && cd pozk-competition/prover && cargo update && cargo build --release --features=sha25665 && mv /prover/pozk-competition/prover/target/release/prover /prover/

# Final image
FROM debian:bullseye-slim

WORKDIR /prover

# Copy our build
COPY --from=builder /prover/prover .

ENTRYPOINT ["./prover"]
