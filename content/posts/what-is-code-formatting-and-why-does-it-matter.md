+++
title = "What Is Code Formatting and Why Does It Matter"
slug = "what-is-code-formatting-and-why-does-it-matter"
description = ""
date = 2022-10-12T21:40:00+02:00
featured = false
draft = true
comment = true
toc = true
reward = true
pinned = false
categories = [
  ""
]
tags = [
  ""
]
series = [
  "Tools for Java"
]
images = []
+++

Can an AutoFormat Tool Increase Productivity?

The answer is yes.

The book “The Pragmatic Programmer” talks about the entropy of the code. Entropy is a physical quantity linked to molecular disorder. This is perhaps the only physical quantity that we can find in software. The larger the disorder, the higher the entropy. The more entropic your software is, the harder it will be to work with it day by day.

To explain why, I'll give you an example.
<!--more-->

When writing an if-then-else in a language like Java where is it better to put parentheses?

You can say how a), b) or c) or you can invent another intermediate combination of your taste.

a)
```java
if (condition) {
    // your "then" code here
} else {
    // your "else" code here
}
```

b)
```java
if (condition)
{
    // your "then" code here
}
else
{
    // your "else" code here
}
```

c)
```java
if (condition) {
    // your "then" code here
}
else {
    // your "else" code here
}
```

The ways to format the code are numerous: where to put the opening of the brace, whether to use the TAB or the space to indent, whether to add a space after the opening of a round bracket, where to put the comments. Even the writing of the logical conditions can be done in many ways:

```java
(a==b)
```

or

```java
(a == b)
```

or

```java
( a == b )
```

I have witnessed several heated discussions on which style is "better" but beyond what can come out of them, what really matters most is consistency.
Why? Because, depending on how much a person may not like a certain style, after a while they will adjust to it, and the reading of the code will become automatic. The code becomes more intuitive.
If the formatting of the code is inconsistent, reading the code becomes more strenuous. Fatigue increases the likelihood of making mistakes. It will take time to find these errors and more time to correct them. Ultimately, not formatting your code consistently results in a decline in productivity.

Keeping your code formatted consistently can be a challenge for even a single developer but in a team, you can rest assured that consistency won't last.
Developers almost always work as a team. For a team, having a shared rule means increasing overall productivity.

Try to reach a consensus about the formatting style, and if you can’t reach the consensus, just impose one.

Even if you encounter some resistance at first, your colleagues will get used to it, will tire less and will be more productive.

Consistency and the resulting order are even more important than the style of the formatting itself.

If we have a tool that not only verifies that the rules are respected but also formats the code automatically, we get a second big advantage which is to save time and stress to apply the formatting.

The definition of a formatting style for a language with an average complex syntax such as that of Java can require dozens of rules and for what has been said above, it is good that these rules are consistent in the various cases that may arise.

What happens if you decide to apply formatting rules to a project that is made up of several files? Do you have to format all files in one go or is it better to proceed gradually by formatting only a few files at a time?

If a project has a history of a few days, formatting all the files in one go does not have a big impact on the traceability of changes in the log of the source management system (git for example) but if the files are hundreds with more developers than they work and maybe in asynchronous mode (as usually happens on an open source project) applying the formatting little by little is really a necessity.

Finally, since it is better not to have to worry every time you tap on the code to format it, it is better to have an automatic activation of the formatting.

Then you need to create a software tool that reads the code, applies the rules, and make it possible that it runs at every build.

Do you want to know how we did all these things in our Happraisal application?

We have chosen a ready-made solution. In the open source world there are some. We chose Spotless because in addition to allowing us to analyze the code and format it, it is also multilingual. Spotless is not limited to Java but also handles C, JavaScript, TypeScript, yaml, JSON, SQL and others.

The Spotless repository is on GitHub: https://github.com/diffplug/spotless

Spotless can be integrated into Maven and Gradle based builds via plugins.

To use Spotless in Happraisal we imported a plugin into Gradle and further down we included a configuration file via the apply command:

build.gradle
```gradle
plugins {
    // other plugins
    id "com.diffplug.spotless" version "6.2.1"
}

// Include the configuration for Spotless
apply from: "gradle / spotless.gradle"
```

Our gradle/spotless.gradle consists of three parts.
The first is ratchetFrom which, if specified, allows you to apply formatting incrementally only to files modified with respect to the specified branch, which in the case of Happraisal to origin/develop because this is the branch towards which merge requests flow. Basically, every time we add code to origin/develop, Spotless forces us not to make the situation worse.
The parameter specified in ratchetFrom can be something more complicated, to learn more you can consult this file:
https://javadoc.io/static/org.eclipse.jgit/org.eclipse.jgit/5.6.1.202002131546-r/org/eclipse/jgit/lib/Repository.html#resolve-java.lang.String-

Linguistic note: The word *ratchet* in English means that the rule can be more stringent (or remain unchanged) but cannot become less stringent.

The second section called “misc” allows you to specify formatting rules for files that can be identified through the extension gradle or md.

The third section is dedicated to Java.
In the Java section there is a call to googleJavaFormat which is an application dedicated exclusively to the formatting of Java code.

Google Java Format can be found on GitHub: https://github.com/google/google-java-format and in the tags you will find all the published versions (for example 1.13).

If you don't like the formatting style imposed by Google and want to orient yourself on a configurable style then you need to switch to an Eclipse formatter. I refer you to the Spotless website for further information.

Note that Spotless also allows you to manage headers for software licenses via the licenseHeader command.

```gradle
spotless {
// optional: limit format enforcement to just the files changed by this feature branch
ratchet From 'origin/develop'

    format 'misc', {
        // define the files to apply `misc` to
        target '* .gradle', '* .md ',' .gitignore '

        // define the steps to apply to those files
        trimTrailingWhitespace ()
        indentWithSpaces () // tab or spaces. indentWithSpaces takes an integer argument, the default value is 4
        endWithNewline ()
    }

    java {
        // don't need to set target, it is inferred from Java

        // apply a specific flavor of google-java-format
        googleJavaFormat ('1.13. 0 '). ReflowLongStrings ()
        // make sure every file has the following copyright header.
        // optionally, Spotless can set copyright years by digging
        // through git history.
        licenseHeader '/ * (C) $ YEAR * /'
    }
}
```

# Summary
Applying strict rules to code formatting reduces the effort and the stress of developers and increases their productivity.
It would be better to share a formatting style on a team level but if you don't get there by consensus, it's worth enforcing it because after a while people get used to it.
Once you've decided on a style, it's good to have an automated tool to check that the formatting is consistent.
Having a tool to apply formatting automatically saves time and eliminates a mechanical task.
Spotless is an open source tool that can be integrated via plugins with Maven and Gradle. Spotless allows you to format your code gradually so you don't have to make a mega commit at the time of its introduction.

If you are reading this sentence, it’s because you read the whole article. Thank you because this is my first article.
If you have any comment on this content or on the format, please leave a comment…

It's by sharing ideas that we progress!

- Author: Daniele Cremonini
- Reviewer: Emmanuel Quinton
