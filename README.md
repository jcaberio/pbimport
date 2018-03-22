# pbimport

Demo for [issue#39](https://github.com/eddelbuettel/rprotobuf/issues/39)

Install package
```
> install_github("jcaberio/pbimport")
```

`inst/proto` contains 3 proto files
```
$ ls inst/proto
generic.proto  request.proto  response.proto
```

both `request.proto` and `response.proto` imports `generic.proto`

Using `readProtoFiles` fails

```
> library(RProtoBuf)
> readProtoFiles(package="pbimport")
generic.proto:6:10:"pbimport.S3Data.object" is already defined in file "/usr/local/lib/R/site-library/pbimport/proto/generic.proto".
generic.proto:7:10:"pbimport.S3Data.bucket" is already defined in file "/usr/local/lib/R/site-library/pbimport/proto/generic.proto".
generic.proto:5:9:"pbimport.S3Data" is already defined in file "/usr/local/lib/R/site-library/pbimport/proto/generic.proto".
/usr/local/lib/R/site-library/pbimport/proto/request.proto:0:1:Import "generic.proto" was not found or had errors.
/usr/local/lib/R/site-library/pbimport/proto/request.proto:8:3:"pbimport.S3Data" seems to be defined in "/usr/local/lib/R/site-library/pbimport/proto/generic.proto", which is not imported by "/usr/local/lib/R/site-library/pbimport/proto/request.proto".  To use it here, please add the necessary import.
Error in readProtoFiles(package = "pbimport") :
  Could not load proto file '/usr/local/lib/R/site-library/pbimport/proto/request.proto'
```

Using the `protoc` compiler with `-I` works
```
$ protoc -I proto proto/* --python_out=.
$ ls
generic_pb2.py  proto  request_pb2.py  response_pb2.py
```
