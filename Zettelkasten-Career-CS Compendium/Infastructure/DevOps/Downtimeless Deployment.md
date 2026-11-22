---
tags:
  - DevOps
  - "#LEAF"
---
Downtime-less deployment refers to a process of deploying new versions of software (or updates) to a system without causing any noticeable downtime or disruption to the users. The goal is to keep the application running smoothly while implementing changes in the background. This approach is crucial for high-availability applications where even short outages can negatively impact user experience or business operations.

There are several strategies to achieve downtime-less deployment:

1. **Blue-Green Deployment**:

   * In this approach, two identical environments are maintained: one (the "Blue" environment) runs the current version of the application, while the other (the "Green" environment) runs the new version.  
   * Traffic is initially routed to the Blue environment. Once the Green environment is fully tested and ready, the traffic is switched to it, making the Green environment live. The Blue environment becomes inactive but can be kept as a backup.  
2. **Canary Releases**:

   * This method involves gradually rolling out new versions of an application to a small subset of users first (the "canaries"). As long as the canary users don't experience issues, the release is progressively rolled out to more users.  
   * It allows teams to monitor the new version in production without impacting all users at once.  
3. **Rolling Deployment**:

   * This involves updating small batches of servers or instances at a time instead of updating everything simultaneously.  
   * As one batch gets updated, the rest of the system continues to serve users, and the deployment process progresses until the entire system is updated.  
4. **Feature Flags**:

   * New features or code changes are deployed to production but are hidden behind feature flags (also known as toggles). This way, the new code is live in production, but it's not visible or active to end users until the flag is toggled.  
   * This allows teams to deploy new code and test it in the live environment without users seeing any difference.  
5. **Database Migration Strategies**:

   * For database schema changes, using techniques like backward-compatible migrations (where the database can support both old and new versions of the application for a time) helps avoid downtime.  
   * This can be paired with the deployment methods above to ensure that database changes don’t cause disruptions.

Downtime-less deployments often require strong monitoring, automated testing, and well-thought-out rollback plans in case issues are encountered during the deployment process.

