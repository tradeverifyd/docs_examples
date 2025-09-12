**Title:** *Tradeverifyd Shipments \<\> CBP API – Postman Collection Guide*

**Introduction  
** This Postman collection provides access to the Tradeverifyd API for managing shipments, issuing verifiable credentials (VCs), and submitting them to CBP. It is designed to support NCAP testing and give Deacero hands-on access to the foundational endpoints.

### **Authentication**

  - **Method:** API Key

  - **Header:** ocp-apim-subscription-key: {{api-key}}

### **Environment Setup (Required Before Use)**

**To use the Postman collection, you’ll need to create a Postman environment with the following variables:**

  - **api-root-v1  
    **
    
      - ***Type:* String  
        **
    
      - ***Example:* https://api.tradeverifyd.com/v1  
        **
    
      - ***Description:*** Root URL for the API.**  
        **

  - **api-key  
    **
    
      - ***Type:*** Secret**  
        **
    
      - ***Value:*** Your Tradeverifyd API subscription key.**  
        **
    
      - **⚠️ Mark this variable as Secret in Postman. This prevents it from being accidentally exposed during screen shares or exports.  
        **

  - **(Optional) Runtime variables created during workflows:  
    **
    
      - **current\_shipment\_id  
        **
    
      - **current\_vc\_id  
        **
    
      - **vcs\_to\_submit\_to\_cbp**

### **Key API Flows**

#### **1. Shipments**

**Create Shipment  
** POST {{api-root-v1}}/shipments  
Example body:  
  
{

"shipment\_name": "Deacero Test Shipment 1",

"status": "Created",

"organizational\_units": \["deacero"\]

}

  - 
  - **Get All Shipments  
    ** GET {{api-root-v1}}/shipments

  - **Get Shipment by ID  
    ** GET {{api-root-v1}}/shipments/{{current\_shipment\_id}}

#### **2. Upload Documents**

  - **Upload Document to Shipment  
    ** POST {{api-root-v1}}/shipment/{{current\_shipment\_id}}/upload
    
      - Form-data: file + path
    
      - *Currently supports document association; auto-VC issuance is being built.  
        *

#### **3. Verifiable Credentials**

  - **Get All VCs for Shipment  
    ** GET {{api-root-v1}}/credentials/list/shipment/{{current\_shipment\_id}}

  - **Get VC Details  
    ** GET {{api-root-v1}}/credentials/details/{{current\_vc\_id}}

  - **Get VC Details as PDF  
    ** GET {{api-root-v1}}/credentials/details/{{current\_vc\_id}}/pdf

#### **4. CBP Integration**

**Submit VCs to CBP  
** POST {{api-root-v1}}/us\_customs/submit\_presentation  
Example body:  
  
{

"shipment\_id": "{{current\_shipment\_id}}",

"credential\_ids": \["{{current\_vc\_id}}"\]

}

  - 
  - **Get CBP Status for VC  
    ** GET {{api-root-v1}}/shipments/{{current\_shipment\_id}}/cbps/{{current\_vc\_id}}

  - **Get All CBP Statuses for Shipment  
    ** GET {{api-root-v1}}/shipments/{{current\_shipment\_id}}/cbps

### **Workflow Example**

1.  Create a shipment.

2.  Upload supporting document(s).

3.  Retrieve associated VCs.

4.  Submit VCs to CBP.

5.  Check CBP status responses.

### **Notes**

  - Manual VC issuance for NCAP forms is supported today.

  - Auto-VC issuance from document upload is *in progress* and will be demonstrated once available.
