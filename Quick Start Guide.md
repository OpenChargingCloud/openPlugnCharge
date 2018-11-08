![slzndjga](https://user-images.githubusercontent.com/44570304/48204391-02558e00-e36a-11e8-9928-146cc7f08f21.jpeg)
**Introduction**

The Quick start guide shows how to quickly test with Hubject openPlugnCharge as an OEM, CPO, MO or anyone who wants to evaluate the ISO 15118 plug and charge use case.

For testing the interfaces of Plug and Charge Systems, a Postman collection is provided, which includes all interfaces of the openPlugnCharge system. Use the provided JSOn collection to an API development environment.

For reference, The Postman is a free tool ([https://www.getpostman.com/](https://www.getpostman.com/)).

In the Test-Stage of openPlugnCharge no authentication will be necessary.

**Important**

- Use this data, info only for testing openPlugnCharge environment.
- This test data should only be used for the understanding the functionality of openPlugnCharge environment.
- The openPlugnCharge only provides basic functionalities of a larger ecosystem
- All provided data are created only for this specific purpose, and are dummy information
- If you see the usage of this data for any purpose other than testing, please contact the Hubject administrator

**Start Testing**

**Step 1:**

- As in the &quot;VDE Application Guide&quot; described, the first step will be the storage of the OEM provisioning certificate in the Provisioning Certificate Pool (PCP). For storing your vehicle OEM provisioning certificate, you must use the **addOEMProvCert** interface.

**PUT /HubjectWS/webresources/v1/oem/provCerts**

Fill in the attributes in the window below (with the data provided in certificates)

```
{"rootAuthorityKeyIdentifier":"",
 "rootIssuerDistinguishedName":"",
   "rootIssuerSerialNumber": "",
   "subCA1Certificate": "",
  "subCA2Certificate": "",
 "vehicleCertificate": ""
}


```

- After successful storing of OEM provisioning certificate, the system will send the http response &quot;201&quot;.

- As an OEM, one can also use the **deleteOEMProvCert** interface to delete OEM provisioning certificates.

**DEL /HubjectWS/webresources/v1/oem/provCerts/{PCID}**





  **Step 2:**

- This step for the MOs, after creating their own **contractData** (EXI), as defined in ISO 15118.
- After customer signs a contract with the MO, the MO can use the **getOEMProvCert** interface with the PCID of customers&#39; EV, to get the OEM provisioning certificate and create a **contractData** for this particular EV. The created **contractData** must be in EXI format, like defined in ISO 15118.



**Step3:**

- After creating **contractData** , MOs have to sign **contractData** with the Certificate Provisioning Service (CPS) by using **createAndForwardSignedContractData**

The **createAndForwardSignedContractData** interface signs the **contractData** of MO and stores directly in the Hubject Contract Certificate Pool (CCP).

**PUT /HubjectWS/webresources/v1/cps/forward/signedContractData**

Fill in the attributes in the window below (with the data provided in certificates)

```
{ "xsdMsgDefNamespace": "", 
 "contractData":{ 
 "emaid":"", 
 "contractSignatureCertChain":{   
"contractSignatureCertChainSubCertificates": [ 
 "", 
  ""   ], 
"contractSignatureCertChainCertificate": "" }, 
 "contractSignatureEncryptedPrivateKey": "", 
"dHpublickey": "" 
 } 
}
 ```



**Step 4:**

- If the contract between the customer and the MO has been terminated, the MO can use **deleteSignedcontractData** interface to delete **contractData** from the Hubject Contract Certificate Pool (CCP).

**DEL /HubjectWS/webresources/v1/ccp/signedContractData/{EMAID}**

**Step 5:**

- In this step, the charge point operator calls the **getSignedcontractData** interface to receive **contractData.**

**POST /HubjectWS/webresources/v1/ccp/signedContractData**

```
{
 "certificateInstallationReq": "",
"xsdMsgDefNamespace": ""
}

```

The request must be an **certificateInstallationRequest** , as defined in ISO 15118. In the response, the CCP delivers all available contracts of the vehicle.
