# CIS-890-E-Lab-1
Be familiar with Mininet
## Virtual Machine Summary
Memory: >= 8GB

Storage: 50GB

CPU: 2 cores, AMD64 Architecture

Installation Disc: [ubuntu-22.04.4-desktop-amd64.iso](https://old-releases.ubuntu.com/releases/22.04/)

## Build and Run Mininet VM

1. Install mininet
  ```
  sudo apt-get update
  sudo apt-get install mininet
  ```
2. Play mininet with default topology
   (1) 
   ```
   sudo mn
   ```
   (2) In the mininet terminal:
   ```
   pingall
   ```
   (3) Observe the result and find the reasons:
   ```
   sudo ovs-ofctl -O OpenFlow13 dump-flows s1
   ```
   (4) Describe what you observe and why
3. Play mininet with preconfigured topology
   (1)
   ```
   sudo mn --topo tree,depth=3,fanout=2
   ```
   (2) In the mininet terminal:
   ```
   pingall
   ```
   (3) Observe the result and find the reasons:
   ```
   sudo ovs-ofctl -O OpenFlow13 dump-flows s1
   ```
4. Play mininet with customized topology
   (1) Customize your topology, create a file named "topo-2sw-2host.py", copy below into the file.
   ```
   """Custom topology example

Two directly connected switches plus a host for each switch:

   host --- switch --- switch --- host

Adding the 'topos' dict with a key/value pair to generate our newly defined
topology enables one to pass in '--topo=mytopo' from the command line.
"""

from mininet.topo import Topo

class MyTopo( Topo ):
    "Simple topology example."

    def build( self ):
        "Create custom topo."

        # Add hosts and switches
        leftHost = self.addHost( 'h1' )
        rightHost = self.addHost( 'h2' )
        leftSwitch = self.addSwitch( 's3' )
        rightSwitch = self.addSwitch( 's4' )

        # Add links
        self.addLink( leftHost, leftSwitch )
        self.addLink( leftSwitch, rightSwitch )
        self.addLink( rightSwitch, rightHost )


topos = { 'mytopo': ( lambda: MyTopo() ) }
```
(2) Run mininet with the customized topology
   ```
sudo mn --custom ~/topo-2sw-2host.py --topo mytopo --switch ovsk,protocols=OpenFlow13 --controller remote,ip=127.0.0.1,port=6653
```
(3) In the mininet terminal:
   ```
   pingall
   ```
   (4) Observe the result and find the reasons:
   ```
   sudo ovs-ofctl -O OpenFlow13 dump-flows s1
   ```
   
