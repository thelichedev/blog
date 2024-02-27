Title: A simple software quality evaluation framework
Date: 2024-02-21
Tags: software quality

Why should software development teams care about quality ?

I'm not going to elaborate much on the "why" here, I'll just borrow from the [Quality is free book by Philip Crosby][1]:

> Quality always pays for itself

And in my experience, quality is in fact a key competitive advantage.

Teams I've worked on, that adopted a quality standard of "zero-defect", actually delivered new product value orders of magnitude
faster than teams that "could never afford quality, because they needed to deliver fast".

But how do we know if we're doing ok on quality ?

<!-- end-of-preview -->

## Why do we need a framework to evaluate defects ?

I've always found that measuring quality directly is not easy. However measuring nonconformance always seemed to be easier.
When we find a defect in our product, there is evidence to analyse, and we can often evaluate the cost of the defect.
At least far better than the direct benefits of focusing on quality.

[Quality is free][1] also confirmed to me that the measurement of quality is the price of nonconformance.

I approach management of a software project as a basic series of steps:
- identify our current situation
- decide our objectives, where we want to be
- plan how we go from where we are, to where we want to be (or, how do we ensure we stay in the right place)

However, over the year I've worked on software developement teams, I've never seen teams monitor the price of their nonconformance.

Defects are discovered continuously but no effort is made to qualify them, to evaluate the current load of nonconformance on the team's work,
and even less to monitor tendencies.

So, deprived of any quality metric measurements to help me manage my teams' quality practices, 
I've had to develop an empiric model, first to evaluate my team current situation regarding quality.
This blog is an attempt to exteriorize this model.

Were all our defects an unavoidable cost to delivering new product value ? 

Should we improve our development or dectection tools ?

Are some defects symptomatic of a lack of quality consciouness in our practices ?

Please note that the approach described here is mainly oriented towards monitoring the quality tools and practices,
not finding the root causes of defects, or deciding on the corrective measures specific to each defect.

## First a few definitions

Again, a lot of this is borrowed from [Quality is free][1], because it greatly relates to my experiences as a software developer.

**Quality** is conformance to requirements. In a product-oriented team, requirements should mostly mean end-users requirements.

**Defect** or nonconformance as a consequence, is any aspect of our product that does not meet the end-user requirements.
The user is trying to use our product to do something, but our product is unable to provide this service to the user.

This is intentionally a much larger definition than the usual term of "software bug".

Defects can be caused by our team's failure to deliver a working feature, but also by required features altogether missing in our product.

**Customers vs users** are any human being who will have to use our products, including for example our coworkers in customer support teams.
Customers are users giving us money to use our product.

Here I use the two words indistinctly, because I don't distinguish between paying and non-paying users when I think of product quality.

I think this is legitimate since in both situations, nonquality will incur important costs:

- customers will not give us money
- support operation costs will rise

Different dynamics, same consequence: we loose money and users are not happy.

**Team vs developer vs manager** in this blog I'll refer to the "software team" for simplicity,
but I use the same framework at a more intimate level to evaluate my personal approach to quality.

In the same spirit, management expectations mostly apply to actual managers and lead developers at the team level,
but I also use them to "manage" my personal quality practices.

**Mature vs Immature team** (and developers, managers...) I've seen a wide range of quality approaches in my professional experience.

I'll speak about mature teams to describe teams where quality is a competitive advantage, and immature teams where quality is seen mainly as an unaffordable cost.

Mature teams exhibit (often inconsciously) control over their quality practices, immature teams exhibit an absence of control, or even of awareness of their quality problems.

## A simple sotware defect evaluation grid

I mainly use two axes to evaluate the defects in my teams' products:

- quality of delivery
- maturity of discovery

### Quality of Delivery

First, I try to evaluate the **quality of the delivery**:

- **delivery failures**: the user value was never delivered.
  Our users' expectations were never met by the product.

  This could be caused by defects in our implementation (what we call traditionnally "bugs") or by a missing part in the feature (we "forgot" to implement part of what was expected).

- **regression failures**: the user value was delivered, but was lost later on.
  Our users' expectations were met at some time, but now they're not satisfied anymore.

  This could be caused by errors in our implementation of a new feature (leading to the loss of an older feature),
  or any change in the operation context that prevents our old feature to work
  (eg. external services behaviour changes, tools retrocompatibility failures, load increases rendering the product unusable, etc).

- **missing requirement**: we never intended to satisfy this particular requirement of our users.

  It could be because we just discovered a feature we didn't know our users needed, 
  or because some process is not yet automated in our product and we accept that the user needs are answered by human customer support, 
  or even that we decide the opportunity cost to deliver the feature to the user is too high and we choose not to include it in our scope.

