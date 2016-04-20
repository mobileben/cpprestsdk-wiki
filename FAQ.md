# Frequently Asked Questions

### What is C++ Rest SDK and why should I care about it?

C++ Rest SDK is your toolbox for accessing connected applications using modern C++ features and best practices.

Historically C++ developers have been lacking a basic set of tools that enable them to access and author REST services in a productive, scalable and asynchronous manner. C++ Rest SDK aims to alleviate some of the pain felt by developers by providing a cross-platform library that is modeled on simplicity, extensibility and composition.

### C++ Rest SDK uses tasks in many of the APIs. What are tasks?

The tasks library shipping with C++ Rest SDK is a port of the PPL tasks library that’s available with Visual C++ in Visual Studio 2012\. In order to make it work alongside the existing PPL library, we changed the namespace to “pplx” in the ported library.

Tasks are emerging as the fundamental mechanism to compose asynchronous operations across a variety of APIs and runtime environments. C++ developers writing code for Windows 8 with Visual Studio 2012 can use them, .NET has had them for a while and increasing their use in .NET 4.5, and JavaScript provides the same concept in its promises pattern.

More information on tasks is available at the following locations:

*   [Task Parallelism on Msdn](http://msdn.microsoft.com/en-us/library/dd492427.aspx)
*   [Channel 9: Asynchronous Programming for C++ Developers: PPL Tasks and Windows 8](http://channel9.msdn.com/Blogs/Charles/Asynchronous-Programming-for-C-Developers-PPL-Tasks-and-Windows-8)
*   [Blog: Building Responsive GUI Applications with PPL Tasks](http://blogs.msdn.com/b/nativeconcurrency/archive/2011/03/23/building-responsive-gui-applications-with-ppl-tasks.aspx?Redirected=true)
*   [Blog: Tasks and Continuations: Available for Download Today!](http://blogs.msdn.com/b/nativeconcurrency/archive/2011/03/09/tasks-and-continuations-available-for-download-today.aspx)

### Asynchrony is mentioned a lot in casablanca. Why should I care?

Asynchrony is about efficiency and responsiveness. If you are writing a graphical user interface client, you want to use an asynchronous operation whenever there is a chance of it taking more than a few milliseconds to execute, or the UI will be jerky and possibly freeze. PPL tasks make it really easy to write this kind of code.

If you are writing server-side code, you are likely doing a lot of I/O: reading files, logging data, using network resources, accessing databases. These activities all involve waiting for potentially long-running operations, and asynchrony helps your code wait efficiently. You are likely to want your services to scale to serve many simultaneous client requests, which can be challenging when using blocking (synchronous) APIs, since each waiting operation holds on to its thread while waiting. Threads are very expensive resources.

### Can I use C++ Rest SDK on iOS, Linux, Android, or other platforms?

The API surface of C++ Rest SDK is platform-agnostic, and it had already supports many platforms including iOS and Android. Please refer to our [documentation](https://casablanca.codeplex.com/documentation) for the full list of currently supported platforms.

### What is utility::string_t and the 'U' macro?

The C++ REST SDK uses a different string type dependent on the platform being targeted. For example for the Windows platforms utility::string_t is std::wstring using UTF-16, on Linux std::string using UTF-8\. The 'U' macro can be used to create a string literal of the platform type. If you are using a library causing conflicts with the 'U' macro, for example Boost.Iostreams it can be turned off by defining the macro '_TURN_OFF_PLATFORM_STRING' before including the C++ REST SDK header files.

### Is Visual Studio 2010 supported?

Our latest releases [no longer support Visual Studio 2010](https://casablanca.codeplex.com/discussions/471309). This is due to both engineering cost and we utilize some C++11 features that aren't available. [1.2.0](https://casablanca.codeplex.com/releases/view/111094) was the last release that included binaries for Visual Studio 2010.

### You have a JSON library but not an XML library. What’s up with that?

Two things influenced this decision:

*   For many developers of REST services, JSON is the preferred format for exchanging text-serialized data. It’s simpler than XML and it easier to read. Given this preference, we thought it important to prioritize JSON over XML.
*   There are already several good native XML libraries for both Windows and Linux. For example, on Windows, we’re using XmlLite internally in the C++ Rest SDK libraries in a couple of places. That library has a lot of Win32 “jargon” to it, and we may eventually wrap it in something that is thin but looks like standard C++, but XmlLite is a very functional library and has excellent performance. There are also open-source solutions available.

### C++ Rest SDK supports accessing REST services. Why not SOAP?

To do a good job at supporting SOAP services, you need more tooling than what we could get into this release. If you do need to access SOAP services from native code, there’s Windows Web Services. We may add SOAP as a C++ Rest SDK scenario in the future, so please give us feedback on this if it’s important to you.

### You asked for feedback. I have feedback. Where do I provide it?

We would love hear from all of you. For general questions or to give feedback visit the [forums](https://casablanca.codeplex.com/discussions)

Visit the link to learn how to submit [contributions or report a bug](https://casablanca.codeplex.com/wikipage?title=Make%20a%20contribution&referringTitle=FAQ).