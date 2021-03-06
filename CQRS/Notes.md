CQRS - for separation, speed, scalability and testability
=========================================================

Presentation at http://prezi.com/ugduiowcpmt5/cqrs-for-separation-speed-scalability-and-testability/

also, https://docs.google.com/a/7digital.com/document/d/1rbQuM1SA4a5ouvnAHVhhJPi-3ivYWAv-DF0349l2Kj4/edit

Separation: So we can change code in response to changing requirements

Speed: Both of execution and development

Testability: means both automated tests and human interaction tests.

General Principals
------------------
* Different views are different domains
 - Different domains should be completely separable (deployable on different servers on different continents separable)
* Everything is either completely exposed or utterly hidden
 - Design by contract. Exposed means I can poke at it and use it to fulfill acceptance criteria
 - Exposure happens at well-defined endpoints
 - Endpoints are generally best grouped into hierarchies of concerns (cmds/qrys wrapped in services, wrapped in servers)
 - Splits in code also follow this -- methods wrapped in classes wrapped in solutions (one exposure project per solution works well)
* Don't wait for work (command THEN query, not command-and-query)
 - Do not be scared of eventual consistency.
 - Be careful with SQL setup. Prefer ability to read over ability to write
 - Denormalise data as far as is reasonable
* Every endpoint should have a very clear responsibility (SRP - what am I trying to do?)
* Chatty is better than heavy
 - Simpler to understand and predict
 - easier to fit to a purpose (think *NIX tools vs. big GUIs)
 - easier to change functionality or implementation
 - Be fast -- really, really fast!
* SRP: Don't forget it!

Examples
--------
I should go into some depth with these, probably showing some level of TDD (a good chance to show BDD!). They would 
probably flow quite well as a building-an-entire-system-from-the-ground-up type demo.

* Single domain CQ seperation (admin cmd, admin qry)
* Simple domain/view dislocation (publication)
* Multiple subscriber transforms per message (content: disc vs. del)
* Using endpoints to build layers (internal SDKs: PDKs)

Collateral improvements
-----------------------
* Different parts of a large project scale differently
 - 50 admins, 7x10^7 users: One server in the office for admin, 100s worldwide for disc/del
* Safety in separation
 - Fault tolerance (store&forward, redundant query endpoints, neither cmd nor qry can bring the whole system down)
 - Write-only databases and secured admin & messaging
* Message sending proxies are great testing endpoints
 - No need to test persistence or actually send messages in a command, just sending the right message is enough.
 - No need to fake existing DBs in message handling, create a sample message and check you do the right thing.
 - Denormalised databases are very easy to test
* Dependency de-coupling
 - Because solutions are independent (no shared dbs or configs etc), dependency versions aren't so important.
 - This means dependencies can be handled locally (PlatformBuild as an example)
 - This means less time in TeamCity! :-)

Anti-patterns
-------------
* Never send logging info through messaging (the queue is the log. Use event stores and dashboards)
* Queues are serial, try not to flood them. Don't try and do parallel tasks (this needs examples and careful explns.)

Other smells and principals (need to explain these well)
--------------------------------------------------------
* Don't log
* Never expose SQL, never share SQL
* Avoid SQL views
* BAN stored procedures
 - They are the wrong place to separate; they cause dependencies that can't be inverted and difficult to break. They can't be easily refactored and make other refactoring a pain.

Things to briefly mention
-------------------------
* Can swap out one service for another very easily (as long as contracts are OK)
* Can playback live messages into staging systems

A picture of communications
---------------------------
```
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
```

A picture of code
-----------------
```
SevenDigital.Something
 |
 + TestSpecs
 + Contracts
 | + ... (links to common contracts) ...
 + Persistence
 + Service
 + Host

```

Useful notes
------------
* http://nservicebus.com/ArchitecturalPrinciples.aspx