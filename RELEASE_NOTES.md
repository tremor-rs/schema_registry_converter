## Release notes

### 3.1.0

Fix a problem with missing checks on some proto common types, like timestamp.
Added functions to get the context when using protobuf.
Added functions to get the Avro schema when using avro.

### 3.0.0

Several breaking changes in the API, making it easier to use as in most places we don't need a mutable reference anymore.
Move to apache-avro for avro, which contains several fixes.
Made some additional things public for other use cases. Added some methods to the API in case the used schema is required.
Protobuf common types are 'supported' as long as the import is in the main schema, the schema's will be added to the list giving to protofish, so it can be deserialized.

### 2.1.0

Dependencies updated and ci is now run in Github Actions also some improvements where made making it easier to use, and open up some additional use cases.

#### Issues
- It's now possible to set additional options on the `reqwest` client, and use that to create the SrSettings. Mainly for custom security requirements.
- It's possible to use the `rustls_tls` feature to let `reqwest` use `rustls-tls`.
- For each async converter an `easy` variant was added. This makes it easier to use the library, as internally an arc is used, making it easier to use.
- For the protobuf encoders an `encode_single_message` method was eded to encode when the schema contains only one message. The full name of the proto message is not needed for this.

### 2.0.2

Updated dependencies

### 2.0.1

Maintenance release with mainly updated dependencies, making the blocking sr settings cloneable and no longer needs `kafka_test` feature to use both blocking and async in the same project.

### 2.0.0

This release has a breaking change in the SubjectNameStrategy where the supplied schema now is in a Box, to keep the size of the Enum smaller.
Another breaking change is that the protocol  (http or https) needs  to be included in the schema registry url.
Since besides avro also protobuf and json schema, and to some degree custom formats are supported, avro is no behind a feature flag, and in its own module.
Also add support for authentication and the use of a proxy in this version.
Another major change is by default support for async.

To use the new version of the library, and continue to use it in a blocking way like it was before, you need to use the library like:
```toml
schema_registry_converter = { version = "2.0.2", default-features = false, features = ["avro", "blocking"]}
```
Also the Converters are moved to the blocking module, and to create the converters you need a SrSettings object, which can be created with just the
schema registry url. 
```rust
let sr_settings = SrSettings::new(String::from("http://localhost:8081"));
```

#### Issues

- Add json schema support.
- Add protobuf support.
- Support references in schema registry.
- Add authentication proxies, timeouts, etc, by using reqwest instead of curl.
- Support async/non-blocking by default
- For Avro, make it possible to use the encode_struct function with primitive values.

### 1.1.0

This release makes it easier to work with structs, instead of the raw Value type in a vector.
To use structs with avro you need to add `#[derive(Debug, Deserialize, Serialize)]` above your
struct and also have a dependency on serde with the derive feature enabled. like:
```toml
[dependencies.serde]
version = "1.0"
features = ["derive"]
```

#### Issues

- Added support for the decoder to also get the name of the schema, this
can be used to determine witch crate to use the values for. In the [tests](https://github.com/gklijs/schema_registry_converter/blob/master/src/lib.rs#L577)
there is an example.
- Added support the the encoder to take a struct. To do this use the `encode_struct`
instead of the `encode` function on the encoder.

### 1.0.0

#### Issues

- Made it easier to use the crate by changing some values to owed strings.
- Fixed to issues related to sending the schema to the schema registry.
- Added integration tests, to test against a kafka cluster.
- Make post_schema public so it can be used directly.

#### Contributors

- [@cbzehner](https://github.com/cbzehner)
- [@j-halbert](https://github.com/j-halbert)  
- [@kitsuneninetails](https://github.com/kitsuneninetails)
- [@naamancurtis](https://github.com/naamancurtis)
- [@MariellHoversholm-Paf](https://github.com/MariellHoversholm-Paf)
- [@SergeStrashko](https://github.com/SergeStrashko)