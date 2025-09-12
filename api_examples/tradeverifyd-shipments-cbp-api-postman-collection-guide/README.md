\1

\1
** This Postman collection provides access to the Tradeverifyd API for managing shipments, issuing verifiable credentials (VCs), and submitting them to CBP. It is designed to support NCAP testing and give Deacero hands-on access to the foundational endpoints.

\1 \2
  - **Method:** API Key

  - **Header:** ocp-apim-subscription-key: {{api-key}}

\1 \2
\1

  - **api-root-v1  
      - **\1:** String  
      - **\1:** https://api.tradeverifyd.com/v1  
\1
  - **api-key  
\1
\1
      - **⚠️ Mark this variable as Secret in Postman. This prevents it from being accidentally exposed during screen shares or exports.  
  - **(Optional) Runtime variables created during workflows:  
      - **current\_shipment\_id  
      - **current\_vc\_id  
\1
\1 \2
\1 \2
\1
\1 {{api-root-v1}}/shipments  
Example body:  
  
{

"shipment\_name": "Deacero Test Shipment 1",

"status": "Created",

"organizational\_units": \["deacero"\]

}

  - 
  - **Get All Shipments  
    \1 {{api-root-v1}}/shipments

  - **Get Shipment by ID  
    \1 {{api-root-v1}}/shipments/{{current\_shipment\_id}}

\1 \2
  - **Upload Document to Shipment  
    \1 {{api-root-v1}}/shipment/{{current\_shipment\_id}}/upload
    
      - Form-data: file + path
    
      - *Currently supports document association; auto-VC issuance is being built.  
        *

\1 \2
  - **Get All VCs for Shipment  
    \1 {{api-root-v1}}/credentials/list/shipment/{{current\_shipment\_id}}

  - **Get VC Details  
    \1 {{api-root-v1}}/credentials/details/{{current\_vc\_id}}

  - **Get VC Details as PDF  
    \1 {{api-root-v1}}/credentials/details/{{current\_vc\_id}}/pdf

\1 \2
\1
\1 {{api-root-v1}}/us\_customs/submit\_presentation  
Example body:  
  
{

"shipment\_id": "{{current\_shipment\_id}}",

"credential\_ids": \["{{current\_vc\_id}}"\]

}

  - 
  - **Get CBP Status for VC  
    \1 {{api-root-v1}}/shipments/{{current\_shipment\_id}}/cbps/{{current\_vc\_id}}

  - **Get All CBP Statuses for Shipment  
    \1 {{api-root-v1}}/shipments/{{current\_shipment\_id}}/cbps

\1 \2
1.  Create a shipment.

2.  Upload supporting document(s).

3.  Retrieve associated VCs.

4.  Submit VCs to CBP.

5.  Check CBP status responses.

\1 \2
  - Manual VC issuance for NCAP forms is supported today.

  - Auto-VC issuance from document upload is *in progress* and will be demonstrated once available.
