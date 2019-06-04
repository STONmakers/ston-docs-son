.. _release_enterprise:

Appendix E: Release Notes  ``[Enterprise]``
***********************

v19.x
====================================

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
