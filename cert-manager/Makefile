NAMESPACE=cert-manager
VERSION=v1.12.0

.PHONY: help
help:
	@LC_ALL=C $(MAKE) -npq : 2> /dev/null | awk -v RS= -F: '$$1 ~ /^[^.#%]+$$/ { print "- " $$1 }' | sort

setup:
	microk8s helm repo add jetstack https://charts.jetstack.io
	microk8s helm repo update
	microk8s curl -L -o kubectl-cert-manager.tar.gz https://github.com/jetstack/cert-manager/releases/latest/download/kubectl-cert_manager-linux-amd64.tar.gz
	microk8s tar xzf kubectl-cert-manager.tar.gz
	sudo mv kubectl-cert_manager /usr/local/bin

up:
	microk8s helm install cert-manager jetstack/cert-manager --namespace ${NAMESPACE} --create-namespace --version ${VERSION} --set installCRDs=true
	microk8s kubectl apply -f https://github.com/jetstack/cert-manager/releases/download/${VERSION}/cert-manager.crds.yaml

create-ca:
	@microk8s kubectl create -n ${NAMESPACE} -f cert-manager-ss-issuer.yaml
	@microk8s kubectl create -n ${NAMESPACE} -f cert-manager-ca-cert.yaml
	@microk8s kubectl create -n ${NAMESPACE} -f cert-manager-ca-issuer.yaml

delete-ca:
	@microk8s kubectl delete -n ${NAMESPACE} -f cert-manager-ss-issuer.yaml || true
	@microk8s kubectl delete -n ${NAMESPACE} -f cert-manager-ca-cert.yaml || true
	@microk8s kubectl delete -n ${NAMESPACE} -f cert-manager-ca-issuer.yaml || true

down: delete-ca
	@microk8s helm delete cert-manager
	@microk8s kubectl delete namespace ${NAMESPACE}

get-ca:
	@microk8s kubectl -n ${NAMESPACE} get secret test-ca-secret -o jsonpath='{.data.ca\.crt}' | base64 -d | openssl x509 -text

get-issuer:
	@microk8s kubectl get issuer,clusterissuers -A

get-cert:
	@microk8s kubectl get certificate -A

get-all:
	@microk8s kubectl get all -n ${NAMESPACE}
