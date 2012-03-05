CQRS - for separation, speed, scalability and testability
=========================================================

Separation: So we can change code in response to changing requirements

Speed: Both of execution and development

Testability: means both automated tests and human interaction tests.

General Principals
------------------
*Different views are different domains
*Everything is either completely exposed or utterly hidden
 - Design by contract. Exposed means I can poke at it and use it to fulfill acceptance criteria
 - Exposure happens at well-defined endpoints
*Don't wait for work (command THEN query, not command-and-query)
*Every endpoint should have a very clear responsibility (SRP - what am I trying to do?)
*Chatty is better than heavy
 - Simpler to understand and predict
 - easier to fit to a purpose (think *NIX tools vs. big GUIs)
 - easier to change functionality or implementation
 - Be fast -- really, really fast!
*Different domains should be completely separable (deployable on different servers on different continents separable)
*SRP: Don't forget it!
*Do not be scared of eventual consistency.

Examples
--------
*Single domain CQ seperation (admin cmd, admin qry)
*Simple domain/view dislocation (publication)
*Multiple subscriber transforms per message (content: disc vs. del)
*Using endpoints to build layers (internal SDKs: PDKs)

Collateral improvements
-----------------------
*Different parts of a large project scale differently
 - 50 admins, 7x10^7 users: One server in the office for admin, 100s worldwide for disc/del
*Safety in separation
 - Fault tolerance (store&forward, redundant query endpoints, neither cmd nor qry can bring the whole system down)
 - Write-only databases and secured admin & messaging
*Message sending proxies are great testing endpoints
 - No need to test persistence or actually send messages in a command, just sending the right message is enough.
 - No need to fake existing DBs in message handling, create a sample message and check you do the right thing.
 - Denormalised databases are very easy to test

Anti-patterns
-------------
*Never send logging info through messaging (the queue is the log. Use event stores and dashboards)
*Queues are serial, try not to flood them. Don't try and do parallel tasks (this needs examples and careful explns.)

Other smells and principals (need to explain these well)
--------------------------------------------------------
*Don't log
*Never expose SQL, never share SQL
*Avoid SQL views
*BAN stored procedures
 - They are the wrong place to separate; they cause dependencies that can't be inverted and difficult to break. They can't be easily refactored and make other refactoring a pain.


A picture
---------
          +---------------+
(cmd) --> | Cmd responder | --> +----------+              +---------+     +---------+
          +---------------+     | serv bus |              | msg bus | --> | Handler |
                __|__           +----------+              +---------+     +---------+
               (_____)                |                        |             __|__
               |_____|            ( store ) -- forward --> ( store )        (_____)
                                                                            |_____|
                                                                               |
                                                                           +---------------+
                                                               (qry) <--   | Qry responder |
                                                                           +---------------+
