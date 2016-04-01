# WebSocket Client
Everything for using websockets is located in the `ws_client.h` header file inside the `web` and `web::websockets::client` namespaces.

    #include <cpprest/ws_client.h>
    using namespace web;
    using namespace web::websockets::client;

The [`websocket_client`](http://microsoft.github.io/cpprestsdk/classweb_1_1websockets_1_1client_1_1websocket__client.html)class is used to create and maintain a connection to a WebSocket endpoint. Once you have your client, you must connect to the remote endpoint using the `connect()` function and pass in a URI specifying where this client wants to connect. This function returns a `pplx::task` which can be waited upon.

    client.connect(U("ws://localhost:1234")).then([](){ /* We've finished connecting. */ });

Once the client is connected, you can start sending and receiving data. Like the rest of the C++ Rest SDK, this is done in an asynchronous way.

    websocket_outgoing_message msg;
    msg.set_utf8_message("I am a UTF-8 string! (Or close enough...)");
    client.send(msg).then([](){ /* Successfully sent the message. */ });

    client.receive().then([](websocket_incoming_message msg) {
        return msg.extract_string();
    }).then([](std::string body) {
        std::cout << body << std::endl;
    });

(Note: only one receive() call will be fulfilled per received message)

We support sending and receiving string and binary messages.

    websocket_outgoing_message msg;
    concurrency::streams::producer_consumer_buffer<uint8_t> buf;
    std::vector<uint8_t> body(6);
    memcpy(&body[0], "a\0b\0c\0", 6);

    auto send_task = buf.putn(&body[0], body.size()).then([&](size_t length) {
        msg.set_binary_message(buf.create_istream(), length);
        return client.send(msg);
    }).then([](pplx::task<void> t)
    {
        try
        {
            t.get();
        }
        catch(const websocket_exception& ex)
        {
        std::cout << ex.what();
        }
    });
    send_task.wait();


Once you're done with the client, you should close it.

    client.close().then([](){ /* Successfully closed the connection. */ });

Sometimes if you need to receive a lot of messages from the server it can be tedious and error prone to repeatedly call  `websocket_client::receive()` and handle each task. We have another class `websocket_callback_client` that allows setting a callback for messages from the server. Here is a simple example registering a callback:

    websocket_callback_client client;
    client.connect(U("ws://localhost:1234")).then([](){ /* We've finished connecting. */ });

    // set receive handler
    client.set_message_handler([](websocket_incoming_message msg)
    {
        // handle message from server...
    });
