### Problem

Currently, Microcks only supports importing a single .proto file at a time. This limitation poses a challenge when working with Protobuf definitions that depend on other .proto files, either within the same project or from external sources (e.g., Google's Protobuf libraries like timestamp or duration).

The actual workarorund is to mount the entire 'protos' directory to the `/tmp` directory of the container (see line `31` of `docker-compose.yml`). However, this approach is a hack and can lead to unintended side effects.

### How to reproduce the problem

1. Be careful to be on branch `import`
2. Run `docker compose up`.
3. Connect, then go to `http://localhost:8080/#/importers`
4. Click on `upload`
5. Import `protos/test-service.proto` => crash

   ```
    Import "protos/common.proto" was not found or had errors.
   ```

6. Run `docker compose down`
7. Comment out the hack lines `30`, `31` inside `docker-compose.yml`
8. Run `docker compose up`
9. Retry the import => works flawlessly

### How to repoduce the problem with external proto

Sometimes, it's necessary to use external .proto files, such as those provided by Google, like timestamp or duration. To test this scenario:

1. Get google proto: `wget -O /tmp/googleapis.zip https://github.com/googleapis/googleapis/archive/f6b54cd5c1690b03eaa3de22628420587bd40af1.zip`
2. Unzip it and place it to your `/tmp`

3. Comment out line `13` inside `protos/common.proto` in order to use it in your proto
4. Try to upload `protos/test-service.proto` => crash

5. Comment out the hack lines `32` inside `docker-compose.yml`
6. Relaunch microcks with docker compose
7. Retry the import => works flawlessly

### Possible solution

To address this, I propose adding an option such as `includeDir` or `protoPath` to Microcks. This feature would allow users to specify directories containing multiple .proto files. By including this option, Microcks could resolve dependencies between .proto files and ensure that all necessary files are imported together, thereby simplifying the process of working with complex Protobuf definitions.

This enhancement would not only solve the issue of importing multiple files but also make it easier to work with Protobufs that reference external resources. Implementing this feature would greatly enhance the flexibility and usability of Microcks in gRPC scenarios.
