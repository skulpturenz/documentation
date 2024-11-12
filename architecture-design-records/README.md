# Architecture design records

Our architectural decisions are documented loosely based on the [`arc42`](https://arc42.org/overview) template

Each project should be:

- Self contained - it should include necessary configurations to deploy to production
- Easy to setup - environments treated as ephemeral so that we are able to setup and teardown projects with ease
- Secure - dependencies up to date and access to services should be isolated at a network level and easy to cut off access to

To achieve these goals, the standard setup utilises:

- Self contained: infrastructure deployed using IaC, we use [Pulumi](https://www.pulumi.com)
- Easy to setup: [devcontainers](https://containers.dev) to set up development environments and Dockerfiles to build production environments
- Secure and scalable: [dependabot](https://github.com/dependabot) for keeping dependencies up to date and services should be isolated using Docker with a reverse proxy in front which is exposed publicly. Network requests should only be allowed from Cloudflare. Docker Swarm for scaling up, Kubernetes was not chosen because of its operational complexity both for deployment to production and local testing
