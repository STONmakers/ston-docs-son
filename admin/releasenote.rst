.. _release:

Appendix D: Release Notes ``[CDN]``
***************************

v2.5.x
====================================

2.5.11. (JAN 25, 2018)
----------------------

**Feature/Policy Updates**
    - SSL: Supports SHA384
    - SSL: The ROBOT attack mitigation added
    - Client Request/Response Header Modification: Request Method condition added
    - VirtualHost Access Control: POST method control added
    - Web Management: HTTPS download added in the Caching Status


2.5.10. (DEC 18, 2017)
----------------------
**Feature/Policy Updates**
    - DIMS: 'Round' command added
    - HTTP Header Modification: a new reserved word (PROTOCOL) 
    - STONX empty directory deletion policy added (configurable)
    - Individually upploadable XML configuration 

**Bug Fix**
    - API call response fixes in JSON 

2.5.9. (NOV 30, 2017)
----------------------

**Bug Fixes**
    - DIMS: resizing not working when the width value is missing
    - HLS: Fixed infrequent playback errors at some iOS devices
    
   
2.5.8. (NOV 9, 2017)
----------------------

**Feature/Policy Updates**
    - DIMS: ResizeCrop added
    - DIMS: Animated GIF frame control added
    - PROTOCOL condition for ACLs
    - Max DNS TTLs

**Bug Fixes**
    - Statistical data error generated from too many resolved IP accumulation
    - Web Management: ACL config user interface broken
    - Web Management: ModifyHeader config initialization
    
    
2.5.7. (OCT 13, 2017)
----------------------

**Bug Fixes**
    - Transfer-Encoding content memory optimization failure [2.5.5~2.5.6]
    - Abnormal termination from refreshing MP4HLS content [2.4.6~2.5.6]

2.5.6. (SEP 28, 2017)
----------------------

**Feature/Policy Updates**
    - Supports HTTP OPTIONS method.

**Bug Fix**
    - Fixed applying SNMP settings when missing the backups
    - Fixed initialized TTLs if compression is turned on

2.5.5. (AUG 30, 2017)
----------------------
    - Supports content DRM.
    - Assigns a response code if the content cannot be updated.

**Feature/Policy Updates**
    - Enhanced stability for Memory-Only mode
    - Cluster information query API added
    - Apache security advisories applied

**Bug Fix**
    - Unintended DIMS request bypass (pass-through) caused from the compressed content I/O failure

2.5.4. (AUG 10, 2017)
----------------------

**Bug Fix**
    - A decrease in 'Byte Hit Ratio' 
    
2.5.3. (JUL 10, 2017)
----------------------

**Bug Fix**
    - Infrequent SSL errors (2.5.0 ~ v2.5.2)

2.5.2 (JUL 6, 2017)
---------------------------

**Feature/Policy Updates**
    - DIMS: ‘Trim’ and ‘Crop Center’ added
    - DIMS: false geometric data processing updated

**Bug Fixes**
    - Unintended disk cleanup in Memory-Only Mode  
    - Infrequent switching error in Virtualhost Link   
    
2.5.1 (JUN 8, 2017)
-----------------------------

**Feature/Policy Update**
    - POST Caching: pass through clients' Content-Type to origin servers

**Bug Fixes**
    - Fixed cached file initialization when PartSize is activated (version 2.5.0)
    - Fixed Write statistics missing when PartSize is activated (version 2.5.0)
    - Fixed double quotation mark input error from Web Management when HTTP header modification is activated

2.5.0 (MAY 25, 2017)
---------------------------

**Feature/Policy Updates**
    - Supports HTTPS-SNI
    - Supports 'Memory-Only' mode
    
v2.4.x
====================================

2.4.11 (MAY 18, 2017)
---------------------------

**Bug Fix**
    - Pseudo-streaming error from loading MP4 files over 4GB AND the header at the end 
   
2.4.10 (MAY 11, 2017)
---------------------------

**Bug Fix**
    - MP4HLS: Infrequent audio desync from serving big header MP4s in HLS
    
2.4.9 (APR 24, 2017)
---------------------------

**Feature/Policy Update**
    - MP4HLS: Improved compatibility for SPS and PPS data in all keyframes

**Bug Fixes**
    - Missing an hour part of 5-min statistics data if the system time is changed
    - Infrequent termination with the health-checker activated
    - Infrequent termination with excluded disk(s) AND bypass sessions activated 
    - Compressed logs missing (partly)
    - Infrequent initial disconnection serving large media file headers with the part-size feature
    
2.4.8 (JUL 6, 2017)
---------------------------

**Bug Fix**
    - Fixed infrequent termination from over about 2 billion files generated in a single virtual host
    

2.4.7 (APR 11, 2017)
---------------------------
**Bug Fix**
    - High CPU usage and system load from SSL transactions (2.4.5 and 2.4.6 only)


