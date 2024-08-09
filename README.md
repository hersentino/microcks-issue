### Problem

Unable to import gRPC service that import other grpc file unless we mount all the "protos" directory in the "/tmp" of the container (`l31` from `docker-compose.yml`)

### How to Reproduce the Problem

1. Run `docker compose up`.
2. Connect, then go to `http://localhost:8080/#/importers`.
3. Click on `upload`
4. Import `protos/test-service.proto` => crash

   ```
    Import "protos/common.proto" was not found or had errors.
   ```

5. Run `docker compose down`.
6. Comment out line `30`, `31` inside `/Users/francoishersent/hiventive/microcks_issue/docker-compose.yml`.
7. Run `docker compose up`.
8. Retry the import => works flawlessly.
