# Builder
FROM rust:bullseye AS builder
RUN update-ca-certificates
ENV CARGO_NET_GIT_FETCH_WITH_CLI=true

WORKDIR /prover

RUN git clone https://github.com/ace-quest/ace-pozk.git && cd ace-pozk/prover && cargo update && cargo build --release && mv /prover/ace-pozk/prover/target/release/prover /prover/

# Final image
FROM debian:bullseye-slim

RUN apt-get update
RUN apt-get install -y openssl libssl-dev
RUN apt-get clean && rm -rf /var/lib/apt/lists/*

WORKDIR /prover

# Copy our build
COPY --from=builder /prover/prover .

ENTRYPOINT ["./prover"]
