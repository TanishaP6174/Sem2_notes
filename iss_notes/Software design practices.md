
Let me go through every concept in this slide deck thoroughly.

---

# SOFTWARE OFFERINGS — SaaS, PaaS, IaaS and more

The core idea is a **spectrum of responsibility**. The further up the stack you go, the more the provider manages and the less you worry about.

## IaaS — Infrastructure as a Service

The provider gives you raw computing infrastructure — virtual machines, storage, networking. You manage everything above that: the OS, runtime, middleware, your application.

**You control**: Operating system, runtime, application code, data, middleware. **Provider controls**: Physical servers, networking hardware, data center, virtualization layer.

**Examples**: Amazon EC2, Google Compute Engine, Rackspace.

**When you use it**: You want full control over the environment. You're running something unusual that doesn't fit a standard platform. You need to fine-tune the OS or install specific software.

**Analogy**: Renting an empty plot of land. You build the house yourself, but you don't own the land.

## PaaS — Platform as a Service

The provider manages the infrastructure AND the runtime/OS layer. You just deploy your code and data. You don't think about servers, patching OS, or scaling infrastructure.

**You control**: Application code and data. **Provider controls**: Everything below — servers, OS, runtime, middleware, scaling.

**Examples**: Microsoft Azure App Service, Google App Engine, Heroku, AWS Elastic Beanstalk, Salesforce Platform.

**When you use it**: You want to focus purely on writing your application. You don't care what server it runs on. You want automatic scaling without managing infrastructure.

**Analogy**: Renting a fully furnished apartment. You bring your stuff (code), but the building, plumbing, electricity are all handled.

## SaaS — Software as a Service

A complete application delivered over the internet. You just use it through a browser or app. You control nothing technical — only your own data and configuration.

**You control**: Your data, user preferences, configuration options. **Provider controls**: Literally everything else.

**Examples**: Gmail, Slack, Salesforce CRM, Office 365, Zendesk, G Suite.

**When you use it**: You need a standard business function (email, CRM, project management) and don't need to build it yourself.

**Analogy**: Staying in a hotel. Everything is managed, you just show up and use it.

## Additional layers from the pyramid

**STaaS — Storage as a Service**: Cloud storage specifically. You store files/objects and access them via API without managing the storage hardware. Examples: Dropbox, Google Drive, Amazon S3, OneDrive, iCloud.

**DaaS — Data as a Service**: Managed database platforms. The database engine, replication, backups are managed for you — you just query data. Examples: DynamoDB, CouchDB, Cassandra, SQL Azure, Oracle Data Cloud.

**FaaS — Function as a Service** (also called Serverless): You deploy individual functions, not whole applications. The platform runs your function in response to events and you pay per invocation. No servers to manage at all — not even a "running application." Examples: AWS Lambda, Google Cloud Functions, Azure Functions, Apache OpenWhisk.

**The key insight about the pyramid**: Each layer abstracts away the one below it. IaaS at the bottom — most control, most responsibility. SaaS at the top — least control, least responsibility, least setup.

---

# SOFTWARE LICENSES

This is directly from slides 6 and 7. Understanding the differences matters conceptually.

## Public Domain

No copyright at all. Anyone can do anything — copy, modify, distribute, sublicense, use commercially. The creator has given up all rights. Examples: SQLite, ImageJ.

## Permissive FOSS (Free and Open Source Software)

Open source, copyright retained by creator, but very few restrictions on what you can do with it. You can use it commercially, modify it, distribute it, even sublicense it. The main requirement is usually just attribution — keep the original license notice. Examples: Apache web server, BSD-licensed code, MIT license.

**Key trait**: Derivative work can be released under a _different_ license, including a proprietary one. So a company can take MIT-licensed code, modify it, and sell the modified version as closed-source. This is why many corporations love permissive licenses.

## Copyleft FOSS (e.g., GPL — General Public License)

Open source, but with a crucial condition: if you modify and distribute the software, your derivative work must also be open source under the same license. This is called "viral" licensing — the openness propagates.

**Key traits**: Source must be open. Derivative work cannot be released as proprietary software. You must attribute the original creator. Cannot sublicense (release under a different license).

**Why this matters**: If you build a product using GPL code and distribute it, you're forced to release your source code too. This is why corporations are often cautious about GPL dependencies.