### Maturity of Discovery

Second, I try to evaluate **the maturity of the discovery** of nonconformance: the control my team or myself have over the defects.

I do this by looking at how far the defects made it into the product before they were found.

- **before development**: the defect never made it into the product implementation.

  It was found or prevented before the developer coded a new feature, or made impossible by the development process/tools.

- **during development**: the defect was found during the development of the feature.

  The developer's quality process/tools found or prevented the defect.

- **after developement but before delivery**: the defect was found before being delivered to the users.

  The users were never affected by the defect and in fact could never "see" it.

- **in delivery but before noticeable user impact**: the defect made it into delivery, but was found out before users operations were noticeably affected.
  Most importantly, we found (and fixed) the defect before users feedback came back through the customer support.

  Fixing the defect does not imply massively fixing users data, or informing the users of the defect.

- **after noticeable user impact**: we found the defect through direct users feedback, increase of customer support load, or discovery of users data corruption.

  Fixing the defect implies fixing users data, sometimes informing the users of the defect and apologizing.

### Defect evaluation grid

Those 2 axes give me a nice little grid, on which to place the defects found by my teams:

|                        | delivery failures | regression failures | missing value |
|:-----------------------|:-----------------:|:-------------------:|:-------------:|
| **before development** |         👾        |          👾         |       👾      |
| **during development** |         👾        |          👾         |       👾      |
| **before delivery**    |         👾        |          👾         |       👾      |
| **before user impact** |         👾        |          👾         |       👾      |
| **after user impact**  |         👾        |          👾         |       👾      |

The limits on the two axes are not clear-cut and are subject to appreciation:

- a defect can make a new feature unusable because of an interaction with an old feature, breaking the old feature in the process.
  The defect is a mix of delivery and regression failure.
- discovery of a missing feature often comes after the delivery of a new feature that does not solve the problem correctly.
  Analysis of user usage and feedback improves our knowledge of the user's needs, and we realize the value was never delivered, or required ;
  and another feature is required to meet the user's needs.
- if you include your customer support teams into the "users" of your product (as I always do), 
  the limit beetween a defect that impacts the users or a defect caught before impact, will vary accordingly.
  The same is true for the product managers. When a product manager finds a defect on a pre-delivery test environment,
  after your team considered the feature delivered and integrated in the product, does it count as a defect found before delivery or after ?

The Maturity of Discovery axis is a continuum, and the limits are more of a guide helping me to decide how I should react to a specific defect, or which tools to use to prevent/discover it.

## Evaluating quality with the grid

### Quality of Delivery

**Delivery failures are not acceptable** in a mature team, in our day and age.

A mature team should be able deliver new user value that works, reliably.
We have a lot of tooling and methodology to help us dramatically reduce delivery failures.

The worst immature teams I've worked on, made it a habit to deliver features that don't work reliably,
and showed no interest in improving on this point.

Immature teams can also try to deliver working software, but using such methods and tools that the costs are indeed too high for their context,
reinforcing the management's perception that quality is a cost that they cannot afford.

**Regression failures are much harder to control.**

Interactions between existing and new features are hard to predict and discover.

Again, we have tools at our disposal to detect those defects as soon as possible,
but in my experience it is unavoidable that some of these will find their way into delivery,
and impact users.

Some of the Regression failures also come from changes in our operating context,
like an external service changing a behaviour we rely on, and we have limited control over this.
Defensive programming techniques can help up detect the problem sooner but most of the time
it will impact our users.

**Missing requirements are actually a desirable kind of defects**.

You're discovering user value and you're updating your knowledge of the users expectations,
allowing you to adapt and maintain you competitive advantage.

However, in the most immature teams, making the difference between a Missing feature and a Delivery or Regression failure
is already a challenge in itself. Operations are constantly swamped by defects of all kinds,
and the delivered product value is poorly documented and thus poorly understood by the team.

### Maturity of Discovery

In my opinion there are 2 importants barriers:

- **delivery to the users** is the risk pivot point.
- **users discovery of the defects** is the trust pivot point.

**Every defect caught before delivery implies less risk** than after.

Of course, techniques and tools that actively prevent defects are invaluable.
As long as their cost does not make implementing new features prohibitive.
(We are engineers, our work is all about cost/quality tradeoffs).

On the opposite, **a defect being delivered to the user comes with a big risk**: the cost of discovery, diagnostic and the probability of
massively impacting operations are an order of magnitude above defects caught before delivery.

