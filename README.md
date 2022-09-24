# Logging Guidelines

These guidelines aim to provide a good operational experience when it comes to logs, considering quality and quantity.  
We need a good balance so they are 1) not lacking 2) not cluttering and distracting from the relevant information (while being expen$ive).

## Baseline
At the very minimum, I recommend logging:
* The beginning of the interactions in your application and their outcomes, e.g. an http request and response, the consumption of a message, the execution of a cron job, etc. Include information enough to find a specific interaction when needed, e.g. an appropriate id given the context.
* The bootstrapping and shutdown of your application. It's common to have different components in your application where ordering of startup and shutdown matters - e.g. an http server or a message consumer and a database connection pool -, make sure to capture them and you will thank yourself when facing related problems. Your application may also have different configuration values for different environments, consider logging enough information to understand how it's been run.

Associated with those, I also recommend these practices:
* Use a centralized logging solution
* Use structured logging
* Use correlation ids for distributed systems (or a more sophisticated distributed tracing solution)
* Don’t log sensitive info
* Include exception in the log (and consider an error monitoring and reporting tool)
* Configure log rotation
* Don't use logs for what you should use metrics

## I want more!
In some cases that baseline will be enough. In other cases, you might want to log more information to efficiently operate your application - notice the baseline does not log the internal flows of your application when it's running.  
These are a few common cases I recommend logging:
* Fallbacks. Your application interacts with a dependency that is not a hard dependency and you can actually recover and continue from it. Capture that as a warning.
* TODO...

## A picture is worth a thousand words
Example of logging in a basic application flow:


    +---------+
    |  HTTP   +-----------------+
    |  Layer  |                 |
    +---------+                 |  +---------+
                                +->+  API    +--------+
    Log.info                       |  Layer  |        |
    + request/response             +---------+        |  +-----------+
                                                      +->+  Service  +--------+
                                   Log.info              |  Layer    |        |
                                   + public method       +-----------+        |  +---------------+
                                                                              +->+  Persistence  |....+
                                   Log.debug             Log.info                |  Layer        |    .
                                   + interesting         + public method         +---------------+    .
                                     internal methods                                                 .
                                                         Log.debug               Log.info             .  P
                                                         + interesting           + public method      .  o
              +-------------+                              internal methods                           .  t
              |  Exception  |     +------------+                                 Log.debug            .  e
              |  Handling   |     |  Can       |                                 + interesting        .  n
              |  Logic      +<----+  recover?  +<-+                                internal methods   .  t
              +-------------+ No  +------------+  |      +------------+                               .  i
                                                  |      |  Can       |                               .  a
              Log.error           Yes             +------+  recover?  +<---+                          .  l
                                  Log.warn         No    +------------+    |      +-------------+     .
                                                                           |      |  Exception  |     .
                                                         Yes               +------+  raised     +<....+
                                                         Log.warn                 +-------------+


And:





* In asynchronous flows, consider a logging callback
* Set _your application code_ log level to 'debug' in your tests and 'info' in production (change dynamically to 'debug' when in need)



These are general guidelines, know when to break the rules!

# Contributing
If you would like to help making this project better, see the [CONTRIBUTING.md](CONTRIBUTING.md).  

# Maintainers
Send any other comments, flowers and suggestions to [André Schaffer](https://github.com/andreschaffer).

# License
This project is distributed under the [MIT License](LICENSE).
