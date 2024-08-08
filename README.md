### Problem

Unable to import gRPC proto that contains an enum from another file.

### How to Reproduce the Problem

1. Run `docker compose up`.
2. Connect, then go to `http://localhost:8080/#/importers`.
3. Import `protos/test-service.proto` => crash

   ```
   com.google.protobuf.Descriptors$DescriptorValidationException: hello.UnaryRequest.type: ".hello.EnumTest" is not an enum type.
   ```

4. Comment out line `13` inside `protos/test-service.proto`.
5. Retry the import => works flawlessly.
