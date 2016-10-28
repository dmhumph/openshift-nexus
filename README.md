# openshift-nexus

How to add persistent storage to nexus deployment using local storage:

----
mkdir /tmp/sonatype-work

chmod 777 sonatype-work

oc create -f - <<-EOF
{
    "apiVersion": "v1",
    "kind": "PersistentVolume",
    "metadata": {
        "name": "sonatype-work",
        "labels": {
           "type": "local"
        }
    },
    "spec": {
        "hostPath": {
            "path": "/tmp/sonatype-work",
            "server": "ocp-master.cloudapps.ocp.home"
        },
        "accessModes": [
            "ReadWriteMany"
        ],
        "capacity": {
            "storage": "5Gi"
        },
        "persistentVolumeReclaimPolicy": "Retain"
    }
}
EOF
----

Mount PVC to nexus and remove empty dir:
----
oc volumes dc/nexus --add --name 'nexus-volume-1' --type 'pvc' --mount-path '/sonatype-work/' --claim-name 'nexus-pv' --claim-size '3Gi' --overwrite --claim-mode='ReadWriteMany'
----
Add policy to user to have default user permissions:
----
oadm policy add-scc-to-user anyuid -z default
----

Restart failed POD.
