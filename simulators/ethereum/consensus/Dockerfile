# This simulation runs the ethereum consensus tests.
FROM golang:1-alpine as builder
ARG GOPROXY
ENV GOPROXY=${GOPROXY}
RUN apk add --update git ca-certificates gcc musl-dev linux-headers

# Clone the tests repo.
RUN git clone --depth 1 --branch legacy-collisions-pr17 --recurse-submodules https://github.com/flcl42/tests.git /tests && \
    test "$(git -C /tests rev-parse HEAD)" = dffee992a3e27229723c66c0e8c0d338c4e0a62a

# Build the simulator executable.
ADD . /source
WORKDIR /source
RUN go build -v .

# Build the simulator run container.
FROM alpine:latest
ADD . /source
WORKDIR /source
COPY --from=builder /source/consensus .
COPY --from=builder /tests /tests
ENV TESTPATH /tests

ENTRYPOINT ["./consensus"]
