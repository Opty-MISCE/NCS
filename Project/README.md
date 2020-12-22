# Medical Test Records

## CA

This Module is the Certificate Authority.

## HospitalServer

This Module implements a Server which Provides the Services defined in [HSEmployeeContract](#HSEmployeeContract).

All Patient Records are stored in a PSQL Database along with its Date Creation. Also, the Test Results are stored along with its LabId, Certificate and Signature to be able to provide an Employee a way that it's possible to Verify the Test Authenticity.

- Communication with an Employee is Secure because it's used SSL gRPC, for that to be possible it needs to Generate a Certificate signed by the CA.
- Communication with a PartnerLab is also Secure, contrarily to the Employee it is not establish a TLS Connection, but there is also a Handshake where Certificates are exchanged, and a SecretKey is computed using Diffie-Hellman in order to provide Confidentiality to the Connection, in all Messages exchanged there is also a Freshness and Integrity part which allows the receiver to verify if the Message is Authentic or not.

## Employee

This Module is the Employee Interface for it to be able to Request Services from the HospitalServer.

## PolicyAuthoring

This Module implements a Policy Decision Point (PDP) which has the ability of Decide whether a Request is Accepted or Denied. For fulfilling this Purpose it uses RBAC XACMl Policies based on the Request Subject, Resource, Action and Environment. It Provides a Service defined in [HSPolicyAuthoringContract](#HSPolicyAuthoringContract).

- Communication with the Hospital Server is Secure because it uses SSL gRPC, for that to be possible it needs to Generate a Certificate signed by the CA.

## PartnerLab

This Module implements the mechanisms that allow a PartnerLab to safely establish a connection to HospitalServer, which Provides the Services defined in [HSPartnerLabContract](#HSPartnerLabContract).

- Communication with the HospitalServer is Secure, which is achieved by implementing the Custom Protocol described in the Project Proposal. There is a Handshake where Certificates are exchanged, and a SecretKey is computed using Diffie-Hellman in order to provide Confidentiality to the Connection, in all Messages exchanged there is also a Freshness and Integrity part which allows the receiver to verify if the Message is Authentic or not.

## HSEmployeeContract

This Module contains a definition of all Messages exchanged via gRPC between an Employee and Hospital Server. As well as an Enum with ErrorMessages which can be Sent in Exceptions when an Employee invokes a Service. Services defined in this Proto:
- Login: Which Logs an Employee in
- Read: Which Provides Patient Records to Hospital Employees
- Write: Which allows an Employee to append a Patient Record
- TResAuth: Which allows to Check the Authenticity of a Test Result
- Logout: Which Logs an Employee out
- CreateEmployee: Which allows an Employee to be Created
- CreatePatient: Which allows a Patient to be Created
- PatientDetails: Which allows to Find a Patient By NIF, Name or Id
- ChangeMode: Which allows the Hospital Server Mode to be Changed
- CheckMode: Which allows an Employee to Check the Hospital Server Current Mode

## HSPartnerLabContract

This Module contains a definition of all Messages exchanged via gRPC between a PartnerLab and Hospital Server. As well as an Enum with ErrorMessages which can be Sent in Exceptions when a PartnerLab invokes a Service. Services defined in this Proto:
- Hello: Which allows the Certificates to be exchange and Provides the PartnerLab with an AccessToken
- DH: Which allows a PartnerLab, and a HospitalServer to compute the same SecretKey using Diffie-Hellman
- Login: Which allows a PartnerLab to active its AccessToken by Logging himself in
- SubmitTResults: Which allows a PartnerLab to Submit a Set of Test Results
- PatientDetails: Which allows a PartnerLab to discover a PatientId by its Name or Nif
- Logout: Which allows a PartnerLab to deactivate its Session by Logging himself out

## HSPolicyAuthoringContract

This Module contains a definition of all Messages exchanged via gRPC between the Policy Authoring and Hospital Server. Services defined in this Proto:

- Decide: Which decides whether a Given Request is Accepted or Denied

## MTR-Lib

This Module is a Library which agglomerates a few Functionalities:
- Crypto Package to Manage Public & Private Keys & Certificates
- Error Package containing a Generic Assert Exception

---

## Relevat Links:
- Project Overview Available [(Here)](https://github.com/Opty-Forks/Project-Overview-2021_1)
- Project Topic (3<sup>rd</sup>) Available [(Here)](https://github.com/Opty-Forks/Project-Topics-2021_1#3-medical-test-records)

---

| Name | University | Email |
| ---- | ---- | ---- |
| Ricardo Grade | Instituto Superior Técnico | ricardo.grade@tecnico.ulisboa.pt |
| Sara Machado | Instituto Superior Técnico | sara.f.machado@tecnico.ulisboa.pt |
| Rafael Figueiredo | Instituto Superior Técnico | rafael.alexandre.roberto.figueiredo@tecnico.ulisboa.pt |
