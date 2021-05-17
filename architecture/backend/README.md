# Backend

The full-backend-stack consists of multiple applications to provide api/data services.

## Applications

Name                        | Short Name(s)   | Repository                                                                      | Description
----------------------------|-----------------|---------------------------------------------------------------------------------|--------------------------------
Public Website              | ct_web          | [Link](https://github.com/minvws/nl-covid19-coronatester-website-private)       | Static website generated by Jekkyl.
Backend Services            | ct_backend_pota | [Link](https://github.com/minvws/nl-covid19-coronatester-app-backend-private/)  | Makes the CL cryptography accessible via an http-rest service.
Backend for Frontend (BFF)  | ct_bff          | [Link](https://github.com/minvws/nl-covid19-coronatester-app-bff-private)       | Links together several services to provide a single endpoint for the Android/IOS mobile applications.
CtCl (Camenisch-Lysyanskaya)| ctcl_issue      | [Link](https://github.com/minvws/nl-covid19-coronatester-ctcl-core-private)     | Provides multiple libraries used to access the CL cryptography in the Backend Services and Mobile apps.
Morrie                      | morrie          | [Link](https://github.com/91divoc-ln/morrie/)                                   | Provides a GUI to edit different facilities, test types, test validity, test locales, etc in the database.
Snorrie                     | snorrie         | [Link](https://github.com/91divoc-ln/snorrie/)                                  | Provides a `rest-api` and has an `upload service` to get/push read only data from morrie(s) database.
Ansible (Deployment)        |                 | [Link](https://github.com/91divoc-ln/ansible/)                                  | Scripts used to deploy all services


## Data Validation
The data submitted to the backend by the mobile apps is checked on authenticity and correctness.

Data Check  | Check                 | Performed by        | How
------------|-----------------------|---------------------|----------------
Test Result | payload authenticity  | MobileApps, BFF**   | Payload and signature are checked against a known list of authorized test providers provided by `snorrie`
Test Result | `testType`            | MobileApps, BFF**   | Test type must be present in a currently accepted list of test types provided by `snorrie`
Test Result | `sampleDate`          | MobileApps, BFF**   | Sample date must be in the past and still be valid after taking `max_validity` into consideration. Max validity data is provided by `snorrie`. `sampleDate < now AND sampleDate + max_validity < now`

** The BFF is responsible for blocking any invalid request to the backend where a `signed test result` is created. The checks performed in the mobile applications are for display purposes only.

TODO: Add new 'verification box' that sits in between the BFF and CTCL nodes. This will (also) perform the data validation checks to ensure in-depth data validation.

## PKI Cert/Key Storage

Item                        | Storage                   | Used by
----------------------------|---------------------------|---------------
Website SSL/TLS Certificate | Ansible Vault             | BFF for https termination
Website SSL/TLS Key         | Ansible Vault             | BFF for https termination
CMS Signature Certificate   | Ansible Vault             | Backend Services to generate CMS signatures
CMS Signature Key           | Ansible Vault, later HSM  | Backend Services to generate CMS signatures
CL Public Key               | Ansible Vault             | Backend Services to create `signed test result`
CL Secret Key               | Ansible Vault, later HSM  | Backend Services to create `signed test result`
