# Write-up: VM vs App Service for Article CMS

## Analyze, choose, and justify the appropriate resource option for deploying the app.

### Virtual Machine (VM)

**Cost:** VMs are generally more expensive than App Services for small-scale apps. Even the cheapest option (Standard B1ls) incurs costs 24/7 unless manually deallocated. You also pay for OS-level maintenance and any additional software installations.

**Scalability:** VMs offer full control over scaling, but horizontal scaling requires manual configuration of load balancers and scale sets. This is more complex than App Service's built-in auto-scaling features.

**Availability:** VMs can be highly available with proper configuration (availability sets, zones), but achieving high availability requires significant manual effort and additional cost.

**Workflow:** Deploying a Flask app on a VM requires manual steps: setting up nginx, installing Python dependencies, configuring SSL, managing system services, and handling OS updates. CI/CD pipelines can be configured but require more setup work.

---

### App Service

**Cost:** App Service offers a Free F1 tier for small applications, making it significantly cheaper for development and small-scale deployments. Costs scale predictably with usage tiers.

**Scalability:** App Service has built-in auto-scaling capabilities. You can scale up (more powerful hardware) or scale out (more instances) with just a few clicks in the Azure Portal, with no infrastructure management needed.

**Availability:** App Service provides built-in high availability with a 99.95% SLA on paid tiers. Azure manages the underlying infrastructure, OS patches, and restarts automatically.

**Workflow:** Deployment is streamlined via GitHub Actions CI/CD integration. Every push to the main branch automatically deploys to Azure. No server management, no SSH, no nginx configuration required.

---

### My Choice: App Service

I chose **App Service** to deploy the Article CMS for the following reasons:

1. The App Service Free F1 tier eliminates infrastructure costs entirely for this project, whereas a VM would incur compute costs even when idle.
2. The built-in GitHub Actions CI/CD integration means any code update is automatically deployed without manual SSH or file transfer steps, which greatly simplifies the development workflow.
3. App Service handles all OS-level maintenance, security patches, and uptime automatically, whereas a VM would require me to monitor and manage these concerns myself, introducing unnecessary risk and overhead for a simple CMS application.

---

## Assess app changes that would change your decision.

I would reconsider switching to a VM if the application requirements grew in the following ways:

1. **Custom system dependencies:** If the app required specific OS-level libraries, custom runtime versions, or specialized software (e.g., a particular database driver or GPU support) that is not supported in Azure App Service's managed environment, a VM would be necessary to provide that level of control.
2. **High-volume compute workloads:** If the CMS expanded to include resource-intensive background processing, video transcoding, or machine learning inference, the App Service pricing tiers might become cost-prohibitive compared to a right-sized VM.
3. **Networking and security requirements:** If the app needed to be placed inside a private virtual network with strict firewall rules, custom routing, or compliance requirements that demand full infrastructure control, a VM with a custom network configuration would be more appropriate than a managed App Service environment.