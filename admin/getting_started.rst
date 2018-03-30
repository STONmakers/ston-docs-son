.. _getting-started:

Chapter 2. Getting Started
**************************

This chapter will cover the installation and configuration of the system as well as how to set up a sample virtual host. A simple text editor is all that is necessary.

STON was developed to run on standard Linux servers, and minimize dependence on the hardware, OS, or file system, among others. However, it is still important to choose the most suitable equipment for the job, in order to set up the server to match the service's individual qualities.


.. toctree::
   :maxdepth: 2
   

.. _getting-started-serverconf:

Setting Up the Server
====================================

Generally, setting up a server means considering the CPU, memory, and disk. For example, if a service requires high performance on the level of 10 Gbps throughput, then each component must meet the requirements to reach the desired performance.

-  CPU
    A CPU with at least four cores (quad-core) is recommended. Because STON gains scalability when it comes to multi-core CPUs, the per-second throughput increases with more cores. However, higher throughput does not necessarily mean higher traffic.

    .. figure:: img/10g_cpu.jpg
      :align: center

      The more clients there are, the more useful it is to have many CPUs.
    
    Transferring a 4 KB file around 260,000 times takes the same amount of bandwidth as transferring a 1 GB file once. The most important criterion in choosing a CPU is the number of parallel connections the service must make.
-  Memory
    At least 4GB of memory is recommended to be used in memory indexing (see also :ref:`adv_topics_mem`). Content that is frequently accessed will always be stored in memory, but content that is not will have to be loaded from disk. As such, if there is a lot of content with a large spread (a long-tailed distribution), then the load on the disk will be higher and performance may decline. If disk I/O load is high because of the size of the content, regardless of the amount of content, then memory can simply be added to decrease the load.

-  Disk
    At least three disks, including the OS, is recommended. As one would expect, more disks lead to better performance, as I/O load will be dispersed and more content can be cached.
   
    .. figure:: img/02_disk.png
      :align: center
      
      The OS and STON will always be installed on a disk separate from the content.
   
    In general, STON will be installed on the same disk as the OS. The log is also generally installed on the same disk. Because the log must record the service status in real time, it will always create write load.
   
   The STON disks will be used in the RAID 0 setting. The correlation between performance and RAID changes depending on the client service's qualities. However, if file changes are infrequent and content size is much larger than physical memory, increasing read speed with RAID may be effective.

.. _getting-started-os:

Setting Up the OS
====================================

In its most basic form, STON will operate normally on standard 64-bit Linux distributions (CentOS 6.2 or higher, Ubuntu 10.04 or higher), and does not require any particular package.


.. _getting-started-install:

Installation
====================================

#. Download the latest version of STON. ::

      [root@localhost ~]# wget  http://foobar.com/ston/ston.2.0.0.rhel.2.6.32.x64.tar.gz
      --2014-06-17 13:29:14--  http://foobar.com/ston/ston.2.0.0.rhel.2.6.32.x64.tar.gz
      Resolving foobar.com... 192.168.0.14
      Connecting to foobar.com|192.168.0.14|:80... connected.
      HTTP request sent, awaiting response... 200 OK
      Length: 71340645 (68M) [application/x-gzip]
      Saving to: “ston.2.0.0.rhel.2.6.32.x64.tar.gz”
      
      100%[===============================================>] 71,340,645  42.9M/s   in 1.6s
      
      2014-06-17 13:29:15 (42.9 MB/s) - “ston.2.0.0.rhel.2.6.32.x64.tar.gz” saved [71340645/71340645]


#. Extract the downloaded package. ::

		[root@localhost ~]# tar -zxf ston.2.0.0.rhel.2.6.32.x64.tar.gz

#. Run the installation script. ::

		[root@localhost ~]# ./ston.2.0.0.rhel.2.6.32.x64.sh

