API endpoints
=============

This is a lightweight web service with a REST interface, which provides an easy way to access the bio.tools database. 
An API (Application programming interface) is a protocol intended to be used as an interface by software components to communicate with each other. 

If you find a bug, have any questions or suggestions, please `get in touch with us <mailto:registry-support@elixir-dk.org>`_.

List resources
------------------
List and search through all the available resources. Can sort, filter, and search the results.

*HTTP GET*

.. code-block:: text

    https://bio.tools/api/tool/
    https://bio.tools/api/t/

Endpoint Parameters
"""""""""""""""""""
===========    ========  =======================================  ==========  ============================================
Parameter      Required  Type                                     Default     Description        
===========    ========  =======================================  ==========  ============================================
page           No        Integer                                  1           Result page number 
format         No        String(json, xml, api)                   json        Response media type
q              No        String                                               Query term, used for searching, 
                                                                              matches all attributes
sort           No        String(lastUpdate,                       lastUpdate  Sorts the results by choosen value
                         additionDate, name, affiliation, score)              (score only available when there is a query)
ord            No        String(desc, asc)                        desc        Orders the results by either 
                                                                              Ascending or Descending order
<attribute>    No        String                                               Filter by <attribute>. 
                                                                              List of supported attributes below.
===========    ========  =======================================  ==========  ============================================



Filtering
"""""""""
To filter the results by attribute name, the attribute name has to be added as a parameter to the URL, with the value being the desired search term, e.g. ``?name=signalp``

.. _Attributes:

Attributes
~~~~~~~~~~~~~~~~

These are attributes supported by bio.tools

.. code-block:: js

  id, name, topic, function, operation, input, 
  inputDataFormat, inputDataType, output, outputDataFormat, 
  outputDataType, homepage, description, version, 
  accessibility, toolType, collection, contact, 
  elixirInfo, maturity, operatingSystem, language, 
  cost, license, documentation, link, download, publication, 
  credit, owner


Example
"""""""""""""""""""

.. code-block:: bash

   curl -X GET "https://bio.tools/api/tool/?page=1&format=json&name=signalp&sort=name&ord=asc&q=protein-signal-peptide-detection"

Response data
"""""""""""""""""""
================== ========================================================================== =========================
Key Name           Description                                                                Example
================== ========================================================================== =========================
count              The total resource count results for your query                            2313
previous           Link to the previous page                                                  ?page=4
next               Link to the next page                                                      ?page=6
list               An array with multiple resources                                           ARRAY
                   and their relative information 
================== ========================================================================== =========================


Resource detail
------------------
Obtain information about a single resource.

*HTTP GET*

.. code-block:: text

    https://bio.tools/api/tool/:id/
    https://bio.tools/api/t/:id/

Endpoint Parameters
"""""""""""""""""""
=========  ========  ======================  =======  ===================
Parameter  Required  Type                    Default  Description        
=========  ========  ======================  =======  ===================
id         Yes       String                           Resource unique ID 
format     No        String(json, xml, api)  json     Response media type
=========  ========  ======================  =======  ===================


Example
"""""""""""""""""""

.. code-block:: bash

   curl -X GET "https://bio.tools/api/tool/signalp/?format=json"


List resource versions
----------------------
Obtain information about avaliable versions of a single resource.

*HTTP GET*

.. code-block:: text

    https://bio.tools/api/tool/:id/version/
    https://bio.tools/api/t/:id/version/

Endpoint Parameters
"""""""""""""""""""
=========  ========  ======================  =======  ===================
Parameter  Required  Type                    Default  Description        
=========  ========  ======================  =======  ===================
id         Yes       String                           Resource unique ID 
format     No        String(json, xml, api)  json     Response media type
=========  ========  ======================  =======  ===================

Example
"""""""""""""""""""

.. code-block:: bash

   curl -X GET "https://bio.tools/api/t/signalp/version/"

.. _Resource_version_detail:

Resource version detail
-----------------------
Obtain information about a specified version of a single resource.

*HTTP GET*

.. code-block:: text

    https://bio.tools/api/tool/:id/version/:version_id
    https://bio.tools/api/t/:id/version/:version_id

Endpoint Parameters
"""""""""""""""""""
==========  ========  ======================  =======  ==========================
Parameter   Required  Type                    Default  Description        
==========  ========  ======================  =======  ==========================
id          Yes       String                           Resource unique ID 
format      No        String(json, xml, api)  json     Response media type
version_id  Yes       String                           Resource version unique ID
==========  ========  ======================  =======  ==========================

Example
"""""""""""""""""""

.. code-block:: bash

   curl -X GET "https://bio.tools/api/tool/signalp/version/4.1?format=json"


Register a resource
-------------------

.. note:: This method requires the user to be authenticated. Learn how to :ref:`Token`.

*HTTP POST*

.. code-block:: text

    https://bio.tools/api/tool/
    https://bio.tools/api/t/

Endpoint Parameters
"""""""""""""""""""
=========  ========  ======== ====================================================================================================================================
Parameter  Required  Type     Description        
=========  ========  ======== ====================================================================================================================================
data       Yes       Resource Resource you wish to register.
                              See an `example resource <https://bio.tools/api/tool/SignalP?format=json>`_.
=========  ========  ======== ====================================================================================================================================

.. note:: It is possible to specify editing permissions for resources. Learn how to manage :ref:`Editing_permissions`.

.. note:: It is possible to create multiple versions of the same resource. Learn how to use :ref:`Resource_versioning`.

Headers
""""""""""
=============  ========  =========================================  ==============================================================================================
Parameter      Required  Allowed values                             Description        
=============  ========  =========================================  ==============================================================================================
Content-Type   Yes       String(application/json,                   Resource media type
                         application/xml)   
