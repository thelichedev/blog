Title: A taxonomy of software defects
Date: 2024-03-04
Tags: software quality

Defects are a big part of software development.
Those little or big critters keep crawling back into our beautifully crafted products.

When I was a young programmer, each new defect was a new adventure, a new monster to be slain.
After all those years doing this job, all defects look like all acquaintances.

> Why hello there mister validation bug, how are you this morning ? Long time no see since last week.
>- TheLicheDev, every Monday morning

Learning to recognize types of defects is an important part of my software development practice.

<!-- end-of-preview -->

A quick google search for "software bug taxonomy" always gives me the same kind of high-level approach.

Bugs are classified in general categories like "functionality / performance / usability / security / compatibility"
which makes sense and can be useful in some context. These taxonomies classify more the type of expectations impacted by the nonconformance.

It can help your team to manage the expectations of other teams or users
on the kind of delay to expect for a defect resolution, or the mitigation practices to enact after a defect.

I feel this kind of high level classification does not allow to improve the defect prevention practices and tools though.
My own mental framework is more oriented towards classifying the developer/team's "mistake" root causes.

I don't care if an off-by-one algorithm bug impacted only a minor functionality or introduced a big safety risk,
the tools I use to prevent this defect from reaching the user are still the same, and letting these bug reach the user
reflects just as badly on my development quality practices.

<aside>
Now, defects always have a wide range of root causes: the original code defect, but also the quality practices of the developer,
the quality culture of the team, management pressure, etc.

Here I focus only on the "technical root cause/code defect", because I'm only trying to evaluate and improve the quality practices of the developers.
</aside>

So how do you proceed to classify your newborn defect ?

You play a sort of little "guess who ?" game with your defect.

You ask a series of questions that leads (hopefully) to an identified type of defect,
and from here you can think about how best to react to this defect, which tool could catch this defect, etc.

## Guess who ?

Ask yourself these questions, in order:

