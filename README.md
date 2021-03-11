# Unsupported registry configuration
## Summary:
To add Unsupported operators not included within the offical red hat supported set modifications to the container.d/registry.conf file is required. All of the openshift infrastructure control plane is kept in state by the machine configurations. To modify the registry.conf for unsupported operators/containers a new macine config is needed.

Offline supported operators have verified digests via the image streams to pull but anything beyond that set list comes back invalid unless you modify the registry.conf tricking it into thinking quay.io and redhat.io are fully available.

### Prereqs:
```
git clone https://github.com/RedHatGov/scripts.git
cd scripts
```
### 1. Registry.conf configurations
```
sed -i 's/localhost/registry.<fqdn>/g' registry.conf
base64 registry.conf

Copy the base64 content to the unsupported-operators-mc.yaml and replace ${registry-conf} with the content.
```  

### 2. Apply configuration
```
oc apply -f unsupported-operators-mc.yaml
watch oc get mcp
```
Once the machine config pools for the workers have updated the operators/containers will be available.
