Title: Defect prevention tool: Static Types
Date: 2024-03-05
Tags: software quality, tools

Now that I've explained my two main evaluation frameworks for software development quality,
I can start to evaluate the most common defect prevention tools.

Each tool has its strengths but also its costs, some of these tools can in fact cost a lot more than most teams expect.

Knowing the strength of a tool will allow the team to invest in the best tools 
to catch delivery or regression defects before delivery, and 
to ensure a good coverage of the defects taxonomy, at an optimal cost.

Let's start with the first one: **Static Types**.

<!-- end-of-preview -->

## What Static Types ?

I've only worked professionally with C/C++ (for 10 years) and Typescript (for 2 years), a bit with Java (on Android) for a few years.

These are the kind of Static Types tools I know the best. There are others, more evolved Statically Typed languages (Ada, FORTRAN, Haskell, OCaml, Elixir, etc.)
I've read books about them, I've played with some on personal projects, but I've never seen a professional team use them in my life.

There is also a whole debate on type systems, between static or dynamic types, strongly or weakly typed languages, etc.
In the end every software project has a type system of some sort.

Here **I'm only considering the most used form of "static types" systems a-la Java/C++/Typescript**.
Since they seem to represent 95% of our industry practices.

### Expert or Average usage ?

There is a ton of literature on how best to use your types system to develop software.

Each language documentation has a section on best practices, advanced usages, etc. 
Most of them lack an anti-pattern or common pitfall section though (which already tells us a lot about these tools and the damage they can yield).

I feel that in the following discussion, there is a need to distinguish between two different types of usage:
- the ideal, "best practices" usage by a rhetorical team of infinitely wise developers.
- the concrete, everyday usage by a team of real world developers, including some inexperienced members, and some moth programmers.

I'm mainly interested in the concrete usage, and the advantages an average "real life" team can get from these tools.

I'm sure a team of experts programmers could achieve more with static types, at a lower costs, but let's be realistic: 
most teams are not comprised only of expert programmers (most would even be happy to have just one).

So in the following discussion **I'll focus on the concrete usage I've seen in the field**,
and add remarks where I see fit if the concrete usage differs from the ideal usage.

## Quality evaluation grid

Obviously static types are a developer's tool that only impacts code before delivery:
most static types systems cease to exist at runtime.

Static Types are designed to catch bugs during development, and before delivery in your CI chain for example.

As such they are ideally placed on the delivery axis, most importantly they catch bugs before they can impact users.

Static types are well known for catching defects in new code, but also for providing a regression safety net on the whole project.

Can static types discover value missing in the product ?

Due to the poor discipline and expertise of most teams using types in professional projects, the answer is no.

I've never seen a project where the attentive study of the types system lead to a product discovery.

I've read books, however, on Type Driven Design, that made me think that it should be possible, in theory.
But I've never worked on a team with this level of maturity.

|                        | delivery failures | regression failures | missing value |
|:-----------------------|:-----------------:|:-------------------:|:-------------:|
| **before development** |                   |                     |               |
| **during development** |         👾        |          👾         |               |
| **before delivery**    |         👾        |          👾         |               |
| **before user impact** |                   |                     |               |
| **after user impact**  |                   |                     |               |

## Defects taxonomy

### Algorithm bugs

**Contract bugs** are what the Static Types are designed for.

They are probably the fastest tool to catch those bugs, and if your project is fully typed, they will provide a good constant coverage.

Unfortunately they are also not sufficient to catch all those defects.

Widely used types systems are not capable to enforce type checks beyond the basic data shape.

