**DRAFT**

## Introduction & goals
### Requirements
`landing` is the landing page for https://skulpture.xyz and it comprises of a lambda function deployed to a GCP VM and a frontend which is deployed to Cloudflare Pages. 

Landing pages are the first point of contact for someone who is new to an organization and the main motivation for them is to introduce the organization and increase conversions. As such, landing pages are usually simple in terms of business logic and API implementation, they're usually frontend focused projects.

There are many website builders available for the frontend such as Squarespace and Wix which allows for WYSIWYG editing of the frontend. In this case [Astro](https://astro.build) was chosen instead primarily due to familiarity which eases maintenance and using a code first approach also makes versioning changes simpler. As for provisioning a VM instead of a lambda, this was done mainly to simplify local testing and eliminate the risk of unexpected costs. 

Enquiries are stored in a Google Sheet and any associated files are stored on Google Drive so that it is easy to work with.

### Quality goals
- Maintainability: It should be simple to adapt and update designs
- Scalability and cost: ability to scale with a relatively flat cost curve
- Efficient: Expected and should be able to handle a potentially large amount of traffic
- Accessible: Data should be easy to access, and work with

### Stakeholders
- [Naveen Mathew](mailto:naveen@skulpture.xyz)

### Constraints
- Easy to make design changes
- Easy to test API locally
- Easy to access enquiries

### Solution strategy
- Deployments are done using Docker Swarm with a publicly accessible reverse proxy (NGINX) sitting in front
- Frontend is built as a static site and deployed to Cloudflare Pages and proxied by API which is proxied by a publicly accessible reverse proxy
  - Deploying to Cloudflare pages allows for viewing previews of changes
  - Proxying Cloudflare Pages instead of adding a DNS record for the pages site allows the API and frontend to be accessible under the same domain name
- API integrates with `telemetry` using OpenTelemetry
- API integrates with Google Cloud Platform using application default credentials and there are additional authorization scopes which need to be granted to the service account linked to the VM to allow access to Google Drive and Google Sheets

### Deployment view
![image](https://github.com/user-attachments/assets/12f0849e-d2ca-4a22-9c3c-79eb3a5697fc)

### Architecture decisions
- 17/11/24
  - There is only one VM provisioned for services due to cost. Both `master` and `dev` are deployed to the same VM and NGINX is used to route traffic appropriately
  - To keep the deploy scripts the same across `shared-resources` and `landing`, NGINX is restarted each time a deployment happens which means deployments are not zero downtime