2.4.6 (MAR 19, 2017)
---------------------------
**Feature/Policy Updates**
   - MP3HLS: supports mp3 segmentation
   - MP4HLS: PCR formulae changed for player compatibility 

.. warning::

    NO BACKWARD COMPATIBILITY.  If you have older versions of STON Edge Server, upgrade to 2.4.6 or higher. DO NOT mix older versions with newer ones on the same network. 

**Bug Fix**
    - Infrequent termination while swapping disks for 404 responses

2.4.5 (FEB 16, 2017)
---------------------------

**Bug Fixes**
    - Fixed the termination from 'Transfer-Encoding: chunked' origin response for DIMS
    - Fixed the connection closing with ECDHE SSL CipherSuite at Chrome browsers
    - Fixed the infrequent termination from logging adjustment


2.4.4 (FEB 8, 2017)
---------------------------

**Bug Fix**
    - Fixed infrequent DIMS request bypass (pass-through) caused from origin server errors

2.4.3 (JAN 20, 2017)
---------------------------

**Bug Fix**

  - Fixed infrequent Content-Encoding headers missing from compressed content responses

2.4.2 (JAN 18, 2017)
---------------------------

**Feature/Policy Updates**

  - Vhost-Link feature added

**Bug Fixes**

  - Fixed the unintended termination with negative Content-Length header value from origin servers
  - Fixed the unintended termination from unstable origin server communications for MP3HLS packetizing

2.4.1 (NOV 24, 2016)
----------------------------

**Feature/Policy Updates**

  - Processes origin HTTP responses with missing Reason-Phrase’s 
  - DIMS: supports canvas resizing

**Bug Fixes**

  - Compression integrity improved
  - VLC media player compatibility for M4A HLS playback
  - Abnormal termination from missing DIMS resize dimensions
  
2.4.0 (NOV 07, 2016)
----------------------------

**Feature/Policy Updates**

  - Modifiable HTTP request URLs to origin 
  - Support M4A HLS

**Bug Fix**

  - Enhanced processing for invalid MP4 size headers.
  
v2.3.x
====================================

2.3.9 (OCT 28, 2016)
----------------------------

**Bug Fix**

 - TTL: Content was not updated few seconds in some circumstances


2.3.8 (OCT 13, 2016)
----------------------------

**Bug Fix**

 - Enhanced processing for invalid MP4 size headers


2.3.7 (SEP 26, 2016)
----------------------------

**Feature/Policy Updates**

  - DIMS: allocates system resource for image conversion 
  - Origin Health Checker: also includes stand-by origin servers

**Bug Fix**

  - Compression on/off


2.3.6 (AUG 16, 2016)
----------------------------

**Feature/Policy Updates**

 - Client socket processing policy update
 - DIMS: PNG alpha compositing update for JPG conversion

**Bug Fix**

 - Unintended termination from a Hardpurge API call in DIMS processing


2.3.5 (JUL 1, 2016)
----------------------------

**Feature/Policy Updates**

 - Improved native HLS player compatibility
 - DIMS image cropping in the unfixed aspect ratio

**Bug Fix**

 - Unintended termination upon an origin status reset API call with Origin Health-Checker activated
 
 
2.3.4 (JUN 3, 2016)
----------------------------

**Feature/Policy Updates**

 - Supports large MP4 files with 32-bit mdat atoms (4GB or more)
 - Supports Host header value in unknown access logs
 - WM installation : Apache Manual folder deleted for security
 - WM installation : "winesoft" Apache account as nologin
   
**Bug Fixes**

 - HLS: CPU overload upon some videos
 - Termination upon bypassing HTTP requests
 - Client IP shown as 0.0.0.0 in access logs
 - Configuration backup failure for over 260 virtualhosts generated


2.3.3 (APR 26, 2016)
----------------------------

**Bug Fixes**

   - Unintended 404 responses upon origin host, DIMS and compression configured [2.3.0 ~ 2.3.2]
   - Unintended CPU overload upon SNMP View deletion
   - WM - 0 value input error from SNMP GlobalMIn


2.3.2 (MAR 22, 2016)
----------------------------

**Feature/Policy Update**

   - HLS index file compatibility improved 

**Bug Fixes**

   - Unintended termination from encryption/decryption with a bad SSL handshake
   - Occasional termination from active ACLs


2.3.1 (FEB 23, 2016)
----------------------------

   - Supports MP3 streaming in HLS

**Feature/Policy Updates**

   - Custom access log format 
       | %..y Request HTTP header size
       | %..z Response HTTP header size
   
**Bug Fix**

   - WM : unintended failure in no destination port configured
   

2.3.0 (FEB 3, 2016)
----------------------------

   -  Supports on-the-fly compression.

