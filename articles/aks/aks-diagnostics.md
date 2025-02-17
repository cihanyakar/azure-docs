---
title: Azure Kubernetes Service (AKS) Diagnostics Overview
description: Learn about self-diagnosing clusters in Azure Kubernetes Service.
ms.topic: conceptual
ms.date: 11/15/2022
---

# Azure Kubernetes Service Diagnostics (preview) overview

Troubleshooting Azure Kubernetes Service (AKS) cluster issues plays an important role in maintaining your cluster, especially if your cluster is running mission-critical workloads. AKS Diagnostics (preview) is an intelligent, self-diagnostic experience that:

* Helps you identify and resolve problems in your cluster.
* Is cloud-native.
* Requires no extra configuration or billing cost.

[!INCLUDE [preview features callout](./includes/preview/preview-callout.md)]

## Open AKS Diagnostics

To access AKS Diagnostics:

1. Sign in to the [Azure portal](https://portal.azure.com)
1. From **All services** in the Azure portal, select **Kubernetes Service**.
1. Select **Diagnose and solve problems** in the left navigation, which opens AKS Diagnostics.
1. Choose a category that best describes the issue of your cluster, like _Cluster Node Issues_, by:

    * Using the keywords in the homepage tile.
    * Typing a keyword that best describes your issue in the search bar.

![Homepage](./media/concepts-diagnostics/aks-diagnostics-homepage.png)

## View a diagnostic report

After you click on a category, you can view a diagnostic report specific to your cluster. Diagnostic reports intelligently call out any issues in your cluster with status icons. You can drill down on each topic by clicking **More Info** to see a detailed description of:

* Issues
* Recommended actions
* Links to helpful docs
* Related-metrics
* Logging data 

Diagnostic reports generate based on the current state of your cluster after running various checks. They can be useful for pinpointing the problem of your cluster and understanding next steps to resolve the issue.

![Diagnostic Report](./media/concepts-diagnostics/diagnostic-report.png)

![Expanded Diagnostic Report](./media/concepts-diagnostics/node-issues.png)

## Cluster insights

The following diagnostic checks are available in **Cluster Insights**.

### Cluster Node Issues

Cluster Node Issues checks for node-related issues that cause your cluster to behave unexpectedly. Specifically:

- Node readiness issues
- Node failures
- Insufficient resources
- Node missing IP configuration
- Node CNI failures
- Node not found
- Node power off
- Node authentication failure
- Node kube-proxy stale

### Create, read, update & delete (CRUD) operations

CRUD Operations checks for any CRUD operations that cause issues in your cluster. Specifically:

- In-use subnet delete operation error
- Network security group delete operation error
- In-use route table delete operation error
- Referenced resource provisioning error
- Public IP address delete operation error
- Deployment failure due to deployment quota
- Operation error due to organization policy
- Missing subscription registration
- VM extension provisioning error
- Subnet capacity
- Quota exceeded error

### Identity and security management

Identity and Security Management detects authentication and authorization errors that prevent communication to your cluster. Specifically,

- Node authorization failures
- 401 errors
- 403 errors

## Next steps

* Collect logs to help you further troubleshoot your cluster issues by using [AKS Periscope](https://aka.ms/aksperiscope).

* Read the [triage practices section](/azure/architecture/operator-guides/aks/aks-triage-practices) of the AKS day-2 operations guide.

* Post your questions or feedback at [UserVoice](https://feedback.azure.com/d365community/forum/aabe212a-f724-ec11-b6e6-000d3a4f0da0) by adding "[Diag]" in the title.
