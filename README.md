
![hubject](https://user-images.githubusercontent.com/44570304/48123837-3900ab00-e27b-11e8-9c03-4602f6e02291.png)
## **HUBJECT Open PlugnCharge**

## **Introduction**

The Hubject Open PlugnCharge enables OEMs, Mobility Operators and Charge Point Operators to use the ISO 15118 standard.

The Open PlugnCharge provides a V2G root CA for the managing of the ISO 15118 certificates and pools for publishing and signing service for trusted exchange. The root certificate of the Hubject V2G Root CA is the trust anchor for all participants of ISO 15118, which is published in the Root Certificate Pool of Hubject.

The interfaces of this service are fully compatible with ISO 15118-2: 2014 and the VDE application rule (&quot;VDE Anwendungsregel&quot;), to make the platform interoperable for all relaying parties. In the implementation, the international best practices are considered for the security of communication and protection of data.

In this service, Hubject Open PlugnCharge provides the Certificate pools and Certificate Provisioning Service

The certificate pools and Certificate Provisioning Service is for publishing certificates and contracts between all ISO15118 party&#39;s.

The technical implementation of Hubject PlugnCharge is fully compatible with ISO15118-2: 2014 and the VDE application rule (&quot;VDE Anwendungsregel&quot;).

## **System Components**

#### Root Certificate Pool – RCP

The Root Certificate Pool is used for communication between the Root Certificate Pool and the various Certificate Authorities of ISO 15118 participants (V2G, OEM, MO).

The Root Certificate Pool provides root certificates for ISO 15118 participants. The stored root certificates are checked regularly with automated processes and expired or revoked certificates are deleted. The storage of root certificates executed manually by Hubject administrators.

#### Provisioning Certificate Pool – PCP

The Provisioning Certificate Pool (PCP) Service provides interfaces to exchange provisioning certificates between OEMs and MOs. For this purpose, MOs send the PCID of a provisioning certificate issued by the OEM and receives the appropriate provisioning certificate and the corresponding sub-CA certificate chain. It is ensured that the safety precautions of ISO 15118 are adhered to and that only trustworthy MOs are granted access. In addition, no confidential OEM data such as the number of available electric vehicles can be displayed when querying the available OEM provisioning certificates.

The stored OEM provisioning certificates are checked regularly with automated processes, expired and revoked certificates and linked contracts will be deleted.

The OEM provisioning certificates of each OEM will be stored in separate containers. The defined access rules prevents access to other OEM containers.

Each OEM can manage (create/update/delete) only OEM provisioning certificates of their company.

#### Certificate Provisioning Service - CPS

The CPS provides interfaces for generating and signing contract data of MOs. MOs can provide a contract data to sign in CPS or send contract information to generate contract data in Hubject Mobility Operator CA. The signed contract data are either returned to the MO or stored in the CCP.

#### Contract Certificate Pool – CCP

The CCP stores the signed contract data of the MOs and provides it to the CPO and OEM backend. The CPO backend can request signed contract data in the form of certificateIntallationRequest, which is defined in ISO 15118-2:2014. It also provides signed contract data to OEM backend.

By the CPS, signed contract data will be stored in CCP and assigned to their respective PCID. In the CCP, it is possible to store multiple contracts for each car.

The CCP keeps contracts of each MO separate. The defined access rules prevent access to other MO contracts.

Each MO can manage (create/update/delete) only contracts of their company.

The Hubject administration creates a node for each MO after the confirmation of provider ID by the issuing authority.



## **Processes**

The VDE Application Rule details each process flow for further understanding. Figure 1 shows the overall process with components and flows, which are based on the VDE Application Rule.

![fig1](https://user-images.githubusercontent.com/44570304/48123862-47e75d80-e27b-11e8-9c06-3223e0ccd6b5.png)
Figure 1 – The process of an optimized PKI for the Plug &amp; Charge

During contract provisioning, several sub processes are also required, which can be divided into four main parts (see Figure 2 – Process map).
![fig2](https://user-images.githubusercontent.com/44570304/48123929-6b120d00-e27b-11e8-9a8d-ba74449f7de9.png)Figure 2 – Process map

The VDE Application Rule proposes the following sub processes.

1. Vehicle production and preparation of contract-based billing
  - Providing root certificates for public charging and contract-based billing
  - Production of vehicles and storing provisioning certificate
2. Contract conclusion and vehicle assignment
  - Conclusion of contract with customer and receiving vehicle certificate from the vehicle certificate pool.
  - Providing contract data to the Certificate Provisioning Service
  - Providing contract information to Hubject Mobility Operator CA
3. (Periodic) Provisioning of contract data
  - Signing contract data and storing in the CCP
4. Installation of contract
  - Providing signed contract data to CPO-backend on request

**Providing Root Certificates for Public Charging and Contract-Based Billing**

The mutual trust between participants is a precondition for ISO 15118. For this purpose, a root certificate pool provided for the of all root certificates. Each participant can receive the root certificates of other participants to validate the trust chain of each certificate.

![fig3](https://user-images.githubusercontent.com/44570304/48123947-749b7500-e27b-11e8-8acb-31a4e920e18e.png)
Figure 3 – Providing root certificates for public charging and contract-based billing



**Production of Vehicles and Storing Provisioning Certificate**

With the production of the vehicle, the OEM must create a provisioning certificate for each vehicle with a unique provisioning certificate identifier – PCID. The OEM sends this unique OEM provisioning certificate corresponding subordinate CA certificates to Provisioning Certificate Pool confidentially.

The customers must also receive the PCID of their vehicles to give it to the MOs, during the conclusion of a contract.

The required V2G root certificates must also be stored in vehicle for the trusted communication with charging devices and verifying contract data.

![fig4](https://user-images.githubusercontent.com/44570304/48123966-80873700-e27b-11e8-8020-522bd5b58934.png)
Figure 4 – Production of vehicles and storing provisioning certificate

**Conclusion of Contract and Receiving Vehicle Certificate from PCP**

This process describes, the conclusion of contract between customer and MO and delivery of OEM provisioning certificate of vehicle to the MO.

The MO must receive the contract information from a customer including the PCID of the vehicle. The PCID must be sent by the MO to the PCP. The PCP delivers OEM provisioning certificate, including the corresponding sub CA chain.

After the verifying the authenticity of trust chain with OEM root certificate (which has been received from the Root Certificate Pool), the MO can generate an unique e-mobility account identifier – EMAID for customer and generate contract data with the following parts:

- contractSignatureCertChain,
- dhPublicKey,
- contractSignatureEncryptedPrivateKey,
- EMAID

![fig5](https://user-images.githubusercontent.com/44570304/48123992-8da42600-e27b-11e8-99d0-0fd72d4c2ef5.png)
Figure 5 – Conclusion of contract and receiving vehicle certificate from vehicle Certificate Pool

**Providing Contract Data to Certificate Provisioning Service**

The created contract data must be sent to the CPS for signing purposes.

There are to possibilities for MOs, signing and storing of contract data in the CCP, or after signing receiving the signed contract data back.

![fig6](https://user-images.githubusercontent.com/44570304/48124007-97c62480-e27b-11e8-94e2-89a29dd6c6f1.png)
Figure 6 – Providing contract data to certificate Provisioning Service

**Providing Contract Information to Hubject Mobility Operator CA**

In this process, MOs do not need to create a contract data. After receiving a PCID from the customer and generating an EMAID, the MO sends the contract information to the Hubject Mobility Operator CA to create a contract data. The contract information includes PCID, EMAID and contracts begin and end dates.

![fig7](https://user-images.githubusercontent.com/44570304/48124034-a44a7d00-e27b-11e8-91d0-61e4f88e30ec.png)
Figure 7 – Providing contract information to Hubject Mobility Operator CA

**Signing Contract Data and Storing in CCP**

The CPS signs the delivered contract data with from V2G root CA derived private key and either stores it in the CCPS for provisioning for the CPO- and OEM-backend, or sends back to the MO.

![fig8](https://user-images.githubusercontent.com/44570304/48124045-ae6c7b80-e27b-11e8-9ca4-ccce08912f32.png)
Figure 8 – Signing contract data and storing in CCP



**Providing Signed Contract Data to CPO-Backend on Request**

After a successful handshake between EV and charging device, the EV sends a certificate installation request to the charging device, which will be forwarded via the CPO backend to the CCP.

The CCP finds the contracts of this EV, verifies the validity of each certificate and delivers it back to the CPO backend.

![fig9](https://user-images.githubusercontent.com/44570304/48124060-b9271080-e27b-11e8-94d7-f12981db8169.png)
Figure 9 – Providing signed contract data to CPO backend on request
