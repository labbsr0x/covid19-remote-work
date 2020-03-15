# COVID-19 Remote Work Plan

- **OpenVPN Server** running in Docker containers (https://github.com/kylemanna/docker-openvpn)
- **Virtual Box Virtual Machine creator** created on demand for each Employee composed by: t3270, Firefox, OpenVPN client
- **Website** a landing page that explains the step by step of how to download and Install this VPN solution to the employees computers as well as makes available to the users the form to fill out their info to request their machine.
- **Backend** Receives credentials and validates it on the corporate bases. Starts a **Conductor** service for each new user to build a specific Virtual Machine Imagem for him/her. This image comes with: t3270, Firefox and OpenVPN client, etc.....
- **Conductor**: API de build de imagem, API de controle de emissão de certificados com a URL da imagem - envio da URL pro e-mail do cara.
- **Object Storage**: store the generated Installer so the user can download anytime. Currenly, we are usning Azure Blobs.
- **API de build de imagem de VM**: API que invoca os scripts que montam uma VM em VirtualBox
- **API de controle de emissão de certificados com a URL da imagem - envio da URL pro e-mail do cara.**: API que gera os certificados pra cada usuário para serem usados para autenticar na VPN.
- **Web server com print da Plataforma BB**: servidor WEB que recebe na porta 80 mas que só é acesível via VPN.
