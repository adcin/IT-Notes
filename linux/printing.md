# Common Unix Printing System (CUPS)
CUPS - a standards-based, open source printing system.

- [wikipedia.org - CUPS](https://en.wikipedia.org/wiki/CUPS)
- [github.io - OpenPrinting CUPS](https://openprinting.github.io/cups/)
- [GitHub.com - OpenPrinting/cups](https://github.com/openprinting/cups)
- [cups.org - Apple CUPS](https://www.cups.org)
- [opensource.com - How to set up your printer on Linux](https://opensource.com/article/21/8/add-printer-linux)
- [linuxconfig.org - Linux cups tutorial for beginners](https://linuxconfig.org/linux-cups-tutorial-for-beginners)
- [baeldung.com - Common UNIX Printing System (CUPS) Installation and Listing Available Printers](https://www.baeldung.com/linux/cups)
- [baeldung.com - CUPS Printer Management and Job History](https://www.baeldung.com/linux/common-unix-printing-system-lpstat-job-history)

> [!quote] &nbsp;Excerpt from man page - DESCRIPTION
> 
> ```
> CUPS is the software you use to print from applications like word processors, email readers, photo editors, and web browsers. It converts the page descriptions produced by your application (put a paragraph here, draw a line there, and so forth) into something your printer can understand and then sends the information to the printer for printing.
> 
> Now, since every printer manufacturer does things differently, printing can be very complicated. CUPS does its best to hide this from you and your application so that you can concentrate on printing and less on how to print. Generally, the only time you need to know anything about your printer is when you use it for the first time, and even then CUPS can often figure things out on its own.
> ```

## Web interface
CUPS has a built in web interface: [http://localhost:631](http://localhost:631)

## Commands

##### CUPS CLI

`cancel`  - cancel jobs
- The `cancel` command cancels print jobs.  If no destination or id is specified, the currently printing job on the default destination is canceled.
`cupsaccept` / `cupsreject` - accept/reject jobs sent to a destination
- The `cupsaccept` command instructs the printing system to accept print jobs to the specified destinations.
- The `cupsreject` command instructs the printing system to reject print jobs to the specified destinations. The -r option sets the reason for rejecting print jobs. If not specified, the reason defaults to "Reason Unknown".
`cupsdisable` / `cupsenable` - stop/start printers and classes
- `cupsenable` starts the named printers or classes while `cupsdisable` stops the named printers or classes.

##### BSD command-line printing utility
CUPS also accepts these legacy/deprecated commands:

`lpc` - line printer control program (deprecated)
- `lpc`  provides  limited control over printer and class queues provided by CUPS. It can also be used to query the state of queues.
- If no command is specified on the command-line, `lpc` displays a prompt and  accepts  commands  from the standard input.
`lpq` - show printer queue status
- `lpq` shows the current print queue status on the named printer. Jobs queued on the default destination will be shown if no printer or class is specified on the command-line.
- The +interval option allows you to continuously report the jobs in the queue until the queue is empty; the list of jobs is shown once every interval seconds.
`lpr` - print files
- `lpr` submits files for printing. Files named on the command line are sent to the named printer or the default destination if no destination is specified. If no files are listed on the command line, `lpr` reads the print file from the standard input.
`lprm` - cancel print jobs
- `lprm` cancels print jobs that have been queued for printing. If no arguments are supplied, the current job on the default destination is canceled. You can specify one or more job ID numbers to cancel those jobs or use the - option to cancel all jobs.