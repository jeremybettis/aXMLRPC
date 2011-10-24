What is aXMLRPC?
================

aXMLRPC is a Java library with a leightweight XML-RPC client. XML-RPC is
a specification for making remote procedure calls over the HTTP protocol
in an XML format. The specificationc an be found under http://www.xmlrpc.com/spec.

The library was developed for the use with Android. Since it has no dependencies to 
any Android library or any other 3rd-party library, it is fully functional in any
common java virtual machine (not only on Android).

You can control the client with some flags to extend its functionality. See the section
about flags.

How to include it?
==================

How to include the aXMLRPC client into your project?
There are two ways to do that:

### Include the source code

You can just include all the source code from the `src` directory into the sourcecode
of your project. If you use git yourself, you can use submodules to include the code 
as a module to yours. So you will always stay up to date with the library.

### Compile it as library

The library itself is a NetBeans project and can be compiled either with NetBeans or
with `ant`. The resulting JAR file can be used as a dependency in your project. If you
use NetBeans you can make a dependency directly to the project.

How to use the library?
=======================

You can use the library by initiating an `XMLRPCClient` and make calls over it:

	try {
		XMLRPCClient client = new XMLRPCClient(new URL("http://example.com/xmlrpc"));
	
		Boolean b = (Boolean)client.call("isServerOk");
		Integer i = (Integer)client.call("add", 5, 10);
	
	} catch(XMLRPCServerException ex) {
		// The server throw an error.
	} catch(XMLRPCException ex) {
		// An error occured in the client.
	} catch(Exception ex) {
		// Any other exception
	}

The data types
--------------

The specification give some data tags for the server response. If you want to work on the
type you must cast the returning `Object` from the `call` method to its specific type.
Which type to cast which XML server response, tells the following list:

`i4`,`int`	=> Integer
`boolean`	=> Boolean
`string`	=> String
`double`	=> Double
`dateTime.iso8601`	=> Date
`base64`	=> Byte[]
`array`		=> Object[]
`struct`	=> Map<String,Object>

`i8`		=> Long (see Flags)


Flags
-----

The client takes as second parameter (or third if an user agent is given) 
a combination of multiple flags. It could work like the following example:

   // ...
   XMLRPCClient client = new XMLRPCClient(url, FLAGS_STRICT | FLAGS_8BYTE_INT);
   // ...

The following flags are implemented:


#### FLAGS_STRICT

The client should parse responses strict to specification.
It will check if the given content-type is right.
The method name in a call must only contain of A-Z, a-z, 0-9, _, ., :, /
Normally this is not needed.


#### FLAGS_8BYTE_INT

The client will be able to handle 8 byte integer values (longs).
The xml type tag <i8> will be used. This is not in the specification
but some libraries and servers support this behaviour.
If this isn't enabled you cannot recieve 8 byte integers and if you try to
send a long the value must be within the 4byte integer range.

License
=======

The library is licensed under [MIT License] (http://www.opensource.org/licenses/mit-license.php).
See the LICENSE file for the license text. 

For the uninformed reader: What does MIT mean?

- You can copy this, modify it, distribute it, sell it, eat it.
- You don't need to notice me about anything of the above.
- If you make changes to it, it would be nice (but not obliged), if you would share it with me again.
- Put the copyright notice and the LICENSE file in any copy you make.