Authorization  Yes       String('Token <authorization token>')      Authorization header.
                                                                    Learn how to :ref:`Token`.
=============  ========  =========================================  ==============================================================================================

Example
"""""""""""""""""""

.. code-block:: bash

   curl -X POST -H "Content-Type: application/json" \
   -H "Authorization: Token 028595d682541e7e1a5dcf2306eccb720dadafd7" \
   -d '<resource>' "https://bio.tools/api/tool/"


Validate registering a resource
-------------------------------

Test registering a resource without it actually being saved into the database.

.. note:: This method requires the user to be authenticated. Learn how to :ref:`Token`.

*HTTP POST*

.. code-block:: text

    https://bio.tools/api/tool/validate/
    https://bio.tools/api/t/validate/

Endpoint Parameters
"""""""""""""""""""
=========  ========  ======== ====================================================================================================================================
Parameter  Required  Type     Description        
=========  ========  ======== ====================================================================================================================================
data       Yes       Resource Resource you wish to validate.
                              See an `example resource <https://bio.tools/api/tool/SignalP?format=json>`_.
=========  ========  ======== ====================================================================================================================================


Headers
""""""""""
=============  ========  =========================================  ==============================================================================================
Parameter      Required  Allowed values                             Description        
=============  ========  =========================================  ==============================================================================================
Content-Type   Yes       String(application/json,                   Resource media type
                         application/xml)   
Authorization  Yes       String('Token <authorization token>')      Authorization header.
                                                                    Learn how to :ref:`Token`.
=============  ========  =========================================  ==============================================================================================

Example
"""""""""""""""""""

.. code-block:: bash

   curl -X POST -H "Content-Type: application/json" \
   -H "Authorization: Token 028595d682541e7e1a5dcf2306eccb720dadafd7" \
   -d '<resource>' "https://bio.tools/api/tool/validate/"


Update a resource
------------------
Update a resource description.

.. note:: This method requires the user to be authenticated. Learn how to :ref:`Token`.

*HTTP PUT*

.. code-block:: text

    https://bio.tools/api/tool/:id/
    https://bio.tools/api/t/:id/

Endpoint Parameters
"""""""""""""""""""
=========  ========  ======== ====================================================================================================================================
Parameter  Required  Type     Description        
=========  ========  ======== ====================================================================================================================================
id         Yes       String   Resource unique ID 
data       Yes       Resource Description with which you wish to update the resource
                              See an `example resource <https://bio.tools/api/tool/SignalP?format=json>`_.
=========  ========  ======== ====================================================================================================================================

.. note:: It is possible to specify editing permissions for resources. Learn how to manage :ref:`Editing_permissions`.

.. note:: It is possible to create multiple versions of the same resource. Learn how to use :ref:`Resource_versioning`.

Headers
""""""""""
=============  ========  =========================================  ==============================================================================================
Parameter      Required  Allowed values                             Description        
=============  ========  =========================================  ==============================================================================================
Content-Type   Yes       String(application/json,                   Resource media type
                         application/xml)   
Authorization  Yes       String('Token <authorization token>')      Authorization header.
                                                                    Learn how to :ref:`Token`.
=============  ========  =========================================  ==============================================================================================

Example
"""""""""""""""""""

.. code-block:: bash

   curl -X PUT -H "Content-Type: application/json" \
   -H "Authorization: Token 028595d682541e7e1a5dcf2306eccb720dadafd7" \
   -d '<resource>' "https://bio.tools/api/tool/SignalP"



