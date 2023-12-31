### Recommendation for OpenSourcing Components within the Prometheus X Ecosystem ###

#### Prometheus X Delivery Overview and Philosophy ####
Prometheus X aims to build and maintain open source building blocks for 
 * operating ecosystems with a focus on education and skills data and services
 * facilitating access and integration for participants of those ecosystems.

These building blocks are developed by partner companies recognized for their expertise. In some cases, the partner opens source existing components and services part of their existing commercial offering.

In other cases, the partner designs and develop a new component which they may use as part of their commercials offering.

Prometheus X intends to provide stewardship and funding of these buildings blocks, and does not intend to operate these services.

To build a thriving community, it is important that :
 * The building blocks can be used without intellectual property limitations (data or code) from the partner
 * Building blocks should only depend on open source software and data
 * Building blocks may provide integration with intellectual property (data or code) from the partner if an open alternative is provided and as long as it can provide value without said intellectual property

A component delivery by a partner should comprise:
 * The list of repositories to be provided on the Github Prometheus X account.

For each of these repositories, Prometheus X :
 * License under which the code is released (MIT or equivalent)
 * Name and description of services and components included in the repository,
 * Description of dependencies : technology stack, integrated API, datasets, required to operate the building block

This information can be provided as an architecture diagram as long as this information is represented.

It should be noted that delivery should also include Dockerfiles to build container images and docker-compose.yml (and/or K8S deployment descriptors, Helm charts etc) and instructions in markdown form to test and operate the building blocks.

The documentation may identify the partner in charge of the building block as a note in the README.md,  it should however recognize first and foremost that it is a Prometheus X building block, reinforcing brand recognition.

This documentation is to guide teams through releasing code on the `https://github.com/Prometheus-X-association/` GitHub account, ensuring best practices are maintained throughout the lifecycle of the project.

#### Release process diagram ####

![Release process diagram](ptx-architecture-diagram.png)

APIs are defined using OpenAPI or GraphQL.
Components are delivered as docker images, stored in a private registry operated by Prometheus X.
Orchestration is provided as a `docker-compose.yml` file for integration and interoperability tests. This file includes all dependencies. Components should not rely on managed components (such as a Relational Database Service).
CI/CD is provided by Github Actions. Sample actions are provided in this repository.
Functional and integration tests are described as  [K6](https://k6.io) scenarii.


**1. Releasing Code**

* **Repository Creation**: Contact Eric to create the appropriate repositories for your projects in  [GitHub Prometheus X Organization](https://github.com/Prometheus-X-association/).
Initially your projects will be hosted in private repositories. Include with your request the emails of your team members that will work on the project.

* **Licensing**: Code is to be released in open source. As such, it needs to be licensed. At this time, opensource licenses such as MIT, BSD, and GPL are supported. It is however a work in progress, and may be subject to change in the future.
Add license information in the LICENSE file.


* **README**: Start with a detailed README file. This file should include:
  - A brief description of the component.
  - Setup and installation instructions.
  - Contribution guidelines.
  - A link to more detailed documentation, if applicable.

**2. The 12-Factor Service**
The following are solid guidelines to release code that is easy to use, understand, scale and orchestrate.

**a. Codebase**: 
- Maintain a single codebase that can be used to deploy multiple instances. Each component should have its own repository under the `https://github.com/Prometheus-X-association/` GitHub organization.

**b. Dependencies**: 
- Declare all dependencies explicitly. Use tools like `package.json` for JavaScript projects or `requirements.txt` for Python projects.
- Avoid relying on system-wide packages which can lead to discrepancies between environments.

**c. Configuration**:
- Store configuration settings (e.g., database URLs, API keys) in environment variables.
- Do not hard-code configurations. This ensures security and flexibility across different deployment environments.

**d. Backing Services**:
- All services the app consumes (databases, messaging systems, caching layers) should be treated as attached resources, meaning they can be attached and detached by the app without code changes.

**e. Build, Release, Run**:
- Strictly separate the build, release, and run stages.
- Once a build has been compiled, it should be packaged with configurations for a release, and then run in the execution environment without changes.

**f. Processes**:
- Execute the app as one or more stateless processes. Ensure that all data that needs to persist is stored in a stateful backing service, like a database.
- This ensures that any data remains through processes' start and stops cycles.

**g. Port Binding**:
- Your component should be self-contained and not rely on runtime injection of a webserver into the execution environment to create a web-facing service.
- Essentially, it should be capable of binding to a port and listening for incoming requests.

**h. Concurrency**:
- Scale out your app horizontally, not vertically. This means adding more instances rather than beefing up a single instance's capacity.
- Consider using microservices for modular functionality.

**i. Disposability**:
- Processes should start quickly and shut down gracefully. This enables rapid scaling and release of new versions without affecting users.
- Handle unexpected terminations gracefully.

**j. Dev/Prod Parity**:
- Maintain as much parity between the development, staging, and production environments as possible. This reduces the number of bugs and issues found only in production.
- Use similar backing services across all environments.

**k. Logs**:
- Treat logs as event streams. Do not depend on the file system to store logs.
- Implement a logging framework that pushes logs to a centralized location where they can be monitored and analyzed.

**l. Admin Processes**:
- Run administrative/management tasks as one-off processes in the same environment as the app's regular long-running processes.
- These tasks can include database migrations or data analysis.


**3. Documentation**

* **Comprehensive**: Documentation should detail how to setup the service, 

* **API Documentation**: API should be documented through an OpenAPI specification document.

* **Tutorials and Use Cases**: Offer users guidance through tutorials or use case scenarios to showcase the capabilities of your component.

**4. Release Versioning**

Adopt [Semantic Versioning (SemVer)](https://semver.org/). This includes versioning in the format of `MAJOR.MINOR.PATCH`:
* `MAJOR` changes when you make incompatible API changes.
* `MINOR` changes when you add functionality in a backwards-compatible manner.
* `PATCH` changes when you make backwards-compatible bug fixes.

**5. Handling Support and contributions**

* **Issue Tracker**: Use GitHub's issue tracker. Encourage users to report bugs and request features here.

* **Code of Conduct**: We recommend adopting the [Contributor Covenant](https://www.contributor-covenant.org/version/2/1/code_of_conduct/code_of_conduct.md)

* **Contributing Guidelines**: Provide clear guidelines on how individuals can contribute to your project.

**6. Continuous Integration/Continuous Deployment (CI/CD)**

* **Automated Testing**: Every pull request or merge should automatically trigger a suite of tests to ensure code quality. Integrate with Github Actions for triggering tests. 

* **Building**: Integrate with Github Actions to automatically build artefacts.

* **Triggers**: For each push on the component repository :
  - on the main branch, a beta image is built and pushed to the Prometheux X registry.
  - with a tag in semver format, the image is built, tagged both with version and `latest` and pushed to the Prometheus X registry

* **Self-hosted runners**: Github Actions self-hosted runner capacity can be provided upon request.

**7. Orchestration**:

* **Monitoring and Performance**: It is recommended to provide a [Prometheus](https://prometheus.io) endpoint for all components.

* **Docker Containers**: Every component should be available as a Docker container to ensure easy setup and scalability. The Dockerfile should be part of the repository, ensuring anyone can build the container from the source.

* **Docker Compose**: Include a `docker-compose.yml` file for quick evaluation. This will allow users to spin up your component along with any dependent services with a single command.

* **Kubernetes**: Additionally provide deployment descriptors or a Helm Chart to enable quick deployment to production.

**Point of contact**

For all requests, questions and suggestions about this document, please contact Eric at [eric@escapevelocity.fr](eric@escapevelocity.fr)
