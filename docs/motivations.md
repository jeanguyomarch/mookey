---
title: Motivations
index: 10
---

# Motivations: Passwords Mobility

Passwords contribute the improved privacy and security by restricting accesses
to services or resources to their holders. Although they can be bypassed by
exploiting software and hardware vulnerabilities, or obtained through social
engineering or dubious methods, passwords are still the central defense for
most people.

Users are advised to choose [one password per service][2], complying with
[various][1] [recommendations][3]. In practice, these cannot be remembered
without using a patterns (which could be easily defeated once identified).  A
common option checking these boxes is to use a [password manager][4]: a
third-party program is tasked with generating passwords and securely storing
them somewhere safe. [Firefox Lockwise][8] is one of these tools, but is an
online service you must trust enough to provide with all your passwords bank.
[KeePass][6] can be seen as an alternative, as it provides a self-hosted
infrastructure, version 2.10 notably received a [CSPN certification][7] in
[2011][5]. However, both families of these solutions come short when network is
unavailable or if you must access passwords outside of the range of your
self-hosted solution.

`mookey` addresses the problem of **passwords mobility**: how do people share
passwords among different computers when they cannot rely on password managers
over a network?

## Existing Solutions

You will find below a collection of *already existing solutions* that address
the problem: of **passwords mobility**. This list is likely to be *incomplete*,
or may be inaccurate (because these solutions were not tested directly by the
authors of this text). If you feel it is *impartial*, *incorrect* or *biased*,
please consider the [Hanlon's razor][18] and don't assume malevolent intentions.
Please [open an issue][19] to improve and/or correct this content.

### Yubikey

The [Yubikey][10], a proprietary solution from [Yubico][9] attempts to overcome
these limitations (while providing additional features). It costs [around
50€][11]. It seems that making backups of a Yubikey is [quite tedious][15], but
it has received overall [good reviews][14].  As a proprietary solution, you
shall trust the company behind this solution (the hardware and software are not
open source), and evaluate how an unfortunate disappearance of the company affect
your secrets management model.

### USB Armory

[USB Armory][16] is an open source software and hardware project implementing
a secure computer you can interact with through the USB bus. Being and open
project, it can be built from scratch (given you buy third-party electronic
components). Prebuilt solutions can be bought online for [around 100€][20].

### Nitrokey

[Nitrokey][17] is also an open source software and hardware project that
proposes targeted features, such as encryption, or secure login. Pricing
ranges from [20€ to 100€][21].

### Wookey

The [Wookey][12] project initiated by the [ANSSI][13] is also an open source
software and hardware project similar to [USB Armory][16] and [Nitrokey][17].
[Slides from SSTIC 2018][22] detail how it is positioned with respect to
these two alternatives.


## Why would you use mookey?

As hinted above, there are already plenty of advanced solutions to password
mobility that use intricate micro-kernels or custom hardware designs,
addressing difficult security requirements. Some of these are even available at
an industrial scale for a decent pricing.  However, we think that there is a
cheaper and simpler alternative for password mobility for the daily routine of
most users, considering a **less constrained threat model**. `mookey`
definitely does not play in the same league for security, privacy and
resilience than the aforementioned solutions, but could offer a **free** (as in
free beer and as in free speech), easy to roll-in alternative.

Risking a [selection bias][23], a (maybe unfortunate?) observation of how
people around us roll out with password management is that (almost) nobody use
any aforementioned solutions. Passwords are all the same for all accounts;
sometimes they transit through email in plain text, when they are not written
on a sheet of paper under the keyboard. Existing solutions have not been
adopted by general users (including people familiar with a development
environment). This could partially be caused by an unwillingness to adapt to a
new technology, that requires investing time (and money) to overturn old
habits. It is based on this *idea* (though, not supported by any scientific
survey I know of) that `mookey` has been developed.

Most people have a plain, simple USB key from ten years, taking dirt in a
drawer or attached to a keyring. USB has been known to be a source of
vulnerabilities and opportunities for attackers to access computers (i.e.
[BadUSB][24]): crafted firmwares can be used to serve an attacker's purpose,
and there is convincing mitigation defense against these. It is probably
considering this threat model that makes aforementioned solutions appealing.
However one could argue that *legit* USB devices are *secure* enough, to a
certain extend. This *assumes* that first-hand purchases to non-malevolent
industrials (that do not add built-in backdoors for dubious reasons or
government pressure) cannot lead to privacy or security issues. To some people,
this is quite a strong hypothesis, that is not based on enough evidence to be
valid. And they are probably right! If your threat model makes you ask this
question and if you cannot just dogmatically accept the hypothesis presented
earlier, then `mookey` is definitely not secure enough for you, and you should
consider alternatives. However, this seems *acceptable*, than you may want to
give `mookey` a try.




[1]: https://wiki.archlinux.org/index.php/Security#Choosing_secure_passwords
[2]: https://www.ssi.gouv.fr/guide/mot-de-passe/
[3]: https://pages.nist.gov/800-63-3/sp800-63b.html
[4]: https://wiki.archlinux.org/index.php/List_of_applications/Security#Password_managers
[5]: https://www.ssi.gouv.fr/administration/certification_cspn/keepass-version-2-10-portable/
[6]: https://wiki.archlinux.org/index.php/KeePass
[7]: https://www.cryptoexperts.com/services/cspn/
[8]: https://www.mozilla.org/en-GB/firefox/lockwise/
[9]: https://www.yubico.com/
[10]: https://wiki.archlinux.org/index.php/YubiKey
[11]: https://www.yubico.com/fr/store/
[12]: https://wookey-project.github.io/
[13]: https://www.ssi.gouv.fr/en/
[14]: https://boxmining.com/yubico-yubikey-review-and-guide/
[15]: https://www.yubico.com/blog/backup-recovery-plan/
[16]: https://inversepath.com/usbarmory.html
[17]: https://www.nitrokey.com/
[18]: https://en.wikipedia.org/wiki/Hanlon%27s_razor
[19]: https://github.com/jeanguyomarch/mookey/issues/new
[20]: https://www.mouser.fr/Search/Refine?Ntk=P_MarCom&Ntt=189419053
[21]: https://shop.nitrokey.com/shop
[22]: https://www.sstic.org/media/SSTIC2018/SSTIC-actes/wookey_usb_devices_strike_back/SSTIC2018-Slides-wookey_usb_devices_strike_back-michelizza_lefaure_renard_thierry_trebuchet_benadjila_WUAopX7.pdf
[23]: https://en.wikipedia.org/wiki/Selection_bias
[24]: https://srlabs.de/wp-content/uploads/2014/07/SRLabs-BadUSB-BlackHat-v1.pdf
