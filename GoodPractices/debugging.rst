Debugging
=========

Debugging. More commonly known as that thing you don't need to do because your code works always. Debugging is a natural part of the programming process, but knowing the best way to do it can be a challenge. Mainly because there isn't a 'right way' to do it.

Print statements
----------------
Print statements allow for quick and dirty debugging. They can be used to determine where in the code execution stops by placing many of them in succession in code. They can be as simple as printing out the letter 'A', then 'B' a few lines later, etc. or you can put in useful statements. This type of debugging is meant to be short-lived, and should be removed once the problem has been fixed.

Loggers
-------
Loggers can be used for debugging, but are more long term. Loggers are usually built into the language you're using because they require a bit more code than just print statements. Loggers generally include timestamps, as well as possible stacktraces, and can write to files more easily.

Filters
^^^^^^^
Filters are also an integral part of logging. They allow you to..well...filter what gets logged. This can be used to only allow logs of certain degrees of severity

.. tabs::

   .. code-tab:: java

       SEVERE
       WARNING
       INFO
       CONFIG
       FINE
       FINER
       FINEST

   .. code-tab:: c++

        // C++ doesn't include builtin logging. You will have to look online for some or create your own.
	


   .. code-tab:: py
        CRITICAL	50
        ERROR	40
        WARNING	30
        INFO	20
        DEBUG   10
        NOTSET  0

        Python log levels have numerical values associated, so setting the log level to 30 is the same as setting it to WARNING

By filtering out certain level codes, you can have logs that only show the most important info, and therefore you can sift through them faster, giving you more time to fix any bugs in the code.

NetConsole
----------

When you output anything to the console (via print statements to stdout) it will show up in both the log viewer on the dashboard, as well as on NetConsole. You can connect your computer to a RoboRIO and open the console viewer (using either the NI tool or `pynetconsole <https://github.com/robotpy/pynetconsole>`_) to view the console remotely.
