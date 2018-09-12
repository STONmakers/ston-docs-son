.. _filesystem:

Chapter 18. File System
***********************

This chapter will explain how to utilize STON as if it were a local disk. STON is based on `FUSE <http://fuse.sourceforge.net/>`_ and is mounted on the Linux VFS (Virtual File System). All files in the mounted directory will be cached the moment they are accessed, but other processes will not notice. You can consider this system as a **ReadOnly disk with a Caching function**.

.. figure:: img/conf_fs1.png
   :align: center
      
   Fuse structure.

If the Linux Kernel delivers structural File I/O function calls directly to STON, no other elements (e.g. physical File I/O, socket transmission) can interfere with the process. This architecture makes extremely high performance possible. Using STON's memory caching, you can expect performance that's better than physical disk access.

.. toctree::
   :maxdepth: 2

Mount
====================================

Mount is configured in global settings (server.xml). ::

   # server.xml - <Server><Cache>

   <FileSystem Mount="/cachefs" DotDir="OFF" Separator="^">OFF</FileSystem>   
    
-  ``<FileSystem>``

   -  ``OFF (default)`` Does nothing.
   
   -  ``ON`` STON will be mounted onto the path given by the ``Mount`` property.
   
This was developed in such a way that the existing HTTP structure is preserved, but a file system that can access the cache module is added. As such, regardless of where the access comes from, caching occurs only once and is given service by either HTTP or File I/O. The file system is a new bridge added that allows access to the cache module.
   
.. figure:: img/conf_fs2.png
   :align: center
      
   HTTP and File I/O share a cache module.

The content on the origin server can be accessed not only by HTTP but also by File I/O. Using this, you can increase the availability of solutions that are based on local files.

.. figure:: img/conf_fs3.png
   :align: center
      
   Any server is OK.
   
The current list of functions that support the STON File System is as follows.

========= =============== ===========
FUSE	  C	              LINUX
========= =============== ===========
open	  fopen	          open
release	  fclose	      close
read	  fseek, fread	  seek, read
getattr	  fstat	          stat
unlink	  remove	      unlink
========= =============== ===========

File I/O goes through several internal steps. It is important to understand what goes on at each step to obtain the best performance.

Searching for Virtual Hosts
====================================

The first step is searching for the virtual host to be accessed. In an HTTP header, the Host header is specified as below, making it easy to find the virtual host. ::

    GET /ston.jpg HTTP/1.1
    host: example.com
    
This can be done in the file system using its first directory. For example, if STON is mounted on the /cachefs directory, local files can be accessed with the following path. ::

    /cachefs/example.com/ston.jpg
        
:ref:`env-vhost-find` will work in the same way. If the ``<Alias>`` of example.com is set to \*.example.com, then the following paths will access the same file. ::

    /cachefs/example.com/ston.jpg
    /cachefs/img.example.com/ston.jpg
    /cachefs/example.example.com/ston.jpg
    
For example, in order to link example.com to the Apache server, you must set the DocumentRoot to /cachefs/example.com/.


File/Directory
====================================

The file system can be configured for each virtual host. Alternatively, a default virtual host can be configured to give all virtual hosts the same settings. ::

   # server.xml - <Server><VHostDefault><Options>
   # vhosts.xml - <Vhosts><Vhost><Options>

   <FileSystem Status="Active" DotDir="OFF">                    
      <FileTime>origin</FileTime>
      <FileStatus>200</FileStatus>
      <DirStatus>301, 302, 400, 401, 403</DirStatus>
      <Unlink>Purge</Unlink>      
   </FileSystem>   

-  ``<FileTime> (default: Origin)``
   Returns the Last-Modified time from the origin server when set to ``Origin``, or the local cached time when set to ``Local``. If the origin server does not return a Last-Modified time when set to ``Origin``, then the file time will be returned as the Unix epoch as seen below.
   
   .. figure:: img/fs_filetime.png
      :align: center
    
-  ``<FileSystem>``
   The file system cannot be accessed if ``Status`` is ``Inactive``. It must be set to ``Active``.

-  ``<FileStatus> (default: 200)``
   Configures the origin server HTTP response code that will be recognized as a file. Generally, 200 is used, but there are no specific restrictions.
   