Validate updating a resource
-----------------------------
Test updating a resource without it actually being saved into the database.

.. note:: This method requires the user to be authenticated. Learn how to :ref:`Token`.

*HTTP PUT*

.. code-block:: text

    https://bio.tools/api/tool/:id/validate/
    https://bio.tools/api/t/:id/validate/

Endpoint Parameters
"""""""""""""""""""
=========  ========  ======== ====================================================================================================================================
Parameter  Required  Type     Description        
=========  ========  ======== ====================================================================================================================================
id         Yes       String   Resource unique ID 
data       Yes       Resource Description with which you wish to update the resource for validation
                              See an `example resource <https://bio.tools/api/tool/SignalP?format=json>`_.
=========  ========  ======== ====================================================================================================================================

Headers
""""""""""
=============  ========  =========================================  ==============================================================================================
Parameter      Required  Allowed values                             Description        
=============  ========  =========================================  ==============================================================================================
Content-Type   Yes       String(application/json,                   Resource media type
                         application/xml)   
Authorization  Yes       String('Token <authorization token>')      Authorization header.
                                                                    Learn how to :ref:`Token`.
=============  ========  =========================================  ==============================================================================================

Example
"""""""""""""""""""

.. code-block:: bash

   curl -X PUT -H "Content-Type: application/json" \
   -H "Authorization: Token 028595d682541e7e1a5dcf2306eccb720dadafd7" \
   -d '<resource>' "https://bio.tools/api/tool/SignalP/validate/"

.. _Resource_versioning:

Resource versioning
-------------------
All resources can have a specified version assigned to them. This allows for example new versions of resources to be registered while keeping an older version of the resource intact. In order to create a new version of a given resource, the following parameters need to be added to the resource data: 

===========  =================  =======================================================================
Parameter    Type               Description        
===========  =================  =======================================================================
versionId    String             ID of a new resource version to be created. Once created, the version id becomes permanent and is used to uniquely identify a specific version. See :ref:`Resource_version_detail` for more information.
latest       1 or 0             Specify if the created resource version is the latest. All previous resources marked as 'latest' will no longer be considered that after a new version gets marked as 'latest'.  
===========  =================  =======================================================================

.. _Editing_permissions:

Editing permissions
-------------------
It is possible to manage editing permissions for the registered resources. There are currently three types of editing permissions supported by the system:

.. _Private:

Private
"""""""
A private resource can only be edited by the creator of the resource. This is the default option. In order to set this kind of permission, add the following info into the resource data:

.. code-block:: text

    "editPermission": {
        "type": "private"
    }

.. _Public:

Public
""""""
Public resource can be modified by any user registered in the system. In order to set this kind of permission, add the following info into the resource data:

.. code-block:: text

    "editPermission": {
        "type": "public"
    }

.. _Group:

Group
"""""
Specify a list of users in the system that can edit the resource. In order to set this kind of permission, add the following info into the resource data:

.. code-block:: text

    "editPermission": {
        "type": "private",
        "authors": [
            "registered_user_1", "registered_user_2"
        ]
    }


Delete a resource
------------------

Removes a resource from the registry.

.. note:: This method requires the user to be authenticated. Learn how to :ref:`Token`.

*HTTP DELETE*

.. code-block:: text

    https://bio.tools/api/tool/:id/
    https://bio.tools/api/t/:id/

Endpoint Parameters
"""""""""""""""""""
=========  ========  ======== ====================================================================================================================================
Parameter  Required  Type     Description        
=========  ========  ======== ====================================================================================================================================
id         Yes       String   Resource unique ID
=========  ========  ======== ====================================================================================================================================


