## Submit Verifiable Credentials to CBP

### What this feature does
This feature lets authorized users submit Verifiable Credentials (VCs) related to a shipment to U.S. Customs and Border Protection (CBP). You can submit one or more VCs from the shipment page and see whether CBP has received them.

### Who can use it
- Users with the appropriate organization permissions (VC issuer role)
- Organizations with the “Submit to CBP” feature enabled

If you don’t see the option to submit, contact your administrator.

### Prerequisites
- Your shipment must contain at least one validated VC file
- You must be signed in and have permission to submit to CBP

### How to submit VCs to CBP
1. Open a shipment in the portal.
2. In the files table, identify the VC files you want to submit.
3. Click “Submit to CBP”.
4. Select the VC files in the dialog and confirm.
5. The system submits a Verifiable Presentation (VP) on your behalf.

### What you will see
- For each VC, the CBP column shows the most recent submission result:
  - Check icon: Received by CBP (HTTP 200)
  - X icon: Not received or an error occurred
- The submission dialog lists past attempts with dates and a success/failure indicator.

### Status meanings
- Received by CBP: CBP responded successfully to the submission.
- Not received / Error: The last attempt returned a non-200 response or an error occurred during submission. You can try submitting again later.

### Common scenarios
- I don’t see “Submit to CBP”
  - You may not have the required permission or your organization’s feature flag isn’t enabled. Contact your admin.

- I clicked submit but don’t see a check icon
  - It can take a moment to reflect the latest status. Refresh the page or reopen the submission dialog to check recent attempts.

- I submitted the wrong VC
  - Submit the correct VC. If you need to remove a document, use file management permissions or contact support.

### Troubleshooting
- Permission errors
  - Ensure your account has the VC issuer role and the feature is enabled for your organization.

- Consistent failure to submit
  - Try again in a few minutes. If the issue persists, contact support with the shipment ID and VC file name.

- Missing VC in the selection list
  - Make sure the VC has been generated and validated in the shipment folder. If you’re expecting a VC that isn’t present, contact your internal team.

### Support
If you need help, provide:
- Shipment ID
- VC file name(s)
- Date/time of your last submission attempt
- A brief description of the issue


