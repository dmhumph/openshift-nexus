# openshift-nexus

Mount PVC to nexus and remove empty dir:

oc volumes dc/nexus --add --name 'nexus-volume-1' --type 'pvc' --mount-path '/sonatype-work/' --claim-name 'nexus-pv' --claim-size '3Gi' --overwrite
