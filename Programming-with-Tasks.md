# Programming with Tasks

All long running or potentially blocking APIs in Casablanca are asynchronous, for example any API which makes a call going across a network. PPL tasks are used to represent the completion of some asynchronous work. In order to make Casablanca usable also with Visual Studio 2010 and on other non Windows platforms, we have built a special version of PPL tasks and included it in the Casablanca release. In order for it peacefully co-exist with PPL, we placed the special version in a different namespace, “pplx.”  

A task represents an operation that may or may not have finished once a function producing it returns. For example, when sending an HTTP message, we will eventually get a response message back. A task represents this eventually available response:  

```c++
web::http::client::http_client client(U("http://localhost:80"));
pplx::task<web::http::http_response> resp = client.request(web::http::methods::GET, U("/foo.html"));
```

</div>

Here, the ‘resp’ task provides us a placeholder for a value that will be available in the future.  

Once you have a task, you need to (eventually) get a value out of it. Since it is possible that the operation finishes so fast that the value is available immediately, it may be reasonable to ask the task whether it is “done” or not:  

```c++
bool done = resp.is_done(); 
```

</div>

If this function returns true, the value can be retrieved using `get()`, which is guaranteed not to block when `is_done()` returned true:  

```c++
web::http::http_response response = resp.get(); 
```

</div>

If `is_done()` returns false, calling `get()` will risk blocking the thread, which defeats the purpose of using an asynchronous API in the first place (it will keep GUIs from refreshing and servers from scaling). For this situation, we need to take a different approach: attach a handler function to the task, which will be invoked once the task completes. We do this using the `then()` function:  

```c++
resp.then([=] (web::http::http_response  response)
{
   ...
}); 
```

</div>

The function object passed to then() of a task<T> should take an argument either of type T or task<T>, the latter being the only way to observe exceptions raised by the operation – a call to get() inside the response handler will re-throw any exception that resulted from the operation. Thus, the following example also works, but it also allows exceptions to be seen and handled:  

```c++
resp.then([=](pplx::task<web::http::http_response> task)
{
    web::http::http_response  response = task.get();
    ...
}); 
```

</div>

Since we know the task has finished, calling ‘get()’ inside the callback function object is never going to block.  

More [information on PPL tasks](http://msdn.microsoft.com/en-us/library/dd492427(v=vs.110).aspx) is available on MSDN, including information on how to wait for more than one task with a single call.