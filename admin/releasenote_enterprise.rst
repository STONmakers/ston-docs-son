.. _release_enterprise:

Appendix E: Release Notes  ``[Enterprise]``
***********************

v19.x
====================================

19.11.0 (2019.11.28)
----------------------------
**Feature/Policy Changes**
    - ImageTool (DIMS): Improved parameter exception handling 
    - Header Modification: #PORT added to the reserved words

**Bug Fixes**
    - WM: Clustering error caused by 50+ HTTPS certificate installation
    - Unintended RRD process termination


19.10.1 (2019.10.29)
----------------------------
**Feature/Policy Changes**
    - Optimized for large-sized file delivery in LTE networks

19.10.0 (2019.10.10)
----------------------------
**Bug Fixes**
    - HTTPS: Infrequent POST bypass (pass-through) malfunction
    - Unintended termination if origin servers are excluded AND recovered in one second
    
    
19.09.0 (2019.9.26)
----------------------------
**Feature/Policy Changes**
    - Origin Connection: Supports AWS S3 Authentication (AWS Signature Version 4)
    - ImageTool (DIMS): Matched Content-Type header values to the converted image formats
    - WM: Apache HTTPD version update (from v2.2.24 to v2.4.41)

19.08.0 (2019.8.14)
----------------------------
**Feature/Policy Updates**
    - HTTPS: ECDSA key file compatibility improvements

**Bug Fixes**
    - Web Management: GeoIP database uploading errors
    - Web Management: Custom TTL value entry errors
    - HTTPS: Unintended termination from certificate swapping (from DSA to RSA)

19.07.0 (2019.7.4)
----------------------------
**Feature/Policy Changes**
    - Supports RRD data storage configuration
    - Custom TTL: Origin response condition added
    - Origin Server Exclusion On/Off added
    - ImageTool(DIMS): Automatic Rotation added

**Bug Fixes**
    - WM: Unintended disk initialization during system configuration 
    - High CPU usage following a hardware info call


19.06.0 (2019.6.4)
----------------------------

**Bug Fixes**

    - ImageTool (DIMS): Unncessary Orientation settings required in ByOriginal 
    - Web Management: Missing ByOriginal settings in duplicated Virtual Hosts


19.05.0 (2019.5.9)
----------------------------

**Feature/Policy Change**

    - ImageTool(DIMS): Image orientation added in ByOriginal configuration

**Bug Fix**

    - Unintended termination from GeoIP2 settings (GeoIP2 does not support database file overwriting)
    
19.04.0 (2019.4.11)
----------------------

**Feature/Policy Updates**
    - Configurable disk cleaning policy 
    - Improved config reloading API response 
    - HTTPS: Improved logging for certificate settings

**Bug Fixes**
    - Web Management: System setting malfunction in English language 
    - Web Management: Memory graph error in English language (showing a negative value)
    - Web Management: Disk setting graphing error in English language 
    - HTTPS: Unintended termination from unsupported certificate key files
    
19.03.0 (2019.3.13)
----------------------

**Feature/Policy Update**
    - HTTPS: Supports TLS v1.3

**Bug Fixes**
    - WM: Missing empty value input for header modification  
    - SNI-based HTTPS: Missing different protocol config for each certificate

19.02.0 (2019.2.11)
---------------------

**Feature/Policy Updates**
    - ImageTool(DIMS): quality option added for converting formats
    - ImageTool(DIMS): max quality option
    - Logging: image conversion included
    - Origin request header modification: client request header value to add

**Bug Fixes**
    - Unintended termination from >50 origin servers
    - Web Management: SNI configuration reset from HTTPS certificate clustering

19.01.0 (2019.1.16)
---------------------
**Feature/Policy Update**
    - Supports GeoIP2

**Bug Fix**
    - ImageTool(DIMS): image quality degradation from WebP reformatting

    
v18.x
====================================

18.9.2 (2018.9.12)
----------------------------

**Bug Fix**

- HTTPS: Infrequent disconnection



18.9.1 (2018.9.7)
----------------------------

**Bug Fix**

- Infrequent extra transaction time in some system environment 


18.9.0 (2018.9.3)
----------------------------

**Feature/Policy Updates**

- Improved HTTPS performance
- ECDSA certificate support for HTTPS
- Expires headers: Max-Age option from the origin 
- Origin HTTPS transaction support


18.08.0 (2018.8.8)
----------------------------

**Feature/Policy Updates**

- Client Request/Response Header Modification: request header values added to the responses
- DIMS: Content-Type header values matching to the converted image formats


18.07.0 (2018.7.10)
----------------------------

**Feature/Policy Updates**

- DIMS: Supports WebP
- Supports Via headers for passed-through (bypassed) responses


**Bug Fixes**

- DIMS: Optimize malfunction from ByOriginal config
- WM: some missing values from cluster duplication
- Unintended termination while file indexing and deleting



18.05.1 (2018.5.29)
----------------------------

**Feature/Policy Updates**

- HLS: Improved key frame interval compatibility

.. warning::

   NO BACKWARD COMPATIBILITY: MPEG2-TS


**Bug Fix**

- Potential 304 response from the very first caching in case of “orlater” config for If-Modified-Since header processing


18.05.0 (2018.5.15)
----------------------------

- Supports HTTP If-Range headers in client/origin requests
- Supports conditional configuration on If-Modified-Since value
- Supports HTTP-PUT requests pass-through (bypass) to origin servers



18.04.0 (2018.4.26)
----------------------------

**Feature/Policy Update**

- DIMS: Supports annotation


.. note::

   Change of versioning from STON Edge Server v2.5.13

   -  ``CDN`` - follows the same versioning such as v2.5.14
   -  ``Enterprise`` - follows the calendar versioning such as v.18.04.0
