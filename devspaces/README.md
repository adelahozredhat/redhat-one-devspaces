oc label namespace openshift-operators openshift.io/cluster-monitoring=true


oc create configmap grafana-dashboard-dwo \
  --from-literal=dwo-dashboard.json="$(curl https://raw.githubusercontent.com/devfile/devworkspace-operator/main/docs/grafana/openshift-console-dashboard.json)" \
  -n openshift-config-managed

oc label configmap grafana-dashboard-dwo console.openshift.io/dashboard=true -n openshift-config-managed

oc create configmap grafana-dashboard-devspaces-server \
  --from-literal=devspaces-server-dashboard.json="$(curl https://raw.githubusercontent.com/eclipse-che/che-server/main/docs/grafana/openshift-console-dashboard.json)" \
  -n openshift-config-managed

oc label configmap grafana-dashboard-devspaces-server2 console.openshift.io/dashboard=true -n openshift-config-managed


SA_KEDA=$(oc get deployment custom-metrics-autoscaler-operator -n openshift-keda -o jsonpath='{.spec.template.spec.serviceAccountName}')
[adelahoz@MiWiFi-R3600-srv crc-linux-2.57.0-amd64]$ echo $SA_KEDA
custom-metrics-autoscaler-operator
[adelahoz@MiWiFi-R3600-srv crc-linux-2.57.0-amd64]$ oc create clusterrolebinding keda-prometheus-reader-final \
  --clusterrole=cluster-monitoring-view \
  --serviceaccount=openshift-operators:$SA_KEDA
clusterrolebinding.rbac.authorization.k8s.io/keda-prometheus-reader-final created
