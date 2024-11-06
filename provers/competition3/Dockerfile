# Builder
FROM rust:bullseye AS builder

RUN update-ca-certificates
RUN curl -L https://risczero.com/install | bash
RUN rzup install
ENV CARGO_NET_GIT_FETCH_WITH_CLI=true

WORKDIR /prover

RUN git clone https://github.com/zypher-network/pozk-competition.git && cd pozk-competition/zkvm && cargo update && cargo build --release && mv /prover/pozk-competition/zkvm/target/release/prover /prover/

# Final image
FROM debian:bullseye-slim

WORKDIR /prover

# Copy our build
COPY --from=builder /prover/prover .

ENTRYPOINT ["./prover"]
