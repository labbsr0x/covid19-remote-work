# COVID-19 Remote Work Plan

## Objectives and Key Results

- **Objetivo estratégico**: "Reduce the mortality of elderly people due to the COVID-19 epidemic in Brazil"
  - **Objetivo tático**: "Reduce crowds in cities"
    - **Objetivo operacional**: "Make remote work happen for 80% of the people who work in BB internal offices by 03/28”
      - **KR**: Reach 60K simultaneous VPNs sessions
      - **KR**: Reach 60K employees with Firefox + VPN + tn3270 on their personal computer connected by VPN to the BB environment
      - **KR**: Reach 30K employees ables to use Office365 on their personal computers, from home

### Why stop the virus spread now?

If we delay the virus spread, the health system could handle it, otherwise the system can collapse and people is going to die rapdly and not only because of COVID-19

![Immediate Spread x Delayed Spread](https://res.cloudinary.com/lexana/image/upload/v1584360379/covid-danger.jpg)

## Job to be done

### For Banco do Brasil

We need a highly scalable VPN infraestructure

![Macro components of the solution](https://res.cloudinary.com/lexana/image/upload/v1584360379/components-black-box.jpg)

- **OpenVPN Server**: running in Docker containers (https://github.com/flaviostutz/openvpn-server)
- **Virtual Box Virtual Machine creator**: created on demand for each Employee composed by: t3270, Firefox, OpenVPN client
- **Website**: a landing page that explains the step by step of how to download and Install this VPN solution to the employees computers as well as makes available to the users the form to fill out their info to request their machine.
- **Backend**: Receives credentials, validates it on the corporate bases and issue the user certificates for the VPN provisioning. Starts a **Conductor** service for each new user to build a specific Virtual Machine Imagem for him/her. This image comes with: t3270, Firefox and OpenVPN client, etc.....
- **Conductor**: The workers will need the following APIs:
  - API to build the user custom made Virtual Machine
  - API to distribute the certificate among the OpenVPN servers
  - API to store the Virtual Machine image in Blob
- **Object Storage**: store the generated Virtual Machine so the user can download anytime by knowing this unique URL. Currently, we are usning Azure Blobs.
- **Web server com print da Plataforma BB**: Web server that listens for GET request on port 80 and returns a PlataformaBB PNG print, but is only accessible via VPN.
- **Customer Service**: Organize FAQ, KB, Chat and phone to assist the staff who are having difficulty performing the steps

![Detailed components](https://res.cloudinary.com/lexana/image/upload/v1584360379/components-gray-box.jpg)


### For Small Business

We need a ready-to-go easy-to-use Remote Desktop solution

- **Website** a landing page where the user will inform his/her e-mail and click "Request remote work kit".
- **Conductor**: Triggered when the remote kit is requested. The workers will need the following APIs:
  - API to build the user custom made Virtual Machine
  - API to distribute the certificate among the OpenVPN servers
  - API to store the Virtual Machine image in Blob
- **OpenVPN Server**: One isolated instance for each client, running in Docker containers https://github.com/flaviostutz/openvpn-server)
- **Virtual Box Virtual Machine creator**: a service that builds one VirtualBox image per client tied (certificate) to the respective OpenVPNServer
- **Virtual Box Virtual Machine creator** created on demand for each Employee composed by: t3270, Firefox, OpenVPN client
- **Backend** Receives credentials and validates it on the corporate bases. Starts a **Conductor** service for each new user to build a specific Virtual Machine Imagem for him/her. This image comes with: t3270, Firefox and OpenVPN client, etc.....
- **Conductor**: The workers will need the following APIs:
  - API to issue the personal certificate
  - API to distribute the certificate among the OpenVPN servers
  - API to store the Virtual Machine image in Blob
- **Object Storage**: store the generated Virtual Machine so the user can download anytime by knowing this unique URL. Currently, we are usning Azure Blobs.
- **Web server com print da Plataforma BB**: Web server that listens for GET request on port 80 and returns a PlataformaBB PNG print, but is only accessible via VPN.
- **Central de suporte**: Organize FAQ, KB, Chat and phone to assist the staff who are having difficulty performing the steps