#. All installation processes are recorded in the install.log file. Any problems that occur during the installation process will be noted there. ::

      #DownloadURL: http://foobar.com/ston/ston.2.0.0.rhel.2.6.32.x64.tar.gz
      #DownloadTime: 13 sec
      #Target: STON 2.0.0
      #Date: 2014.03.03 16:48:35
      Prepare for STON 2.0.0 install process
          Stopping STON...
              STON stopped
      [Copying files]
          `./fuse.conf' -> `/etc/fuse.conf'
          `./libfuse.so.2' -> `/usr/local/ston/libfuse.so.2'
          `./libtbbmalloc_proxy.so' -> `/usr/local/ston/libtbbmalloc_proxy.so'
          `./start-stop-daemon' -> `/usr/sbin/start-stop-daemon'
          `./libtbbmalloc_proxy.so.2' -> `/usr/local/ston/libtbbmalloc_proxy.so.2'
          `./libtbbmalloc.so' -> `/usr/local/ston/libtbbmalloc.so'
          `./libtbbmalloc.so.2' -> `/usr/local/ston/libtbbmalloc.so.2'
          `./libtbb.so' -> `/usr/local/ston/libtbb.so'
          `./libtbb.so.2' -> `/usr/local/ston/libtbb.so.2'
          `./stond' -> `/usr/local/ston/stond'
          `./stonx' -> `/usr/local/ston/stonx'
          `./stonr' -> `/usr/local/ston/stonr'
          `./stonu' -> `/usr/local/ston/stonu'
          `./stonapi' -> `/usr/local/ston/stonapi'
          `./server.xml.default' -> `/usr/local/ston/server.xml.default'
          `./vhosts.xml.default' -> `/usr/local/ston/vhosts.xml.default'
          `./ston_format.sh' -> `/usr/local/ston/ston_format.sh'
          `./ston_diskinfo.sh' -> `/usr/local/ston/ston_diskinfo.sh'
          `./wm.sh' -> `/usr/local/ston/wm.sh'
      [Exporting config files]
          #Export so directory
          /usr/local/ston/ to ld.so.conf
          #Export sysctl to /etc/sysctl.conf
          vm.swappiness=0
          vm.min_free_kbytes=524288
          #Export sudoers for WM
          Defaults    !requiretty
          winesoft ALL=NOPASSWD: /etc/init.d/ston stop, /etc/init.d/ston start, /bin/ps -ef
      [Configuring STON daemon script]
          STON deamon activate in run-level 2345.
      [Installing sub-packages]
          curl installed.
          libjpeg installed.
          libgomp installed.
          rrdtool installed.
      [Installing WM]
          Stopping WM...
          WM stopped
          `./wm.server_default.xml' -> `/usr/local/ston/wm/tmp/conf/server_default.xml'
          `./wm.vhost_default.xml' -> `/usr/local/ston/wm/tmp/conf/vhost_default.xml'
          WM configuration found. Current WM port : 8500
          PHP module for Legacy(CentOS 5.5) installed
          `./libphp5.so.5.5' -> `/usr/local/ston/wm/modules/libphp5.so'
          WM installation almost complete. Changing WM privileges.
      Installation successfully complete


.. _getting-started-license:

Obtaining a License
====================================

New clients can obtain a license via this process.

#. Fill out the `application form <http://www.winesoft.us/lic_req.doc>`_.
#. Email the completed form to license@winesoft.co.kr.
#. The license will be issued after confirmation.

The license file (license.xml) must be in the installation directory for STON to run properly.


.. _getting-started-update:

Update
====================================
Use the "stonu" command to update STON to the latest version. ::

	./stonu 2.0.1
	
See also :ref:`wm-update` from :ref:`wm` to easily update STON.

   .. figure:: img/conf_update1.png
      :align: center


.. _getting-started-run:

Run
====================================

STON will be installed in the following default directory. ::

    /usr/local/ston/

If any of the following files is missing or has invalid XML syntax, STON will not run.

- license.xml
- server.xml
- vhosts.xml

After the initial installation, not every XML file will be present. In this case, the distributed license.xml file should be placed in the directory. The server.xml.default and vhosts.xml.default files should also be placed in the directory. The \*.default files are always included in the latest package.


.. _getting-started-samplevhost:

Hello World
====================================
Open vhosts.xml and edit with the following code. ::

    <Vhosts>
        <Vhost Name="www.example.com">
            <Origin>
                <Address>hello.winesoft.co.kr</Address>
            </Origin>
        </Vhost>
    </Vhosts>   


.. _getting-started-runston:

Running STON
-----------------------------------------------
#. Copy license.xml to the installation directory.

#. Open server.xml and configure <Storage>. ::

    <Server>
        <Cache>
            <Storage>
                <Disk>/cache1/</Disk>
                <Disk>/cache2/</Disk>
            </Storage>
        </Cache>
    </Server>
      
.. note::

   STON normally uses disk storage. A disk must be configured in order to run STON. Disk set-up details can be found in the next chapter.
   