Headers
""""""""""
=============  ========  =========================================  ==============================================================================================
Parameter      Required  Allowed values                             Description        
=============  ========  =========================================  ==============================================================================================
Authorization  Yes       String('Token <authorization token>')      Authorization header.
                                                                    Learn how to :ref:`Token`.
=============  ========  =========================================  ==============================================================================================

Example
"""""""""""""""""""

.. code-block:: bash

   curl -X DELETE \
   -H "Authorization: Token 028595d682541e7e1a5dcf2306eccb720dadafd7" \
   "https://bio.tools/api/tool/SignalP"


List used terms
------------------
Obtain a list of terms registered with tools for some attributes, e.g. a list of names of all tools.

*HTTP GET*

.. code-block:: text

    https://bio.tools/api/used-terms/:attribute

Endpoint Parameters
"""""""""""""""""""
=========  ========  ==============================================================  =======  ==========================================================
Parameter  Required  Type                                                            Default  Description        
=========  ========  ==============================================================  =======  ==========================================================
attribute  Yes       String(name, topic, functionName, input, output, credits, all)           Attribute for which a list of used terms will be returned
format     No        String(json, xml, api)                                          json     Response media type
=========  ========  ==============================================================  =======  ==========================================================


Example
"""""""""""""""""""

.. code-block:: bash

   curl -X GET "https://bio.tools/api/used-terms/name/?format=json"

Response data
"""""""""""""""""""
================== ====================
Key Name           Description         
================== ====================
data               A list of used terms
================== ====================


Create a user account
---------------------

Creates a user account and emails a verification email.

*HTTP POST*

.. code-block:: text

    https://bio.tools/api/rest-auth/registration/

POST data
"""""""""""""""""""
==================  ========  ======  ========================================================================== =========================
Key Name            Required  Type    Description                                                                Example
==================  ========  ======  ========================================================================== =========================
username            Yes       String  Account username                                                           username
password1           Yes       String  Password                                                                   password
password2           Yes       String  Repeated password                                                          password
email               Yes       String  Account email. The verification email will be sent to this address         example@example.org
==================  ========  ======  ========================================================================== =========================

Headers
""""""""""
=============  ========  =========================================  ==============================================================================================
Parameter      Required  Allowed values                             Description        
=============  ========  =========================================  ==============================================================================================
Content-Type   Yes       String(application/json,                   POST data media type
                         application/xml)   
=============  ========  =========================================  ==============================================================================================

Example
"""""""""""""""""""

.. code-block:: bash

   curl -X POST -H "Content-Type: application/json" \
   -d '{"username":"username", "password1":"password", \
   "password2":"password", "email":"example@example.org"}' \
   "https://bio.tools/api/rest-auth/registration/"



Verify a user account
---------------------

Verifies a user account based on the emailed verification key.

*HTTP POST*

.. code-block:: text

    https://bio.tools/api/rest-auth/registration/verify-email/

POST data
"""""""""""""""""""
==================  ========  ======  ========================================================================== ================================================================
Key Name            Required  Type    Description                                                                Example
==================  ========  ======  ========================================================================== ================================================================
key                 Yes       String  Verification key from account creation email                               ndwowtbpmlk5zxdxfrwgu2822xynjidhizhwosycve7hro1of156hjwdsf1f6gbn
==================  ========  ======  ========================================================================== ================================================================

Headers
""""""""""
=============  ========  =========================================  ==============================================================================================
Parameter      Required  Allowed values                             Description        
=============  ========  =========================================  ==============================================================================================
Content-Type   Yes       String(application/json,                   POST data media type
                         application/xml)   
=============  ========  =========================================  ==============================================================================================

Example
"""""""""""""""""""

.. code-block:: bash

   curl -X POST -H "Content-Type: application/json" \
   -d '{"key":"ndwowtbpmlk5zxdxfrwgu2822xynjidhizhwosycve7hro1of156hjwdsf1f6gbn"}' \
   "https://bio.tools/api/rest-auth/registration/verify-email/"


.. _Token:

Log in / obtain token
--------------------------------

Logs the user in and returns an authentication token.

*HTTP POST*

.. code-block:: text

    https://bio.tools/api/rest-auth/login/

POST data
"""""""""""""""""""
==================  ========  ======  ========================================================================== =========================
Key Name            Required  Type    Description                                                                Example
==================  ========  ======  ========================================================================== =========================
username            Yes       String  Account username                                                           username
password            Yes       String  Password                                                                   password
==================  ========  ======  ========================================================================== =========================

Headers
""""""""""
=============  ========  =========================================  ==============================================================================================
Parameter      Required  Allowed values                             Description        
=============  ========  =========================================  ==============================================================================================
Content-Type   Yes       String(application/json,                   POST data media type
                         application/xml)   
=============  ========  =========================================  ==============================================================================================

Example
"""""""""""""""""""

.. code-block:: bash

   curl -X POST -H "Content-Type: application/json" \
   -d '{"username":"username","password":"password"}' \
   "https://bio.tools/api/rest-auth/login/"

Response data
"""""""""""""""""""
================== ====================
Key Name           Description         
================== ====================
key                Authentication token
================== ====================

Get user information
--------------------------------

Returns information about the logged in user account, including a list of registered resource (name, id, version, additionDate, lastUpdate)

.. note:: This method requires the user to be authenticated. Learn how to :ref:`Token`.

*HTTP GET*

.. code-block:: text

    https://bio.tools/api/rest-auth/user/

