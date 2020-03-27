
# <a name="overview_of_changes"></a>Overview of changes between version 10 and version 8.6 of the MobilePay PoS API

Version 10 of the MobilePay PoS API introduces breaking changes from version 8.6 of the API. 
The following list describes the changes in overview:

* Clients need to implement HTTP 1.1 and JSON standards
* Security setup has changed
  * No API key and HMAC validation
  * Use of access tokens
  * Vendor Identification and Version numbering
  * All endpoints requires TLS 1.2
* The Error messages are more informative
* There are changes to the payment flow
  * Inclusion of the Prepare-Ready flow
  * It is only possible to do Reservation-Capture flows
  * API method naming has changed
* There is a new ID structure (see [ID hierarchy](overview_of_changes#id_hierarchy))
* Documentation is live through a developer website (using OpenAPI standards) and GitHub
* Certification can be done through a self-certification tool (see [Self Certification](self_certification))
* Possibility to do age verification and card restrictions for purchases

## <a name="id_hierarchy"></a>ID hierarchy

The following diagram shows the ID-Hierarchy of the Master data in MobilePay PoS.

[![](assets/images/ID_Hierarchy_Changes.png)](assets/images/ID_Hierarchy_Changes.png)
