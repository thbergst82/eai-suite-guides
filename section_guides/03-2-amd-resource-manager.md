# 2. AMD Resource Manager

## Overview

AMD Resource Manager provides platform administrators with tools to oversee and control the platform's computational resources and user access. Key capabilities include cluster management, monitoring, and maintaining teams' access to computational resources.

![AMD Resource Manager Dashboard](../images/03-resource-manager/01-dashboard-overview.png)

### Core Concepts

- **Dashboard**: High-level overview of your clusters, resource allocations, and basic utilization statistics
- **Projects**: Create and manage projects, which organize and isolate work within your system. Project settings include user membership, secrets, storage, and quotas
- **Quota**: Guaranteed resource quota for a project to ensure fair resource allocation
- **Storage**: Configurations providing credentials and connectivity information for storage (e.g., S3)
- **Secrets**: Secure information such as API keys or credentials assigned to projects
- **Users**: Manage users, their roles, and their project membership

------------------------------------------------------------------------

## Monitoring Workloads and Resource Utilization


1. Navigate to the Project’s Details page by selecting your project, “XXX”, from the Projects page. Your previously deployed AIM should be displayed on the dashboard.

2. To view the resource allocation across the entire cluster rather than just a single project, you can monitor workload utilization for all clusters on the Clusters page. As you can see, in our case there are now multiple running workloads in total. Please note, that this depends on the actual resource usage, and you may see a different amount of running workloads.

3. Clicking on a specific cluster, such as the demo-cluster, displays the Cluster Details page. This page provides quota and utilization information for the entire cluster and all associated projects.

4. In addition, by navigating to the Dashboard page you can view high-level quota and utilization information for your projects across all clusters, along with live widgets displaying GPU utilization information.