3. Run STON.  ::

      [root@localhost ~]# service ston start

   To stop STON, use the "stop" command. ::

      [root@localhost ~]# service ston stop


.. _getting-started-runcheck:

Checking the Virtual Host
-----------------------------------------------

(For Windows 7) Add www.example.com in the C:\\Windows\\System32\\drivers\\etc\\hosts file as shown below. ::

    192.168.0.100        www.example.com

If all settings are correctly configured, the following page will be displayed on the browser when www.example.com is accessed.

   .. figure:: img/helloworld3.png
      :align: center


.. _getting-started-rrderr:

If WM is Slow or the Graph Isn't Displayed
-----------------------------------------------

RRDtool is dynamically downloaded and installed during installation, and may not be installed properly under a restricted network. Moreover, :ref:`wm` may run very slowly or :ref:`api-graph` may not work at all. To fix these issues, follow the steps below.


#. Check Installation Status

   Checking the installation status of RRDtool can be done as follows. ::
   
      [root@localhost ston]# yum install rrdtool
      Loaded plugins: fastestmirror, security
      Loading mirror speeds from cached hostfile
      * base: centos.mirror.cdnetworks.com
      * elrepo: ftp.ne.jp
      * epel: mirror.premi.st
      * extras: centos.mirror.cdnetworks.com
      * updates: centos.mirror.cdnetworks.com
      Setting up Install Process
      Package rrdtool-1.3.8-6.el6.x86_64 already installed and latest version
      Nothing to do
      
   (For Ubuntu) ::

      root@ubuntu:~# apt-get install rrdtool
      Reading package lists... Done
      Building dependency tree
      Reading state information... Done
      rrdtool is already the newest version.
      The following packages were automatically installed and are no longer required:
        libgraphicsmagick3 libgraphicsmagick++3 libgraphicsmagick1-dev libgraphics-magick-perl libgraphicsmagick++1-dev
      Use 'apt-get autoremove' to remove them.
      0 upgraded, 0 newly installed, 0 to remove and 102 not upgraded.
      
      
#. RRD Manual Install

   If yum is unable to install RRDtool, the administrator should `download <http://pkgs.repoforge.org/rrdtool/>`_ the right package for the OS and proceed with manual installation.   
   
======================================== =================== ======= ============================
Name                                     Last Modified       Size    Description
======================================== =================== ======= ============================
tcl-rrdtool-1.4.7-1.el5.rf.i386.rpm      06-Apr-2012 16:57   36K     RHEL5 and CentOS-5 x86 32bit
tcl-rrdtool-1.4.7-1.el5.rf.x86_64.rpm	 06-Apr-2012 16:57   37K     RHEL5 and CentOS-5 x86 64bit
tcl-rrdtool-1.4.7-1.el6.rfx.i686.rpm     06-Apr-2012 16:57   35K     RHEL6 and CentOS-6 x86 32bit
tcl-rrdtool-1.4.7-1.el6.rfx.x86_64.rpm	 06-Apr-2012 16:57   35K     RHEL6 and CentOS-6 x86 64bit
======================================== =================== ======= ============================


.. _env-vhost-activeorigin:

Origin Server
============================================

The purpose of the virtual host is to provide content in place of the origin server. Various types of origin servers can be accessed in various ways, depending on what is most suitable for the service platform. ::

    <Vhosts>
        <Vhost Name="www.example.com">
            <Origin>
                <Address>1.1.1.1</Address>
                <Address>1.1.1.2</Address>
            </Origin>
        </Vhost>
    </Vhosts>

-  ``<Address>``

   The address of the origin server from which the virtual host will copy content. There is no limit to the number of addresses that can be added. If there are more than two addresses, selection follows the active/active model (round-robin). If the origin server port is 80, it can be omitted.

For example, if the origin server is using a different port such as 8080, then the port number must be specified as 1.1.1.1:8080. There are eight different ways to format addresses using the {IP|Domain}{Port}{Path} format.

============================== ==========================
Address                        Host Header
============================== ==========================
1.1.1.1	                       Virtual host name
1.1.1.1:8080	               Virtual host name:8080       
1.1.1.1/account/dir	           Virtual host name            
1.1.1.1:8080/account/dir       Virtual host name:8080       
example.com	                   example.com             
example.com:8080	           example.com:8080        
example.com/account/dir	       example.com             
example.com:8080/account/dir   example.com:8080
============================== ==========================

