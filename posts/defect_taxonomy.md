Title: A taxonomy of software defects
Date: 2024-03-04
Tags: software quality

Defects play a significant role in software development.
Those little critters keep crawling back into our beautifully crafted products.

When I was a young programmer, each new defect felt like a new adventure, a new monster to be slain.
After all those years doing this job, all defects look like all old acquaintances.

Recognizing different defect types is crucial in my software development routine.

<!-- end-of-preview -->

A quick online search for "software bug taxonomy" yields to broad classifications, like functionality, performance, usability, security, compatibility.
These groupings primarily address the type of expectations impacted by the nonconformance.

Such taxonomies can help teams to manage the expectations of users or stakeholders,
regarding defect resolution timelines and mitigation strategies.

Management often responds to defects based on their perceived impact severity.
Benign defects may be disregarded, while even minor errors resulting in significant financial or credibility losses can provoke exaggerated corrective actions.

Yet in my opinion, focusing on the consequences of defects is insufficient for improving defect prevention practices and tools.
My own approach leans more towards identifying the nature of the original developer mistake.

Whether a bug affects minor functionality or poses a significant risk,
the tools I use to prevent this defect from reaching the user remain consistent,
and allowing any bug to slip through reflects just as badly on my development quality practices.

<aside>
Defects always have a wide range of root causes: the original code defect, but also the quality practices of the developer,
the quality culture of the team, management pressure, etc.

Here I focus only on the "technical nature" of the defect in code, because I'm only trying to evaluate and improve the quality practices of the developers.
</aside>

So how do we proceed to classify a defect ?

We play a sort of little "guess who ?" game with the defect, asking a series of questions to pinpoint its type.

This idendification can guide our reponses to the defect, and help determine which tools could effectively catch similar issues in the future.

## Guess who ?

![Does the code work as expected, in the expected context?](/assets/defects_taxonomy/01_question_01.svg)

Our code is basically an algorithm expecting to be executed in a specific context.

When coding, we anticipate the possible execution contexts and the expected results.

Yet, ensuring code behaves precisely as intended isn't always a given.
Depending on language and tools used, it might even be quite an accomplishment in itself.

![Are the contracts between code units respected?](/assets/defects_taxonomy/02_question_02.svg)

Code units establish request/response contracts with their client code.

Some function might be unhappy with the way our code units are calling it ?
Maybe we respected the request contract, and the function still gave us an unexpected response ?

![Contract bug](/assets/defects_taxonomy/03_contract_bug.svg)

Usually, when contracts aren't adhered to, execution results in errors or unexpected behavior.
I call this type of bug a **contract bug**.

<aside>
Note: part of the "compatibility" defects are in this categorie, when the problem is caused
by a change in the contract with a dependency of our code (the other part is classified in "validation bugs").
</aside>

Examples:
- We called `filterStuff('2021-01-01T00:00:00Z')` but `filterStuff` expects a date string in the `YYYY-MM-DD` format. `filterStuff` throws an error.
- We called `addStuff(5, 'toto')` but `addStuff` expects two numbers. `addStuff` strangely chooses to return the string `'5toto'` instead of throwing and error.

The most basic of basic bugs.

Resolving contract bugs involves **ensuring adherence to the contract between code units**, either by adjusting the contract of the provider or updating the client code accordingly.

![Algorithm bug](/assets/defects_taxonomy/04_algorithm_bug.svg)

Our code units interact smoothly, but there may be **errors within our algorithms**.

Examples:
- The `addStuff(a, b)` function returns `a * b`. Not adding up to expectations.
- The `loopOverStuff(listOfStuff)` function iterates one more time than the length of `listOfStuff`. Off by one error with undefined behavior !

