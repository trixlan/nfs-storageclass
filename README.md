# Comandos para usar NFS externo

[Youtube Video](https://www.youtube.com/watch?v=6DmEp0kXUOI&t=1s)

```shell
# Creamos el namespace de nfs
oc create ns nfs
# Revisar el nombre del namespace en rbac.yaml, por default esta en nfs
oc create -f rbac.yaml
# Para poder descargar la imagen la puse en un repositorio de git
oc import-image nfs-subdir-external-provisioner:v4.0.2 --from=quay.io/gchavezt/nfs-subdir-external-provisioner:v4.0.2 --confirm -n nfs
# Realizamos el deployment de la imagen
oc create -f Storage/deployment.yaml
oc create -f Storage/storageClass.yaml
oc create role use-scc-hostmount-anyuid --verb=use --resource=scc --resource-name=hostmount-anyuid -n nfs
oc get roles -n nfs
oc adm policy add-role-to-user use-scc-hostmount-anyuid -z nfs-client-provisioner --role-namespace nfs -n nfs
Scalar a 0 y regresar a 1
oc create -f test.yaml