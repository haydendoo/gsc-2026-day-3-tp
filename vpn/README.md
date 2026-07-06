# Connecting to VPN Endpoint
You must first create your own self signed certificate. And then create the endpoint then download openvpn config file.

## Self-Signing Certificate
https://docs.aws.amazon.com/vpn/latest/clientvpn-admin/client-auth-mutual-enable.html

```bash
git clone https://github.com/OpenVPN/easy-rsa.git
cd easy-rsa/easyrsa3
./easyrsa init-pki
./easyrsa build-ca nopass
./easyrsa --san=DNS:server build-server-full server nopass
./easyrsa build-client-full client1.domain.tld nopass
mkdir ~/custom_folder/
cp pki/ca.crt ~/custom_folder/
cp pki/issued/server.crt ~/custom_folder/
cp pki/issued/client1.domain.tld.crt ~/custom_folder
cp pki/private/server.key ~/custom_folder/
cp pki/private/client1.domain.tld.key ~/custom_folder/
cd ~/custom_folder/
aws acm import-certificate --certificate fileb://server.crt --private-key fileb://server.key --certificate-chain fileb://ca.crt
aws acm import-certificate --certificate fileb://client1.domain.tld.crt --private-key fileb://client1.domain.tld.key --certificate-chain fileb://ca.crt # Optional
```

## Making VPN Endpoint
Choose mutual auth and select the server certificate for both server and client certificate arn.

## Connecting
Download openvpn cli then run
```bash
sudo openvpn --cert files/internal.crt --key files/internal.key --config client.ovpn
```
