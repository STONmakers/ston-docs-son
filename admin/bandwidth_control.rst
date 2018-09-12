.. _bandwidth_control:

Chapter 16. Bandwidth
*********************

This chapter will explain various ways to set bandwidth limits for each virtual host. In the past, the objective was generally to prevent bandwidth from exceeding a fixed limit. However, the idea has changed into regulating bandwidth to be effective. It is now possible to analyze content in real time to optimize the use of bandwidth.

.. toctree::
   :maxdepth: 2

Virtual Host Bandwidth Limits
====================================

Limits the maximum bandwidth of a virtual host. This is a concrete method that has the highest priority. ::

   # server.xml - <Server><VHostDefault><Options>
   # vhosts.xml - <Vhosts><Vhost><Options>

   <TrafficCap Session="0">0</TrafficCap>   
    
-  ``<TrafficCap> (default: 0 Mbps)``
   Configures the maximum bandwidth of a virtual host in Mbps. A value of 0 will not limit the bandwidth.
   The``Session (default: 0 Kbps)`` property configures the maximum bandwidth that can be transferred in each client session.

For example, setting ``<TrafficCap>`` to 50 (Mbps) will have the same effect has installing 50 Mbps NIC. The total bandwidth of all clients that connect to the virtual host will be unable to exceed 50 Mbps.

``Session`` behaves as follows.

1. Even if ``Session`` is configured, the total client bandwidth cannot exceed ``<TrafficCap>``.
2. Even if `Bandwidth Throttling`_ is configured, the maximum speed of each client session cannot exceed the ``Session`` value.


.. _bandwidth-control-bt:

Bandwidth Throttling
====================================

Bandwidth Throttling (BT) is a function that can dynamically regulate the client transfer bandwidth for each session. Media files are generally comprised of Video (V) and Audio (A) headers, as seen below.

.. figure:: img/conf_media_av.png
   :align: center
      
   BT is not concerned with headers.

The headers get bigger when the play-time is longer or the key frame cycle is shorter. Therefore, if the media file can be recognized, the headers are transferred without a bandwidth limit for smoother playback. As seen in the following image, it is after the headers are fully transferred that BT starts.

.. figure:: img/conf_bandwidththrottling2.png
   :align: center
      
   Operation scenario
   
::

   # server.xml - <Server><VHostDefault><Options>
   # vhosts.xml - <Vhosts><Vhost><Options>

   <BandwidthThrottling>
      <Settings>
         <Bandwidth Unit="kbps">1000</Bandwidth>
         <Ratio>100</Ratio>
         <Boost>5</Boost>      
      </Settings>
      <Throttling>OFF</Throttling> 
   </BandwidthThrottling>   
    
``<BandwidthThrottling>`` Default operation is configured in the subtags.

-  ``<Settings>`` Configures default operation.
   
   -  ``<Bandwidth> (default: 1000 Kbps)``
      Configures client transfer bandwidth. The ``Unit`` property is used to configure the default unit (``kbps, ``mbps``, ``bytes``, ``kb``, ``mb``).
   
   -  ``<Ratio> (default: 100%)``    
      Configures bandwidth by applying the ratio to the ``<Bandwidth>`` setting.
   
   -  ``<Boost> (default: 5 s)``
      Transfers data to clients with unlimited speed for the set amount of time. The amount of data can be calculated with the equation ``<Boost>`` x ``<Bandwidth>`` x ``<Ratio>``.
         
-  ``<Throttling>``

   -  ``OFF (default)`` Does not apply BT.
   -  ``ON`` Applies BT if the conditions are met.


Bandwidth Throttling Condition List
-----------------------------------