If all our defects are found and corrected before users outside of our team are impacted, we can develop **a relation of trust with our users**.
They will see our product as having a high quality standard.

I've worked on projects that achieved this, and it was always funny to see our users confronted with one of our defects,
but looking everywhere else into their context for the cause, because "it can't be in your application, your stuff always works".

On the opposite, teams that deliver products with a lot a defects impacting users, even mostly harmless ones, **will never build this trust capital**.

The humans using the product will always see its behaviour with suspiciousness.
Anytime they are confronted with a problem, they will suspect a defect in your product, and generate a lot of support requests.

The team's difficulties with diagnostic (the product is constantly flooded with errors) will increase the cost of this mistrust even more ;
it'll take time to exonerate our product when the defect is caused by another part of the context (eg misuse, miscommunication, defects in external services, etc).

It is one of the most important loss of product value that immature teams fail to understand.
Accepting that each new delivery comes with new defects, even mostly harmless ones, will make you loose a lot of trust capital with your users.

### Driving quality

Over the years I've developped my own objetives regarding my and my teams' defects.

|                           | delivery failures | regression failures | missing value |
|:--------------------------|:-----------------:|:-------------------:|:-------------:|
| **before development**    | ideal             | ideal               | ideal         |
| **during development**    | good              | ideal               | ideal         |
| **before delivery**       | average           | good                | ideal         |
| **before user impact**    | risky             | average             | good          |
| **after user impact**     | unacceptable      | risky               | average       |

- **ideal**: good but unattainable for most teams I've worked with
- **good**: in the best teams, most of the defects should be caught at this stage
- **average**: the stage where most of the defects are found if we don't make any particular effort, in the average team.
- **risky**: defects found at this stage, though not always avoidable, should be considered as materialisation of a risk.
- **unacceptable**: finding defects at this stage should not be considered acceptable, and should lead to a team/developper introspection ("never again").

Basically, we want to catch our defects sooner, to reduce their correction cost. So **we want to drive defects detection towards the top of the grid**.

Deliver failures basically mean that we are not in control of our delivery. We fail to deliver the promised product value.
This should make any team react energically to this kind of defects. Unfortunately in most teams it does not.

Missing value discovered before our users requested the feature means we are able to adate faster and maintain a competitive advantage
over our competitors.

**We want to eliminate defects on the left to make the defects on the right more apparent**.

So basically we want to drive defect away from the bottom left, towards the upper right.
**We want to drive defect away from "uncontrolled delivery", towards "new value discovery".**

### Managing quality risk

The grid also shows where I would expect risk management in a mature team.

Developers should **implement risk mitigation on the "average" line**, using enforcing tools to catch defects at this state, 
and monitoring metrics that show if there is a derive in the amount of defects reaching this stage.

Developers and managers should **actively manage the "risky" line**, monitoring metrics and correcting practices when the number of defects increase - independently of the defects gravity/cost.

Unfortunately in the immature teams, no active risk management is peformed on those kind of defects, 
until the consequences are too important to be ignored, but then it's already too late
to fix the underlying team culture.

**Any defect found in the "unacceptable" stage should be acted upon**. 
Actions should be taken to ensure that defects are found before this stage.
Root cause analysis should be performed to understand the underlying management practices that enabled this defect.

Finding excuses or considering those kind of defects as unavoidable should never happen. 

Unfortunately it regularly does in the immature teams.

## Conclusion

As software developers, we know that any software activity worth doing comes with a quality risk (risk of defects).

This is not the same as saying "we cannot afford quality", which is still today the approach to quality management in the vast majority of teams.
The difference is what I call **quality management**.

I feel there is also a scale of maturity for quality management:
- **level 0 management: ignorance** the team only react to defects according to their impact.
- **level 1 management: awareness** the team at least tries to monitor defects, but will still react only to sudden increases in the number of defects.
- **level 2 management: improvement** the team monitors defects, and actively tries to improve tools and culture in order to reach a "zero-defect in delivery" state, but is not there yet.
- **level 3 management: maturity** the team has made quality a core value of the development practice. The team as reached a state of (mostly) "zero-defect in delivery", and is capitalizing on the trust they gained from all actors in their context.

Level 3 is not a lofty ideal, I've worked on teams and projects that had reached this level of quality.

They were also the teams were the delivery was the fastest I ever experienced, orders of magnitude above the speed of level 0 teams, who can't afford quality.

[1]: https://en.wikipedia.org/wiki/Philip_B._Crosby#Quality_is_Free
