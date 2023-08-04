## SDN 
### mininet
#### 搭建自定义拓扑 
##### 1、导入相关模块 
```
from mininet.net import Mininet  
from mininet.node import Controller, RemoteController 
from mininet.cli import CLI 
from mininet.log import setLogLever, info 
from mininet.link import Link, TCLink 
from mininet.topo import Topo
import logging 
import os
``` 
##### 重要函数
``` 
sw1 = self.addSwitch("s1")  
host1 = self.addHost("h1") 
self.addLink(sw1, host1, 2)
self.addLink(sw1, sw2, 1, 1)  
```

##### 示例
``` 
class CreateTopo(Topo):
    def __init__(self, switch_num, host_num, link_list_interSwitch, link_list_interHost):
        logger.debug("Class SimpleTopo init")
        self.switch_list = []
        self.host_list = []
        Topo.__init__(self)
        self.create_switch(switch_num)
        self.create_host(host_num)
        self.create_link_interSwitch(link_list_interSwitch)
        self.create_link_interHost(link_list_interHost)

    def create_switch(self, length):
        for i in range(0, length):
            self.switch_list.append(self.addSwitch('s' + str(i)))

    def create_host(self, length):
        for i in range(0, length):
            self.host_list.append(self.addHost('h' + str(i)))

    def create_link_interSwitch(self, link_list_interSwitch):
        for list_1 in link_list_interSwitch:
           self.addLink(self.switch_list[list_1[0]], self.switch_list[list_1[1]])

    def create_link_interHost(self, link_list_interHost):
        for list_1 in link_list_interHost:
           self.addLink(self.switch_list[list_1[0]], self.host_list[list_1[1]])

def create_topo(switch_num, host_num, link_list_interSwitch, link_list_interHost):
     topo = CreateTopo(switch_num, host_num, link_list_interSwitch, link_list_interHost)
     CONTROLLER_IP = "127.0.0.1"
     CONTROLLER_PORT = 6633
     net = Mininet(topo = topo, link=TCLink, controller=None)
     net.addController('controller', controller=RemoteController, ip=CONTROLLER_IP, port=CONTROLLER_PORT)
     net.start()
     CLI(net)
     net.stop()

if __name__ == '__main__':
     link_list_interSwitch = [[0,1], [0,2], [1,3], [1,4], [2,5]]
     link_list_interHost = [[3,0], [4,1], [5,2]]
     logger = logging.getLogger(__name__)
     setLogLevel('info')
     print 'first'
     if os.getuid() != 0:
         logger.debug("You are Not root")
         
     if os.getuid() == 0:
         
         create_topo(6, 3, link_list_interSwitch, link_list_interHost)
         
   
           
```

#### 拓展自定义功能 
##### 

