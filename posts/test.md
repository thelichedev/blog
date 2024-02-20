Title: My first big bug
Date: 2024-02-20
Tags: rex, bug

I've been a professional software developer for 18 years.
I've caused my fair share of production outages over the years.
My first really big bug in production is still one of my favorite (yes I have favorite bugs, I'm a programmer).

I learned a lot from this bug and it kind of defined my approach to software development for the rest of my career.

# How to break a million euros industrial machine

The story takes place a few months after I've started my first software development job.

I'm working on embedded industrial software, in C, using the Qnx operating system.

After a few months bootstrapping my skills on the project, I'm starting to master the language, the OS
and the standard libraries, and I'm able to take on more complex applications.

One such project is the configuration application for a cutting-edge industrial machine,
the one my firm sells to clients who want to prototype future industrial applications.

The machine is able to produce pieces that no other machine of its type can create in the world,
but the cost is that the configuration is quite complex, which reflects into the code handling this configuration.

Add to this the fact that the clients buying the machine, are trying to prototype their products of tomorrow,
and you can guess that the configuration code must constantly evolve, and try to integrate really disparate client needs.

One of our clients is planning to use the machine to produce revolutionary pieces for Airbus.
They have a demonstration planned with Airbus representatives at their plant.
The machine cost them a pair hundreds thousand euros, and the contract could represent a few millions euros.
Nothing big.

On the eve of the demonstration, I delivered a new version of the configuration application, integrating the features
required by the client for their demonstration.

On the morning of the demonstration, a technician from our customer support team is at the client plant,
installing the new software on the machine, and setting up the demonstration.

Around 10am I receive a phone call from the technician: something is wrong with the machine's configuration.
After installing the new software, the technician setup the correct configuration, but when he tried to start the machine,
the control application complained that it couldn't find the configuration.

I immediately try to help him diagnose the problem, and we stay on the phone working our way through the steps to setup the machine.
We understand rapidly that the client's configuration files have been deleted from the drive.

Thankfully this is not his first rodeo, so he dutifully backed up the configuration files before setting up the new software,
which means we can at least restore the previous configuration.

Unfortunately, the client's demonstration cannot be done with done with the previous version of the control application,
and the new version requires a new version of the configuration, which my application has to generate.
So we can't simply rollback the software and configuration to the previous version.

And each time we use the new version of the configuration application, it seems to delete the configuration files from the drive.

Time is passing fast and as lunch time approaches, I have still no idea of what could cause the problem.
The demonstration is scheduled for the early afternoon, so the pressure is slowly building up, and the technician and me skip lunch
and stay on the phone to try to resolve the problem.

As the client staff and executives comes back from lunch, with Airbus representatives in tow, the machine is still not running.
I can hear the client executives talking in the background on the phone, and our technician explaining the problem to the client's staff.

The afternoon goes by and I still have no idea what the problem is.
The demonstration is canceled and the client must negotiate with Airbus without showing off their new idea.

The end of the work day comes and goes, I stay at work, and our technician stays on the phone.
I can still hear him discuss the situation with the client's staff. Everybody is tired.

Around 10pm I finally find the problem.

```clike
int function generateUserConfiguration() {
   ...
   if (userResetRequested); {
     deleteUserConfiguration();
   }
   ...
}
```

Did you see it ? There is a stray semicolon `;` after the `if` condition.

This is perfectly valid C code and will compile without error.
But instead of deleting the user configuration only when a reset was requested, it will always delete the files.

I can't remember the exact logic which lead me to having a piece of code resetting the user configuration in the function that generates said configuration,
but the result was that instead of generating a new configuration, the user configuration was deleted.

I told our technician that I had found the bug and would deliver a patched version of the configuration application right away.
As the year is 2006, I can't send this new application to the technician over the internet, we barely just got it installed at work :-)
The technician needs to come back to our office tomorrow morning, get the software on a USB key and go back to the client's plant.
Thankfully the client's plant is only a 2 hours drive away from our office.

I go home quite depressed and stressed up, feeling I let everyone down on our team and at the client's, for a really stupid coding mistake.

On the next morning I go back to work, not quite knowing what to expect, but really not happy with myself.

The customer support technician comes to my desk to get the patched software, and I'm expecting him to be rightfully angry at me.

To my big surprise, he's enthusiastic and gives me a high five, congratulating me for my work.

I'm like, "Aren't you angry ? You've had to deal with a lot of pressure from the client yesterday and surely the client must also be quite angry,
what with their big demonstration failing right on the day they're negotiating a big contract with Airbus ?"

The technician laughs and explain to me that no, he's not angry, and the client is in fact quite happy.
Everybody makes mistakes. The negotiation went well despite the lack of demonstration.

What the client saw, was that a software engineer stayed up after work and late at night to find and fix his problem.
A software engineer at the worldwide biggest manufacturer in our sector.
The client is in fact happy to see that they are an important customer for us, and that we'll own our mistakes and not leave them to deal with a broken machine for weeks.

What the customer support technician saw, was a software engineer who did not leave him to deal with the problem and face the client alone,
and who skipped lunch with him and stayed after work to help him fix the problem. Not all software engineers do that for the customer support team.

So, when all was said and done, I broke a hundreds thousands euros machine on the day of a million euros contract negotiation.
And everyone is happy with me, except for myself :-D

# Where did I go from here ?

This bug was my first big learning experience, and would shape a lot of my approach to professional software development in the year to come.

## Our tools suck

And especially our programming languages suck, **hard**.

This code

```clike
if (<condition>); {
  <instruction>;
}
```

Is perfectly valid and compiles in most mainstream languages.
C/C++, Java, C#, Javascript... You name it, if it has C-like syntax this code probably works.

Who, in their right mind, would intentionally write this ? Nobody. 
It CAN'T be what the developer wants.
Yet, our languages make it not only possible, but don't even rise an error or a warning by default.

I was incensed by this 18 years ago, I still am today.
We work with shitty programming languages.
Everybody think this is fine.
It is not.

## RTFM

I couldn't accept the fact that my tools could let this kind of error happen without warning me.

So I read the compiler's documentation.
Remember, 2006. No StackOverflow.

I discovered a few important things:
- `gcc` documentation is **awesome**. Reading it made me a better C programmer.
  There is a truckload of programming wisdom in this documentation, yet I've never since met another programmer who read it.
- there **are** compiler options that triggers errors on this mistake.
  They were not enabled on my team's projects.
  I enabled them.
  I was horrified with what I discovered.

Today `gcc` actually does a good job of warning about this mistake with a simple `-Wall` flag.

```shell-session
warning: suggest braces around empty body in an ‘if’ statement [-Wempty-body]
    5 |   if (a == 1); {
      |              ^
warning: this ‘if’ clause does not guard... [-Wmisleading-indentation]
    5 |   if (a == 1); {
      |   ^~
note: ...this statement, but the latter is misleadingly indented as if it were guarded by the ‘if’
    5 |   if (a == 1); {
```

What did I learn from this ?

**RTFM**. Seriously. Just read the documentation for your tools, and you're already in the top 10% of the developers out there.
Literally nobody does that.
Good tools have good documentation.
In fact it has also become an heuristic of mine before adopting a new shiny tool/language: if the documentation sucks, don't use it.

**Activate all errors report** (in 2006 I didn't even know linters existed). Enable all compiler's errors. Use linters. From the start of the project.
Then maybe disable some of them if they generate too much noise.
But I've worked on too many projects with tons of untraceable bugs that could easily have been caught by just enabling all errors from the start.
Take the time at the start of your project to setup your tool configuration properly.
Too often people just use the insufficient defaults. But then they didn't read the documentation.

### But it's better today right ?

Just for the fun of it, I tested this mistake in my current team's projects.
We work in Javascript/Typescript, and in 2024 surely our tools do a better job of it ?

Well, our typescript setup see no problem with the code. Thankfully `prettier` complains that the indentation is broken.
Fixing the linter errors results in this change

```ts
  if (false); {
    console.log('oops');
  }
  // is changed to
  if (true);
  {
    console.log('oops');
  }
```

Which, hopefully, will attracts the developer's attention to the problem. But our project tools are perfectly happy with the result.

In Javascript our tools (ironically) do a slightly better job of it.

```javascript
  if (false); { // error: code block is redundant
    console.log('oops');
  }
```

The linter complains that the code block is useless, and the automatic lint fix does not fixes this error, meaning the error will be caught before production.

## Towards TDD

This experience, and a lot of other regression bugs in the months that followed, would lead to me question my team's development methods.

We all did our best to test the new versions of our applications before each release, manually, using machine controllers setup
on our desks, or test machines setup in our plant's test rooms.

But as we continuously added new capabilities to our product, and constantly evolved our applications to adapt to new customer needs,
the complexity reached levels were no amount of manual testing could really cover our changes and catch the regression bugs.

I also entered a cycle of software rewrite. As my applications integrated more and more unexpected new features, they started to resist
changes and I eventually needed to re-write them using my new knowledge, in order to keep innovating.
This is actually a good development pattern, far superior to the "write once and never ever rewrite" that most of my future teams enforce,
which invariably kills innovation after a few years.
I still consider it a best practice to fully re-write fast-evolving code at least one a year.

But I was young and naive and it seemed to me that I was lacking in design skills, I was not able to write truly maintainable code.

At this time, and in the next team and project, I was only exposed to big-design-up-front UML design methods.
They never clicked with me because, when you're an inexperienced programmer, how do you achieve a good design upfront ?
A design that can accommodate the future knowledge you know nothing about right now ?

Then I read TDD by example, written by Kent Beck.

And I felt I had finally found a method to solve my problems:
- catch stupid programming mistakes early.
- grow a design organically with iterative refactoring.
- document your knowledge of the code value.
- build a regression test suite that gives you confidence to make changes in your code for the new features.

Of course there is more to TDD than this, but this is what started me on the TDD way.

After 15 years of TDD practice, "preventing stupid programming mistakes" might have been reduced to a nice side-effect of my practice,
but TDD has become the fastest way to deliver product value I know of, 
allowing me to deliver easily 10x faster than the traditional "try stuff until it works and hope you break nothing important" approach.

## Customer support is your first client, and your best ally

This was the first of a long series of mutually benefiting experiences with customer support people.

In the following months, and all the years since, I've come to consider customer support as my first customers.
Delivering value to the customer support is your best approach to making a big impact for your customers if you don't have a direct access to said customers.
Customer support people are power-users of your solution, and they have front-line, hands-on knowledge of how your product is used by the real customers.
They know much more about your product than to your C-levels, middle managers, product managers, or sales managers.

Caring for the customer support needs will also gain you allies, that will save you in times of trouble.
I've seen development/product teams who were cut off from their customer support, leaving the customer support people to fend for themselves,
and, actually quite often, work "around" the development team.
The amount of product knowledge lost in those situations is often enough to kill any competitive advantage for the company.
And when production comes crashing down, the development team is left without inside support.

The other big lesson here is that **bugs rarely impacts customer satisfaction**.
It was not the only time a customer would be happy with my work, despite me having broken the product with a stupid bug.
Customers actually don't want a perfect product. They only want support when the product doesn't work.
They want to see that you care about their problems.
In companies with a customer support service, customer satisfaction actually evaluates the quality of the customer support team.
Not of the software product.

As a developer, your biggest impact on customer satisfaction is not through delivering glitch-free user experience, or eye-candy interfaces,
but in ensuring your customer support can best help your customers: 
- give them power-user admin tools
- value their time, and make it so they don't loose their time on fixing your stupid mistakes.
Help them when things get awry. The customers will always appreciate seeing a well-payed software engineer personally intervene to fix their problems.

Unfortunately in a lot of teams, management enable developers into ignoring the customer support team needs,
not owning the consequences of their work in production ;
and priorities instead the release of new features coming from (product) managers with no real experience of the product.

## Team solidarity

In the months following the incident, I would progressively become more and more isolated in the team, until leaving it after 2 years.
I didn't really feel at my place in the team and I didn't know why.

After all those years and other experiences I often think back to this time, and I can see now some of the problems I was living through at the time.

One of them, was that me and my coworkers never really worked like a team. We worked together but we had no collective ownership of our work.

What is shocking to me in retrospect, is that no one on my team actively helped me to solve the problem.
Sure, nobody criticized me, or blamed me for my mistakes.
But during the whole day spent on the phone trying to find the problem, no coworker went out of their way to (forcefully) help me.

Of course, there is a lot of experience value in letting a young engineer diagnose problems in production by themselves.
They'll hone their diagnostic skills and learn to be responsible for their work.
Too many "senior" engineers do not own the consequences of their work in production.
Sometimes young engineers with a strong sense of ownership will refuse help in those situations, because they feel they have to own their mistakes.

But it can only go so far. It is the duty of more senior engineers to intervene before the situation becomes too stressful 
and the junior engineer starts trashing around instead of diagnosing, like I was on that day.

This experience had a big unconscious influence on my own response to these situations as I became a senior developer, and worked with junior co-workers.
Intervene too much and too early, and the junior developers will come to rely only on your diagnostic skills and never develop their own.
But when I see or hear a junior developer losing heart while facing a problem alone, I feel compelled to immediately stop my work and help them.
After a few days if the problem does not drastically impact production, or immediately if production is down.

And just as with customer support people, helping coworkers in their time of need will also gain you allies, for your future times of trouble.
