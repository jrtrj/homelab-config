
# Homelab Kubernetes (k3s) — Control Node

## Goal

Set up a single-node k3s cluster and understand basic networking and Kubernetes components.

---

## Networking Setup

* Interface: `wlo1`
* Using: `ifupdown`
* Disabled: `dhcpcd` (to avoid conflicts)

### Config

```bash
auto wlo1
iface wlo1 inet dhcp
    wpa-ssid <ssid>
    wpa-psk <password>
```

### Notes

* Only one network manager should be active
* DHCP preferred (hotspot network changes)
* Static IP is unreliable in this setup

---

## Concepts

* IP: device address (e.g. `10.x.x.x`)
* Subnet: network range (`/24`)
* Gateway: network exit
* DNS: resolves domain names

---

## Issue Faced

Duplicate IP assignment due to multiple network managers

Fix:

```bash
sudo pkill dhcpcd
```

---

## k3s Installation

```bash
curl -sfL https://get.k3s.io | sh -
```

---

## Observed Interfaces

* `wlo1`: main network
* `cni0`: pod bridge (`10.42.0.0/24`)
* `flannel.1`: overlay network
* `veth*`: one per pod

---

## Kubernetes Basics

* API Server: control plane entry
* Scheduler: assigns pods
* Kubelet: runs containers
* Pods: smallest unit
* CNI: networking layer

---

## Pod Networking

```
Pod → veth → cni0 → host → flannel → other pods
```

---

## Commands

```bash
kubectl get nodes -o wide
kubectl get pods -A
kubectl describe node <node>
kubectl cluster-info
```

---

## Limitations

* Hotspot network is unstable
* Wi-Fi is less reliable than Ethernet

---

## Next Steps

* Deploy an app (nginx)
* Learn Services
* Add worker node
* Configure Ingress

---

## Summary

Single-node k3s cluster with working networking and clean interface setup.
