# Logging Guidelines

These guidelines aim to provide a good operational experience when it comes to logs, considering quality and quantity.  
We need a good balance so they are 1) not lacking 2) not cluttering and distracting from the relevant information (while being expen$ive).

## Baseline
At the very minimum, I recommend logging:
* The beginning of the interactions in your application and their outcomes - e.g. an http request and response, the consumption of a message, the execution of a cron job, etc. Include enough information to find a specific interaction when needed - e.g. an appropriate id given the context.
* The bootstrapping and shutdown of your application. It's common to have different components in your application where ordering of startup and shutdown matters - e.g. an http server or a message consumer and a database connection pool. Make sure to capture them and you will thank yourself if you ever face related problems. Your application may also have different configuration values for different environments, consider logging enough information to understand how it's being run.

Associated with those, I also recommend these practices:
* Use a centralized logging solution
* Use structured logging
* Use correlation ids for distributed systems (or a more sophisticated distributed tracing solution)
* Don’t log sensitive info
* Include exceptions in the log (and consider an error monitoring and reporting tool)
* Configure log rotation
* Don't use logs for what you should use metrics

## But I want more!
In some cases that baseline will be enough. In other cases, you might want to log more information to efficiently operate your application - notice the baseline does not log the internals of your application flows.  

These are a few common cases I recommend logging when it comes to internals of application flows:
* Fallbacks. Your application interacts with a dependency that is not a hard dependency and you actually recover and continue from it. Capture that as a warning so you understand what happened to produce the final outcome. (Btw if you can't recover, simply propagate the exception and let the proper higher up caller deal with the proper handling and logging. Enrich the exception at least at the bottom as you propagate it to provide it clear context.)
* Branching flows. Your application has important branching flows - e.g. coming from business needs and how you modeled it. Capture which flow your application has taken for the given interaction so you understand what happened. I would put idempotency in this same bucket, for example, and recommend logging when your application hits an idempotency check.
* State transitions. Knowing when and why data has changed is usually important, specially if your system is complex and data changes can originate from multiple flows. Consider not only state changes to your database in this case, but also other dependencies - e.g. a create resource call to another service, publishing an event to a message broker, etc.

## Closing
You probably don't need more than these.  
It's important to be intentional working with logs, and I hope these general guidelines will bring you some value, but as always, adjust to your context, and know when to break the rules!

# Contributing
If you would like to help making this project better, see the [CONTRIBUTING.md](CONTRIBUTING.md).  

# Maintainers
Send any other comments, flowers and suggestions to [André Schaffer](https://github.com/andreschaffer).

# License
This project is distributed under the [MIT License](LICENSE).
