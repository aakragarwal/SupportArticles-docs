---
title: Inaccurate VM availability reported during a VM restart
description: Understand why inaccurate virtual machine (VM) availability is reported during a virtual machine restart in Azure.
ms.date: 5/4/2023
author: DennisLee-DennisLee
ms.author: v-dele
editor: v-jsitser
ms.reviewer: macla, azurevmcptcic
ms.service: virtual-machines
ms.subservice: vm-health
---
# Inaccurate VM availability reported during a VM restart

This article discusses how an inaccurate virtual machine (VM) availability status might be reported during a VM restart that's issued by an authorized user or caused by a Guest OS crash. The inaccurate VM availability status might be published to the following Microsoft Azure resources:

- Azure Monitor
- Azure Resource Graph
- Azure Resource Health

## Symptoms

Azure continues to report a VM as available during certain situations, such as an authorized user restarting the VM, or a crash that is initiated within the Guest OS and causes a VM restart. In these kinds of situations, the following things occur:

- The [VM availability metric in Monitor](/azure/azure-monitor/vm/tutorial-monitor-vm-alert-availability?context=%2Fazure%2Fvirtual-machines%2Fcontext%2Fcontext#view-the-vm-availability-metric) continues to emit a value of **1**. This indicates that the VM is available.

- The [VM availability information in Resource Graph](/azure/virtual-machines/resource-graph-availability) emits a VM availability status of **Available**, and it shows no associated resource annotations.

- The [Resource Health status](/azure/service-health/resource-health-overview#health-status) displays the VM status as **Available**.

## Cause

When the VM is restarted, a 15-second delay might occur while our underlying monitoring service gathers the health status of the VM. This interval is possibly followed by up to three minutes of delay in emitting the health status externally. If a VM restart is initiated and completed within the time window of the accrued delay, the various Azure resources don't indicate any change in health.

## Workaround

If you see that the VM has a status of **Available**, make sure that the status remains unchanged for three minutes and 15 seconds before you proceed to use the VM.

> [!NOTE]  
> We're prioritizing infrastructure improvements to minimize the delay in detection and notification. This investment is part of [Project Flash](https://azure.microsoft.com/blog/advancing-azure-virtual-machine-availability-monitoring-with-project-flash/). This project is intended to maintain highly accurate and actionable VM availability information.

[!INCLUDE [Azure Help Support](../../includes/azure-help-support.md)]
