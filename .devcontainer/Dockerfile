FROM mcr.microsoft.com/devcontainers/base:ubuntu

RUN apt update && apt install -y llvm lld lldb qemu-system-x86 \
    bc libelf-dev clang cpio

ENV PATH="/root/.cargo/bin:${PATH}"

RUN curl https://sh.rustup.rs -sSf | sh -s -- --default-toolchain none -y
RUN rustup override set 1.62.0
RUN rustup component add rust-src
RUN cargo install bindgen@0.56.0 --locked