# Introduction

Terrakube is an open source collaboration platform for running remote infrastructure as code operations using Terraform or OpenTofu that aims to be a complete replacement for close source tools like Terraform Enterprise, Scalr or Env0.

The platform can be easily installed in any kubernetes cluster, you can easily customize the platform using other open source tools already available for terraform (Example: terratag, infracost, terrascan, etc) and you can integrate the tool with different authentication providers like Azure Active Directory, Google Cloud Identity, Github, Gitlab or any other provider supported by [Dex](https://dexidp.io/docs/connectors/).

Terrakube provides different features like the following:

* Terraform module protocol:
  * This allows Terrakube to expose an open source terraform registry that is protected and is private by default using different Dex [connectors](https://dexidp.io/docs/connectors/) for the authentication with different providers.
* Terraform provider protocol:
  * This allows Terrakube to expose an open source terraform registry where you can have create your own private terraform providers or create a mirror of the current terraform providers available in the public Hashicorp registry.
* Handle you infrastructure as code using Organization like closed source tools like Terraform Enterprise and Scalr.
* Easily extended by default integrating with many open source tools available today for terraform CLI.
* Support to run the platform using any **RDBMS** supported by [liquibase](https://www.liquibase.org) (mysql, postgres, oracle, sql server, etc).
* Plugins support to extends functionality using different cloud providers and open source tools using the Terrakube Configuration Languague (TCL).
* Easily extends the platform using common technologies like react js, java, spring, elide, liquibase, groovy, bash, go.
* Native integration with the most popular VCS like:
  * Github.
  * Azure DevOps
  * Bitbucket
  * Gitlab
* Support for SSH key (RSA and ED25519)
* Handle Personal Access Token
* Extends and create custom flows when running terraform remote operations with custom logics like:
  * Approvals
  * Budget reviews
  * Security reviews
  * Custom logic that you can build with groovy and bash scripts



### Sponsors

| Company                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   | Service                                                |
| ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------ |
| <p><a href="https://jb.gg/OpenSource"><img src="https://camo.githubusercontent.com/99d59f1721da5543764f341f9013f478fd27042918fc1109aa367292a8dcca0a/68747470733a2f2f7265736f75726365732e6a6574627261696e732e636f6d2f73746f726167652f70726f64756374732f636f6d70616e792f6272616e642f6c6f676f732f6a625f6265616d2e737667" alt="JetBrains"> </a></p><p><a href="https://jb.gg/OpenSource">JetBrains</a></p>                                                                                                                                                                                                                                                                                                                                                                                    | For providing with free licenses to their great tools. |
| <p><a href="https://www.gitbook.com/"><img src="https://camo.githubusercontent.com/11c54932c8b6d3e3922f93184c94047e8f6abcc49f0812f7837d94c75e5f8b04/68747470733a2f2f75706c6f6164732d73736c2e776562666c6f772e636f6d2f3563333439663930613363643435313564303536343535322f3563363665356234383233386533306531373064613362655f6c6f676f2e737667" alt="Gitbook"></a><img src=".gitbook/assets/image (405).png" alt=""></p><p><img src="https://camo.githubusercontent.com/11c54932c8b6d3e3922f93184c94047e8f6abcc49f0812f7837d94c75e5f8b04/68747470733a2f2f75706c6f6164732d73736c2e776562666c6f772e636f6d2f3563333439663930613363643435313564303536343535322f3563363665356234383233386533306531373064613362655f6c6f676f2e737667" alt="Gitbook"><a href="https://www.gitbook.com/">Gitbook</a></p> | For providing us with free OSS Plan.                   |
| <div><figure><img src=".gitbook/assets/image.png" alt=""><figcaption></figcaption></figure></div><p><a href="https://www.docker.com/">Docker</a></p>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      | For providing us with free OSS Plan.                   |
| <p></p><div><figure><img src=".gitbook/assets/tuta_logo.svg" alt="" width="375"><figcaption></figcaption></figure></div><p><a href="https://tuta.com/">Tuta</a></p>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       | For providing us with free email service               |
