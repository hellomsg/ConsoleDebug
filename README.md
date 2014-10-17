ConsoleDebug (beginner - intermediate)
--------------
This BB10 app shows Momentics console log right on your device screen, without the need to be connected to Momentics.

Have you ever have a user that is experiencing a bug/crash that you can't reproduce on another device? How helpful would it be to get the console logs?

Now you can!

With less than 20 lines of code added to your app, your user will be able to get your app' console logs printed on ConsoleDebug app screen and share them to you to help troubleshoot a problem.


Here's what needs to be added to your project :


1) Copy "Console.cpp" and "Console.h" in your "src" folder

2) Add this to your ".pro" file :
QT += network
LIBS += -lbb

3) In "main.cpp", add everything that isn't already there :
#include "Console.h"  // <-- ADD THIS
#include <QSettings>  // <-- ADD THIS
#include "applicationui.hpp"
#include <bb/cascades/Application>
#include <QLocale>
#include <QTranslator>
#include <Qt/qdeclarativedebug.h>

using namespace bb::cascades;

void myMessageOutput(QtMsgType type, const char* msg) {  // <-- ADD THIS
    Q_UNUSED(type);  // <-- ADD THIS
    fprintf(stdout, "%s\n", msg);  // <-- ADD THIS
    fflush(stdout);  // <-- ADD THIS

    QSettings settings;  // <-- ADD THIS
    if (settings.value("sendToConsoleDebug", false).toBool()) {  // <-- ADD THIS
        Console* console = new Console();  // <-- ADD THIS
        console->sendMessage("ConsoleThis$$" + QString(msg));  // <-- ADD THIS
    }  // <-- ADD THIS
}  // <-- ADD THIS

Q_DECL_EXPORT int main(int argc, char **argv)
    Application app(argc, argv);

    qInstallMsgHandler(myMessageOutput);  // <-- ADD THIS

...
