# Amazon QuickSight <!-- omit in toc -->

## Contents <!-- omit in toc -->

- [1. Introduction](#1-introduction)
- [2. Dashboard \& Analysis](#2-dashboard--analysis)

# 1. Introduction

- Serverless machine learning-powered business intelligence service to create interactive dashboards.
- Fast, automatically scalable, embeddable, with per-session pricing.
- **Use cases**
  - Business analytics.
  - Building visualizations.
  - Perform ad-hoc analysis.
  - Get business insights using data.
- Integrated with RDS, Aurora, [Amazon Athena](Amazon%20Athena.md), [Amazon Redshift](Amazon%20Redshift.md), [Amazon S3](/Storage/Amazon%20S3.md)...
- **In-memory computation using SPICE** engine if data is imported into QuickSight.
- Enterprise edition: Possibility to setup **Column-Level security (CLS)**.

# 2. Dashboard & Analysis

- Define Users (standard versions) and Groups (enterprise version)
  - These users & groups only exist within QuickSight, not IAM !!
- A dashboard...
  - Is a read-only snapshot of an analysis that you can share.
  - Preserves the configuration of the analysis (filtering, parameters, controls, sort).
- **You can share the analysis or the dashboard with Users or Groups.**
- To share a dashboard, you must first publish it.
- Users who see the dashboard can also see the underlying data.