**Bug Fixes**

   - Expires Header: incorrect max-age value from modification 
   - DIMS Statistics: incorrect average value from faulty denominator


v2.2.x
====================================

2.2.5 (JAN 12, 2016)
----------------------------

**Feature/Policy Updates**

   - HTTP response code update: "451 Unavailable For Legal Reasons" 

**Bug Fix**

   - TLS : unintended termination upon attacking packets
   
   
2.2.4 (DEC 11, 2015)
----------------------------

**Bug Fix**

   - HLS : playback termination upon media segmentation 
   
   
2.2.3 (DEC 4, 2015)
----------------------------

**Bug Fix**

   - Virtualhost generation failure from Web Management in 2.2.2
   

2.2.2 (DEC 3, 2015)
----------------------------
   
   - Modifies HTTP request header to origin

**Feature/Policy Updates**

   - HTTP request/response header modification : 'put' action added, which inserts the header


2.2.1 (NOV 19, 2015)
----------------------------
   
**Bug Fixes**

   - TLS-Handshake: overlapping ChangeCipherSpec return upon separate ChangeCipherSpec and ClientFinished messages
   - :ref:`media-dims` : size ratio malfuction from Animated GIF resizing

2.2.0 (NOV 4, 2015)
----------------------------
   
   - Supports TLS 1.2 (including Forward Secrecy and other security policy updates)
   
**Bug Fixes**

   - Abnormal termination upon no disk information
   - TLS-Handshake version choice
       **Before.**  TLSPlaintext.version
       **After.**  ClientHello.client.version
   

v2.1.x
====================================


2.1.9 (OCT 15, 2015)
----------------------------
   
**Bug Fix**

   - :ref:`media-hls` : Video playback malfunction from 2.1.7

2.1.8 (OCT 14, 2015)
----------------------------
   
**Bug Fix**

   - Abnormal termination upon manager port access from blocked IPs (2.1.6 ~ 7)

2.1.7 (OCT 7, 2015)
----------------------------

   - :ref:`media-multi-trimming` : Stitches multiple segments trimmed from the origin videos. 
   
**Feature/Policy Updates**

   - :ref:`admin-log-access` : Supports TrimCIP option for X-Forwarded-For header
   
**Bug Fixes**

   - :ref:`media-hls` : Video jittering from few profiles
   - :ref:`media-dims` : B 500 Internal Error responses with zero TTLs
   - Unintended space characters in X-Forwarded-For c-ip logging 
   
2.1.6 (SEP 9, 2015)
----------------------------

**Feature/Policy Updates**

   - :ref:`media-dims` : Converts only the first frames for :ref:`media-dims-anigif`
   
**Bug Fixes**

   - :ref:`access-control` : IP access control malfuction
   - :ref:`media-dims` : '+' coordinate malfuction for cropping images

2.1.5 (AUG 18, 2015)
----------------------------

   - Virtualhost :ref:`env-vhost-sub-path` : Supports virtualhost sub-path by accessing paths 
   - :ref:`env-vhost-facadevhost`: Supports separate client traffic statstics and access logs by accessing domains
   
2.1.4 (JUL 31, 2015)
----------------------------

**Feature/Policy Updates**

   - Less CPU usage
   - :ref:`https-multi-nic`: listening on multiple NICs
   - URI policy change for Access Control
       **Before.**  keywords omitted (such as MP4HLS) from URIs
       **After.**  the whole URIs
   
**Bug Fixes**

   - :ref:`media-dims` : encoded strings unrecognized
   - :ref:`api-cmd-hardpurge` : case-sensitive error
   - Configuration History: POST request exception missing 
   
2.1.3 (JUN 25, 2015)
----------------------------

**Feature/Policy Updates**

   -  :ref:`adv_topics_syncstale` : All content control (:ref:`api-cmd-purge` , :ref:`api-cmd-expire` and :ref:`api-cmd-hardpurge`) API calls tracked and logged (synchronization with stale logs and index when restarted)
   -  %u expression added to :ref:`admin-log-access-custom` (full-length URIs from client requests logged)

**Bug Fixes**

   - :ref:`media-dims` : image revalidation failure with no Last-Modified header from origin
   - :ref:`media-trimming` : CPU overload for >4GB trimmed MP4s
   - Via header missing in error page responses

2.1.2 (MAY 29, 2015)
----------------------------

    | Web Management - English support

**Feature/Policy Updates**

   -  Single-core CPU support

**Bug Fix**

   - Customized module malfunction in the :ref:`adv_topics_indexing` mode
   

2.1.1 (MAY 7, 2015)
----------------------------

    | :ref:`media-hls` : Provides bandwidth and resolution information in `StreamAlternates <https://developer.apple.com/library/ios/documentation/NetworkingInternet/Conceptual/StreamingMediaGuide/art/indexing_2x.png>`_

