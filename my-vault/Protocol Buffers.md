### Why Protocol Buffers?

#### An evolution of data
* CSV
	no types inferred
	commo separated column values can be tricky
* Relational databases
	no flat types like list or array
	schema definition sharing
	ORM
* JSON
	dynamic schema(forward and backward compatibility)
	redundancy
	no comments, no metadata, no documentation internally (need to use external tools)
* Protocol buffers
	Advantages:
	* typed
	* generates code in various languages
	* simple schema evolution (forward and backward compatibility)
	* comments
	* payload in binary (smaller and CPU friendly)
	Disadvantages:
	* binary (non-human readable)
	* less community support

#### Let's talk about numbers

* 3-10x smaller, 20-100x times faster than XML
* 34% smaller, 21% less times available in JS code(JSON)
* Google have 48,000 messages defined in 12000 proto files
#### How it is being used (share data between diff programming languages)

```proto
syntax = "proto3"

message MyMessage {
  uint32 id = 1;
}
```
Above proto definition can be translated to different programming languages (Go, Ruby, Java etc,.) and then converts to binary.
![[Pasted image 20240529222936.png]]

### Defaults
* A field will now be serialized
* Populated with default value

### Scalar types

#### Number (default 0)
* int32, int64, sint32, sint64
* uint32, uint64
* fixed32, fixed64, sfixed32, sfixed64
* float, double

#### Boolean (default false)
* bool

#### Strings (default empty string)
* string
* UTF-8 or 7 bit ASCII

#### Bytes (default empty byte array)
* bytes

```proto
syntax = "proto3"

message MyMessage {
  uint32 id = 1;
  string name = 2;
  bytes thumbnail = 3;
  bool is_verified = 4
  float height = 5;
}
```


### Tags

* Fields are not important for serialization but tags are.
* Smallest tag -1  Largest tag - 536,870,911
* smallest tags are for the most populated fields on the schema
* Reserved tags(by Google) -  19000 to 19999
* 1 - 15  -> 1 byte
* 16 - 2047 - 2 bytes