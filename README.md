# Logging Guidelines

These guidelines aim to provide a good operational experience when it comes to logs, considering quality and quantity.  
We need a good balance so they are 1) not lacking 2) not cluttering and distracting from the relevant information (while being expen$ive).

## A picture is worth a thousand words
Example of logging in a basic application flow


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
* Use a centralized logging solution
* Use structured logging
* Use correlation ids for distributed systems
* Don’t log sensitive info
* Include exception in the log (and consider an error monitoring and reporting tool)
* In asynchronous flows, consider a logging callback
* Set _your application code_ log level to 'debug' in your tests and 'info' in production (change dynamically to 'debug' when in need)
* Configure log rotation
* Don't use logs for what you should use metrics

These are general guidelines, know when to break the rules!

# Contributing
If you would like to help making this project better, see the [CONTRIBUTING.md](CONTRIBUTING.md).  

# Maintainers
Send any other comments, flowers and suggestions to [André Schaffer](https://github.com/andreschaffer).

# License
This project is distributed under the [MIT License](LICENSE).
