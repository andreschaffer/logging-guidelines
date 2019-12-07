# Logging Guidelines

These guidelines aim to provide a good operational experience when it comes to logs, considering quality and quantity.  
We need a good balance so they are 1) not lacking 2) not cluttering and distracting from the relevant information (while being expensive).

## A picture is worth a thousand words
Example of logging in a basic application flow

    +------------+
    |            |
    | HTTP layer +------------------+
    |            |                  |  +-------------+
    +------------+                  |  |             |
                                    +->+  API layer  +-----+
    Log.info                           |             |     |  +-----------------+
    + request/response                 +-------------+     |  |                 |
                                                           +->+  Service layer  +-----+
                                       Log.info               |                 |     |  +-------------------+
                                       + public method        +-----------------+     |  |                   |
                                                                                      +->+ Persistence layer |....+
                                       Log.debug              Log.info                   |                   |    .
                                       + interesting          + public method            +-------------------+    .
                                         internal methods                                                         .
                                                              Log.debug                  Log.info                 .  P
                                                              + interesting              + public method          .  o
                                                                internal methods                                  .  t
                  +-------------+                                                        Log.debug                .  e
                  |  Exception  |     +--------------+                                   + interesting            .  n
                  |  Handling   +<----+ Can recover? +<-+                                  internal methods       .  t
                  |  Logic      | No  +--------------+  |                                                         .  i
                  +-------------+                       |      +--------------+                                   .  a
                                      Yes               +------+ Can recover? +<-+                                .  l
                  Log.error           Log.warn           No    +--------------+  |                                .
                                                                                 |       +------------------+     .
                                                               Yes               +-------+ Exception raised +<....+
                                                               Log.warn                  +------------------+

* Use a centralized logging solution
* Use structured logging
* Use correlation ids for distributed systems
* Don’t log sensitive info
* Include exception in the log
* By the way don’t use exceptions to control application flow

These are general guidelines, know when to break the rules!