-  ``<DirStatus> (default: 301, 302, 400, 401, 403)``
	Configures the origin server HTTP response code that will be recognized as a directory. The default values are usually 301, 302, 400, 401, or 403.
    
-  ``<Unlink> (default: Purge)``
   Configures the behavior to be used for a file deletion request, choosing from ``Purge``, ``Expire``, or ``HardPurge``.
  
Each origin server can interpret HTTP response codes in different ways. As such, it is important to configure how each HTTP response code should be interpreted.

In most cases, if a file exists on the origin server the response will be **200 OK**. If a directory is accessed, the response will be **403 Forbidden** or a redirect to another page with **302 Found**. Multiple response codes can be set with commas (,) to identify the Body of corresponding HTTP response codes as files or directories. Response codes that are not configured will be considered non-existing, and File I/O will fail.

File Properties
====================================

In general, the first step of File I/O is to obtain the properties of the file. It is obvious to obtain the file information before opening the file. The process of the Kernel providing the file properties as seen by STON is portrayed in the figure below. (/cachefs is the mounted directory and is omitted by the Kernel.)

.. figure:: img/conf_fs4.png
   :align: center
      
   The process of obtaining the file properties.

In Linux, files are not distinguished from directories, so obtaining the file properties can be more complicated than it seems. As can be seen from the above figure, as the number of subfolders increases, more unnecessary virtual host searches and file accesses will occur, lowering performance. In particular, requests for inaccessible directories like /one or /one/two will be made, causing load on the origin server. Of course, if the file is cached, the origin server will not be accessed during the TTL, but it is clear that this is not an elegant solution.

A heuristic solution for this structural load is to add a ``DotDir`` property. ``DotDir`` is a function that will recognize paths without a dot (.) as a directory. The above figure is the result of ``DotDir`` having been set to ``OFF``. If ``DotDir`` is set to ``ON``, the following will occur.

.. figure:: img/conf_fs5.png
   :align: center
      
   Enabling (``ON``) the global ``DotDir``.

There is no change in the process or number of Kernel calls. However, if the requested paths do not contain a dot (.), it will not go all the way to the virtual host and instead return immediately as a directory, allowing the virtual host and files to be accessed only when necessary. This function is based on the observation that most programmers do not assign extensions to directories. It is important that you check how directories are set up before using this function.

``DotDir`` is a global property of ``<FileSystem>``. In other words, if none of the virtual hosts use dots (.) for directories, it will be very effective to set ``DotDir`` to ``ON``. Of course, even if ``DotDir`` is set to ``OFF``, you can still configure it on each virtual host separately. Doing so can lower performance slightly, as shown below.

.. figure:: img/conf_fs6.png
   :align: center
      
   Enabling (``ON``) the virtual host ``DotDir``.

Virtual host searches will still occur, but files will only be accessed if there is a dot (.). As the system's performance is affected by how many times it is called, it is highly recommended to understand this function thoroughly.


Reading Files
====================================

Though the process to obtain file properties is complicated, reading files is much simpler. First, the file is opened. All files will of course be read-only, so accessing a file with write permissions will fail. When a file is accessed for the first time, the file will be cached from the origin server just like the HTTP service. While downloading the requested file, the File I/O service is run concurrently so that the process is not delayed.

.. figure:: img/conf_fs7.png
   :align: center
      
   Opening a file.

Once a file is opened, the behavior will be identical to the HTTP service. HTTP is more advantaged in file transfer because sequential file access occurs from an initially determined range. On the other hand, File I/O can generate a large number of read accesses on the scale of 1 KB regardless of file size. STON has implemented `Readahead <http://en.wikipedia.org/wiki/Readahead>`_ in the cache module in order to maximize performance, especially File I/O performance.

If the function to close a file (e.g. fclose) is called or a process is terminated, the file handle is turned in by the Kernel, which is the same as an HTTP transaction being closed.


Deleting Files
====================================
Cached files are managed by STON, but the process can send a request to delete a file. STON offers several :ref:`api-cmd-purge` methods to respond to these requests.

For example, if ``<Unlink>`` is set to ``expire``, the corresponding file will be expired upon a file deletion request. If the Kernel tries to access the file again, the file must be checked for modification on the origin server because the file is expired. If the file was not modified, it can then be provided again.


