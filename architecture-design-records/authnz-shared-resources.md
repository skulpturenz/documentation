## Introduction & goals
### Requirements
Identity services are a fundamental requirement of every modern web application. 

Self-hosting is not a necessity, there are numerous Identity as a Service solutions available such as Auth0, Firebase, Okta, Cognito and Ory. As for self hosted solutions, the most prominent ones include: Keycloak, IdentityServer, Authentik and Authelia.

A username and password will suffice in most scenarios but being able to integrate with other platforms, provide multi-factor authentication and not having to expend effort on setting up authentication and authorization services is also immensely beneficial which is the main motivation for the service. Using a battle-tested IdP also brings with it far greater security guarantees compared to custom implementations and also promotes standardization across different languages and frameworks.

### Quality goals
1. Data ownership: we should be able to provide strong guarantees relating to privacy and there should be a straightforward way to export our data off the IdP and import it into another context
2. Scalability and cost: ability to scale with a relatively flat cost curve
3. Integration: we should be able to integrate with other platforms such as Google, Facebook, and Twitter and similarly we should be able to login to other platforms using our IdP
4. Multi-tenancy: support for separation between different products and environments
5. Stability: an unstable IdP means that users have to regularly reestablish their profiles and we would have to accomodate mechanisms to reestablish links to their existing data. Any form of data migration and in particular those which relate to identity are a difficult task and introduces signficant risk and complexity

In light of these requirements, Keycloak was chosen because:
1. Data ownership: data is stored on managed database cluster with daily backups allowing us to access the data anytime we need to with ease if the need arises
2. Scalability and cost: self-hosting on a VM allows us to scale efficiently, not having to worry about the number of active users which is the primary measure for tiers with IaaS solutions
3. Integration: support for SSO, SAML and client libraries for a wide variety of languages
4. Multi-tenancy: ability to create different spaces with no limit to the amount
5. Stability: a CNCF incubation project and has been under active development as an OSS project for the past decade

### Stakeholders
- [Naveen Mathew](mailto:naveen@skulpture.xyz)

## Constraints
- A language and framework agnostic approach to authentication and authorization
- Support for SSO and SAML
- Client facing aspects should be customizable
- Easy to scale
- Easy to integrate with
- Easy to export data out
- Easy to configure
- Well documented and active community

## Solution strategy
- Deployments are done using Docker Swarm with a publicly accessible reverse proxy (NGINX) sitting in front
- Keycloak version is stored in GitHub variables so that we can easily perform updates, upgrading major versions does not require manual intervention. In our context the important changes we need to track are the configuration changes and not so much the version of Keycloak
- VM hosted on GCP and only allows requests from Cloudflare
- Database does not allow connections except from whitelisted addresses and is backed up daily
- An optimized and customized image with health and metrics enabled is built for each deployment and versioned by either `master`, `staging` or `dev`
- Customized using [`keycloakify`](http://keycloakify.dev) which is built on [React](https://react.dev)
- Services to ship logs, metrics and uptime information from Docker and VM to Elastic have been configured (but not actively used)
- `staging` amounts to a patch (and preview) release and production a minor or major release as applicable. The idea is to run staging for a few months so that issues have enough time to surface and only then make a production release (which is not preview)

## Deployment view
![image](https://github.com/user-attachments/assets/eb2f987d-43dc-4cf8-96a0-11cd99f4716b)

## Concepts
- Identity provider (IdP): https://www.cloudflare.com/en-gb/learning/access-management/what-is-an-identity-provider/
- OAuth: https://www.cloudflare.com/en-gb/learning/access-management/what-is-oauth/
- Single sign on (SSO): https://www.cloudflare.com/en-gb/learning/access-management/what-is-sso/
- Security Assertion Markup Language (SAML): https://www.cloudflare.com/en-gb/learning/access-management/what-is-saml/
- Identity and access management (IAM): https://www.cloudflare.com/en-gb/learning/access-management/what-is-identity-and-access-management/
- Authorization vs authentication: https://www.cloudflare.com/en-gb/learning/access-management/what-is-access-control/

## Architecture decisions
- 14/11/24
   - There is only one VM provisioned for services due to cost. Deployments to `staging` and `dev` environments will take down production. A production release has not been made yet only staging

## Quality
- Find a different proxy so that we can do zero-downtime deployments

## Risks & technical debt
- At the moment NGINX refers to each Keycloak node specifically instead of the Docker Swarm LB and since NGINX caches IP addresses of services, it has to be restarted each deployment. There are ways to force NGINX to resolve DNS each time (see: https://serverfault.com/a/1098464) but it might be better to find a proxy which is better suited instead of NGINX
- NGINX uses IP hashing so that a request goes back to the same node, sticky sessions are a paid feature, see: https://docs.nginx.com/nginx/admin-guide/load-balancer/http-load-balancer/#enabling-session-persistence. There are proxies available which have sticky sessions out of the box
