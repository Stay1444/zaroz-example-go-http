# zaroz-example-go-http

A Zaroz Cloud **git-build example**: a small Go HTTP service that uses only the standard library.
There is no `Dockerfile`, so Zaroz builds it from `go.mod` with Railpack.

- `main.go`: `GET /` and `GET /healthz`, listens on `$PORT` (8080 when unset) and shuts down
  cleanly on SIGTERM, so a deploy never cuts a request short.
- `main_test.go`: handler tests with `httptest` (no network).

## Run it locally

```bash
go test ./...
go run .                       # http://localhost:8080
curl localhost:8080/
```

## Deploy it on Zaroz

> **Status:** deploying straight from a git repository is in development on Zaroz Cloud. This
> repository is one of the sample apps it is built and tested against, so the steps below describe
> the planned flow. A `zaroz.toml` example will be added once that file's format is published; this
> app needs none, because the defaults are enough.

1. Fork this repository.
2. In the Zaroz console, create a **Workload** from a git repository, connect GitHub (GitLab,
   Forgejo or any git URL also work) and pick your fork.
3. Zaroz builds every push to the default branch. With no `Dockerfile` it uses
   [Railpack](https://railpack.com), which detects Go from `go.mod`, takes the Go version from it
   and builds the `main` package at the root.
4. The service listens on the `PORT` environment variable, which Zaroz sets.

## CI

`.github/workflows/ci.yml` runs `go vet` and `go test`, then builds the binary, starts it and
checks `/healthz`.

MIT licensed.
