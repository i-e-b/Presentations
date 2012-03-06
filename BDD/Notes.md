BDD: Principles, practice, workflows
====================================

Maybe do this as "a day in the life of a BDD team"?

To cover:

* Effective pairing
 - Clear roles (driver, navigator) and their responsibilities
* Reducing friction
 - Anything that is not decision is friction (and what that means)
   * Waiting for software (esp unit tests, but including VS), typing, debugging.
   * decision is [team|lone] discussions, thought, business analysis, story breakdowns...
 - Don't build systems that can't be moved.
   * Requiring a specific version of an OS is baaaad! (a minimum version is OK)
   * Should be able to deploy to new environments with the minimum of fuss. Auto-hookup is great, but minimum configuration is OK
* Test behaviour
 - Anything that affects behaviour (incl. config files etc.)
 - Behaviour happens at many levels (from the method all the way up to the whole system)
   * Each solution should be only working at one or maybe two levels!
* Tests that match acceptance criteria
 - Fluency
 - Don't test anything smaller than a method. If your methods do more than one thing, they are too big. If they change lots of state they are probably wrong.
 - Orthogonal to production code (separate namespaces, units != methods etc.)
   * when code starts maching behavioural tests, you're going the right way :-)
* Team ownership vs. product ownership
 - Team must own 'good', to be guided but not decided by PO
 - Product owner has full control of priority

* Ensuring focus
 - Meaningful board -- asking "what are you doing" is banned! (or at least, should be redundant)
 - Catch Yak Shaving, acknowledge and minimise thrash (in self and team-mates)
 - Build servers stay green. A red build is all-hands-on-deck.
 - Randomisation is your enemy! Reduce distractions (silence emails, im; get people used to waiting)
 - If needed, block out time for other duties
 - Don't wander off. Your team is your first responsibility.
* Maintain flow
 - done != business value
 - Make sure acceptance crit are met in a professional and production-ready way
 - Tracking business value is for prioritisation.

Workflow
--------
* Planning and priority
 - Short roadmap, short backlog, regular prioritisation (not random!)
* Studdles and in-team estimation
 - Spikes and discussions
 - Work out disputes before estimation
 - Stick to estimate even if it seems inaccurate later
* Maintain visibility
 - All work is to acceptance criteria (no task-based stories)
 - Everything in 'done' represents system as it is on live
 - Everything in 'delivered' represents UAT
 - Backlog represents how we want the system to be