Examples: Linux kernel, GIMP, OBS.

## Freeware

Free to use, but NOT open source. You can't see the source code, can't modify it, may not be able to redistribute freely. The copyright owner retains full control — they've just decided to offer it at no cost. Examples: Winamp, Irfanview.

## Proprietary License

Standard commercial software. All rights reserved. You typically pay for a license to use it. No right to copy, modify, or distribute. Source code is secret. Examples: Windows, Spotify, most commercial software.

## Copyright vs Copyleft vs Permissive vs Creative Commons summary

**Copyright**: Creator dictates everything. You can use it as they allow.

**Copyleft**: Open but "viral" — derivatives must stay open under same license. Creator is liable for bugs.

**Permissive**: Open with minimal restrictions. You can even take it proprietary. Creator not liable for bugs.

**Creative Commons**: Mainly for creative works (not code). Various flavors (CC-BY, CC-BY-SA, etc.). Generally no restrictions on source code distribution terms. Creator not liable for bugs.

---

# SOFTWARE ENGINEERING TEAMS

From slide 8, these are the roles in a typical software team:

**Product Manager**: Owns the product vision and roadmap. Decides what to build and why, based on business goals and user needs. Prioritizes the backlog. The "what and why" person.

**Business Analyst**: Bridges business stakeholders and the tech team. Translates business requirements into specifications developers can work with. Documents processes.

**UI/UX Designer**: Designs the user interface and experience. Creates wireframes, mockups, prototypes. Ensures the product is usable and intuitive.

**Engineering Manager**: Manages the engineering team — hiring, performance, process, team health. Less hands-on coding, more people and process management.

**Team (Technical) Lead**: Senior technical person leading the development team. Makes technical decisions, does code reviews, mentors developers, ensures technical quality.

**Software Architect**: Designs the high-level structure of the system. Decides on technology choices, system components, how services communicate. Thinks about scalability, security, maintainability at the system level.

**Software Developers**: Write the actual code. Implement features, fix bugs, write tests.

**Tester / QA Engineer**: Tests the software to find bugs before users do. Writes test plans, performs manual and automated testing, ensures quality.

**Scrum Master**: Facilitates the Scrum process. Removes blockers for the team, runs ceremonies (standups, retrospectives), protects the team from external interference. Not a manager — a facilitator.

---

# SOFTWARE DEVELOPMENT LIFE CYCLE (SDLC)

The SDLC is the process of planning, creating, testing, and deploying software. Several models exist:

## Waterfall

Sequential, linear phases: Requirements → Design → Implementation → Testing → Deployment → Maintenance. Each phase completes fully before the next begins. You don't go back.

**Problem**: By the time you finish, requirements may have changed. Bugs found in testing are expensive to fix because you have to go back through the whole process. Very rigid.

## Agile

Iterative and incremental. Work is done in short cycles (sprints, typically 1-4 weeks). Each sprint delivers working software. Requirements evolve. Constant feedback from stakeholders.

**Core values (Agile Manifesto)**:

- Individuals and interactions over processes and tools
- Working software over comprehensive documentation
- Customer collaboration over contract negotiation
- Responding to change over following a plan

## Scrum — the specific Agile framework shown in slide 9

Scrum is the most common Agile implementation. It has specific roles, ceremonies, and artifacts.

**Roles**:

- Product Owner: Owns the product backlog, prioritizes work, represents stakeholders
- Scrum Master: Facilitates the process, removes impediments
- Development Team: Self-organizing team that builds the product

**Artifacts**:

- **Product Backlog**: The master list of all work — features, bug fixes, improvements. Ordered by priority. Maintained by the Product Owner.
- **Sprint Backlog**: The subset of product backlog items the team commits to completing in this sprint.
- **Increment**: The working software produced at the end of a sprint.

**Ceremonies**:

- **Sprint Planning**: Team selects items from product backlog for the sprint and plans how to do them.
- **Daily Scrum (standup)**: 15-minute daily sync. Each person answers: what did I do yesterday, what will I do today, any blockers?
- **Sprint Review**: Demo working software to stakeholders at end of sprint. Get feedback.
- **Sprint Retrospective**: Team reflects on the process — what went well, what to improve.

