NAMESPACE=whoami

.PHONY: help
help:
	@LC_ALL=C $(MAKE) -npq : 2> /dev/null | awk -v RS= -F: '$$1 ~ /^[^.#%]+$$/ { print "- " $$1 }' | sort

up:
	@microk8s kubectl get namespace | grep -q "^${NAMESPACE} " || microk8s kubectl create namespace ${NAMESPACE}
	@microk8s kubectl apply -f whoami.yaml -n ${NAMESPACE}

down:
	@microk8s kubectl delete -f whoami.yaml -n ${NAMESPACE} || true
	@microk8s kubectl delete namespace ${NAMESPACE} || true

get-cert:
	@microk8s kubectl -n ${NAMESPACE} get secret whoami-k8s-tls -o jsonpath='{.data.tls\.crt}' | base64 -d | openssl x509 -text

get-service:
	@microk8s kubectl get service -n ${NAMESPACE}

get-all:
	@microk8s kubectl get all -n ${NAMESPACE}
