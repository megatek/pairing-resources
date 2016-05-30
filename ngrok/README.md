### Running

##### On Server:
./ngrokd -tlsKey=server.key -tlsCrt=server.crt -domain="ngrok.megatek.io" -httpAddr=":8080" -httpsAddr=":8081"

##### On Client:
./ngrok -subdomain testing -config=[YOUR_CONFIG_FILE_NAME] 80

### Followed instructions from:
https://www.svenbit.com/2014/09/run-ngrok-on-your-own-server/

### Important
Golang needs to be installed and the version needs to be greater than 1.3

cd ~/tmp
git clone https://github.com/inconshreveable/ngrok.git ngrok
cd ngrok

openssl genrsa -out base.key 2048
openssl req -new -x509 -nodes -key base.key -days 10000 -subj "/CN=ngrok.megatek.io" -out base.pem
openssl genrsa -out server.key 2048
openssl req -new -key server.key -subj "/CN=ngrok.megatek.io" -out server.csr
openssl x509 -req -in server.csr -CA base.pem -CAkey base.key -CAcreateserial -days 10000 -out server.crt

cp base.pem assets/client/tls/ngrokroot.crt

make release-server release-client

cd bin
chmod +x ngrokd
chmod +x ngrok
