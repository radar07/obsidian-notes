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