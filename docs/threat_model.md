---
title: Threat Model
permalink: /threat_model/
index: 30
---

# Threat Model: when Can I Use mookey?

In the [motivations][1], you had a brief overview of what `mookey` attempts to
do, and you had a glance of some of its by-design limitations. This document
describes more into details the [threat model][2] of this project. It indicates
whether `mookey` satisfies all your security, privacy and reliability
requirements. **If not, you should consider alternatives.**

## The Hardware is Trusted

This is maybe the biggest security concern when storing secrets on a hardware:
am I able to trust the physical support? If you want to store highly-sensitive
data (e.g. industrial secrets, government stuff), the answer is probably **no**,
and you probably take much lower risks by developing custom hardware solutions,
where you control (almost) everything.

In `mookey`, we make the assumption that **yes**, the hardware is *somehow*
safe. You trust your vendor enough to insert a rightfully **first-hand** bought
USB key in your computer, and not expect "bad things" to happen. Of course, you
should not have the same confidence level in a USB key found in the wild.




[1]: {% link motivations.md %}
[2]: https://en.wikipedia.org/wiki/Threat_model
