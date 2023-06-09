NAMESPACE=traefik

.PHONY: help
help:
	@LC_ALL=C $(MAKE) -npq : 2> /dev/null | awk -v RS= -F: '$$1 ~ /^[^.#%]+$$/ { print "- " $$1 }' | sort

setup:
	microk8s helm repo add traefik https://helm.traefik.io/traefik
	microk8s helm repo update

up:
	@microk8s kubectl get namespace | grep -q "^${NAMESPACE} " || microk8s kubectl create namespace ${NAMESPACE}
	#microk8s kubectl apply -f storage.yaml -n ${NAMESPACE}
	@microk8s helm upgrade --install traefik traefik/traefik --values traefik-values.yaml -n ${NAMESPACE}
	@microk8s kubectl apply -f cert.yaml -n ${NAMESPACE}
	@microk8s kubectl apply -f dashboard.yaml -n ${NAMESPACE}

down:
	@microk8s kubectl delete -f dashboard.yaml -n ${NAMESPACE} || true
	@microk8s kubectl delete -f cert.yaml -n ${NAMESPACE} || true
	#@microk8s kubectl delete -f storage.yaml -n ${NAMESPACE} || true
	@microk8s kubectl delete namespace ${NAMESPACE} || true
	@microk8s helm delete traefik

get-cert:
	@microk8s kubectl -n ${NAMESPACE} get secret traefik-k8s-tls -o jsonpath='{.data.tls\.crt}' | base64 -d | openssl x509 -text

get-service:
	@microk8s kubectl get service -n ${NAMESPACE}

get-all:
	@microk8s kubectl get all -n ${NAMESPACE}
