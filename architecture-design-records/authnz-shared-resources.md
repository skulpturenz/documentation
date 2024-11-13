**DRAFT**

## Introduction & goals
### Requirements
Identity services are a fundamental requirement of every modern web application. 

Self-hosting is not a necessity, there are numerous Identity as a Service solutions available such as Auth0, Firebase, Okta, Cognito and Ory. As for self hosted solutions, the most prominent ones include: Keycloak, IdentityServer, Authentik and Authelia.

A username and password will suffice in most scenarios but being able to integrate with other platforms and not having to expend effort on setting up authentication and authorization services is also immensely beneficial which is the main motivation for the service. Using a battle-tested IdP also brings with it far greater security guarantees compared to custom implementations and also promotes standardization across different languages and frameworks.

### Quality goals
1. Data ownership: we should be able to provide strong guarantees relating to privacy and there should be a straightforward way to export our data off the IdP and import it into another context
2. Scalability and cost: ability to scale with a relatively flat cost curve
3. Integration: we should be able to integrate with other platforms such as Google, Facebook, and Twitter and similarly we should be able to login to other platforms using our IdP
4. Multi-tenancy: support for separation between different products and environments
5. Stability: an unstable IdP means that users have to regularly reestablish their profiles and we would have to accomodate mechanisms to reestablish links to their existing data. Any form of data migration and in particular those which relate to identity are a difficult task and introduces signficant risk and complexity

In light of these requirements, Keycloak was chosen because:
1. Data ownership: data is stored on managed database cluster with daily backups allowing us to access the data anytime we need to with ease if the need arises
2. Scalability and cost: self-hosting on a VM allows us to scale efficiently, not having to work about the number of active users which is the primary measure for tiers with IaaS solutions
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

1. Deployments are done using Docker Swarm with a publicly accessible reverse proxy (NGINX) sitting in front
2. Keycloak version is stored in GitHub variables so that we can easily perform updates, upgrading major versions does not require manual intervention. In our context the important changes we need to track are the configuration changes and not so much the version of Keycloak
3. VM hosted on GCP and only allows requests from Cloudflare
4. Database does not allow connections except from whitelisted addresses and is backed up daily
5. An optimized and customized image with health and metrics enabled is built for each deployment and versioned by either `master`, `staging` or `dev`
6. Customized using [`keycloakify`](http://keycloakify.dev) which is built on [React](https://react.dev)
7. Services to ship logs, metrics and uptime information from Docker and VM to Elastic have been configured (but not actively used)

## Deployment view
TODO

## Concepts
TODO

## Architecture decisions
TODO
- there is only a production service running for the moment
- upgrades are not zero downtime

## Quality
TODO
- zero down time is nice to have but requires migrating to a different proxy, not important for the moment


Sessions need to be sticky and NGINX caches IP addresses of services if we don’t refer to the Docker Swarm LB so NGINX has to be restarted each time = deployments are not zero down time

`kamal-proxy` available but it does not do load balancing (on the TODO)

authnz-service-proxy to put in front of anything that needs to be authenticated. function: keep redirecting to `authnz` until authenticated.
