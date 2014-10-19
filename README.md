Annotated Service Discovery
---

### Why?
Build a semi language agnostic service discovery dsl built into JAVADOC like comments. This should allow for documentation to be generated with no run time dependencies.

Parameter and response information should be in JSON schema formats, and can be a relative path.

### Goals

1. asd will be fast and can be ran using a watch or gaurd.
2. asd will include no runtime dependencies.
3. asd will be semi language agnostic.
4. asd will be written in Go.
5. asd will provide tools to upload versioned endpoints to the service portal.
6. asd will strive to be standards compliant and open-source-able.
7. asd will be self contained and require no runtime dependencies.

### Annotations

#### Document your controllers with JSON schemas

```
/**
 * @ENDPOINT
 * @route: POST "/v1/music"
 * @params: {
 *   properties: {
 *     music: {
 *       type: "object",
 *       required: "true",
 *       properties: {
 *         album: { type: "string", required: true },
 *         year: { type: "string", format: "ISO 8601", require: true },
 *         title: { type: "string", required: true },
 *         artist: { type: "string", required: false }
 *         trackNumber: { type: "number", maximum: 99, minimum: 0, required: false },
 *         genre: { enum: ["rock", "blues"] }
 *       }
 *     }
 *   }
 * @response: @params
 * @ENDPOINT
 **/
```

#### Use schema paths for uncluttered documentation

```
/**
 * @ENDPOINT
 * route: POST "/v1/music"
 * params: "/v1/music_params.json"
 * response: "/v1/music_response.json"
 * @ENDPOINT
 **/
```

```
##
# @ENDPOINT
# ...
# @ENDPOINT
##
```

### Usage

```
asd --files app/controllers/**/*.rb --output /tmp/services.json --schemas doc/endpoints/
```

```
asd --config .asd.yml
```

##### Config (yml)

```
files: src/main/java/com.foo.bar/web/endpoints/**/*.java
output: documentations/services.json
schemas: doc/endpoints/
endpoint_description_annotation: @CONTROLLER
use_sha_for_version: true
use_date_for_version: false
environment: $SPRING_ENV
```

#### Todo
1. Write prototype
  a. Start Go project (internal)
  b. Write lexer
    i. Allow parse errors to be easily found (and ignorable)
    ii. Reject unknown attrs
  c. Write writer
  e. Create CLI

2. Create demo projects in rails, sinatra, spring, and go
3. Measure performance
4. ???
4. Profit
