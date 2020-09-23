.DEFAULT_GOAL := helper

# Uses the required .env for docker-compose to find root location.
CERTS_LOCATION ?= $(shell cat .env | grep -i CERTS_LOCATION | cut -f2 -d\=)
CA_LOC ?= $(CERTS_LOCATION)/ca
# Default location for client certificates to be stored.
CLIENT_LOC ?= $(CERTS_LOCATION)/client_certificates_remote

# Specific files for easier reference and changes. May change this in the future.
CA_CRT ?= $(CA_LOC)/ca.crt
CA_KEY ?= $(CA_LOC)/ca.key
SERV_KEY_SEC ?= $(CERTS_LOCATION)/server.key.secure
SERV_CSR ?= $(CERTS_LOCATION)/server.csr
SERV_CERT ?= $(CERTS_LOCATION)/server.crt
SERV_KEY_INSEC ?= $(CERTS_LOCATION)/server.key
CLI_KEY ?= $(CLIENT_LOC)/client.key
CLI_CSR ?= $(CLIENT_LOC)/client.csr
CLI_CERT ?= $(CLIENT_LOC)/client.crt
CLI_PFX ?= $(CLIENT_LOC)/client.pfx

helper: # Adapted from: https://marmelab.com/blog/2016/02/29/auto-documented-makefile.html
	@echo "Available targets..."
	@grep -hE '^[a-zA-Z_-]+:.*?## .*$$' $(MAKEFILE_LIST) | sort | awk 'BEGIN {FS = ":.*?## "}; {printf "\033[36m%-30s\033[0m %s\n", $$1, $$2}'

allNew: genCA genServerCert genClientCert ## This will create all components from scratch

genCA: ## Generate a new CA
	@mkdir -p $(CA_LOC) || echo "File exists"
	openssl genrsa -des3 -out $(CA_KEY) 4096
	openssl req -new -x509 -days 365 -key $(CA_KEY) -out $(CA_CRT) -subj '/C=AU/ST=NSW/L=Sydney/O=marjamis/OU=IT/CN=portal'

genServerCert: ## Generate a new server certificate
	openssl genrsa -des3 -out $(SERV_KEY_SEC) 4096
	openssl req -new -key $(SERV_KEY_SEC) -out $(SERV_CSR) -subj '/C=AU/ST=NSW/L=Sydney/O=marjamis/OU=IT/CN=portal'

	# Dont specify a CN for this cert as it can cause issues.
	openssl x509 -req -days 365 -in $(SERV_CSR) -CA $(CA_CRT) -CAkey $(CA_KEY) -set_serial 01 -out $(SERV_CERT)

	# Exporting and making clear text certs to not require passwords for servers (there are better approaches).
	openssl rsa -in $(SERV_KEY_SEC) -out $(SERV_KEY_INSEC)

genClientCert: ## Generate a new client certificate
	@mkdir $(CLIENT_LOC) || echo "File exists"
	openssl genrsa -des3 -out $(CLI_KEY) 4096
	# Note: Lacks a CN as it call a certificate issue. Further investigation required.
	openssl req -new -key $(CLI_KEY) -out $(CLI_CSR) -subj '/C=AU/ST=NSW/L=Sydney/O=marjamis/OU=IT'
	openssl x509 -req -days 365 -in $(CLI_CSR) -CA $(CA_CRT) -CAkey $(CA_KEY) -set_serial -1 -out $(CLI_CERT)
	openssl pkcs12 -export -out $(CLI_PFX) -inkey $(CLI_KEY) -in $(CLI_CERT) -certfile $(CA_CRT)

test: ## Once the website is up this test a sample endpoint which shouldn't have access
	@if [ $(shell curl https://portal/freshrss/ --resolve portal:443:localhost -Lks -o /dev/null -w "%{http_code}") != 403 ] ; then echo "Unauthorised access."; exit 1; fi
