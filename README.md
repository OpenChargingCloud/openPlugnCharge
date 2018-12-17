![hubject](https://user-images.githubusercontent.com/44570304/48123837-3900ab00-e27b-11e8-9c03-4602f6e02291.png)
## **HUBJECT Open Plug&Charge**

## **Introduction**
Welcome to Open Plug&Charge, an open platform which would help you get acquainted with Hubject's Plug&Charge ecosystem.

If you here, it is because you are in someway connected to e-mobility. You could be part of an OEM, a Charge Point Operator, an e-mobility Partner or Service Provider, an EV driver or just an e-mobility enthusiast.  

No matter who you are, you are welcome to get a feel of what Hubject's Plug&Charge platform does, just be following easy steps.

Plug&Charge is a use-case derived from the technical standard ISO 15118.

ISO 15118 standard is a technical standard that provides the necessay information exchange between the EV and the charging infrastructure. Hubject has created its own Plug&Charge platform which has processes and components that make up the Plug&Charge ecosystem.

Hubjects processes and components, which will be described in detail later, are also compliant to the VDE application guide (&quot;VDE Anwendungsregel&quot;)

At the pinnacle of the Plug&charge solution, lies the V2G Root certificate authority (V2G Root CA) which is the trust anchor for all participants of ISO 15118. The Hubject's Plug&Charge ecosystem provides a V2G root CA for the managing of the ISO 15118 certificates and  Certificate Pools for publishing and signing service for trusted exchange.
The role of the pools is to store certificates (vehicle provisional, contract), making access to them easy and secure.

The interaction between the stakeholders and pools happens via exchange of interfaces of the Plug&Charge service, which are fully compatible with ISO 15118-2: 2014 and the VDE application rule, and thus make the platform interoperable for all relaying parties.

The next chapter would define the different pools and processes involved in Hubject's Plug&Charge ecosystem.

## **System Components**

#### Pools and Services




#### Root Certificate Pool – RCP


The Root Certificate Pool contains all the Root certificates in one location maintained by Hubject. The pool is used for access of Root certificates which enables validation of certificate chains and authentication of identity by exchange of interfaces, during communication between different participants (V2G, OEM, MO) of ISO 15118.
The stored root certificates are checked regularly with automated processes and expired or revoked certificates are deleted. 
The storage of root certificates executed manually by Hubject administrators.

#### Provisioning Certificate Pool – PCP

The Provisioning Certificate Pool (PCP) contains all the Vehicle Provisional certificates. These certificates are stored by the OEM and are accessible to different participants (OEMs and MOs) of ISO 15118 through interface exchanges.The MOs send the PCID of a provisioning certificate issued by the OEM and receives the appropriate provisioning certificate and the corresponding sub-CA certificate chain.
Certificates are checked regularly with automated processes and removed.

#### Certificate Provisioning Service - CPS

The CPS provides interfaces for signing contract data of MOs. Signing of MO contract data is important for security reasons, as this certificate should only be opened by the EV.
The signed contract data are stored in the contact certificate pool.

#### Contract Certificate Pool – CCP

The CCP stores the signed contract data of the MOs and provides it to the CPO. The CPO backend can request signed contract data in the form of **certificateIntallationRequest** , which is defined in ISO 15118-2:2014.

By the CPS, signed contract data will be stored in CCP and assigned to their respective PCID. In the CCP, it is possible to store multiple contracts for each car.

The CCP keeps contracts of each MO separate. The defined access rules prevent access to other MO contracts.

## **Processes**

The VDE Application Rule details each process flow for further understanding. Figure 1 shows the overall process with components and flows, which are based on the VDE Application Rule.

![capture](https://user-images.githubusercontent.com/44570304/49576965-b2f18600-f946-11e8-8a8c-8df505f5fda0.PNG)
Figure 1 – The process of an optimized PKI for the Plug &amp; Charge

During contract provisioning, several sub processes are also required, which can be divided into four main parts (see Figure 2 – Process map).

![fig2](https://user-images.githubusercontent.com/44570304/48353749-7fe20c80-e690-11e8-8782-5d7b7dba7611.png)Figure 2 – Process map

The VDE Application Rule proposes the following sub processes.

1. Vehicle production and preparation of contract-based billing

1. Providing root certificates for public charging and contract-based billing
2. Production of vehicles and storing provisioning certificate

1. Contract conclusion and vehicle assignment
  1. Conclusion of contract with customer and receiving vehicle certificate from the vehicle certificate pool.
  2. Providing contract data to the Certificate Provisioning Service
2. (Periodic) Provisioning of contract data

1. Signing contract data and storing in the CCP

1. Installation of contract

1. Providing signed contract data to CPO-backend on request

**Providing Root Certificates for Public Charging and Contract-Based Billing**

The mutual trust between participants is a precondition for ISO 15118. For this purpose, a root certificate pool provided for the of all root certificates. Each participant can receive the root certificates of other participants to validate the trust chain of each certificate.

![fig3](https://user-images.githubusercontent.com/44570304/48353758-82dcfd00-e690-11e8-8e9e-24a4b357fa5b.png)Figure 3 – Providing root certificates for public charging and contract-based billing



**Production of Vehicles and Storing Provisioning Certificate**

With the production of the vehicle, the OEM must create a provisioning certificate for each vehicle with a unique provisioning certificate identifier – PCID. The OEM sends this unique OEM provisioning certificate corresponding subordinate CA certificates to Provisioning Certificate Pool confidentially.

The customers must also receive the PCID of their vehicles to give it to the MOs, during the conclusion of a contract.

The required V2G root certificates must also be stored in vehicle for the trusted communication with charging devices and verifying contract data.

![fig4](https://user-images.githubusercontent.com/44570304/48354506-372b5300-e692-11e8-8a16-bc8a6e768c71.png)

Figure 4 – Production of vehicles and storing provisioning certificate

**Conclusion of Contract and Receiving Vehicle Certificate from PCP**

This process describes, the conclusion of contract between customer and MO and delivery of OEM provisioning certificate of vehicle to the MO.

The MO must receive the contract information from a customer including the PCID of the vehicle. The PCID must be sent by the MO to the PCP. The PCP delivers OEM provisioning certificate, including the corresponding sub CA chain.

After the verifying the authenticity of trust chain with OEM root certificate (which has been received from the Root Certificate Pool), the MO can generate an unique e-mobility account identifier – EMAID for customer and generate contract data with the following parts:

- contractSignatureCertChain,
- dhPublicKey,
- contractSignatureEncryptedPrivateKey,
- EMAID

![fig5](https://user-images.githubusercontent.com/44570304/48354513-398dad00-e692-11e8-8b8c-b9048f3c74ec.png)

Figure 5 – Conclusion of contract and receiving vehicle certificate from vehicle Certificate Pool

**Providing Contract Data to Certificate Provisioning Service**

The created contract data must be sent to the CPS for signing purposes.

![fig6](https://user-images.githubusercontent.com/44570304/48353784-8a040b00-e690-11e8-9a27-7b4f87622503.png)

Figure 6 – Providing contract data to certificate Provisioning Service

**Signing Contract Data and Storing in CCP**

The CPS signs the delivered contract data with from V2G root CA derived private key and stores it in the CCPS for provisioning for the CPO.

![fig7](https://user-images.githubusercontent.com/44570304/48353790-8c666500-e690-11e8-91ef-73a76deb0e88.png)
Figure 7 – Signing contract data and storing in CCP



**Providing Signed Contract Data to CPO-Backend on Request**

After a successful handshake between EV and charging device, the EV sends a certificate installation request to the charging device, which will be forwarded via the CPO backend to the CCP.

The CCP finds the contracts of this EV, verifies the validity of each certificate and delivers it back to the CPO backend.

![fig9](https://user-images.githubusercontent.com/44570304/48353796-90928280-e690-11e8-8456-0f8845965356.png)Figure 8 – Providing signed contract data to CPO backend on requestFigure 8 – Providing signed contract data to CPO backend on request

## **Ready to Kickstart**
Now that you have read the components and processes involved in Plug&Charge, you must be now ready to get a feel of being a part of it.
The Open Plug&Charge will be the start of your journey to the world of ISO 15118. Click on the ([Quick Start Guide](https://github.com/hubject/openPlugnCharge/blob/master/Quick%20Start%20Guide.md)) to get more details on the steps to use the Open Plug& Charge.

## **More Information**

The immense potential of Plug &amp; Charge becoming a common and user-friendly standard within the field of eMobility is highly dependent on a well-functioning ISO 15118 ecosystem, in which every participant possesses the technical and operational capabilities to play an active and value-adding role. Due to this fact, Hubject has decided not only to offer pure PKI services in the market, but also to support organizations on their way towards becoming a player within the ISO 15118 ecosystem. Knowledge transfer as well as strategic and operational support come in the form of workshops or consulting projects.

Please consult the Hubject team for more information regarding content and pricing for your individual support package. [https://www.hubject.com/iso15118/plugcharge/](https://www.hubject.com/iso15118/plugcharge/)