A slightly more evolved bug but still pretty basic.
What I call "stupid programmer mistakes".
We tend commit these mistakes at a rate of a dozen per minute, particularly [when the developer is sober](https://xkcd.com/323/).

The solution lies in **implementing the correct algorithm** (uh).

![Is the context conform to expectations ?](/assets/defects_taxonomy/05_question_03.svg) 

So first, congratulations are in order. We actually wrote code that behaves as we think it does, in the context we expect.
If we're writing code in C++ or Javascript, we're already part of the elite.

However, we still need to classify our defect.

Could it be that the context in which our code executes doesn't align with our expectations ?

![Is the context valid?](/assets/defects_taxonomy/06_question_04.svg)

It's possible our code executed in an unexpected context, but that does not mean the context is at fault.

Maybe we didn't think of all the legitimate real world scenarios ?

![Incomplete algorithm bug](/assets/defects_taxonomy/07_incomplete_algorithm_bug.svg)

This type of bug, which I call an incomplete algorithm bug, arises when **the context is in an unforeseen yet legitimate state**, causing our code to fail.

Example:
- Our code synchronize banking data, reading banking transactions from an external service.<br />
  Some transactions legitimately have a total amount of zero.<br />
  As we never anticipated that, our code that calculates VAT rates results in a division by zero.<br />
  Months later, we're astonished to find `NaN` amounts in the VAT declarations of certain customers !

The remedy is simple: **adapt our code to function seamlessly within this legitimate context**.

![Validation bug](/assets/defects_taxonomy/09_validation_bug.svg)

Much of our code context originates from external sources such as user inputs or external services.
It's impossible to control every input or foresee all potential data from these sources.

Sometimes the problem comes from **invalid data infiltrating our execution context**.

Perhaps the user entered incorrect information into a form, or an external service provided a response that doesn't adhere to the expected schema ?

<aside>
Note: part of the "compatibility" defects are in this categorie, when the problem is caused
by a change in the data returned by a dependency, that breaks the behavior of our code (the other part is classified in "contract bugs").
</aside>

Regardless of the cause, the problem lies in our code failure to validate its context before execution.

Examples: 
- Our application requires users to input an amount value, but the UI doesn't validate whether the input is a number.<br />
  Consquently, our code happily stores `NaN` values in the database.
- Our application utilizes an external banking service to manage credit cards. <br />
  It reads the list of credit cards belonging to a specific customer in the external service.<br />
  One day, the service unexpectedly returns all the credit cards of all customers instead of just those belonging to the specified customer.<br />
  Since our code never validated that the returned credit cards belong to the expected customer,
  our application mistakenly displays all other customers' credit cards to each user.

The solution is to **prevent our code from being invoked with invalid values**.

![State machine defect](/assets/defects_taxonomy/10_state_machine_defect.svg)

Sometimes no external sources are responsible for an unexpected context.

Consider our code as vaste state machine:
each code unit takes the current state (context) and produces a new state (context with updated information).

Every state transition might be correctly implemented, but **our state machine might still be flawed.**

In essence, while our code units function in isolation, they don't play nice together.

They contradict each other, leading to issues such as:
- state transitions leading to new states that other parts of the code cannot handle properly.
- transitions that undo previous work or hinder future transitions.

Examples: 
- In our system, banking transactions can be linked to receipt documents, with these links being exclusives: a receipt document can belong to only one transaction.<br />
  Transactions can be soft-deleted, and re-activated.<br />
  Up to now, receipts remained linked to soft-deleted transactions. When a transaction is then re-activated, it is still linked to its receipts.<br />
  Some users are complaining that the receipts linked to their soft-deleted transactions are not available to be linked to other transactions.<br />
  We choose to unlink receipts when a transaction is soft-deleted, to make them available for other transactions.<br />
  Some other users are now complaining that their re-activated transactions are not linked to their receipts anymore, like they were before.

The solution lies in **redesigning the state machine** to either prevent unexpected states/transitions or ensure compatibility across all code units.

![Is there a problem with time ?](/assets/defects_taxonomy/11_question_06.svg)

So our code executes correctly in the expected context, validates its external inputs, and the state machine is flawless.

What can possibly still go wrong ?

Well, one of the biggest enemies of developers is Time itself.

![Did things not happen in the expected order ?](/assets/defects_taxonomy/12_question_07.svg)

With Time, the problems usually fall into two categories:
- things do not happen in the expected order
- things takes too long to happen

![Scheduling defect](/assets/defects_taxonomy/13_scheduling_defect.svg)

Despite a well-designed state machine, **transitions may not occur in the expected order**.

This can lead to two scenarios: either our code rejects a transition as invalid (we're lucky),
or the transition executes but leaves the context in an invalid or corrupted state (more likely).

Example:
- Our application implements a webhook, that an external service calls to update invoice documents in our system.<br />
  Occasionally, we receive an update event for an invoice, before receiving the create event for the same invoices.<br />
  Because, network stuff.<br />
  However, our code doesn't know how to update an invoice that doesn't exists in its database, resulting in the update being lost.<br />
  If we're lucky, at least the invoice is still created, but we miss out on the latest version.

The solution is to **make your code robust to out-of-order events**.
You can either explicitly reject them to protect the context or find a way to accept them out of order.

![Time performance defect](/assets/defects_taxonomy/14_time_performance_defect.svg)

If everything occurs in the right order and we still have a problem with time,
the only explanation left is that **our code takes too damn long to execute**.

Most likely this results in user frustration due to sluggishness rendering the product unusable.
But it could also result in nasty errors as some calls to external services time out,
and our application crashes under the weight of delayed processing.

Example:
- Consider implementing an audit to ensure no banking transactions are missing from customer accounts.<br />
  The algorithm is very clever and compare all transactions with their bank account balances history.<br />
  It's all fun and games, until the audit runs on your production server. And the server crashes.<br />
  The audit's computation is taking so long that your server's request queue just fills up all the available memory. Then everything chokes to death.

The solution is to either **improve your code's performance or make it resilient to poor performance**.

![Cost performance defect](/assets/defects_taxonomy/16_cost_performance_defect.svg)

While our code may provide value to users, sometimes **the cost of operations outweighs the price your users are willing to pay** for your services.

Example:
- We are designing a new generation of brushless motors drives, and opt to implement a cutting-edge current control algorithm in a FPGA chip.<br />
  The FPGA required to run our algorithm cost 150$ each, while the target market price for our drives is only 100$.<br />
  Hu-oh.

The solution is to **reduce operation costs**, which may involve:
- improving performance.
- discarding less valuable but resource-hungry features.
- finding cheaper providers for external services.

Alternatively, we could **increase our own prices**, ideally after trapping customers into your product.

![Usability defect](/assets/defects_taxonomy/17_usability_defect.svg)

Our code functions, but it's either unknown to most users or they struggle to use it effectively,
resulting in it **not being utilized as much as it could be, if at all**.

The solution likely involves **improving the design of your product or enhancing communication**
to ensure users understand its capabilities and how to use it effectively.

![Product knowledge defect](/assets/defects_taxonomy/18_product_knowledge_defect.svg)

The good news is, our code actually works.

The bad news is, nobody cares.

We did everything perfectly except **understanding the users' expectations.**

Some users may use the feature once and never return, while others keep requesting similar features without realizing they already exist within your product.

To address this issue, we need to **revisit understanding what our users truly need** and ensure that our product aligns with those needs effectively.

![Safety defect](/assets/defects_taxonomy/19_safety_defect.svg)

Product safety encompasses a broad range of defects, including issues related to privacy, data/work loss, and physical safety.

**Privacy**: Did our product just leak our users' personal data online?

**Data/work loss**: Can our users lose hours of work and personal data with a single click, without confirmation ?
(While this could be classified as a usability problem, data loss often requires significant effort to fix and can have a severe psychological impact on users.)

**Physical safety**: While not applicable to all software products, some may pose physical risks to users due to unsafe design, leading to potential harm or undue stress.

<aside>
Why separate safety defects from other types?

While the root causes of safety defects may overlap with other defect types such as Algorithm Bugs, State Machine Defects, or Usability Defects, safety defects have unique characteristics:
- The link between the bug and safety nonconformance is often challenging to identify before its impact.
- The consequences of safety defects are typically more severe or have the potential to be.
- Implementing solutions for safety defects, especially those related to psychological or physical safety, can be exceptionally challenging.

Thus, while safety defects could technically fall under other defect types, the distinct nature of their resolution or mitigation warrants a separate classification.
</aside>

![No problem](/assets/defects_taxonomy/20_no_problem.svg)

Well if you're here you have no problem. Why are you here ?

## Complete questions tree

![Complete questions tree](/assets/defects_taxonomy/21_complete_tree.svg)

## Defects taxonomy

The final list of defects looks like this:

![Defects list](/assets/defects_taxonomy/22_defect_types.svg)

The intentional switch from "bugs" to "defects" underscores an important distinction:

**Bugs are trivial issues that should ideally never reach the user.**
Their presence in the product reflects a software development malpractice.
Unfortunately a lot of experimented developers still deliver trivial bugs in their day to day work.

**Defects, on the other hand, represent significant nonconformance problems.**
They come with their own complexity and require constant prevention efforts.
While they are never entirely avoidable, many practices can significantly reduce their occurrence rate.

### Not all defects have the same complexity, nor the same nonconformance or fixing costs

It's crucial to acknowledge that not all defects are created equal in terms of complexity, or the costs associated with fixing or avoiding them.

**Contract bugs** are frequently encountered during development, but they typically cost next to nothing to address.
With robust development tools and practices, these bugs are often immediately caught, and their resolution is straightforward.
In fact, most of them are glaringly obvious if developers take the time to run their code before delivery.

**State machine defects**, on the other hand, are often identified during peer reviews.
Developers who are unfamiliar with a code unit or lack sufficient quality practices may inadvertently make modifications that disrupt other parts of the state machine.
Addressing these defects often requires intervention from an expert to explain the issue, making them more complex and costly to resolve compared to contract bugs.

**Product knowledge defects** are commonly encountered in product teams, and they may not be discovered until long after the feature is delivered to the user.
These defects can be incredibly costly to fix, and in some cases they may even be impossible to rectify, remaining in the product indefinitely.

In essence, **each main category of defects implies an order of magnitude difference in terms of complexity and cost**.
For instance, algorithm bugs are typically ten times easier to resolve than state machine defects, which are themselves ten times easier than time defects, and so on.
Understanding this hierarchy can help prioritize efforts to prevent and address defects effectively.

![Costs](/assets/defects_taxonomy/23_costs.svg)

### Impact on tools returns on investment

Each tool comes with its own setup and operational costs, and it's essential to consider these factors when assessing their effectiveness.

**Defect prevention tools should offer a good return on investment** based on both their cost and their ability to catch defects. Here's how the equation typically works:
- A tool that only catches trivial defects but has high operational costs may not be worthwhile.
  The investment in operating the tool might outweigh the benefits gained from catching relatively insignificant defects.
- Conversely, a tool with high operational costs but the ability to catch complex defects can often be justified.
  The value derived from preventing major issues can outweigh the costs associated with using the tool.

Ultimately, the best tools strike a balance by having **low operational costs relative to the costs of the defects they catch**.

## How to use this taxonomy

At a glance, this defect taxonomy serves as a comprehensive overview of potential challenges in day-to-day software development.

Software development inherently entails complexity (even when the developers don't add unrequired complexity themselves),
spanning various levels of conceptual intricacy, from simple algorithm bugs to profound misunderstandings of user expectations,
encompassing integration and performance issues.

### Selecting effective defect detection tools

Software developers have access to a range of tools for detecting and preventing defects, each with its strengths, costs, and scope of application.

By categorizing the defects encountered during development, it becomes possible to pinpoint the most suitable tool for prevention.

Reflect on why current tools failed to identify certain defects.
Is there a gap in our toolkit that requires a new tool ?
Have we overlooked a crucial step in utilizing existing tools effectively ?

### Assessing our quality approach

Furthermore, this taxonomy facilitates an evaluation of our current delivery quality.

Recognize that not all defects are equal; while some are trivial and should never reach the final product,
others demand significant design considerations or stem from broader product design practices beyond our immediate control.

## Conclusion

Next I'll look at some of you defect prevention tools and see how we can use this taxonomy,
and the [Defect Evaluation Grid](./defect_evaluation.html), to evaluate the return on investment of each tool.