**Bug Fix**

   - Abnormal termination caused by broken header MP4 video analysis
   


2.1.0 (APR 15, 2015)
----------------------------

    | :ref:`adv_topics_indexing` added
    | Animated GIF :ref:`media-dims` supported
    | :ref:`media-dims` statistics supported

**Feature/Policy Updates**

   -  Directory expression removed from :ref:`caching-purge` (purge, expire, hardpurge, expireafter)
        URL by directory expression (example.com/img/) caches the returned file from the origin.
        Directory expression (example.com/img/) merged with pattern (example.com/img/*)
   -  API expressions added
       | /monitoring/average.xml
       | /monitoring/average.json
       | /monitoring/realtime.xml
       | /monitoring/realtime.json
       | /monitoring/fileinfo.json
       | /monitoring/hwinfo.json
       | /monitoring/cpuinfo.json
       | /monitoring/vhostslist.json
       | /monitoring/geoiplist.json
       | /monitoring/ssl.json
       | /monitoring/cacheresource.json
       | /monitoring/origin.json
       | /monitoring/coldfiledist.json
   -  WM - resolv.conf editing removed


v2.0.x
====================================

2.0.7 (JUN 25, 2015)
----------------------------

**Bug Fixes**

   - :ref:`media-dims` : image revalidation failure with no Last-Modified header from origin
   - :ref:`media-trimming` : CPU overload for >4GB trimmed MP4s
   - Via header missing in error page responses


2.0.6 (APR 28, 2015)
----------------------------

**Feature/Policy Updates**

   -  WM - resolv.conf editing removed

**Bug Fix**

   - abnormal termination from MP4 analysis with broken headers
   
   
2.0.5 (APR 1, 2015)
----------------------------

**Feature/Policy Updates**

   - Serves trimmed MP4 by HLS
     The following expressions trim the original media file (/vod.mp4) from 0 to 60 seconds and serve in HLS.
     | /vod.mp4?start=0&end=60/**mp4hls/index.m3u8**
     | /vod.mp4**/mp4hls/index.m3u8**?start=0&end=60
     | /vod.mp4?start=0/**mp4hls/index.m3u8**?end=60
   - HLS index file (.m3u8) update
     **Before.** Version 1
     **After.** Version 3 (changeable back to version 1)

**Bug Fixes**

   - abnormal termination in HLS conversion with HTTP encoded special characters 
   - overloaded CPU for MP4 media with broken headers 
   - audio/video synchronization failure while serving MP4 with uneven audio keyframe in HLS
   - RRD - statistics bug: average response time shown in total
   - WM - forcing formatting new disks removed 


2.0.4 (FEB 27, 2015)
----------------------------

**Feature/Policy Updates**

   -  ``Hash`` algorithm update at :ref:`origin-balancemode`
   
     | **Before.** hash(URL) / servers
     | **After.** `Consistent Hashing <http://en.wikipedia.org/wiki/Consistent_hashing>`_
     |     
   - Client requested URI is usable as a parameter when redirecting by :ref:`access-control-vhost`.
   
**Bug Fix**

   - Disk full due to unremoved caching files
   
   

2.0.3 (FEB 9, 2015)
----------------------------

**Feature/Policy Updates**

   - DIMS internalization and enhancement
   - WM - traffic alert messages added
   
**Bug Fix**

   - WM - Virtual host generation failure


2.0.2 (Jan 28, 2015)
----------------------------

- Able to pass User-Agent header value from clients when requesting to the origin server.

**Bug Fixes**

   - Failed to trim MP4 files with MDAT length 1.
   - WM - failed to show other clustered servers' graph.
   - WM - showing other clustered server's status as the relevant one.



2.0.1 (DEC 30, 2014)
----------------------------

**Bug Fix**

   - No HitRatio graph value


2.0.0 (DEC 17, 2014)
----------------------------

- Disk space optimization: just as downloaded from the origins. (please refer to :ref:`origin_partsize` )
- :ref:`env-cache-resource` added
- TLS 1.1 support
- :ref:`https-aes-ni` support by AES-NI.
- ECDHE CipherSuite support (please refer to :ref:`https-ciphersuite` )
- :ref:`admin-log-dns` added
- Policy Update: Seprate TTLs for each IP if the origin server is configured by domain
- origin :ref:`origin_exclusion_and_recovery` added
- origin :ref:`origin-health-checker` added
- :ref:`adv_topics_sys_free_mem` added
- etc.

  - Supported operating system updaated: CentOS 6.2 or later, Ubuntu 10.01 or later
  - NSCD daemon included in the installation package
  - :ref:`media-dims` included
  - Restart required after :ref:`getting-started-reset`
  - ``<DNSBackup>`` removed
  - ``<MaxFileCount>`` removed
  - ``<Distribution>`` removed, integrated into :ref:`origin-balancemode` 