Endpoint Parameters
"""""""""""""""""""
=========  ========  ==============================================================  =======  ==========================================================
Parameter  Required  Type                                                            Default  Description        
=========  ========  ==============================================================  =======  ==========================================================
format     No        String(json, xml, api)                                          json     Response media type
=========  ========  ==============================================================  =======  ==========================================================

Headers
""""""""""
=============  ========  =========================================  ==============================================================================================
Parameter      Required  Allowed values                             Description        
=============  ========  =========================================  ==============================================================================================
Authorization  Yes       String('Token <authorization token>')      Authorization header.
                                                                    Learn how to :ref:`Token`.
=============  ========  =========================================  ==============================================================================================

Example
"""""""""""""""""""

.. code-block:: bash

   curl -X GET \
   -H "Authorization: Token 028595d682541e7e1a5dcf2306eccb720dadafd7" \
   "https://bio.tools/api/rest-auth/user/?format=json"

Response data
"""""""""""""""""""
================== ========================================================
Key Name           Description         
================== ========================================================
username           Account username
email              Account email
resources          List of registered resources 
                   (limited to name, id, version, additionDate, lastUpdate)
================== ========================================================


Log out
------------------

.. note:: This method requires the user to be authenticated. Learn how to :ref:`Token`.

*HTTP POST*

.. code-block:: text

    https://bio.tools/api/rest-auth/logout/

Headers
""""""""""
=============  ========  =========================================  ==============================================================================================
Parameter      Required  Allowed values                             Description        
=============  ========  =========================================  ==============================================================================================
Authorization  Yes       String('Token <authorization token>')      Authorization header.
                                                                    Learn how to :ref:`Token`.
=============  ========  =========================================  ==============================================================================================

Example
"""""""""""""""""""

.. code-block:: bash

  curl -X POST 
  -H "Authorization: Token 028595d682541e7e1a5dcf2306eccb720dadafd7" \
  "https://bio.tools/api/rest-auth/logout/"


Reset user password
--------------------------------

Sends a password reset email.

*HTTP POST*

.. code-block:: text

    https://bio.tools/api/rest-auth/password/reset/

POST data
"""""""""""""""""""
==================  ========  ======  ========================================================================== =========================
Key Name            Required  Type    Description                                                                Example
==================  ========  ======  ========================================================================== =========================
email               Yes       String  Account email                                                              example@example.org
==================  ========  ======  ========================================================================== =========================

Headers
""""""""""
=============  ========  =========================================  ==============================================================================================
Parameter      Required  Allowed values                             Description        
=============  ========  =========================================  ==============================================================================================
Content-Type   Yes       String(application/json,                   POST data media type
                         application/xml)   
=============  ========  =========================================  ==============================================================================================

Example
"""""""""""""""""""

.. code-block:: bash

   curl -X POST -H "Content-Type: application/json" \
   -d '{"email":"example@example.org"}' \
   "https://bio.tools/api/rest-auth/password/reset/"

Confirm password reset
--------------------------------

Confirms a password reset using uid and token from a password reset email.

*HTTP POST*

.. code-block:: text

    https://bio.tools/api/rest-auth/password/reset/confirm/

POST data
"""""""""""""""""""
==================  ========  ======  ========================================================================== =========================
Key Name            Required  Type    Description                                                                Example
==================  ========  ======  ========================================================================== =========================
uid                 Yes       String  UID from password reset email                                              MQ
token               Yes       String  Token from password reset email                                            4ct-67e90a1ab4f22fbb9b9f
password1           Yes       String  New password                                                               new_password
password2           Yes       String  New password repeated                                                      new_password
==================  ========  ======  ========================================================================== =========================

Headers
""""""""""
=============  ========  =========================================  ==============================================================================================
Parameter      Required  Allowed values                             Description        
=============  ========  =========================================  ==============================================================================================
Content-Type   Yes       String(application/json,                   POST data media type
                         application/xml)   
=============  ========  =========================================  ==============================================================================================

Example
"""""""""""""""""""

.. code-block:: bash

   curl -X POST -H "Content-Type: application/json" \
   -d '{"uid":"MQ", "token":"4ct-67e90a1ab4f22fbb9b9f", \
   "password1":"new_password", "password2":"new_password"}' \
   "https://bio.tools/api/rest-auth/password/reset/confirm/"

Stats
-----
Compile stats about a the registry.

*HTTP GET*

.. code-block:: text

    https://bio.tools/api/stats

Example
"""""""""""""""""""

.. code-block:: bash

   curl -X GET "https://bio.tools/api/stats/?format=json"