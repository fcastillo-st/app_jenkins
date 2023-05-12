# Sincornización de nodos ocp

creación del archivo chrony

```

server 172.16.1.190 iburst
driftfile /var/lib/chrony/drift
makestep 1.0 3
rtcsync
logdir /var/log/chrony
keyfile /etc/chrony.keys

```

se convierte a variable de entorno pasado en base64

```

chrony_base64=`base64 -w0 chrony.conf`
```

se genera el yaml por cada rol (master, worker, infra)

```

for i in master worker infra; do
cat << EOF > ./${i}-chrony-configuration.yml
apiVersion: machineconfiguration.openshift.io/v1
kind: MachineConfig
metadata:
  labels:
    machineconfiguration.openshift.io/role: ${i}
  name: ${i}-chrony-configuration
spec:
  config:
    ignition:
      config: {}
      security:
        tls: {}
      timeouts: {}
      version: 2.2.0
    networkd: {}
    passwd: {}
    storage:
      files:
      - contents:
          source: data:text/plain;charset=utf-8;base64,${chrony_base64}
          verification: {}
        filesystem: root
        mode: 420
        path: /etc/chrony.conf
  osImageURL: ""
EOF
 done

```

aplicar los archivos generados a la configuracion

```
oc apply -f infra-crhony-configuration.yml
oc apply -f worker-crhony-configuration.yml
oc apply -f master-crhony-configuration.yml

```
