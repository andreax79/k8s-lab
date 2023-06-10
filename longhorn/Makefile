NAMESPACE=longhorn-system

.PHONY: help
help:
	@LC_ALL=C $(MAKE) -npq : 2> /dev/null | awk -v RS= -F: '$$1 ~ /^[^.#%]+$$/ { print "- " $$1 }' | sort

up:
	@microk8s kubectl -n ${NAMESPACE} apply -f dashboard.yaml 

down:
	@microk8s kubectl -n ${NAMESPACE} delete -f dashboard.yaml 

get-cert:
	@microk8s kubectl -n ${NAMESPACE} get secret longhorn-k8s-tls -o jsonpath='{.data.tls\.crt}' | base64 -d | openssl x509 -text

get-service:
	@microk8s kubectl get service -n ${NAMESPACE}

get-all:
	@microk8s kubectl get all -n ${NAMESPACE}