![Question #01](/assets/defects_taxonomy/01_question_01.svg)

Your code is basically an algorithm expecting to be executed in a specific context.
As a developer, while you code, you think of the possible contexts and the expected results.

But writing a code that runs as you intend, is actually not guaranteed.
Depending on your language and tools it might even be a prowess in itself.

![Question #02](/assets/defects_taxonomy/02_question_02.svg)

Code units define request/response contracts with their client code.

Some function might be unhappy with the way your code units are talking to it.

![Contract bug](/assets/defects_taxonomy/03_contract_bug.svg)

Usually, **not respecting the request contract gives you an unexpected result.**
But maybe your respected the request contract, and the code unit still gave you an unexpected result.

Either it throws back an error to your face or, worse, it chooses to talk bad in return too.

<aside>
Note: part of the "compatibility" defects are in this categorie, when the problem is caused
by a change in the contract with a dependency of my code (the other part is classified in "validation bugs").
</aside>

Examples:
- You called `filterStuff('2021-01-01T00:00:00Z')` but `filterStuff` expects a date string in the `YYYY-MM-DD` format. `filterStuff` throws an error.  
- You called `addStuff(5, 'toto')` but `addStuff` expects 2 numbers. `addStuff` strangely chooses to return the string `'5toto'` instead of throwing and error.

The most basic of basic bugs.

The **fix is to respect the contract between the code units** (either change the contract of the provider, or change the client).

![Algorithm bug](/assets/defects_taxonomy/04_algorithm_bug.svg)

Your code units are calling each other nicely, but **in your algorithms some calculations or choices are wrong.**

Examples:
- The implementation of `addStuff(a, b)` is `return a * b`. Not adding up to expectations.
- The implementation of `loopOverStuff(listOfStuff)` loops one stuff more than there is in `listOfStuff`. Off by one error !

A slightly more evolved bug but still pretty basic.
What I call "the stupid programmer mistake".
I usually write those at a rate of a dozen per minute when I'm sober.

**The fix is to implement the correct algorithm** d'oh !

![Question #03](/assets/defects_taxonomy/05_question_03.svg) 

So first, congratulations, you actually wrote code that behaves as you think in the context you expect.
If you're writing code in Javascript you're already part of the elite.

But you still have a problem.
Is that because the context in which your code execute does not match your expectations ?

![Question #04](/assets/defects_taxonomy/06_question_04.svg)

Well your code executed in an unexpected context, but that does not mean this is the context's fault.
Maybe you didn't think of all the legitimate situations out there in the real world ?

![Incomplete algorithm bug](/assets/defects_taxonomy/07_incomplete_algorithm_bug.svg)

**The context is in an unforeseen but legitimate state.**

Of course your code doesn't handle it well.

Example:
- Your code synchronize banking data. You read banking transactions in an external service.<br />
  Some of the transactions (legitimately) have a total amount equal to zero.<br />
  As you never expected that, some part of your code that calculate VAT rates executes a division by zero.<br />
  After a few months you're stupefied to find `NaN` amounts in the VAT declarations of some of your customers !

**The fix is just to adapt your code to run in this legitimate context.**

![Validation bug](/assets/defects_taxonomy/09_validation_bug.svg) 

A lot of your code context comes from external sources: user inputs, external services, etc.
You can't control everything that enters your context, and you can't figure out in advance every possible data coming from those external sources.

Sometimes the problem is that **some invalid data made it into your execution context**.

Maybe the user typed some invalid input in a form, or the external service gave you a response that doesn't conform to the expected schema ?

<aside>
Note: part of the "compatibility" defects are in this categorie, when the problem is caused
by a change in the data returned by a dependency, that breaks the behavior of my code (the other part is classified in "contract bugs").
</aside>

Anyway the problem is that your code doesn't validate its context before trying to execute.

Examples: 
- Your customer is required to provide an amount value. Your UI input does not validate that the value typed is a number.<br />
  Your code happily stores `NaN` values in your database.<br />
- Your application use an external banking service to provide credit cards to your users.<br />
  Your read the list of credit cards belonging to a specific customer in the external service.<br />
  One day, the external service returns all the credit cards of all customers instead of just those belonging to the specified customer.<br />
  Since you never validated that the returned credit card belong to the expected customer,
  your application happily display the credit card of the all other customers, to each of your customers.

**The fix is to make it impossible to call your code with invalid values**.

![State machine defect](/assets/defects_taxonomy/10_state_machine_defect.svg)

Sometimes no external sources are responsible for an unexpected context.

Your code can be seen as a giant state machine: 
each code unit takes the current state (the context) and returns the new state (the context with some information updated).

**Every state transition might be correctly implemented, but your state machine could still be wrong.**
Basically, your units works in isolation, but they don't play nice together. They contradict themselves.

- some transitions might bring the context to a new state that can't be handled properly by the rest of your code.
- some transitions might undo the work of previous transitions, or prevent future transitions from working correctly.

Examples: 
- In your system, banking transactions can be linked to receipt documents.<br />
  These links are exclusives: a receipt document cannot be linked to multiple transactions.<br />
  Transactions can be soft-deleted, and re-activated.<br />
  Up to now, transactions-receipts are preserved while the transaction is in a soft-deleted state.<br />
  When a transaction is re-activated, its receipts are still linked to it.<br />
  Some users are complaining that the receipts linked to their soft-deleted transactions are not available to be linked to other transactions.<br />
  You choose to unlink receipts when a transaction is soft-deleted, making them available to other transactions.<br />
  Some other users are now complaining that their re-activated transactions are not linked to their receipts anymore, like they were previously.<br />
  Good luck !

**The fix is to redesign the state machine**, either to make the unexpected states/transitions impossible, or to make them compatible with all your code units.

![Question #06](/assets/defects_taxonomy/11_question_06.svg)

So your code execute correctly in the expected context, validates its external inputs, and the state machine is flawless.

What can possibly still go wrong ?

Well, one of the biggest enemies of developer is Time itself.
Time is a fickle bitch.

![Question #07](/assets/defects_taxonomy/12_question_07.svg)

With Time the problems usually fall into two categories:
- things do not happen in the expected order
- things takes too long to happen

![Scheduling defect](/assets/defects_taxonomy/13_scheduling_defect.svg)

Sometimes your state machine is well designed, but **the transitions do not occur in the order your expected**.

As a consequence, either your code rejects a transition as invalid in the current state (you're lucky)
or the transition is executed but leaves the context in an invalid, corrupted state (this is most likely).

Example:
- Your application implements a webhook, that an external service can call to inform your system of updates on some invoice documents.<br />
  Your application need to create or update the invoice documents in its database.<br />
  Sometimes you receive an update event for an invoice, before receiving the create event for the same invoices.<br />
  Because, network.<br />
  Your code however doesn't know how to update an invoice that doesn't exists in its database.<br />
  The update is lost. If you're lucky, at least the invoice is still created, you just don't have the last version.

**The fix is to make your code robust to out-of-order events.**
Either explicitly rejects them (and protect the context) or find a way to accept them out of order.

![Time performance defect](/assets/defects_taxonomy/14_time_performance_defect.svg)

So if everything happens in the right order and you still have a problem with time,
the only explanation left is that your code works, but **it takes too damn long to execute**.

Most likely this results in user frustration as your product is so sluggish it becomes unusable.
But it could also ends up with nasty errors as some calls to external services never resolve.
Your application might even crash after piling up thousands of state transitions all waiting for some other stuff to happen.

We've lost martian rovers to this stuff.

Example:
- You implement an audit to check that no banking transactions are missing in your customers account.<br />
  The algorithm is very clever and compare all transactions with their banking accounts balances history.<br />
  It's all fun and games, until the audit runs on your production server.<br />
  The server crashes.<br />
  The audit's computation is taking so long that your server's request queue just fills up all the available memory.<br />
  Then everything chokes to death.

**The fix is either to improve your code performance, or make your code resilient to bad performance.**

![Cost performance defect](/assets/defects_taxonomy/16_cost_performance_defect.svg)

Code might deliver value to the users but sometimes **the cost of the operation is too high**
and exceeds the price your users are willing to pay for your services.

Example:
- You are designing a new generation of brushless motors drives.<br />
  You choose to implement a bleeding edge current control algorithm in a FPGA chip.<br />
  The FPGA required to run your algorithm cost 150$ a piece.<br />
  The target market price for your drives was 100$.<br />
  Oops.

**The fix is to reduce the operation costs** (surprise !):
- improve performance
- discard less valuable but resources hungry features
- find cheaper providers for your external services

Or, just increase your own prices (ideally, after jailing your customers into your product).

![Usability defect](/assets/defects_taxonomy/17_usability_defect.svg)

Your code works but almost nobody knows it exists.
Those users who use your code struggle to make it fit their usage expectations.

One way or another, **your users don't use your code** as much as they could (or even not at all).

**The fix is probably a better design of your product, or just sometimes a better communication**.

![Product knowledge defect](/assets/defects_taxonomy/18_product_knowledge_defect.svg)

The good news is, your code actually works.

The bad news is, nobody cares.

You did everything perfectly unfortunately **you misunderstood yours users' expectations.**

- Some users use the feature once and never use it again.
- Some users continue to request similar features, and don't seem to acknowledge that "your product already does this stuff".

**The fix is to go back to understanding what your users need.**

![Safety defect](/assets/defects_taxonomy/19_safety_defect.svg)

Product safety includes a wide range of defects that could be the topic of another blog post.

Here I will just say that I consider safety with a broad scope:
- **privacy**: did you just leak your users personal data all over the internet ?
- **data/work loss**: (psychological safety ?) can your user loose hours of their work and personal data just by clicking on a button without confirmation ?
  (note: could be classified as a usability problem but data loss often cost a lot more to fix than usability or true accessibility,
  and the psychological effect on the user are more that their safety expectations are not met, rather than usage frictions)
- **physical safety**: well it doesn't apply to all software products, but sometimes a perfectly working product
  can actually be dangerous to operate for the user, and cause physical harm, or even just undue stress.
  And here I'm not talking about incidents caused by a software malfunction, but by an unsafe design of the product.

<aside>
Why is there a separate defect type for safety ?
After all, the root causes of most safety defects are probably one of Algorithm bug/State machine defect/Usability defect.

Yes, but in my experience, when a defect affects safety:
- the link between the bug and the safety nonconformance is very hard to discover before its impact.
- the consequences are usually much more disastrous (or have the potential to be).
- the solutions can also be very hard to implement in the case of psychological or physical safety.

So, while strictly speaking safety defects could indeed be classified as one of the preceding types of defects,
their special dimension make them a class apart in my mind, and I can't NOT give them a special entry of their own in this list.
</aside>

![No problem](/assets/defects_taxonomy/20_no_problem.svg)

Well if you're here you have no problem. Why are you here ?

## Complete questions tree

![Complete questions tree](/assets/defects_taxonomy/21_complete_tree.svg)

## Defects taxonomy

The final list of defects looks like this:

![Defects list](/assets/defects_taxonomy/22_defect_types.svg)

The switch from "bugs" to "defects" is intentional.
- **bugs** are trivial and you should aim to **never to deliver bugs to the user**.
  Consider the presence of bugs in the product as a software development malpractice.
  Unfortunately a lot of experimented developers still deliver trivial bugs in their day to day work.
- **defects** are the real nonconformance problems. They come with their own complexity,
  and require a real, constant prevention effort. They are never totally avoidable,
  but a lot of practices can have a big impact on their occurrence rate.

### Not all defects have the same complexity, nor the same nonconformance or fixing costs

The list above orders the defects from the most trivial to the most complex.
From the cheaper to detect, fix and prevent, to the most expensive.

**Contract bugs** happen all the time in code, and **they cost next to nothing** since good development tools and practices should immediately catch them.
Most of them are obvious if the developers at least make the effort to actually run their code just once before delivery.

**State machine defects** are a good example of bugs that are regularly caught in peer review.
Developers unfamiliar with a code unit, or with an insufficient quality practice, make modifications to the code
that will break other parts of the state machine, and **an expert need to intervene to explain the problem**.

**Product knowledge defects** regularly happen in product teams, and they are often found (long) after the expected feature
is delivered to the user. **They cost a lot to fix**, sometimes they are even impossible to fix and they just stay in the product.

**Each main category implies an order of magnitude in the defect complexity and cost.**
Algorithms bugs are 10 times easier than State machine defects, which are 10 times easier than Time defects, etc.

![Costs](/assets/defects_taxonomy/23_costs.svg)

### Impact on tools returns on investment

This cost scaling effect is important when you evaluate defect prevention tools.
Each of these tools has its own setup and operation costs, nothing is free in our work.

**Defect prevention tools should have a good return on investment** regarding their cost and the cost of the defects they can catch.

- A tool that only catches the most trivial tools but has high operation costs is of little interest.
- Using a tool with a high operation cost but able to catch the most complex defects can be justified.

**The best tools have a low operation costs in regard to the costs of the defects it catches.**

## How to use this taxonomy

On a superficial level, this defect taxonomy shows **everything that can go wrong in my day to day work**.

Software development is inherently complex (even when the developers don't add unrequired complexity themselves)
and the range of defects span multiple levels of conceptual complexity: from the humble algorithm bug
to the high level misunderstanding of the user's expectations, going through all type of integration and performance defects.

### Evaluate and choose the right defect detection tools

As a software developer, you have a set of tools at your disposition to detect and prevent defects.
Each tool has its strengths and costs, its scope of application.

Classifying the defects you find in your delivery allow to **identify the tool best adapted to prevent them**.

Ask yourself why your current tools didn't catch the defect ?  
Do you have a hole in my net that should be covered by a new tool ?  
Did you miss a step in my usage of my current tools ?  

### Evaluate your quality approach

It also allows you to **evaluate your current level of delivery quality**.

Not all bugs are equals, some are really trivial and should never make it into the delivered product.

Others are more complex, and require hard design choices to be contained.

Some are even consequences of the product design practice of my team and can totally escape my control.

## Conclusion

Next I'll look at some of you defect prevention tools and see how we can use this taxonomy,
and the [Defect Evaluation Grid](./defect_evaluation.html), to evaluate the return on investment of each tool.