Most notably they can't enforce advanced or invariant properties in code.
- Want to enforce that in `addYear(dateString : string, count : number)`, `dateString` should use a `YYYY-MM-DD` format ? Good luck
  (feasible in Typescript but I've never actually seen a team bother with that)
- Want to enforce that the `sort(list)` function return a list comprised of exactly the same values as its argument ? Can't do that.
- Want to enforce that `min <= max` in `{ min: integer, max: integer }` ? Nope (or not easily)
- Want to say that `userId` is a `string` but you can't do stringy stuff with it since, well, in fact it's an unique identifier, and
  you can only compare it with another `userId` ? Again, feasible, but complex and most teams don't bother with this.

Static types suck at catching **Algorithm bugs**

`function add(a : number, b : number) : number { return a * b; }` is perfectly fine for types, and also totally wrong.

Static types are totally unable to catch most algorithm bugs: off-by-one errors, rounding errors, inverted conditions...

The most common joke about this is the famous sentence "If it compiles it works". Which, of course, is total bullshit.

(Not sure the joke is not lost on most static types enthusiasts though).

Static type are almost useless for **Incomplete algorithm bugs** too.

At best they can detect missing check conditions in a `switch` statements but that's really cutting edge stuff
(lol who still use `switch` statements in 2024 ?).

And that's if you thought about all the legitimate context values anyways.

If you didn't know about those legitimate values, types can do nothing for you.

Static type carefully avoid helping you with **Validation bugs**, by not being present at runtime.

At best some open source developer built a JSON-Schema validation tool that will generate static types to use in your code,
but Static Types tools themselves rarely bother actually doing this work.

In the age of the communicating software system, this is totally archaic.

**Conclusion: Static types only catch basic (data shape) contract bugs**

### State machine defects

In theory, with Type Driven Design, types can help you design a robust state machine.

In practice, I've never seen a team reach this level of practice.

Most types are built too permissive by default, union types are a relatively new and advanced concept for most developers,
and the use of automatically-generated, globally-shared, all-inclusive types throughout the whole code base,
prevent any serious state machine design based on types.

### Time defects

Guess what ? Yup. Useless.

### Costs defects

Useless ? Yup yup yup.

### Product defects

At the very best types can help document the product value implemented by the code.

In practice, you can't expect developers that don't know how to express business value in their code,
to fare much better in their types. Since they don't even identify their types as an opportunity to document the product.

### Safety defect

Advanced type systems can provide safety checks on basic operations (Ada comes to mind).

In my experience though, Static Types tools have almost no impact on safety-related defects.
Every study I've seen on the subject seem to confirm this. Types give no safety-related advantages.

## Static types feedback speed

For a defect prevention tool, feedback speed is an important quality.

Of all the common tools, Static Types probably have the fastest feedback.

Most modern IDE now propose integration with the types checker, allowing for immediate feedback
as the code is written, with no need to wait for a full compilation cycle.

## Static Types cost

In some way, the cost of Static Types could be seen as null. Most software projects use a unique fixed programming language,
and most languages are either statically/strongly typed or not.

So most developers will never "feel" the cost of static types, unless they switch job (most go to work on another project in the same language anyway),
or somehow the language used for they everyday project changes.

I've started my career with 10 years of statically types languages, ending with some pretty heavy Object Oriented Programming code
(or as I like to call it: "Classes Oriented Programming").

Then I switched to dynamically typed languages. Now I'm coming back (under duress) to statically typed languages.

I've seen both sides of the coin.

The cost of static types is real. And probably greatly underestimated by most developers. (Again, many lack references for a real comparison).

**First there is the tooling cost**: most of those tools lead to a higher building cost for the code.

This could seem to be negligible in our day and age, but this has a big impact on all the ecosystem:
- first build time are often big (making systematic "clean slate" approaches less practical)
- refresh/reload times are greatly impacted (eg for Unit Tests).
- confidence in the version of code executed during development is reduced (Did my compiler see my codes changes ? Had it finished compilation when I did my last exploration ?)
- CI execution times and prices increase greatly.
- deployment delays can also rise sensibly, increasing the roundabout time for hotfixes etc.

Having introduced Typescript in my team's CI/CD chain during the past years, I can tell you that the work involved to keep everything working as fine as before,
and the increase in CI and CD times and costs, were really a big deal. Today most of our CI jobs spend more time compiling Typescript than actually running tests.

**Second there is the development cost**, and most notably the price of the complexity introduced by static types.

You see, most Static Types systems start out fine and simple. But then, as the developers require always more features, the complexity creeps in.
[Most types systems end up Turing-complete](https://github.com/microsoft/TypeScript/issues/14833).

Which, in simple words, means the developers can write programs in types. With loops, conditional and all this shit.

Mind you, these programs in the types-word have mostly no value to the customers. They "do" nothing.
They would be hardly pressed to, since most Static Types cease to exist at runtime.

This means that enthusiastic developers can spend A LOT of time fooling around with the type system,
creating monstrosities all in the name of "type safety", and meanwhile delivering zero value to the customers.

I've seen this. A lot. There are no guardrails for mad developers in types systems.

Another very common cost is the additional complexity of the types hierarchy, and the coupling that comes with it.

Un-typed languages have only one kind of hierarchy and coupling: the function call hierarchy. Functions depends on each other at runtime and that's it.

With static-types, most of the time complex types hierarchies are introduced (all the usual interfaces/implementations, inheritance, factory of factory of hammer bullshit),
and modules can become coupled by sharing global types (let's use a tool to derive types from our database schema, then let's make those whole types global and use them everywhere in all our modules).

Even in contract-oriented types systems like Typescript, all the traditional types bad practices are still largely dominant.

**Third, there is a big opportunity cost**: static types systems make it impossible to write some pattern of highly re-usable code.

Now, you might say that a program that can't be written in say, Typescript, is probably hard to maintain anyway, so it's actually a good feature of static types.

But in my experience, there are a lot of really useful patterns of software composition that are made next to impossible with static types.

For example, Typescript documentation cite [Point Free Programming](https://www.typescriptlang.org/docs/handbook/typescript-in-5-minutes-func.html#point-free-programming)
as a programming style that is ill supported, and best avoided in Typescript.  
Functional wrappers composition is another one.

Teams working with static types are condemned never to use these kind of programming styles, to the point that most of their developers
have no idea they exists, and have never seen a code base written in these styles.

It is counter intuitive for tools that aim at improving maintainability, but I see it in my work everyday: 
static types limit the developers ability to design maintainable code.

The best examples are statically typed dependency-injection tools. They are all a maintenance nightmare.
Highly-maintainable dependency-injection patterns, like functional wrappers composition, are all but impossible to write with static types,
and totally unknown to developers used to working with static types.

**Overall I'd rate static types cost as high**, amongst the highest of the common defect prevention tools I've used in my professional life,
with **a big risk factor**, in that they require a strong expertise and team discipline in order to maintain their cost low (and most teams lack exactly that).

## Conclusion

Delivery quality impact:

|                        | delivery failures | regression failures | missing value |
|:-----------------------|:-----------------:|:-------------------:|:-------------:|
| **before development** |                   |                     |               |
| **during development** |         👾        |          👾         |               |
| **before delivery**    |         👾        |          👾         |               |
| **before user impact** |                   |                     |               |
| **after user impact**  |                   |                     |               |

Defects taxonomy coverage:

| Defect type               | Coverage          |
|:--------------------------|:------------------|
| **Algorithm bugs**        | only basic Contract bugs |
| **State machine defects** | - |
| **Time defects** | - |
| **Costs defects** | - |
| **Product defects** | - |
| **Safety defect** | - |

As we can see Static Types globally suck at finding but the most basic of basic bugs.

Speed: **fast**

Cost: **high, with high maintenance risks**

As I hope is made evident, **Static Types are in fact a poor defect prevention tool**.

Sure, they are the best tool to prevent basic Contract bugs, but other tools (automated tests) can do that too, with a negligible speed/cost difference.

And, let's be honest, Contract bugs are like, the dumbest and cheapest of all the defects that afflicts software projects.
And the easiest to fix.

I've often felt, that the belief that Static Types allow a developer to deliver "working code", is a sign of a
**very naive view of the nature of software defects**, and of the difficulty of delivering working product value.

Sure it's useful to have a clean code base devoid of basic bugs, in order to focus on more complex defects, but again, 
**other tools provide a better defects coverage at a similar cost**.

If you factor in the high probability that **the types system will be afflicted by the same bad practices as production code**
(over-design, complexity, etc.), you can see the types system as another opportunity to shoot your maintainability in the foot.
One that has absolutely no value whatsoever for your customers.

In my professional experience, the teams that performed worst on delivery quality were definitely the ones working on statically typed OOP code bases.
Most of these code base had become totally unworkable and sclerotic under the weight of a poorly designed types hierarchy.

And ironically, despite their heavy usage of static types, they were plagued with a lot of defects impacting directly their users.
