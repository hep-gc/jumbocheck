jumbocheck 0.2.1
================

Checks to see if a list of hosts can be reached without having
to fragment ping packets. 

It sends two pings, one set with a smaller size packet (default 64 bytes) that should fit in a normal 
1500 byte packet and one larger that should require jumbo frames (default 8000 bytes).

This program depends on ping and as a result will be fragile if
the output of this program changes. It has only been tested on Scientific Linux 6.

Requirements
----------

Obviously to get anthing useful from this script you need to have an interface which
supports jumbo frames. I suspect this this will work on most python 2.4+ but I haven't tested. 
It also depends on your ping working something like my ping.

Usage
-------
    $ ./jumbocheck --regular-mtu 64 --jumbo-mtu 8000 --file host-list.txt


Where `host-list.txt` is a text file containing a list of host, one per line, that you want to test.

This should give you some output that looks like this:

```
64    x host1.edu.au                        dropped
64 8000 host2.hepnetcanada.ca               jumbo
64    x host3.something.com                 fragmented
x     x host4.paranoid.com                  unreachable
```

| *code*        | *meaning* |
| ------------- | ------- |
| unreachable   | Host is not reachable with an ICMP ping. It Could be blocked or the hosts is down. |
| jumbo_dropped | The jumbo packet was dropped. |
| fragmented    | The ping worked but the packet was fragmented |
| jumbo         | The packet reached the host intacted at full size |


Installing
---------

With git:

    $ git clone git://github.com/hep-gc/jumbocheck.git
    $ cd jumbocheck
    $ ./jumbocheck -f ~/host-list.txt

Without git:

    $ wget https://github.com/hep-gc/jumbocheck/archive/v0.2.tar.gz -O jumbocheck-0.2.tar.gz
    $ tar xzvf jumbocheck-0.2.tar.gz
    $ cd jumbocheck-0.2/
    $ ./jumbocheck -f ~/host-list.txt
    

Hints for LCG sites
------------------

Make your list of hosts:

    $ lcg-infosites --vo atlas se | awk '/SRM/' | awk '{print $4;}' | sort -u > unique-srms.txt
    
Contributors
-------------

Ian Gable <igable@uvic.ca>
Florent Parent <florent.parent@calculquebec.ca>
    

