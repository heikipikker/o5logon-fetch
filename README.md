o5logon-fetch
=============

A small Java program that attempts to exploit CVE-2012-3137 on vulnerable Oracle 11 servers. By exploiting this vulnerability, you can run offline brute force attacks until you discover a given user’s password, without any apparent audit trail.

# Vulnerability Details

A good writeup on the vulnerability, including ways to protect yourself from it, are available [here](http://www.teamshatter.com/topics/general/team-shatter-exclusive/oracle-database-11g-stealth-password-cracking-vulnerability-in-logon-protocol-cve-2012-3137/).

# Alternative Tools

An alternative to this tool is an mmap script located [here](http://nmap.org/nsedoc/scripts/oracle-brute-stealth.html).

If you don’t want to use a tool, you can do what these tools do manually by:

* Obtaining SQL*Plus as part of an Instant Client package from [Oracle](http://www.oracle.com/technetwork/indexes/downloads/index.html).
* Starting a packet capture tool such as [Wireshark](https://www.wireshark.org/).
* Using SQL*Plus to login as the user you want to attack, with a fake password.
* Using your packet capture tool, find the packet sent by the server with “AUTH_SESSKEY” and “AUTH_VFR_DATA” in the payload, both followed by a long hex string.

# Complimentary Tools

Once you use this tool or another method, the next step is to begin brute force attacks against the information you’ve collected.

* A proof of concept brute forcer is available [here](http://www.exploit-db.com/exploits/22069/).
* [John the Ripper’s bleeding jumbo branch](https://github.com/magnumripper/JohnTheRipper/tree/bleeding-jumbo) has a o5logon plugin format.

# Requirements

This project was designed with the following in mind:

* OpenJDK 6, usually easily installed with your OS package manager
* An older ojdbc6.jar, such as the one included with the basic Instant Client 11.2.0.1

## Different JVMs

If you are using a Java implementation besides OpenJDK 6, some of the reflection code in the MitMSocket package may need a few tweaks to work. For example, the default socket implementation may not be called “PlainSocketImpl”.

## Instant Client version

As discussed in the writeup linked to above, a new protocol was introduced in later versions of both Instant Client and Oracle servers that inhibits the damage potential of this vulnerability. That said, both clients and servers support older protocols, and by default will allow them. 