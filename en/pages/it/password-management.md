# Password Management Approach

## Summary

This page describes my approach how to manage my passwords (including how to sync between devices and share passwords with other people) using [FOSS](https://en.wikipedia.org/wiki/Free_and_open-source_software).

## About

This descripion serves several purposes.

* It helps to explain my approach to people I want to share passwords with.
* Others might like the approach and adopt it. (I spend some time investigating different tools and approaches and thinking about the concepts I implemented. I hope other people can benefit from the effort I already spent on this topic.)
* Others could review my approach. If they find errors or gaps in the concepts that make the password management vulnerable we can enhance the approach to resolve those issues ; if they do not find vulnerabilities it increases my confidence that I am using a fine approach.
* Last but not least: It helps me to remember my own ideas ;)

## Software

IMHO a password management software **must** be open source. I would not rely on any closed source software that it neither has design flaws or software bugs that cause relevant vulnerabilities nor back doors that expose the passwords to the author of the software or a 3rd party.

I chose [KeypassXC](https://keepassxc.org/) as password management software. One benefit over Keepass and KeepassX is that it is being actively developed by more than one developer. I have more trust in a software developed by a community that in a software developed mainly by a single author. Also at the time of writing the original Keepass did not have *any* browser integration that did not come with a disclaimer that the plugin has known security issues and should rather not be used. KeepassXC on the other hand has a just newly developed, state fo the art browser integration for Chrome and Firefox (and others).

For synchronization among devices I use [Syncthing](https://syncthing.net/).

## Password Databases

I have several password databases: two for my private passwords and one or two for each group of people I share passwords with.

If I was Alice and hab some passwords I want to share with Bob and some I want to share with Bob, Carol and Dave, I would have

* two databases for my private passwords, one for common and one for sensible passwords, named `alice_common` and `alice_sensitive`,
* two databases for the passwords shared with Bob named `alicebob_{common,sensitive}` (or `albo_{common,sensitive}`)
* one database for the passwords shared with Bob, Carol and Dave named `alicebobcaroldave`(or `albocada`).

### Common and Sensible Databases

I use one database for all ordinary passwords that I uses every day and where exposure of passwords is annoying but not harmful, e.g. email accounts, webshop logins, social media accounts. Exposure would allow other people to get some information intended to be private or to send messages or make orders on behalf of me, but most of the bad things one could do with these passwords could be repaired and would not cause great harm (financially or otherwise).

This database is intended to be synced among all devices where I want to access my passwords. This includes particularly my smart phone which might get lost or stolen much easier and I consider less trustworthy than my Linux computers.

I use another database for all sensitive passwords where exposure might be harmful, e.g. encryption keys (including PGP and SSH passphrases), bank accounts. This database shall not be synced with devices that are less trustwothy or might get stolen easily.

The sensible databases shall have very strong encryption passphrases; the common databases may have less strong (i.e. shorter and/or easier to type) passphrases.

I have separate databases for common and sensible passwords for my private passwords and for passwords I share with just one other person. For passwords shared with several persons I use only a single database where all passwords are stored because

* there are not much sensible passwords I would share with a group,
* the destinction between common and sensible passwords is not always obvious and other people I share the databases with might have other ideas which is common and which is sensible data
* the greater the group, the more likely someone accidentally exposes the passwords, so sensible passwords should not be shared with greater groups at all and a `_sensible` database would give a false sense of confidentiality.

### Master Database

TODO

KeypassXC supports that one when opening one database this one opens other databases -> AutoOpen. Create a master database that opens all other databases automatically. The master database may have a very short passphrase if it resides on a (securely) encrypted storage *exclusively* and is *not* being synced among devices.

## Browser Integration

TODO

KeypassXC -> Connect Browser Plugin -> Name for Database Conection: `<database>/<browser>.<user>@<device>[:<operating-system>]`, e.g. `alice_common/firefox.alice@alice_pc_at_home` or `alice_bob_common/chrome.bob@bobs_laptop:arch_linux_2019-04`

## Synchronization Setup

All databases need to reside in their own directory so that every database has its own Syncthing Folder that defines with which devices to synchronize the file.

It is suggested to repeat the name of the database in the directory name (or an directory hierarchy) and the filename itself, like `./alice_common/alice_common.kdbx` (or `./alice/common/alice_common.kdbx`). The directory (-ies) should be named accordingly to make it obvious for Syncthing what the folder contains and the database file should be named accordingly to have the appropriate name appear as tab caption in KeepassXC.

My database files' hierarchy is like this:
```
/home/
      syncthing/passwords/
                          alice/
                                common/alice_common.kbdx
                                sensitive/alice_sensitive.kdbx
                          albo/
                               common/albo_common.kdbx
                               sensitive/albo_sensitive.kdbx
                          albocada/albocada.kdbx
      alice/**/master.kdbx
```
