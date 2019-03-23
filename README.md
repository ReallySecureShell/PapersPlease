
# PapersPlease

<div align="center">

<img src="./Assets/Papers_Please_Logo.png" width="20%" />
<br><br>
</div>

## Description

PapersPlease is an exploit that allows an attacker to remotely crash an HP printer.
So far residential class printers have been observed to be exploitable by this vulnerability.
The tool also comes with the ability to mass print documents from either a single printer 
or all printers on a network by abusing HPs printing protocol found on port 9100.

Notice that the program will stop you from connecting to known government external 
IP addresses. While not blocking all of them, it is there to stop skiddies from accidentally 
(or purposefully) scanning government addresses… as that would be bad.  

Also note that while exploiting a network range, you will only be able to specify a private IP address. 
As I’m not going to be responsible for some kid taking every port-forwarded printer down 
on the Internet, as funny as that would be. 

## Usage

### CLI Options

`-c`, `--color=NAME` Set a colorscheme.  
`-m`, `--minimal` Only show CPU, Mem and Process widgets.  
`-r`, `--rate=RATE` Number of times per second to update CPU and Mem widgets [default: 1].  
`-V`, `--version` Print version and exit.  
`-p`, `--percpu` Show each CPU in the CPU widget.  
`-a`, `--averagecpu` Show average CPU in the CPU widget.  
`-s`, `--statusbar` Show a statusbar with the time.  
`-b`, `--battery` Show battery level widget (`minimal` turns off). [preview](./assets/battery.png)


