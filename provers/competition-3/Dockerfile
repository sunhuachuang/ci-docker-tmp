# Builder
FROM rust:bullseye AS builder

RUN update-ca-certificates
RUN apt-get update && apt-get install -y --no-install-recommends ca-certificates clang curl libssl-dev pkg-config build-essential
RUN curl -L https://risczero.com/install | bash
RUN ~/.risc0/bin/rzup install cpp && ~/.risc0/bin/rzup install risc0
ENV CARGO_NET_GIT_FETCH_WITH_CLI=true

WORKDIR /prover

RUN git clone https://github.com/zypher-network/pozk-competition.git && cd pozk-competition/zkvm && cargo update && cargo build --release && mv /prover/pozk-competition/zkvm/target/release/prover /prover/

# Final image
FROM debian:bullseye-slim

WORKDIR /prover

# Copy our build
COPY --from=builder /prover/prover .

ENTRYPOINT ["./prover"]
