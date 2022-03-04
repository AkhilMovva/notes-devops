# K8s Key Topics

## k8s Security

### RABC

### Service Account and Cluster

- IAM Role is all about specific implementation (AWS in this case)
- Pods need kubernetes Cluster access as well
- ClusterRoles grant access to cluster-specific resources
  - Create/Delete/List/etc. Nodes
  - Create/Delete/List/etc. pods
  - Create/Delete/List/etc. Namesspces
  - etc.

### Role VS ClusterRole

- Both Role(Namespace) and ClusterRole represent set of permissions
- A Role sets permisssion within a particuler namespace - have to specify namespace
- ClusterRole is non-namespaced