As long as the host header is not specified in the :ref:`origin-httprequest`, the host header from the table above will be transmitted.::

    <Vhosts>
        <Vhost Name="www.example.com">
            <Origin>
                <Address>origin.com:8888/account/dir</Address>            
            </Origin>
        </Vhost>
    </Vhosts>

For example, the above configuration will request the following to the origin server. ::

   GET / HTTP/1.1
   Host: origin.com:8888

.. note::

   If a path is added to the origin server's address (e.g. example.com/account/dir), then the requested URL will be placed after the path. If a client requests /img.jpg, the resulting address becomes example.com/account/dir/img.jpg.


.. _env-vhost-standbyorigin:

Standby Origin Server Address
------------------------------------------------

The standby origin server can be configured as follows.::

    <Vhosts>
        <Vhost Name="www.example.com">
            <Origin>
                <Address>1.1.1.1</Address>
                <Address>1.1.1.2</Address>
                <Address2>1.1.1.3</Address2>
                <Address2>1.1.1.4</Address2>
            </Origin>
        </Vhost>
    </Vhosts>
    
-  ``<Address2>``

   If all ``<Address>`` es are working without problems,  ``<Address2>`` will not be used. However, if any failures are detected in the active servers, a standby server will be used as a replacement until the failed server recovers. If a failure occurs in a standby server, the server will never be used until it can recover.


.. _api-etc-help:

API Call
====================================

STON provides HTTP-based API. API calls are authorized by :ref:`env-host`. If a call is unauthorized, the connection will be terminated immediately.

The STON version can be checked as follows. ::

   http://127.0.0.1:10040/version
    
The same API can be called with Linux shell commands. ::

   ./stonapi version

.. note::

   The ampersand (&) is recognized as a delimiter for QueryStrings in the HTTP API, but it means something different in a Linux console. When running commands or using arguments that contain &s, you must wrap the string in quotes ("/...&...").


Hardware Status
====================================

The following will look up hardware information. :: 

   http://127.0.0.1:10040/monitoring/hwinfo
   
The results are returned in JSON format. ::

   {
      "version": "1.1.9",
      "method": "hwinfo",
      "status": "OK",
      "result":
      {
         "OS" : "Linux version 3.3.0 ...(omitted)...", 
         "STON" : "1.1.9", 
         "CPU" : 
         { 
            "ProcCount": "4", 
            "Model": "Intel(R) Xeon(R) CPU           E5606  @ 2.13GHz", 
            "MHz": "1200.000", 
            "Cache": "8192 KB"
         }, 
         "Memory" : "8 GB", 
         "NIC" : 
         [ 
            { 
               "Dev" : "eth1", 
               "Model" : "Intel Corporation 82574L Gigabit Network Connection", 
               "IP" : "192.168.0.13", 
               "MAC" : "00:25:90:36:f4:cb" 
            } 
         ], 
         "Disk" : 
         [ 
            { 
               "Dev" : "sda", 
               "Model" : "HP DG0146FAMWL (scsi)", 
               "Total" : "238787584", 
               "Usage" : "40181760" 
            },
            {
               "Dev" : "sdb", 
               "Model" : "HITACHI HUC103014CSS600 (scsi)", 
               "Total" : "144706478080", 
               "Usage" : "2101075968" 
            },
            {
               "Dev" : "sdc", 
               "Model" : "HITACHI HUC103014CSS600 (scsi)", 
               "Total" : "144706478080", 
               "Usage" : "2012160000" 
            }
         ]
      } 
   }


Restart/Quit
====================================

The following commands restart or quit STON. To avoid unintended results, STON asks for confirmation for restart/quit commands on the web page. ::

   http://127.0.0.1:10040/command/restart
   http://127.0.0.1:10040/command/restart?key=JUSTDOIT       // Immediately executes command
   http://127.0.0.1:10040/command/terminate
   http://127.0.0.1:10040/command/terminate?key=JUSTDOIT       // Immediately executes command
   

.. _getting-started-reset:

Caching Reset
====================================

The following commands stop the service and discard all cached content. The commands will format all disks and resume the service when completed. ::

   http://127.0.0.1:10040/command/cacheclear
   http://127.0.0.1:10040/command/cacheclear?key=JUSTDOIT       // Immediately executes command
   
In the console window, the following commands will reset all or one of the virtual hosts. ::

   ./stonapi reset
   ./stonapi reset/ston.winesoft.co.kr