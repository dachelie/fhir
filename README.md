Intervention Engine FHIR Server [![Build Status](https://travis-ci.org/intervention-engine/fhir.svg?branch=stu3_mar2016)](https://travis-ci.org/intervention-engine/fhir)
===================================================================================================================================================================

This project provides [HL7 FHIR STU3](http://hl7.org/fhir/2016May/index.html) models and a generic FHIR server implemented in Go and using MongoDB as storage. This is not a complete implementation, as it is tuned toward the primary use cases of the [Intervention Engine](https://github.com/intervention-engine/ie) project.

Currently, this server supports:

-	JSON representations of all resources
-	Create/Read/Update/Delete (CRUD) operations
-	Conditional update and delete
-	Some but not all search features
	-	All defined resource-specific search parameters except composite types and contact (email/phone) searches
	-	Chained searches
	-	\_include and \_revinclude searches (*without* \_recurse)
-	Batch bundle uploads (POST, PUT, and DELETE entries)

Currently, this server does *not* support the following major features:

-	XML representations of resources
-	History (versions, etc.)
-	Extension of primitive types and resource sub-components
-	Conformance statements

*NOTE: Most of the fhir source code is generated by the [fhir-golang-generator](https://github.com/intervention-engine/fhir-golang-generator). In most cases, updates to source code in the fhir repository need to be accompanied by corresponding updates in the fhir-golang-generator.*

Building and Running fhir Locally
---------------------------------

For information on installing and running the full Intervention Engine stack, please see [Building and Running the Intervention Engine Stack in a Development Environment](https://github.com/intervention-engine/ie/blob/master/docs/dev_install.md).

For information on installing and running only the FHIR server, please begin by referencing the following sections of the above guide:

-	(Prerequisite) [Install Git](https://github.com/intervention-engine/ie/blob/master/docs/dev_install.md#install-git)
-	(Prerequisite) [Install Go](https://github.com/intervention-engine/ie/blob/master/docs/dev_install.md#install-go)
-	(Prerequisite) [Install MongoDB](https://github.com/intervention-engine/ie/blob/master/docs/dev_install.md#install-mongodb)
-	(Prerequisite) [Run MongoDB](https://github.com/intervention-engine/ie/blob/master/docs/dev_install.md#run-mongodb)
-	[Clone fhir Repository](https://github.com/intervention-engine/ie/blob/master/docs/dev_install.md#clone-fhir-repository)

Before you can run the FHIR server, you must install its dependencies via `go get` and build the `fhir` executable:

```
$ cd $GOPATH/src/github.com/intervention-engine/fhir
$ go build
```

The above commands do not need to be run again unless you make (or download) changes to the *fhir* source code.

Once the executable is built, you can run it without any arguments:

```
$ ./fhir
```

If you are concurrently modifying the *fhir* source code, sometimes it is easier to combine the build and run steps into a single command (forcing a recompile on every run):

```
$ go run server.go
```

The *fhir* server accepts connections on port 3001 by default.

If you wish to test the server with synthetic patient data, please reference [Generate and Upload Synthetic Patient Data](https://github.com/intervention-engine/ie/blob/master/docs/dev_install.md#generate-and-upload-synthetic-patient-data).


Development
-----------

This project uses [godep](https://github.com/tools/godep) to manage dependencies. It
also takes advantage of the /vendor directory support introduced in Go 1.6. To
properly run the test suite, execute the following command:

```
$ go test ./models ./search ./server ./upload
```

This will ensure that the vendor directory is not included when running the test
suite.

Testing Interceptors
--------------------

This server supports "interceptors", functions that allow interaction with resources before, after, and following a failed database operation. These are invoked at the data access layer. To test the interceptors:

```
$ cd $GOPATH/src/github.com/intervention-engine/fhir/test
$ go build
$ ./test
```
This runs a testing variant of the fhir server with some test interceptors installed. Follow the testing instructions in `test/interceptor.go` to verify that the interceptors are working correctly.

License
-------

Copyright 2016 The MITRE Corporation

Licensed under the Apache License, Version 2.0 (the "License"); you may not use this file except in compliance with the License. You may obtain a copy of the License at

http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software distributed under the License is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied. See the License for the specific language governing permissions and limitations under the License.
