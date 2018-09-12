.. _access-control:

Chapter 15. Access Control
**************************

This chapter will explain how to deny access to unwanted clients. Denying access can be done by setting up a blacklist in the Access Control List (ACL), but for the sake of convenience, a whitelist can also be configured.

Access control can be divided into server access control, which occurs during connection, and virtual host access control, which can be configured for each virtual host. Because the access time and standards are different for each level, it is important to choose what is most effective. All access control is logged.

.. toctree::
   :maxdepth: 2


.. _access-control-serviceaccess:

Server Access Control
====================================

When the client connects to the server, the server decides whether to deny access based on the IP address. As this occurs during connection, the process is decisive and swift. This can be configured in global settings (server.xml) and has the highest priority. ::

   # server.xml - <Server><Host>

   <ServiceAccess Default="Allow">
      <Deny>192.168.7.9-255</Deny>
      <Deny>192.168.8.10/255.255.255.0</Deny>
   </ServiceAccess>

-  ``<ServiceAccess>``   
   Configures ACL using the IP address. Supports the four formats of IP, IP Range, Bitmask, and Subnet. The order is important, with the highest configuration taking priority.
   ``Default (default: Allow)`` 
   Configures how to process requests that do not meet any of the conditions. If set to ``Deny``, IP addresses to be allowed should be specified in ``<Allow>`` tags.
   
Denied IP addresses are logged in the :ref:`admin-log-deny`.


.. _access-control-geoip:

GeoIP
====================================

GeoIP can be used to deny access based on geographical location. Binary databases in `GeoIP databases <http://dev.maxmind.com/geoip/legacy/downloadable/>`_ are linked to `GEOIP_MEMORY_CACHE and GEOIP_CHECK_CACHE <http://dev.maxmind.com/geoip/legacy/benchmarks/>`_ to apply changes in real time. ::

   # server.xml - <Server><Host>

   <ServiceAccess GeoIP="/var/ston/geoip/">
      <Deny>AP</Deny>
      <Deny>GIN</Deny>
   </ServiceAccess>
    
The directory for GeoIP databases is configured in the ``GeoIP`` property of ``<ServiceAccess>``. The supported country codes are <http://en.wikipedia.org/wiki/ISO_3166-1_alpha-2>`_ and 
`ISO 3166-1 alpha-3 <http://en.wikipedia.org/wiki/ISO_3166-1_alpha-3>`_.

.. note::

   Because GeoIP has a reserved file name, you must use the local path it is stored in. Also, because service changes go into effect automatically, there is no need to reload the configuration deliberately.
   
   
If GeoIP is configured, the following will check for the files in the given directory. If it is not configured, a 404 NOT FOUND response is returned. ::
   
   http://127.0.0.1:10040/monitoring/geoiplist
   
The results are returned in JSON format. ::

   {
       "version": "2.0.0",
       "method": "geoiplist",
       "status": "OK",
       "result":
       {
           "path" : "/usr/ston/geoip/",
           "files" :
           [
               {
                   "file" : "GeoIP.dat", 
                   "size" : 766255
               },
               {
                   "file" : "GeoLiteCity.dat", 
                   "size" : 12826936
               }
           ]
       }
   }
   

.. _access-control-vhost:

Virtual Host Access Control
====================================

Controls access for each virtual host. The virtual host decides whether to deny access when the client sends an HTTP request. This is because the virtual host cannot be found without an HTTP request. ::

   # server.xml - <Server><VHostDefault><Options>
   # vhosts.xml - <Vhosts><Vhost><Options>
   
   <AccessControl Default="Allow" DenialCode="401">OFF</AccessControl>   
    
-  ``<AccessControl>``
   
   - ``OFF (default)`` The ACL is not activated. All client requests are allowed.
    
   - ``ON`` The ACL is activated. Denied requests are responded to with the code set in the ``DenialCode`` property. If the ``Default (default: Allow)`` property is set to ``Allow``, the ACL is used as a blacklist. If it is set to ``Deny``, the ACL is used as a whitelist.
     
Denied requests are logged in the :ref:`admin-log-access` as TCP_DENY.


.. _access-control-vhost_acl:

Virtual Host ACL
---------------------

Determines whether client HTTP requests are allowed/denied/redirected. You can also configure different response codes for each condition. For redirected requests, the response will be **302 Moved Temporarily**. The ACL is configured in the /svc/{virtual host name}/acl.txt file. ::

   # /svc/www.example.com/acl.txt
   # Commas (,) are delimiters, and the order is {condition},{keyword = allow|deny|redirect}.
   # If set to deny, the response code can be set after the keyword.
   # If not specified, the DenialCode of <AccessControl> is used.
   # If set to redirect, the URL to be redirected to can be set after the keyword (stated as a value of a Location header).
   # Multiple conditions can be combined (AND) with an & sign.
   
   $IP[192.168.1.1], allow
   $IP[192.168.2.1-255]
   $IP[192.168.3.0/24], deny
   $IP[192.168.4.0/255.255.255.0]
   $IP[AP] & !HEADER[referer], allow
   $IP[GIN], redirect, /page/illegal_access.html
   $HEADER[cookie: *ILLEGAL*], deny, 404
   $HEADER[via: Apache]
   $HEADER[x-custom-header]
   $HEADER[referer:], redirect, http://another-site.com
   !HEADER[referer] & !HEADER[user-agent] & !HEADER[host], deny
   $URL[/source/public.zip], allow
   $URL[/source/*]
   /profile.zip, deny, 500
   /secure/*.dat
   
A condition can be formated as an IP, GeoIP, Header, or URL.

-  IP
    Represented as $IP[...], in which an IP, IP Range, Bitmask, or Subnet can be input.

-  GeoIP
    Represented as $IP[...] and can only be used if GeoIP is configured.
     
-  Header
    Represented as $HEADER[Key : Value]. The Value can recognize specific or patterned expressions. If Value is an empty string but the colon is there ($HEADER[Key:]), then it refers to an empty header value. If the Key is specified without the colon ($HEADER[Key]), then the condition will refer to any request that has the corresponding header.
    
-  URL
    Represented as $URL[...] and can be omitted. Specific and patterned expressions can be used.

$ means "if the condition is met, perform the action", while ! means "if the condition is not met, perform the action". The following negative conditions are supported. ::

   # If the country is not KOR, deny.
   !IP[KOR], deny
    
   # If the referer header is missing, deny.
   !HEADER[referer], deny
   
   # If not under the /secure/ path, allow.
   !URL[/secure/*], allow

When redirecting, the URI that the client requests might be needed. This can be obtained with the #URI keyword. ::

   # If the referer header is missing, the URI is appended to example.com and redirected.
   # Client requests begin with /, so there's no need to add a / to the redirect URL.
   !HEADER[referer], redirect, http://example.com#URI
