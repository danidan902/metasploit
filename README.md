# Metasploit Framework: Beginner-to-Intermediate Guide

A practical introduction to Metasploit Framework for learning ethical
hacking and penetration testing in an authorized lab.

> **Important safety rule:** Only scan or test systems that you own or
> have explicit written permission to assess. Use an isolated virtual
> lab for hands-on exercises. Never use these instructions against
> public IP addresses, school networks, company systems, or another
> person's device without authorization.

## Table of contents

-   [1. What is Metasploit?](#1-what-is-metasploit)
-   [2. How Metasploit works](#2-how-metasploit-works)
-   [3. Important terminology](#3-important-terminology)
-   [4. Install Metasploit on Kali
    Linux](#4-install-metasploit-on-kali-linux)
-   [5. Start the console](#5-start-the-console)
-   [6. Essential commands](#6-essential-commands)
-   [7. Understand modules](#7-understand-modules)
-   [8. Safe first lab: scan your own local web
    server](#8-safe-first-lab-scan-your-own-local-web-server)
-   [9. Understand module options](#9-understand-module-options)
-   [10. Sessions and payloads](#10-sessions-and-payloads)
-   [11. Build an isolated practice
    lab](#11-build-an-isolated-practice-lab)
-   [12. A responsible penetration-testing
    workflow](#12-a-responsible-penetration-testing-workflow)
-   [13. Common problems](#13-common-problems)
-   [14. Practice exercises](#14-practice-exercises)
-   [15. Glossary](#15-glossary)
-   [16. Official learning resources](#16-official-learning-resources)

------------------------------------------------------------------------

## 1. What is Metasploit?

**Metasploit Framework** is an open-source framework used by security
professionals to assess systems, validate vulnerabilities, and test
security controls.

It provides reusable modules for tasks such as:

-   Finding information about services.
-   Checking for known security weaknesses.
-   Validating vulnerabilities in an authorized environment.
-   Testing defensive controls.
-   Recording and reviewing assessment results.

Metasploit is not a magic button that automatically hacks every
computer. A tester must understand the target, the vulnerability, the
module's requirements, and the possible impact of running it.

### What is penetration testing?

Penetration testing is an authorized security assessment. A tester
attempts to identify and safely validate weaknesses so the system owner
can fix them.

A responsible test has:

1.  Written permission.
2.  A clearly defined scope.
3.  A safe testing window.
4.  A plan to avoid data loss or service interruption.
5.  A report describing findings and fixes.

## 2. How Metasploit works

A typical Metasploit workflow looks like this:

1.  **Understand the scope:** Confirm which lab machines and services
    you are allowed to test.
2.  **Discover services:** Identify what is running on the authorized
    test machine.
3.  **Research weaknesses:** Determine whether a service version or
    configuration may be vulnerable.
4.  **Select a module:** Find a suitable Metasploit module and read its
    documentation.
5.  **Review options:** Check every required setting and understand what
    the module will do.
6.  **Validate safely:** Use a non-destructive check when the module
    supports it.
7.  **Test only with approval:** Exploitation can interrupt services or
    change data, so it requires explicit authorization.
8.  **Document and fix:** Record evidence, recommend fixes, and retest
    after remediation.

## 3. Important terminology

  -----------------------------------------------------------------------
  Term                                Meaning
  ----------------------------------- -----------------------------------
  Framework                           A collection of tools and libraries
                                      that work together.

  Module                              A component that performs a
                                      particular security task.

  Exploit                             Code that takes advantage of a
                                      software or configuration
                                      vulnerability.

  Payload                             Code or an action delivered after
                                      an exploit succeeds. Payloads can
                                      create a session or perform another
                                      permitted test action.

  Auxiliary module                    A supporting module, often used for
                                      discovery, scanning, or other
                                      security checks.

  Session                             An interactive communication
                                      channel created by some successful
                                      tests.

  Listener                            A component waiting for an incoming
                                      connection.

  Post module                         A module designed for authorized
                                      assessment after a session exists.

  Encoder                             A module that transforms payload
                                      representation. Encoding does
                                      **not** guarantee that security
                                      products will fail to detect a
                                      payload.

  RHOSTS                              The remote host address or
                                      addresses a module will assess.

  RPORT                               The remote service port.

  LHOST                               The local address used by certain
                                      payload/listener configurations.

  LPORT                               The local port used by certain
                                      payload/listener configurations.
  -----------------------------------------------------------------------

## 4. Install Metasploit on Kali Linux

On Kali Linux, update the package list:

``` bash
sudo apt update
```

Install the framework package:

``` bash
sudo apt install metasploit-framework -y
```

Check that it is installed:

``` bash
msfconsole --version
```

If Kali reports that the package cannot be found, check the Kali
repository configuration and official installation documentation rather
than downloading an installer from an unknown website.

## 5. Start the console

Start Metasploit:

``` bash
msfconsole
```

The prompt usually looks similar to:

``` text
msf6 >
```

This is the Metasploit command prompt. Enter Metasploit commands here,
not in an ordinary Linux shell.

To leave Metasploit:

``` text
exit
```

## 6. Essential commands

  -----------------------------------------------------------------------
  Command                             What it does
  ----------------------------------- -----------------------------------
  `help`                              Displays available commands.

  `version`                           Displays the framework version.

  `search <term>`                     Searches for modules matching a
                                      term.

  `use <module-path>`                 Selects a module.

  `info`                              Displays information about the
                                      selected module.

  `show options`                      Displays the module's configurable
                                      options.

  `show advanced`                     Displays advanced options, when
                                      available.

  `show targets`                      Displays supported targets, when
                                      available.

  `show payloads`                     Displays compatible payloads, when
                                      available.

  `show auxiliary`                    Lists auxiliary modules.

  `show exploits`                     Lists exploit modules.

  `set NAME VALUE`                    Sets an option in the current
                                      module.

  `get NAME`                          Displays an option value, when
                                      supported.

  `unset NAME`                        Clears an option value.

  `back`                              Leaves the selected module and
                                      returns to the main prompt.

  `check`                             Runs a module's vulnerability
                                      check, if that module supports it.

  `run`                               Runs the selected module.

  `exploit`                           Runs an exploit module.

  `sessions`                          Lists active sessions.

  `sessions -i ID`                    Interacts with a session. Use only
                                      in your authorized lab.

  `jobs`                              Lists background jobs.

  `db_status`                         Displays the status of the
                                      Metasploit database connection.

  `workspace`                         Displays or changes a database
                                      workspace.

  `save`                              Saves console configuration, where
                                      supported.

  `exit`                              Exits the console.
  -----------------------------------------------------------------------

**Remember:** `run` and `exploit` execute modules. Read `info` and
`show options` first. Do not run an unfamiliar module against a real
system.

### Search examples

Search for HTTP-related modules:

``` text
search http
```

Search for auxiliary modules:

``` text
search type:auxiliary
```

Search for a particular product or vulnerability identifier:

``` text
search CVE-2021-44228
```

A search result is not proof that a machine is vulnerable. Research the
affected versions and conditions before drawing conclusions.

## 7. Understand modules

Metasploit contains different module types.

### Auxiliary

Auxiliary modules perform supporting tasks such as service discovery and
certain checks. They do not necessarily exploit a vulnerability.

Example search:

``` text
search type:auxiliary http
```

### Exploit

Exploit modules attempt to take advantage of a specific vulnerability.
They can crash a service, change data, or provide access. Use them only
in a deliberately vulnerable lab and only after understanding the
module.

Example search:

``` text
search type:exploit
```

### Payload

A payload is the action or code associated with an exploit. Some
payloads create a session; others perform a specific action.

Search for payloads:

``` text
show payloads
```

The available payloads depend on the selected exploit module.

### Post

Post modules support additional authorized assessment after a session
has been established.

Search for post modules:

``` text
search type:post
```

### Encoders and NOPs

Encoders transform payload representation. NOP modules provide
no-operation instructions for certain technical uses. These are advanced
topics; they are not required for your first lab.

## 8. Safe first lab: scan your own local web server

This exercise teaches how to start a local service, select a module,
configure options, and read the result. It does not exploit a
vulnerability.

### Step 1: Start a local web server

Open a normal Linux terminal (not the Metasploit console) and run:

``` bash
mkdir -p ~/metasploit-lab
cd ~/metasploit-lab
echo '<html><title>My Local Lab</title><body>Practice server</body></html>' > index.html
python3 -m http.server 8000 --bind 127.0.0.1
```

Keep this terminal open. The web server is listening only on your own
computer's loopback address.

### Step 2: Open another terminal

Start Metasploit:

``` bash
msfconsole
```

### Step 3: Search for an HTTP title scanner

At the `msf6 >` prompt:

``` text
search type:auxiliary http title
```

Find the HTTP title scanner in the results. Module paths can change
between releases, so use the exact path shown by your installed version.

### Step 4: Select the module

For versions that include this module, the path is:

``` text
use auxiliary/scanner/http/title
```

If that path is not found, search again and select the matching module
path shown by your console.

### Step 5: Read its documentation and options

``` text
info
show options
```

Read the descriptions. Confirm that the module is an HTTP title scanner
and understand its required settings before running it.

### Step 6: Configure the local target

``` text
set RHOSTS 127.0.0.1
set RPORT 8000
```

`127.0.0.1` is the loopback address of your own machine. `8000` is the
port used by the local Python web server.

### Step 7: Run the scanner

``` text
run
```

The scanner should connect to your local HTTP server and try to retrieve
its page title. The result may vary by module and framework version.

### Step 8: Return to the main prompt

``` text
back
```

Stop the Python server with `Ctrl+C` in its terminal.

### What you learned

-   `search` finds a module.
-   `use` selects it.
-   `info` explains it.
-   `show options` displays its settings.
-   `set` configures it.
-   `run` executes it.
-   `back` returns to the main console.

## 9. Understand module options

A module may show options such as:

  Option        General meaning
  ------------- -----------------------------------------------
  `RHOSTS`      Remote target host or hosts.
  `RPORT`       Remote service port.
  `THREADS`     Number of concurrent workers, if supported.
  `SSL`         Whether to use SSL/TLS, if supported.
  `TARGETURI`   Web application path, if supported.
  `VERBOSE`     Whether to display more output, if supported.

Not every module has all these options. Always trust the option
descriptions displayed by the module you selected.

A useful rule:

``` text
use <module>
info
show options
```

Do not assume an option from one module exists in another module.

## 10. Sessions and payloads

Some authorized exploit tests create a session. A session is a
communication channel between the tester and the test environment.

List sessions:

``` text
sessions
```

A session may have an ID. In an isolated lab, a tester can interact with
a session using:

``` text
sessions -i <SESSION_ID>
```

Replace the placeholder with an actual session ID shown by your console.
Do not guess an ID.

A listener waits for a connection. Listener and payload configuration is
specific to the test environment and can expose a system if used
carelessly. Learn these concepts only in an isolated lab; do not deploy
payloads on devices or networks without explicit authorization.

## 11. Build an isolated practice lab

A beginner-friendly setup uses two virtual machines:

-   **Kali Linux:** the security-testing machine.
-   **Metasploitable 2:** an intentionally vulnerable training machine.

You can run both using VirtualBox.

### Safety configuration

1.  Create the Kali virtual machine.
2.  Create the Metasploitable 2 virtual machine from its official
    training image.
3.  Configure both machines to use the same **Host-only** virtual
    network.
4.  Do not use Bridged networking for the vulnerable machine.
5.  Do not forward vulnerable-machine ports from your router.
6.  Take a virtual-machine snapshot before exercises.
7.  Keep the lab isolated from production systems and sensitive personal
    files.

The vulnerable machine is deliberately insecure. Never expose it to the
public internet.

## 12. A responsible penetration-testing workflow

Use this checklist for every assessment:

-   [ ] Obtain written authorization.
-   [ ] Define target IP addresses, domains, and ports.
-   [ ] Confirm which techniques are permitted.
-   [ ] Agree on a testing window.
-   [ ] Back up important systems and data.
-   [ ] Identify services and versions.
-   [ ] Research possible vulnerabilities.
-   [ ] Read the selected module's documentation.
-   [ ] Review every module option.
-   [ ] Prefer non-destructive validation.
-   [ ] Stop if the test affects availability or data unexpectedly.
-   [ ] Record evidence without collecting unnecessary personal data.
-   [ ] Report findings and remediation steps.
-   [ ] Retest after the owner applies fixes.

## 13. Common problems

### `msfconsole: command not found`

Check whether Metasploit is installed:

``` bash
dpkg -l | grep metasploit
```

If it is missing, update package information and install the Kali
package:

``` bash
sudo apt update
sudo apt install metasploit-framework
```

### Module not found

Search for it again:

``` text
search <module-name>
```

Use the exact module path shown by your installed framework.

### Connection refused

For the local HTTP exercise, check:

-   Is the Python server still running?
-   Did you use port `8000`?
-   Did you bind it to `127.0.0.1`?
-   Did you set `RHOSTS` to `127.0.0.1`?
-   Did you set `RPORT` to `8000`?

You can check the server from another ordinary terminal:

``` bash
curl http://127.0.0.1:8000
```

### Database not connected

Check database status:

``` text
db_status
```

Some basic modules work without a database. Follow the current Kali and
Metasploit documentation if you need database-backed workspaces or
stored results.

## 14. Practice exercises

Try these exercises in order:

### Beginner

1.  Install Metasploit on Kali.
2.  Start `msfconsole`.
3.  Run `help` and `version`.
4.  Search for HTTP auxiliary modules.
5.  Read a module's `info` output.
6.  Display its options with `show options`.

### Beginner practical exercise

1.  Start the local Python HTTP server from Section 8.
2.  Select the HTTP title scanner.
3.  Configure `RHOSTS` and `RPORT`.
4.  Run the scanner.
5.  Explain what the output tells you.

### Intermediate

1.  Install VirtualBox.
2.  Build an isolated Kali and Metasploitable 2 lab.
3.  Learn basic network concepts: IP addresses, ports, TCP, HTTP, and
    DNS.
4.  Identify services in your lab.
5.  Research a known vulnerability and its affected versions.
6.  Read the corresponding module documentation.
7.  Perform only approved, non-destructive validation before attempting
    any exploit.

## 15. Glossary

-   **IP address:** A network address assigned to a device or interface.
-   **Port:** A numbered endpoint used by a network service.
-   **TCP:** A transport protocol that provides reliable, ordered
    communication.
-   **HTTP:** A protocol used by web applications.
-   **HTTPS:** HTTP protected using TLS.
-   **Vulnerability:** A weakness that could affect confidentiality,
    integrity, or availability.
-   **CVE:** A public identifier for a known cybersecurity
    vulnerability.
-   **False positive:** A result that reports a possible issue that is
    not actually present.
-   **False negative:** A test that fails to identify a weakness that is
    present.
-   **Remediation:** A change made to fix or reduce a security risk.

## 16. Official learning resources

-   [Metasploit Documentation](https://docs.metasploit.com/)
-   [Rapid7 Metasploit
    Documentation](https://docs.rapid7.com/metasploit/)
-   [Metasploit Framework on
    GitHub](https://github.com/rapid7/metasploit-framework)
-   [Kali Linux -- Get Kali](https://www.kali.org/get-kali/)
-   [Metasploitable 2
    documentation](https://docs.rapid7.com/metasploit/metasploitable-2/)
-   [Oracle VirtualBox
    Downloads](https://www.virtualbox.org/wiki/Downloads)

------------------------------------------------------------------------

## Final advice

Do not try to memorize hundreds of commands. First understand what a
module does, what its options mean, and what impact running it can have.

A good learning order is:

1.  Linux terminal basics.
2.  Networking fundamentals.
3.  Metasploit console commands.
4.  Auxiliary modules and safe scanning.
5.  Vulnerability research.
6.  Isolated lab practice.
7.  Reporting and remediation.

**Learn ethically. Test only with permission. Secure what you
discover.**
