FROM rust:1 AS chef 

WORKDIR /backend

RUN cargo install cargo-chef 

FROM chef AS planner

COPY . .

RUN cargo chef prepare  --recipe-path recipe.json

FROM chef AS builder

COPY --from=planner /backend/recipe.json recipe.json

RUN cargo chef cook --release --recipe-path recipe.json

COPY . .

RUN cargo build --release

FROM gcr.io/distroless/cc-debian11

ARG DESCRIPTION

LABEL org.opencontainers.image.description=$DESCRIPTION
LABEL org.opencontainers.image.source=https://github.com/digstack/kube-infra/

COPY --from=builder /backend/target/release/api /

CMD ["./api"]