# AWS EFS CSI Driver Integration with EKS Cluster

## AWS FS CSI Driver
The EFS CSI (Container Storage Interface) Driver enables Kubernetes to seamlessly manage Amazon EFS (Elastic File System) volumes. It allows Kubernetes workloads to dynamically provision and use EFS volumes as PersistentVolumes (PVs) and PersistentVolumeClaims (PVCs). The driver supports features like volume encryption, access control, and lifecycle management of EFS volumes within Kubernetes clusters, ensuring reliable and scalable storage solutions for containerized applications.

## Description
This repository contains Terraform configurations to integrate the **AWS EFS CSI Driver** with an existing AWS EKS cluster. It includes the setup of necessary IAM roles, policies, and attachments, encryption for EFS volumes, and the association of the EKS cluster with the CSI Driver role. Additionally, a Helm chart is deployed to facilitate the dynamic management of EFS volumes within Kubernetes.

## Key Features:
Seamless management of Amazon EFS volumes in Kubernetes using the EFS CSI Driver.
Configuration of IAM roles and policies for secure operation.
Support for encrypted EFS volumes.
Deployment of the AWS EFS CSI Driver addon in the EKS cluster.
Demonstrative deployment of Kubernetes StorageClass for EFS with the CSI driver.

## Prerequisites:
An existing EKS cluster.

## Resources Configured:
- **data "tls_certificate" "eks"** - Fetches the TLS certificate for the OIDC issuer URL of the AWS EKS cluster.
- **resource "aws_iam_openid_connect_provider" "eks"** - Registers the OIDC provider for the AWS EKS cluster, validating clients with the specified TLS certificate fingerprint.
- **resource "aws_efs_file_system" "eks"** - Defines an Amazon EFS file system with encryption enabled and general-purpose performance mode.
- **resource "aws_efs_mount_target" "zone_a"** - Creates a mount target in subnet A for the AWS EFS file system, allowing access from the AWS EKS cluster's security group.
- **resource "aws_efs_mount_target" "zone_b"** - Creates a mount target in subnet B for the AWS EFS file system, similarly allowing access from the AWS EKS cluster's security group.
- **data "aws_iam_policy_document" "efs_csi_driver"** - Defines an IAM policy document that allows the EFS CSI Driver to assume roles under specific conditions using OIDC.
- **resource "aws_iam_role" "efs_csi_driver"** - Creates an IAM role for the EFS CSI Driver, enabling it to perform actions specified in the associated policy document.
- **resource "aws_iam_role_policy_attachment" "efs_csi_driver"** - Attaches the AmazonEFSCSIDriverPolicy to the IAM role, granting necessary permissions for the EFS CSI Driver.
- **resource "helm_release" "efs_csi_driver"** - Deploys the AWS EFS CSI Driver as a Helm chart named "aws-efs-csi-driver" in the kube-system namespace, configuring it with specific service account annotations and dependencies.
- **data "aws_eks_cluster" "eks_v2"** - Retrieves information about the existing AWS EKS cluster.
- **data "aws_eks_cluster_auth" "eks_v2"** - Fetches authentication details (like token and certificate) for accessing the AWS EKS cluster.
- **provider "kubernetes"** - Configures access to the Kubernetes cluster using authentication details fetched from the AWS EKS cluster.
- **resource "kubernetes_storage_class_v1" "efs"** - Defines a Kubernetes StorageClass named "efs" using the AWS EFS CSI Driver, specifying parameters for dynamic provisioning and access permissions.

This repository simplifies the management of EFS volumes within Kubernetes clusters, enhancing capabilities and security in your AWS EKS environment. 

The Kubernetes resources provided in this repository are for demonstrating possible configurations with EFS.