# OpenShift NVIDIA GPU Notes

## Operator Configuration

Only create a `ClusterPolicy`, it will handle driver installation. No need to create a `NVIDIADriver`, they will conflict if you do.

## Configure NVIDIA GPU Time Slicing

[Blog for reference](https://www.redhat.com/en/blog/sharing-caring-how-make-most-your-gpus-part-1-time-slicing)

```bash
oc apply -f time-slicing-config.yaml 

oc patch clusterpolicy \
   gpu-cluster-policy \
   -n nvidia-gpu-operator \
   --type merge \
   -p '{"spec": {"devicePlugin": {"config": {"name": "time-slicing-config"}}}}'

oc label \
--overwrite node node6.ocp.khary.net \
nvidia.com/device-plugin.config=NVIDIA-4060-PCIE-8GB
```

## Test out GPU

```
oc apply -f cuda-test-pod.yaml
```