**Sprint cycle**: 1-4 weeks. At the end of each sprint you have potentially shippable software. You don't wait months to show something working.

## Kanban

Another Agile approach. Work items flow through columns (To Do → In Progress → In Review → Done). No fixed sprints. Continuous flow. The key constraint is **WIP limits** (Work In Progress limits) — you cap how many items can be in each column at once. This prevents overloading and forces finishing before starting new work.

The slide 10 (Lead Time diagram) shows a Kanban board: Backlog → Ready to Develop → In Progress → In Code Review → In Testing → Verified → Done. "Max" indicators on some columns are the WIP limits. **Lead Time** is measured from when an item enters the board to when it reaches Done.

## SAFe — Scaled Agile Framework (slide 11)

SAFe is how large enterprises apply Agile at scale — when you have not one team but 50 teams all working on the same product. It adds layers:

- **Team level**: Individual Scrum/Kanban teams
- **Program level**: Multiple teams aligned in an "Agile Release Train" (ART), working toward shared goals in Program Increments (PI, typically ~10 weeks of work)
- **Portfolio level**: Strategic alignment, budgeting, long-term roadmap

Key concepts: PI Planning (big planning event for all teams), System Demos (periodic demos of integrated work), Continuous Delivery Pipeline.

---

# CI/CD — CONTINUOUS INTEGRATION AND CONTINUOUS DELIVERY

This is one of the most important modern software engineering concepts. Slides 12 and 13 cover it.

## The problem CI/CD solves

In old-style development, developers worked in isolation for weeks or months, then tried to merge everything together. "Integration hell" — everything broke because code had diverged massively. Releasing software took months of manual effort.

## Continuous Integration (CI)

The practice of merging developer code into a shared repository **frequently** (multiple times per day). Every merge triggers an automated pipeline:

1. **Code commit** to version control (Git)
2. **Compile** — build the code
3. **Package** — bundle it
4. **Automated unit tests** — test individual functions/components
5. **Automated UI tests** — test the interface

If any step fails, the developer is notified immediately. The broken code is never allowed to sit unfixed — fix it now while it's fresh.

**Goal**: Always have a working, tested build. Problems are caught within hours, not months.

## Continuous Delivery (CD)

Extends CI through the release pipeline. After CI succeeds, CD automates the delivery process:

1. **Package with instructions** — ready to deploy
2. **Operations team** (or automation) takes over
3. **Auto scripts** deploy to test environment
4. **Test environment** — integration testing, acceptance testing
5. **Testing** — more automated checks
6. **Public/General Availability** — ship to users

With Continuous Delivery, you can release to production at any time at the push of a button. **Continuous Deployment** goes one step further — every passing build is automatically deployed to production with no human intervention.

**Difference**: Continuous Delivery = can deploy anytime, human decides when. Continuous Deployment = deploys automatically every time.

## Tools in the CI/CD ecosystem (slide 13)

**Source control**: GitHub, GitLab — where code lives, triggers CI on push.

**Build tools**: Gradle, webpack — compile and bundle code.

**Testing**: Jest (JS unit testing), Playwright (end-to-end browser testing), JUnit (Java testing).

**CI servers/orchestration**: Jenkins, Buildkite — run the automated pipeline.

**Planning/tracking**: Jira (issue tracking), Confluence (documentation).

**Deployment**: Docker (containerization), Kubernetes (container orchestration), Terraform (infrastructure as code), Argo (GitOps deployment), AWS Lambda.

**Monitoring**: Prometheus (metrics collection), Datadog (monitoring and alerting).

The infinity loop in slide 13 represents the continuous cycle: plan → code → build → test (CI loop) → release → deploy → operate → monitor (CD loop) → back to plan.

---

# DEVOPS AND MLOPS

## DevOps

DevOps is a **culture and practice** that breaks down the wall between Development (who builds software) and Operations (who runs and maintains it). Traditionally these were separate teams with conflicting goals — devs wanted to ship fast, ops wanted stability.

DevOps merges them: the same team builds, deploys, and operates software. "You build it, you run it." This creates accountability — developers care about production stability because they're the ones paged at 3am when it breaks.

The infinity loop in slide 14 shows the DevOps cycle: Plan → Build → Integration → Deploy → Operate — continuous feedback between all stages.

