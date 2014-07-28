# ChatOps Design Patterns

## Introduction and Disclaimers

This document is intended to describe various common patterns and behaviors
that are found when developing plugins and features for a chatbot.  Some of
these behaviors may just be common UI behaviors, but I've tried to describe
everything in the context of a bot being used for "ChatOps", that is, read-
and-write operations being done via a bot from multiple sets of users.

The other disclaimer is that, having never done anything like this document
before, I have no idea what I'm doing.  There will inevitably be mistakes,
errors, and omissions - comments, suggestions, and pull requests are greatly
appreciated.

# A Brief Definition Of ChatOps

For the purposes of this document, "ChatOps" is considered any interaction
over a messaging system (IRC, Campfire, Slack, HipChat, Twitter, etc) with
information to be conveyed or actions to be taken.  This could be in the form
of web-hook-based room notifications, or interaction with a server running
a chatbot.

## Patterns

### Input to a bot

#### Avoiding running the same command multiple times

One pattern is to look for a "smashword", or other unique token, as part of
every message.

For example:
```
user> bot, do something
bot> "do something" is protected from duplicate commands, append ASDF7548 to the end of the command
user> bot, do something ASDF7548
bot> running "do something"
```

### Output from a bot

#### Formatting output

When using transports that allow it, command-line-like output is often better
formatted in monospace fonts.  For example:

```
--------------------------------
| a simple table | col1 | col2 |
--------------------------------
| some words     | val1 | val2 |
--------------------------------
```

### Misc

#### Think of operations in read-only vs. read-write

#### "Watching" and "stalking" a resource to provide status updates

When a resource may change state (either through chatops or through outside
interaction), it's often helpful to provide a command to follow along with
state changes.  Two different ways are possible: "watch", which can provide
a continuous monitoring stream, and "stalk", which can provide a update only
when the state changes.  For example:

```
user> watch server-load
bot> server load 0.0
bot> watching server-load every 30 seconds
(30 seconds later)
bot> server-load 0.0
(30 seconds later)
bot> server-load 0.1
(30 seconds later)
bot> server-load 0.0
user> unwatch server-load
bot> not watching server-load anymore 
```

and

```
user> stalk site-status
bot> site-status: up
bot> stalking site-status every 30 seconds
(later when the resource changes)
bot> site-status: down
user> unstalk site-status
bot> not stalking site-status anymore
```

## License

This work is licensed under the Creative Commons Attribution-ShareAlike 4.0
International License.  To view a copy of this license, visit:

http://creativecommons.org/licenses/by-sa/4.0/
