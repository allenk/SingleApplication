SingleApplication
=================

This is a replacement of the QSingleApplication for ```Qt5```.

Keeps the Primary Instance of your Application and kills each subsequent instance.

Usage
-----
The ```SingleApplication``` class inherits from ```QApplication```. Use it as if you are using the ```QApplication``` class.

The library uses your ```Organization Name``` and ```Application Name``` to set up a Local Socket. The first instance of your Application would start a ```QLocalServer``` and then listen for connections on the socket. Every subsequent instance of your application would attempt to connect to that socket. If successful it will be terminated, while in the Primary Instance, ```SingleApplication``` would emmit the ```showUp()``` signal.

The library uses ```stdlib``` to terminate the program with the ```exit()``` function.

Here is an example usage of the library:
```cpp
#include "singleapplication.h"

int main(int argc, char *argv[])
{
    QApplication::setApplicationName("{Your App Name}");
    QApplication::setOrganizationName("{Your Organization Name}");
    
    SingleApplication app(argc, argv);

    return app.exec();
}
```

The ```Show Up``` signal
------------------------
The SingleApplication class implements a ```showUp()``` signal. You can bind to that signal to raise your application's window when a new instance had been started.

Implementation
--------------
The library is implemented with a QSharedMemory block which is thread safe and guarantees a race condition will not occur. It also uses a QLocalSocket to notify the main process that a new instance had been spawned and thus invoke the ```showUp()``` signal.

To handle an issue with Unix systems, where the operating system owns the shared memory block and if the program crashes the memory remains untouched, the library binds to the following signals and closes the program with error code = ```128 + signum``` where signum is the number representation of the signal listed below.

* ```SIGINT ``` - ```2```
* ```SIGILL ``` - ```4```
* ```SIGABRT``` - ```6```
* ```SIGFPE ``` - ```8```
* ```SIGSEGV``` - ```11```
* ```SIGTERM``` - ```15```


License
-------
This library and it's supporting documentation are released under ```The MIT License (MIT)```.