**Key practices**: Infrastructure as Code (treat servers like code, version it), automated testing, CI/CD pipelines, monitoring everything, fast incident response.

## MLOps (slide 15)

MLOps applies the same DevOps principles to Machine Learning. ML has unique challenges that regular DevOps doesn't handle:

**Why ML is different**:

- You version both code AND data (the training dataset matters as much as the model code)
- Models need to be retrained when data changes or drifts
- Model performance degrades over time even without code changes (concept drift)
- Experiments need tracking (hyperparameters, metrics, model versions)

**MLOps pipeline**:

1. **Data Management**: Data engineers collect, clean, version training data. DataOps CI/CD platform manages data pipelines.
2. **Model Development**: Data scientists train and evaluate models. Feature Store stores computed features (to avoid recomputing them).
3. **Model Deployment**: MLOps CI/CD platform packages and deploys models.
4. **Monitoring and Logging**: Monitor model performance in production. Is accuracy degrading? Are predictions drifting?
5. **Model Governance**: Track which model is in production, who approved it, compliance.

**MLOps = ML + DEV + OPS**:

- ML: Experiment, data acquisition, initial modeling
- DEV: Develop, modeling + testing, CI, CD
- OPS: Operate, continuous delivery, data feedback loop, system + model monitoring

---

# SOFTWARE PRODUCTIVITY METRICS

Slide 16 shows three productivity measurement frameworks.

## DORA Metrics (2018) — 4 key metrics

DORA (DevOps Research and Assessment) identified these four metrics as the best predictors of software delivery performance:

**1. Lead Time for Changes**: Time from code commit to running in production. Measures how fast you can get a change out. Elite performers: less than 1 hour. Poor performers: more than 6 months.

**2. Deployment Frequency**: How often you deploy to production. Elite: multiple times per day. Poor: less than once every 6 months.

**3. Change Failure Rate**: What percentage of deployments cause a failure in production (requiring a hotfix, rollback, or patch). Elite: 0-15%. Poor: 46-60%.

**4. Mean Time to Recovery (MTTR)**: How long it takes to recover from a failure in production. Elite: less than 1 hour. Poor: more than 6 months.

**The insight**: High-performing teams deploy frequently AND have low failure rates AND recover fast. These are NOT in tension — you don't have to choose between speed and stability.

## SPACE Framework (2021) — 5 dimensions

SPACE takes a broader view of developer productivity — it argues you can't measure productivity with one metric.

**S — Satisfaction and Well-being**: Are developers happy? Satisfied with their tools and work? Burnout is a productivity killer.

**P — Performance**: Does the output actually work? Does it meet requirements? Quality of outcomes, not just output volume.

**A — Activity**: Measurable things developers do — commits, PRs merged, code reviews, builds. Useful signal but dangerous if used alone (gaming metrics).

**C — Communication and Collaboration**: How well does the team communicate? How smoothly do reviews and handoffs work?

**E — Efficiency and Flow**: Can developers work without interruptions? Flow state — uninterrupted focused work time — is where deep productive work happens.

**Key insight**: No single dimension tells the whole story. A team might have high activity (lots of commits) but low performance (code is buggy) and low satisfaction (everyone is burning out).

## Flow Metrics (2018) — 5 key metrics

Flow metrics focus on how work items move through the development pipeline:

**Flow Velocity**: How many work items are completed per unit time. How fast is the team delivering?

**Flow Efficiency**: Ratio of active work time to total time. A ticket that takes 10 days but was only actively worked on for 1 day has 10% flow efficiency. High waiting time = low efficiency.

**Flow Time**: Total time from when work starts to when it's done (similar to Lead Time).

**Flow Load**: Number of work items in progress simultaneously. Too much = context switching = slower delivery.

**Flow Distribution**: What types of work are being done? Features vs bugs vs technical debt vs risk. A team spending 80% on bug fixes has a different problem than one spending 80% on new features.

---

# TECHNICAL DEBT

Technical debt is the accumulated cost of shortcuts, quick fixes, and suboptimal decisions made during development. Like financial debt, it accrues interest — the longer you leave it, the more it costs you in slowdown, bugs, and difficulty adding new features.

## The Technical Debt Quadrants (slide 17)

