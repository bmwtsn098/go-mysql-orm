FROM golang:1.18 as builder

COPY . /go-mysql-orm
WORKDIR /go-mysql-orm/mayhem

RUN go install github.com/dvyukov/go-fuzz/go-fuzz@latest github.com/dvyukov/go-fuzz/go-fuzz-build@latest
RUN go get github.com/dvyukov/go-fuzz/go-fuzz-dep
RUN go get github.com/AdaLogics/go-fuzz-headers
RUN apt update && apt install -y clang

RUN go-fuzz-build -libfuzzer -o fuzzMySqlOrm.a && \
    clang -fsanitize=fuzzer fuzzMySqlOrm.a -o fuzzMySqlOrm.libfuzzer

FROM debian:bookworm-slim
COPY --from=builder /go-mysql-orm/mayhem/fuzzMySqlOrm.libfuzzer /