Configures the BT condition list. Conditions must be met for BT to be applied. The list is checked in order to see if any conditions are met. This transfer policy is saved in the file /svc/{virtual host name}/throttling.txt. ::

   # /svc/www.example.com/throttling.txt
   # Commas (,) are delimiters, and the order is {condition},{bandwidth},{ratio},{boost}.
   # All fields except for {condition} can be omitted.
   # Omitted fields use the default values set in <Settings>.
   # All condition formats are identical to settings in acl.txt.
   # The unit for {bandwidth} is the same as the Unit property of <Bandwidth> under <Settings>.
   
   # Transfers data with unlimited speed for 3 seconds, and then limits to to 3 Mbps (3000 Kbps = 2000 Kbps x 150%).
   $IP[192.168.1.1], 2000, 150, 3
   
   # Only defines bandwidth. Transfers data with unlimited speed for 5 seconds (default), and then limits to 800 Kbps.
   !HEADER[referer], 800
   
   # Only defines boost. Transfers data with unlimited speed for 10 seconds, and then limits to 1000 Kbps.
   HEADER[cookie], , , 10
   
   # Does not apply BT for files with the m4a extension.
   $URL[*.m4a], no

By analyzing media files (MP4, M4A, MP3), bandwidth can be obtained from the encoding rate. The file extension must be one of .mp4, .m4a, or .mp3. In order to dynamically extract the bandwidth, you can add an **x** to the bandwidth value, as shown below. ::

   # Finds the bandwidth for /vod/*.mp4 files. If the bandwidth cannot be found, 1000 is used instead.
   $URL[/vod/*.mp4], 1000x, 120, 5

   # Finds the bandwidth if the user-agent header is missing. If the bandwidth cannot be found, 500 is used instead.
   !HEADER[user-agent], 500x

   # Finds the bandwidth for /low_quality/* files. If the bandwidth cannot be found, the default value is used instead.
   $URL[/low_quality/*], x, 200


QueryString Priority Condition
------------------------------

Uses a predetermined QueryString to dynamically configure ``<Bandwidth>``, ``<Ratio>``, and ``<Boost>``. This configuration takes priority over BT conditions.

::

   # server.xml - <Server><VHostDefault><Options>
   # vhosts.xml - <Vhosts><Vhost><Options>
   
   <BandwidthThrottling>
      <Settings>
         <Bandwidth Param="mybandwidth" Unit="mbps">2</Bandwidth>
         <Ratio Param="myratio">100</Ratio>
         <Boost Param="myboost">3</Boost> 
      </Settings>
      <Throttling QueryString="ON">ON</Throttling>
   </BandwidthThrottling>   
    
-  ``Param`` in ``<Bandwidth>``, ``<Ratio>``, ``<Boost>``

    Configures the QueryString key with a different meaning for each tag.
   
-  ``QueryString`` in ``<Throttling>``

   - ``OFF (default)`` Does not redefine conditions as QueryStrings.
   
   - ``ON`` Redefines conditions as QueryStrings.
   
The above configuration allows BT to be dynamically configured based on the URLs requested by clients, as seen below. ::

   # Transfers data with unlimited speed for 10 seconds, and then limits to 1.3 Mbps (1 Mbps x 130%).
   http://www.winesoft.co.kr/video/sample.wmv?myboost=10&mybandwidth=1&myratio=130
    
Not all parameters need to be specified. ::

    http://www.winesoft.co.kr/video/sample.wmv?myratio=150
    
If some conditions are omitted as in the above example, the remaining fields (in this example, bandwidth and boost) are chosen using a condition list. If there is no condition that matches, then the default values in ``<Settings>`` are used. Even if a QueryString is specified, if the condition list is set to not apply (no), then BT will not be applied.

There is potential for confusion with :ref:`caching-policy-applyquerystring` when using QueryStrings. If :ref:`caching-policy-applyquerystring` is set to ``ON``, the QueryStrings in URLs requested by clients will all be recognized except for ``BandwidthParam``, ``BoostParam``, and ``RatioParam``. ::

   GET /video.mp4?mybandwidth=2000&myratio=130&myboost=10
   GET /video.mp4?tag=3277&myboost=10&date=20130726

For example, the above QueryStrings are only used to decide BT and are removed when creating a Caching-Key or sending a request to the origin server. Therefore, they will be recognized as the following. ::

    GET /video.mp4
    GET /video.mp4?tag=3277&date=20130726
    
    
