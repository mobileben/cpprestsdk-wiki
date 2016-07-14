# Using JSON

JSON (pronounced “Jason”) is short for “JavaScript Object Notation.” It has become a lingua franca for simple web services, preferred over XML because of its simplicity and compactness. It is a strict subset of the JavaScript object literal syntax. As such, it is dynamically typed.

If you need to further familiarize yourself with JSON, [Wikipedia](http://en.wikipedia.org/wiki/JSON) is always a good place to start.

In Casablanca, all JSON values are represented by the web::json::value class; it doesn't matter whether it’s a number, a string, or an object, from a static type perspective, it’s a JSON value and nothing but. Dynamically, that is, at runtime, the value has a type, of course: one of those listed earlier. These two uses of the word ‘type,’ static and dynamic, can be confusing, but it is commonplace when discussing programming languages. Just remember, that when we’re discussing the type of a JSON value, it’s always the dynamic type we care about, since all values have the same static type.

## Construction

To construct a JSON value, you can either start with C++ values or a string (or stream) that contains JSON source code, which is then parsed. Let’s begin by looking at the former!

There’s a value factory function for each of the six types of JSON values. In addition, there are overloads of the functions to create string, object, and array values. Here are some examples of constructing JSON values:
```javascript
using namespace web;
...
json::value v0 = json::value::null();
json::value v1 = json::value::number(17);
json::value v2 = json::value::number(3.1415);
json::value v3 = json::value::boolean(true);
json::value v4 = json::value::string(U("Hello Again!"));
json::value v5 = json::value::object();
json::value v6 = json::value::array();
```
For all but the last two, the values are final when returned, i.e. there is no mutation of existing simple values, but objects and arrays may be mutated by adding fields and elements.
## Parsing and Serializing

Almost all interesting uses for JSON have something to do with reading or writing JSON documents that have been produced by, or will be consumed by, some other software component. Therefore, a very common way of constructing a JSON value is by parsing it.

You can pull a json value from a stream using the parse method that takes a stream reference or a string:
```javascript
using namespace web;
...
utility::stringstream_t ss1;
ss1 << U("17");
json::value v1 = json::value::parse(ss1);
```
Going in the opposite direction is similarly easy: 
```javascript
using namespace web;
...
utility::stringstream_t stream;
json::value v1 = json::value::string(U("Hi"));
v1.serialize(stream);
```
##Accessing Data

Besides adding elements, adding fields, and writing a JSON value to a stream, there’s not much you can do with it: JSON values are not intended as a general-purpose dynamic value system, it’s purely meant for input and output of JSON data. For manipulating values, the normal C++ system should be used.

Therefore, we need a way to get C++ values out of JSON values. Rather than providing implicit conversion operators, Casablanca forces you to explicitly ask for C++ values using methods names ‘as_xxx()’ defined for all the simple types.
```javascript
int i = v1.as_integer();
double d = v2.as_double();
bool b = v3.as_bool();
utility::string_t s = v4.as_string();
```
When the internal data does not correspond to the requested type, the conversion will throw an exception of type json::json_exception; it does not, for example, try to convert a string to a double or Boolean when asked.

There also are several ways to access individual json array elements or object member values. One is by using the operator [] function. The subscript operator is not 'const' and can result in modifying the json value, adding a null value if necessary.
```javascript
json::value obj = json::value::parse(U("{ \"a\" : 10 }"));
obj[U("a")] = json::value(12);
obj[U("b")] = json::value(13);
auto nullValue = obj[U("c")];
```
So in the previous code, the value of the string 'a' pair will be changed from 10 to 11 and no pair already existed for 'b' so it is added with the number value 13. However there is no member pair for 'c' so reading it will result in a json pair of 'c' with value null added to the object.

To access json arrays and objects without inserting any elements the json::value::at function can be used. It only returns a reference to the json value if it already exists, otherwise it throws a json::json_exception.
```javascript
json::value obj = json::value::parse(U("{ \"a\" : 10 }"));
auto aValue = obj.at(U("a"));
auto bValue = obj.at(U("b"));
```
So in this code calling the 'at' method with 'a' will return the json number 10, but calling with 'b' will result in an exception being thrown. You can check the size or test for the existence of a json field with the json::value::size() and json::value::has_field() methods.

For more complex json value types like numbers, arrays, and objects there also exist more functionality directly on the json::number, json::array, and json::object classes. These classes contain APIs specific to the json type that doesn't apply to others. For example if you want to retrieve a json number specifically with 64 bit precision this can be found on the json::number class. Similar to the as_bool() or as_double() methods, there are functions as_number(), as_array(), and as_object(). The following example shows how to retrieve a json number as an C++ unsigned 64 bit integer type.
```javascript
json::value num = json::value(88);
int64_t num64 = num.as_number().to_int64();
```