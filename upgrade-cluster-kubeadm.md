# Upgrading K8s with kubeadm


## Relevant Documentation

[Relevant Documentation](https://kubernetes.io/docs/tasks/administer-cluster/kubeadm/kubeadm-upgrade/)

Lesson Reference

First, upgrade the control plane node.

Drain the control plane node.

```
kubectl drain <control plane node name> --ignore-daemonsets
```

&nbsp;
&nbsp;

Upgrade kubeadm.

```
sudo apt-get update && \
sudo apt-get install -y --allow-change-held-packages kubeadm=1.20.2-00

kubeadm version
```

&nbsp;
&nbsp;

Plan the upgrade.

```
sudo kubeadm upgrade plan v1.20.2
```

&nbsp;
&nbsp;

Upgrade the control plane components.

```
sudo kubeadm upgrade apply v1.20.2
```

&nbsp;
&nbsp;

Upgrade kubelet and kubectl on the control plane node.

```
sudo apt-get update && \
sudo apt-get install -y --allow-change-held-packages kubelet=1.20.2-00 kubectl=1.20.2-00
```

&nbsp;
&nbsp;

Restart kubelet.

```
sudo systemctl daemon-reload

sudo systemctl restart kubelet
```

&nbsp;
&nbsp;

Uncordon the control plane node.

```
kubectl uncordon <control plane node name>
```

&nbsp;
&nbsp;

Verify that the control plane is working.

```
kubectl get nodes
```

&nbsp;
&nbsp;

Upgrade the worker nodes.

Note: In a real-world scenario, you should not perform upgrades on all worker nodes at the same time. Make sure enough nodes are available at any given time to provide uninterrupted service.

&nbsp;
&nbsp;

Run the following on the control plane node to drain worker node 1:

```
kubectl drain <worker 1 node name> --ignore-daemonsets --force
```

&nbsp;
&nbsp;

Log in to the first worker node, then Upgrade kubeadm.

```
sudo apt-get update && \
sudo apt-get install -y --allow-change-held-packages kubeadm=1.20.2-00

kubeadm version
```

&nbsp;
&nbsp;

Upgrade the kubelet configuration on the worker node.

```
sudo kubeadm upgrade node
```

&nbsp;
&nbsp;

Upgrade kubelet and kubectl on the worker node.

```
sudo apt-get update && \
sudo apt-get install -y --allow-change-held-packages kubelet=1.20.2-00 kubectl=1.20.2-00
```

&nbsp;
&nbsp;

Restart kubelet.

```
sudo systemctl daemon-reload

sudo systemctl restart kubelet
```

&nbsp;
&nbsp;

From the control plane node, uncordon worker node 1.

```
kubectl uncordon <worker 1 node name>
```

&nbsp;
&nbsp;

Repeat the upgrade process for worker node 2.

From the control plane node, drain worker node 2.

```
kubectl drain <worker 2 node name> --ignore-daemonsets --force
```

&nbsp;
&nbsp;

On the second worker node, upgrade kubeadm.

```
sudo apt-get update && \
sudo apt-get install -y --allow-change-held-packages kubeadm=1.20.2-00

kubeadm version
```

&nbsp;
&nbsp;

Perform the upgrade on worker node 2.

```
sudo kubeadm upgrade node

sudo apt-get update && \
sudo apt-get install -y --allow-change-held-packages kubelet=1.20.2-00 kubectl=1.20.2-00

sudo systemctl daemon-reload

sudo systemctl restart kubelet
```

&nbsp;
&nbsp;

From the control plane node, uncordon worker node 2.

```
kubectl uncordon <worker 2 node name>
```

&nbsp;
&nbsp;

Verify that the cluster is upgraded and working.

```
kubectl get nodes
```