File Expansion
====================================
HTTP can dynamically process a file using an URL as seen below. :: 
    
    # Trims a 0-60 second section of /video.mp4 via HTTP.
    http://www.example.com/video.mp4?start=0&end=60
    
This QueryString format can be used in the same way for both HTTP and the file system. ::

    # Accesses the local file made from trimming a 0-60 second section of /video.mp4.
    /cachefs/www.example.com/video.mp4?start=0&end=60
    
However, putting the processing options at the end of a URL as seen in MP4HLS and DLS can cause problems in File I/O. ::

    /cachefs/image.winesoft.com/img.jpg/12AB/resize/500x500/
    /cachefs/www.winesoft.com/video.mp4/mp4hls/index.m3u8
    
As explained in "File Properties", Linux asks for the properties of each directory in a path. STON is unable to tell whether additional directories are added to the end of a path, so unprocessed files will end up in the service.

To resolve this issue, STON uses the ``Separator (default: ^)`` property to differentiate. ::

    /cachefs/image.winesoft.com/img.jpg^12AB^resize^500x500^
    /cachefs/www.winesoft.com/video.mp4^mp4hls^index.m3u8
    
.. figure:: img/conf_fs9.png
   :align: center
      
   MP4HLS access.

Within STON, the ``Separator`` s are switched to slashes (/) in order to use the HTTP call standard. Using this can eliminate unnecessary File I/O access, as shown below.

.. figure:: img/conf_fs7.png
   :align: center
      
   Extremely optimized access.



Wowza Integration
====================================

Wowza can be integrated using the file system. All you need to do is configure the path that STON is mounted on as the file path for Wowza.

1. [STON - Global settings] Turn on the file system configuration
    Set ``<FileSystem>`` to ``ON`` in the global settings (server.xml). (In this example, the mount path will be set to "/cachefs".)  ::
  
       # server.xml - <Server><Cache>
         
       <FileSystem Mount="/cachefs" DotDir="OFF" Separator="^">ON</FileSystem>     
     
    Alternatively, in WM, go to Global Settings -> File System and set the file system to "On".
  
    .. figure:: img/faq_wowza1.png
       :align: center

       STON must be restarted after configuration for mounting to be successful.
     
#. [STON - Virtual host] Configure file system access permissions and response codes
    File system access for virtual hosts should be set to Active. The recognition of files/directories based on origin server response codes should also be set. The following uses the virtual host default settings (server.xml) as an example, but this can also be configured individually for each virtual host (vhosts.xml). ::
  
       # server.xml - <Server><VHostDefault><Options>
       # vhosts.xml - <Vhosts><Vhost><Options>
     
       <FileSystem Status="Active" DotDir="OFF">
          <FileStatus>200</FileStatus>
          <DirStatus>301, 302, 400, 401, 403</DirStatus>
       </FileSystem>
     
    Alternatively, in WM, go to Virtual Host -> Configuration (File System) and set the virtual host to "accessible".
  
    .. figure:: img/faq_wowza2.png
       :align: center

       Response codes can be configured here.
     
     
#. [Wowza] Storage path configuration
    In the Wowza installed path, the /Conf/Application.xml file should be edited to refer to the path that STON is mounted on, as shown below.  ::

       <Streams>
         <StreamType>default</StreamType>
         <StorageDir>/cachefs/example.com</StorageDir>
         <KeyDir>${com.wowza.wms.context.VHostConfigHome}/keys</KeyDir>
       </Streams>
     
#. [Wowza] VOD path configuration
    In the Wowza installed path, the /Conf/vod/Application.xml file should be edited to refer to the path that STON is mounted on, as shown below.  ::
  
       <Streams>
         <StreamType>default</StreamType>
         <StorageDir>/cachefs/example.com</StorageDir>
         <KeyDir>${com.wowza.wms.context.VHostConfigHome}/keys</KeyDir>
       </Streams>
     
#. Player test
    Using the Wowza test player, videos not saved in local storage (that STON must cache) can be played with RTMP.
  
    .. figure:: img/faq_wowza3.png
       :align: center

       The test needs a good video clip to play.
