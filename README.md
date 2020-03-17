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
- **Backend**: Receives credentials, validates it on the corporate bases and issue the user certificates for the VPN provisioning. Starts a **Conductor** workflow for each new user to build a specific Virtual Machine Imagem for him/her. This image comes with: t3270, Firefox and OpenVPN client, etc.....
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

- **Website**: will have basically 2 pages:
  - a landing page where the user will inform his/her e-mail and click "Request remote work kit"
  - a page with the users RDP shortcuts. The URL of this RDP list will have the following pattern: /user_@_email
- **Backend** Receives the form request of the landing page with the user e-mail. It issues a certificate for that request and starts a **Conductor** workflow to build the custom made infraestructure for that user (small business) passing those certificates along.
- **Conductor**: Will bring up the OpenVPN server for the user with the respective certificate provisioned and will build and store a Virtual Machine (Router) with OpenVPN client with respective certificate configured. So, the workers will need the following APIs:
  - API to bring up (docker up) a OpenVPN Server for the requesting user with a label indicating who is the user of this instance
  - API to build the a custom made Virtual Machine that will act as a "virtual router" and must be installed in at least one of the office internal network machines. This will inspect the OpenVPN server labeled with the user e-mail to check what is its published port to configure OpenVPN client present in the machine
  - API to store the Virtual Machine image in Object Storage
- **OpenVPN Server**: One isolated instance for each client, running in Docker containers https://github.com/flaviostutz/openvpn-server)
- **Virtual Box Virtual Machine creator**: a service that builds one VirtualBox image per client tied (certificate) to the respective OpenVPNServer and is configured to act as a router at the users office.
- **Object Storage**: service to store the generated Virtual Machine so the user can download anytime by knowing this unique URL
- **IP/Nickname agent**: an executable program that runs on each of the office desktops, get its local IP address and prompts for a identification **Nickname** of that machine and sends this information to the **Backend** so it can keep a database of locals Nicknames and IPs and generates RDP clients for download by the user, simplifying the remote desktop use, without needing to know IP address or something like that
- **Customer Service**: Organize FAQ, KB, Video, Chat and phone to assist the staff who are having difficulty performing the steps

