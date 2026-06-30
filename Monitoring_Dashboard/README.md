# Monitoring Dashboard
Tasks: Connect App Service / Functions to App Insights, create metrics dashboard, configure alerts
Document monitoring setup and alerts


## Objective

Design and implement an Azure native dashboard integrating

	• Application Insights
	• Log Analytics
	• Azure Monitor Metrics
	• Azure Monitor Alerts
	• Cost Management
  
The goal is to provide a single pane of glass for operational visibility
	

## Problem Statement

The application team lacked consolidated view of

	• Application performance
	• Platform metrics
	• HTTP errors
	• Resource Utilization
	• Cost visibility

Troubleshooting required navigating multiple Azure blades.

This project delivers a unified monitoring dashboard to improve observability and operational readiness.


## Architecture Overview

• App Service emits platform metrics

• Application Insights collects the application telemetry

• Diagnostic Settings sends platform logs to Log Analytics

• Azure Monitor Alerts detect anomalies

• Azure Dashboard visualizes metrics, logs and costs

• Flow:
<img width="4885" height="2656" alt="image" src="https://github.com/user-attachments/assets/e4df26a1-d7d8-49b5-9f20-c9b131782a73" />



## Components Used

• Web App   (Linux): app-mini-project5

• Application Insights

• Log Analytics Workspace

• Monitor

• Monitor Alerts

• Cost Management

• Dashboard


## Design Decisions

Applications Insights

	• Chosen for unified logging and KQL Support

Log Analytic Workspace

	• Log Analytic Workspace was selected because it provides structured log storage, powerful KQL querying and seamless integration with Application Insights and Azure Monitor

Azure Monitor Metrics for Platform Observability
	
  • Azure Monitor Metrics were implemented to track platform-level signals such as Requests, Response Time, Memory Working Set, and HTTP Server Errors. These metrics provide real-time visibility into application performance and platform behavior.
	
Azure Dashboard for unified visualization

	• Azure Dashboard was used to consolidate metrics, logs, and costs into a single operational view. This provides a reusable shareable, and role-based monitoring surface for the application team.


## Implementation Steps


Step 1: Create Resource Group

	• Created rg-mini-project5
  
Step 2: Create Application Insights

	• Linked to law-mini-project5
  
Step 3: Connect App Service to Application Insights

	• Applied connection string and verified telemetry ingestion.
  
Step 4: Configure Diagnostic Settings

	• Enabled:
		• AppServiceHTTPLogs
		• AppServiceConsoleLogs
    
Step 5: Build Monitoring Dashboard (Metrics)

	• Pinned:
		• Requests
		• Response Time
		• Memory Working Set
		• Http Server Errors
		• Thread Count
Step 6: Add Cost Tile

	• Resource Group → Cost Management → Cost Analysis
  
Step 7: Alerts Implemented

	• All alerts were created individually using Azure Monitor → Alerts → Create Alert Rule, scoped to: app-mini-project5

Resource Type: App Service
Signal Type: Metrics
State: Enabled
Severity: 2 – Warning

	• Alert 1 — High Memory Usage
		• Signal: MemoryWorkingSet
		• Condition: MemoryWorkingSet > 1000000000
		• Severity: 2 – Warning
		• Name: High-Memory-Usage

	• Alert 2 — High 5xx Errors
		• Signal: Http5xx (Http Server Errors)
		• Condition: Http5xx > 10
		• Severity: 2 – Warning
		• Name: Http Server Errors

	• Alert 3 — Memory Usage (Duplicate Rule)
	• (Azure created a second rule with same signal — kept for completeness)
		• Signal: MemoryWorkingSet
		• Condition: MemoryWorkingSet > 1000000000
		• Severity: 2 – Warning
		• Name: Memory Usage
    
	• Alert 4 — High Request Volume
		• Signal: Requests
		• Condition: Requests > 500
		• Severity: 2 – Warning
		• Name: Requests

	• Alert 5 — Slow Response Time
		• Signal: HttpResponseTime (Response Time)
		• Condition: HttpResponseTime > 2
		• Severity: 2 – Warning
		• Name: Response Time
	

## Challenges & Resolutions

  ### Challenge 1: Missing CPU Metrics
    Issue: No “Percentage CPU” or “Processor Time” signals
    Cause: Linux App Service plan does not expose CPU metrics
    Resolution: Used available metrics (Memory, Requests, Response Time, Http5xx)
  ### Challenge 2: No Telemetry in Log Analytics
    Issue: Empty tables
    Cause: Diagnostic settings not configured
    Resolution: Enabled platform logs → Data appeared
  ### Challenge 3: No “Pin to Dashboard” in Cost Analysis
    Issue: Subscription-level Cost Analysis removed pinning
    Cause: New 2026 Cost Management UI
    Resolution: Used Resource Group → Cost Analysis


## Security Considerations

Application Insights

  • Telemetry contains sensitive application data
  • Enforced RBAC at resource level; restricted access to engineering roles only
  
Log Analytic workspace

  • Stores platform, http, console logs
	• Used RBAC, Microsoft Entra ID authentication; no shared keys, retention controlled.
  
App Service

  • Protects Application endpoints and diagnostic channels
	• HTTPS enforced, restricted through Azure RBAC
  
Azure Monitor Alerts

  • Alerts may expose operational details
  • Action groups restricted to authorized teams
	• RBAC applied on alert rules
  
Azure Dashboard

  • Dashboard displays sensitive metrics and logs
	• Shared only with required roles
	• No public access RBAC enforced.


## Cost Estimation

### Application Insights

  • Data ingestion	
  
  • Low for small workloads

### Log Analytics

  • Log ingestion	
  
  • Based on GB/day

### Azure Monitor

  • Metrics	   
  
  • Standard metrics free
  
### Alerts	 

  • Rule execution	
  
  • Minimal cost

### Dashboard

  • Free
  
  • No cost

Estimated monthly cost: Low (< $10 depending on ingestion)
