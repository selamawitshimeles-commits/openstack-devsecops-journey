# OpenStack  Floating IP

## What is a Floating IP?

In OpenStack networking, a floating IP is a public, routable IP address that can be dynamically associated with and disassociated from virtual machines. Think of it as a "dynamic public IP" that you can attach to or detach from your instances as needed.
# Understanding Floating IPs in OpenStack

## What is a Floating IP?

A floating IP is a public, routable IP address that can be dynamically associated with and disassociated from virtual machines in an OpenStack cloud. It serves as a flexible public entry point to instances that reside on private networks.

## Why Do We Need Floating IPs?

When virtual machines are created in OpenStack, they are typically placed on private networks that are isolated from external access. Floating IPs bridge this gap by providing:

- **External Accessibility**: Enables SSH, HTTP, or other services to be reachable from the internet
- **Dynamic Assignment**: Can be moved between different VMs without changing application configurations
- **Security Isolation**: VMs remain on private networks while only exposing necessary services via floating IPs
- **Resource Optimization**: Public IPs are only consumed when actually needed, rather than permanently assigned to every VM

## How Floating IPs Work

The floating IP mechanism relies on OpenStack Neutron, the networking service:

1. A floating IP is allocated from a pool of public IP addresses associated with a `public` network
2. When associated with a VM, Neutron creates a NAT (Network Address Translation) mapping
3. Incoming traffic to the floating IP is translated and routed to the VM's private IP
4. Outgoing traffic from the VM appears to originate from the floating IP
5. The VM itself is unaware of the floating IP and only sees its private network interface

## Prerequisites for Using Floating IPs

Before you can use floating IPs, the following must be in place:

### OpenStack Configuration
- Neutron networking service must be enabled in the OpenStack deployment
- A `public` network must exist and be configured with an external router
- The `public` network must have an available pool of IP addresses
- Your project must have floating IP quota available

### Network Requirements
- Your VM must be connected to a `private` network that has a router gateway to the `public` network
- The private network must have DHCP enabled
- Security group rules must allow the desired traffic (e.g., SSH on port 22)

## Common Operations with Floating IPs

### Create a Floating IP
```bash
openstack floating ip create public

## List Available Floating IPs
```bash
openstack floating ip list

## Associate a Floating IP with a VM
```bash
openstack server add floating ip <vm-name> <floating-ip-address>

## Remove a Floating IP from a VM
```bash
openstack server remove floating ip <vm-name> <floating-ip-address>

## Delete a Floating IP
```bash
openstack floating ip delete <floating-ip-address>

## Typical Workflow
	1.Create a VM on a private network

	2.Allocate a floating IP from the public network

	3.Associate the floating IP with the VM

	4.Access the VM using the floating IP address

	5.If the VM is deleted, disassociate and delete the floating IP to release it back to the pool
