# First steps

Inicia Minikube para desarrollo los requisitos minimos:

```minikube start --driver=docker --cpus=2 --memory=4096 --addons=ingress,metrics-server```

* minikube v1.37.0 en Microsoft Windows 11 Pro 10.0.26100.6584 Build 26100.6584
* Using the docker driver based on user configuration
* Using Docker Desktop driver with root privileges
* Starting "minikube" primary control-plane node in "minikube" cluster
* Pulling base image v0.0.48 ...
* Descargando Kubernetes v1.34.0 ...
    > preloaded-images-k8s-v18-v1...:  337.07 MiB / 337.07 MiB  100.00% 26.28 M
    > gcr.io/k8s-minikube/kicbase...:  488.52 MiB / 488.52 MiB  100.00% 18.62 M
* Creating docker container (CPUs=2, Memory=4096MB) ...
! Failing to connect to https://registry.k8s.io/ from inside the minikube container
* To pull new external images, you may need to configure a proxy: https://minikube.sigs.k8s.io/docs/reference/networking/proxy/
* Preparando Kubernetes v1.34.0 en Docker 28.4.0...
* Configurando CNI bridge CNI ...
* Verifying Kubernetes components...
  - Using image gcr.io/k8s-minikube/storage-provisioner:v5
* After the addon is enabled, please run "minikube tunnel" and your ingress resources would be available at "127.0.0.1"
  - Using image registry.k8s.io/ingress-nginx/controller:v1.13.2
  - Using image registry.k8s.io/ingress-nginx/kube-webhook-certgen:v1.6.2
  - Using image registry.k8s.io/ingress-nginx/kube-webhook-certgen:v1.6.2
  - Using image registry.k8s.io/metrics-server/metrics-server:v0.8.0
* Verifying ingress addon...
* Complementos habilitados: storage-provisioner, metrics-server, default-storageclass, ingress

! C:\Program Files\Docker\Docker\resources\bin\kubectl.exe is version 1.32.2, which may have incompatibilities with Kubernetes 1.34.0.
  - Want kubectl v1.34.0? Try 'minikube kubectl -- get pods -A'
* Done! kubectl is now configured to use "minikube" cluster and "default" namespace by default

**Tunel para LoadBalancer:**

nohup minikube tunnel &  # En background

**Agregar repos Helm**

```ps
helm repo add livekit https://helm.livekit.io
helm repo add bitnami https://charts.bitnami.com/bitnami
helm repo update
```

helm repo add livekit https://helm.livekit.io
"livekit" has been added to your repositories
(base) PS C:\Users\ZUB> helm repo add bitnami https://charts.bitnami.com/bitnami
"bitnami" has been added to your repositories
(base) PS C:\Users\ZUB> helm repo update
Hang tight while we grab the latest from your chart repositories...
...Successfully got an update from the "livekit" chart repository
...Successfully got an update from the "bitnami" chart repository
Update Complete. ⎈Happy Helming!⎈



kubectl create namespace livekit


$MINIKUBE_IP = minikube ip
Write-Output "Usa $MINIKUBE_IP en URIs (e.g., ws://$MINIKUBE_IP:7880)"

Usa 192.168.49.2 en URIs (e.g., ws://)


Desplegar REDIS

Redis es requerido para message bus en modo escalable

Instala Redis via Helm (sin auth para dev; en prod, agrega password):

helm install redis bitnami/redis --namespace livekit --set auth.enabled=false --set master.persistence.enabled=true --set master.persistence.size=2Gi



Verificar
kubectl get pods -n livekit
kubectl exec -it redis-master-0 -n livekit -- redis-cli ping  # Debe responder "PONG"

Si falla, chequea logs: kubectl logs -f livekit-redis-master-0 -n livekit.


Desplegar values-server.yml
helm install livekit-server livekit/livekit-server --namespace livekit --values values-server.yaml


NAME: livekit-server
LAST DEPLOYED: Mon Sep 15 17:43:40 2025
NAMESPACE: livekit
STATUS: deployed
REVISION: 1
TEST SUITE: None
NOTES:
-------------------------------------------------------------------------------

LiveKit v1.9.0 has been deployed!

Please ensure that the following ports on the nodes are open on your firewall.
* WebRTC UDP 50000 - 60000
* WebRTC TCP 7881
Load balancer has been disabled


Verificar 

kubectl get pods,svc,hpa -n livekit

kubectl port-forward svc/livekit-server-ingress 7880:80 -n livekit  # Accede: ws://localhost:7880

Forwarding from 127.0.0.1:7880 -> 7880
Forwarding from [::1]:7880 -> 7880


kubectl apply -f sip-cm.yaml -n livekit


kubectl apply -f sip-deployment.yaml -n livekit

kubectl apply -f sip-service.yaml -n livekit

kubectl apply -f hpa-sip.yaml -n livekit


Verificación: kubectl get svc sip-service -n livekit (SIP URI: sip:$MINIKUBE_IP:5060). Test: lk sip call --url sip:$MINIKUBE_IP:5060 --api-key devkey --api-secret secret --room test-sip --number +1234567890.


Optional

helm install livekit-ingress livekit/ingress --namespace livekit --values values-ingress.yaml
helm install livekit-egress livekit/egress --namespace livekit --values values-egress.yaml

Verificación: kubectl get pods -n livekit | grep ingress. Test RTMP: Usa OBS a rtmp://$MINIKUBE_IP:1935/live/test.