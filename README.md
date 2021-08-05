# prevail_availability_test

## Openshift operators installation
Openshift jaeger
Kiali
Red Hat Elasticsearch
Openshift Service Mesh


## bookinfo namespace and application deployment
oc new-project bookinfo
oc apply -f https://raw.githubusercontent.com/Maistra/istio/maistra-2.0/samples/bookinfo/platform/kube/bookinfo.yaml
oc get pods
oc create -f https://raw.githubusercontent.com/Maistra/istio/maistra-2.0/samples/bookinfo/networking/bookinfo-gateway.yaml
oc get routes -n istio-system istio-ingressgateway

## exporting the hostname
export INGRESS_HOST=<HOST>

//fake traffic generation
for i in {1..20}; do sleep 0.5; curl -I $INGRESS_HOST/productpage; done

//install chaostoolkit
source ~/.venvs/chaostk/bin/activate
python3 -m venv ~/.venvs/chaostk
pip install -U chaostoolkit
chaos --help

//install chaos toolkit kubernetes extension

pip install -U chaostoolkit-kubernetes
chaos discover chaostoolkit-kubernetes

//start kill experiments
chaos run chaos/terminate-pod.yaml

Now go and check kiali, grafana, jaeger dashboards to analyse
