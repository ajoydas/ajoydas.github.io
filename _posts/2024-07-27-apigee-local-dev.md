---
title: "APIGEE: From Dashboard-Based Development to Local Development 🚀"
tags: [apigee, cloud, google cloud, development, api gateway]
style: border 
color: info
description:
---
<br/>
<img src="/assets/img/posts/apigee-cover.png" alt="APIGEE Cover" style="width: 100%; height: auto;"/>
<br/>

APIGEE, Google Cloud’s API proxy management SaaS, stands out as one of the most comprehensive and feature-rich API proxy solutions available.

I have recently completed the transition of an APIGEE project from a dashboard-based development to a local development setup. While the dashboard-based approach might seem more intuitive and user-friendly at first glance, the local development setup offers several crucial advantages that make it a superior choice for many use cases:
<br/><br/>

**Version Control 📁**

Local development allows the use of source code version control tools such as Git. This enables tracking of changes made by different developers, providing better collaboration and traceability. Additionally, it simplifies the process of creating and maintaining releases and sub-releases of API proxies in the cloud environment.
<br/><br/>

**Comprehensive Code Review Cycle 🔍**

With local development, you can implement a more robust code review process. Code reviews can be integrated into the development workflow, allowing for peer review and quality assurance before deploying changes to the cloud environment.
<br/><br/>

**Improved Deployment Cycle 🚀**

The local setup streamlines the deployment cycle by allowing developers to use automated scripts and CI/CD pipelines for consistent and reliable deployments. This automation reduces the risk of human error and accelerates the release process. For example, you can use Google cloud's CLI (aka "glcoud") to easily deploy to the cloud environments once the proxies are ready.
<br/><br/>

**Testing Capabilities 🧪**

Another key benefit is the ability to incorporate testing into the development workflow. Developers can write and run unit tests or integration tests locally, ensuring that the proxies behave as expected before they are deployed. For example, I have used Docker and Pytest to write testcases for the API proxies and Flask to write a mock server that replaces the target servers for better control over the test environment. 
<br/><br/>

**Parallel Development 🛠️**

Local development supports parallel development of API proxies alongside the APIs themselves. This allows teams to work on multiple features or fixes concurrently, reducing the overall development time and enabling faster iterations.
<br/><br/>

**Enhanced Tooling ⚙️**

The use of local development tools such as GitHub Copilot provides better code completion and suggestions, improving developer productivity and reducing the learning curve for new team members.
<br/><br/>

I think transitioning to a local development setup has significantly enhanced the development and deployment process for our API proxies, offering better control, improved collaboration, and a more efficient workflow.

If you have worked with APIGEE before, I would love to hear your thoughts and experiences with different development approaches. Thanks!

Feel free to share your thoughts on this [LinkedIn post](https://www.linkedin.com/posts/ajoy-das_overview-of-local-development-with-apigee-activity-7241768213372362753-SO4_?utm_source=share&utm_medium=member_desktop&rcm=ACoAACFoy40BiX6eAmWacHlefJGB6wKBjkuteKs).
<br/>