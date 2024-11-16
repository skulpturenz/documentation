**DRAFT**

## Introduction & goals
### Requirements
`landing` is the landing page for https://skulpture.xyz and it comprises of a lambda function deployed to a GCP VM and a frontend which is deployed to Cloudflare Pages. 

Landing pages are the first point of contact for someone who is new to an organization and the main motivation for them is to introduce the organization and increase conversions. As such, landing pages are usually simple in terms of business logic, they're usually frontend focused projects and any associated API would be to primarily process forms and persist their data or integrate with other systems.

There are many website builders available for the frontend such as Squarespace and Wix which allows for WYSIWYG editing of the frontend. In this case [Astro](https://astro.build) was chosen instead primarily due to familiarity which eases maintenance and the ability to version changes.

### Quality goals
- Maintainability: It should be simple to adapt and update designs
- Efficient: Expected and should be able to handle a potentially large amount of traffic

### Stakeholders
- [Naveen Mathew](mailto:naveen@skulpture.xyz)

setup:
- lambda to dump to gsheets and cf pages proxied by go
- cf pages proxy in go instead of nginx so that nginx config stays simple and go proxy not too dificult to setup
