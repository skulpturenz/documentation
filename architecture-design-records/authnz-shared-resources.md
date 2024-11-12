**DRAFT**

Identity services are a fundamental requirement of every modern web application. 

Self-hosting is not a necessity, there are numerous Identity as a Service solutions available such as Auth0, Firebase, Okta, Cognito and Ory. As for self hosted solutions, the most prominent ones include: Keycloak, IdentityServer, Authentik and Authelia.

Goals:
1. Data ownership
2. Scalability and cost: ability to scale with a relatively flat cost curve
3. Integration
4. Multi-tenancy: different realms for different apps
5. Stability 

In light of these requirements, Keycloak was chosen because:
1. Stability: a CNCF incubation project and has been under active development as an OSS project for the past decade
2. Data ownership
3. Customisability: keycloakify for user facing customisation, worst case forking keycloakify or FreeMarker

Sessions need to be sticky and NGINX caches IP addresses of services if we don’t refer to the Docker Swarm LB so NGINX has to be restarted each time = deployments are not zero down time

`kamal-proxy` available but it does not do load balancing (on the TODO)

authnz-service-proxy to put in front of anything that needs to be authenticated. function: keep redirecting to `authnz` until authenticated.