Two axes: **Deliberate vs Inadvertent** and **Reckless vs Prudent**.

**Deliberate + Reckless — "We don't have time"**: The team knows the right way to do it but consciously cuts corners because of deadline pressure. This is the worst kind — knowingly doing the wrong thing. Debt is invisible to management and accumulates fast.

**Inadvertent + Reckless — "We don't know how"**: The team doesn't even realize they're creating debt. Junior developers writing messy code not knowing better. This happens when there's no code review, no mentorship, no standards.

**Deliberate + Prudent — "We'll deal with it later"**: The team makes a conscious, informed decision to take a shortcut now and schedule cleanup later. This is sometimes the right business decision — shipping fast has value. But "later" must actually happen.

**Inadvertent + Prudent — "We shouldn't have done that"**: The team made a reasonable decision at the time but later realized it was wrong as they learned more. This is unavoidable and acceptable — you can't know everything upfront.

**The key insight**: Not all technical debt is bad. Deliberate+Prudent debt is a legitimate business tool. The dangerous kinds are the reckless ones — especially inadvertent reckless, because you don't even know you have it.

**Paying off technical debt (Refactoring)**: Restructuring existing code without changing its external behavior. Cleaning up messy code, renaming things properly, breaking large functions into smaller ones. This reduces debt.

---

# SOFTWARE DEVELOPMENT WASTE (slide 18)

These are the 9 categories of waste in software development — things teams do that don't contribute to delivering value:

**Building the wrong feature or product**: Spending time building something nobody asked for or nobody uses. The most expensive waste — all that effort produces zero value. Root cause: poor requirements gathering, insufficient user research.

**Mismanaging the backlog**: Having a backlog full of irrelevant, outdated, or improperly prioritized items. Teams waste time discussing and re-discussing items that should have been removed. Symptoms: huge backlogs nobody reads, items that have been there for years.

**Rework**: Fixing things that were done incorrectly the first time. Every hour of rework is a double spend — you paid once to build it wrong, and again to fix it. Root causes: unclear requirements, insufficient testing, poor code review.

**Unnecessarily complex solutions**: Over-engineering. Building a complex, general solution when a simple specific one would do. "We might need this flexibility someday" — but someday never comes. The complexity slows down everyone who touches that code forever.

**Extraneous cognitive load**: Mental overhead that doesn't contribute to the work itself. Confusing codebases, bad variable names, unnecessary documentation, too many tools, constant context switching. When developers have to spend mental energy figuring out the system rather than solving the problem, productivity drops.

**Psychological distress**: Stress, anxiety, burnout, conflict within the team. A team where people are afraid to speak up, or where there's interpersonal conflict, or where people are overworked, delivers far less than a healthy team. Often invisible to managers until people quit.

**Knowledge loss**: When knowledge exists only in one person's head and that person leaves, the team suffers. Poor documentation, no knowledge sharing, key-person dependencies. Every time you have to rediscover how something works, that's waste.

**Waiting/multitasking**: Time spent waiting for decisions, approvals, other teams, environments, code reviews. Waiting is pure waste — no value is created. Multitasking as a response to waiting creates its own waste — context switching costs significant mental overhead. Studies show it can take 20+ minutes to fully regain focus after an interruption.

**Ineffective communication**: Miscommunication between team members, between teams, between developers and stakeholders. Requirements misunderstood, features built differently than expected, time spent in meetings that produce no decisions. Every miscommunication creates rework downstream.

---

## How everything connects

The whole picture is this: software teams use **Agile methods** (Scrum/Kanban) to organize work. They use **CI/CD** to deliver that work continuously and safely. **DevOps** culture ensures the people building and running the software are aligned. **MLOps** extends this to ML systems. Productivity is measured with frameworks like **DORA, SPACE, and Flow**. Teams accumulate **technical debt** over time and must manage it consciously. And throughout, they try to identify and eliminate **waste** in all its forms.

For your exam, the most conceptually rich areas are: the CI/CD pipeline (what each stage does and why), the DORA metrics (what they measure and what "good" looks like), the technical debt quadrants (be able to classify a given scenario into a quadrant), and the waste categories (be able to recognize an example of each). The cloud service models (IaaS/PaaS/SaaS) are almost certainly worth a question — focus on what each layer manages and who controls what.