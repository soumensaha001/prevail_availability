# prevail_availability_test

## Create project istio-system
oc new-project istio-system</br>
## Openshift operators installation
Openshift jaeger </br>
Kiali </br>
Red Hat Elasticsearch</br>
Openshift Service Mesh</br>

## Configure Openshift Service Mesh Control Plane to create a sidecar system which will track the cluster microservices

## Configure Openshift Service Mesh Member Rolls by using below yaml (in this example bookinfo app has been used)
'''
apiVersion: maistra.io/v1
kind: ServiceMeshMemberRoll
metadata:
  name: default
  namespace: istio-system
spec:
  members:
    - bookinfo
'''

## bookinfo namespace and application deployment
oc new-project bookinfo </br>
oc apply -f https://raw.githubusercontent.com/Maistra/istio/maistra-2.0/samples/bookinfo/platform/kube/bookinfo.yaml </br>
oc get pods </br>
oc create -f https://raw.githubusercontent.com/Maistra/istio/maistra-2.0/samples/bookinfo/networking/bookinfo-gateway.yaml </br>
oc get routes -n istio-system istio-ingressgateway </br>

## exporting the hostname
export INGRESS_HOST= 'HOST'</br>

## fake traffic generation</br>
for i in {1..20}; do sleep 0.5; curl -I $INGRESS_HOST/productpage; done</br>

## install chaostoolkit </br>
source ~/.venvs/chaostk/bin/activate</br>
python3 -m venv ~/.venvs/chaostk</br>
pip install -U chaostoolkit</br>
chaos --help</br>

## install chaos toolkit kubernetes extension </br>

pip install -U chaostoolkit-kubernetes</br>
chaos discover chaostoolkit-kubernetes</br>

## start killing application pods experiments </br>
chaos run chaos/terminate-pod.yaml</br>

## Now go and check kiali, grafana, jaeger dashboards to analyse availability/ resiliency of the full system
