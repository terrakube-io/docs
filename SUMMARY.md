# Table of contents

* [Introduction](README.md)

## Getting started

* [📐 Architecture](getting-started/design-and-architecture.md)
* [🔐 Security](getting-started/security.md)
* [🚀 Getting Started](getting-started/getting-started.md)
* [📥 Deployment](getting-started/deployment/README.md)
  * [🔨 Helm Chart](getting-started/deployment/docker-compose.md)
* [🤵 User Management](getting-started/user-management/README.md)
  * [Azure Active Directory](getting-started/user-management/azure-active-directory.md)
  * [Google Cloud Identity](getting-started/user-management/google-cloud-identity.md)
  * [Amazon Cognito](getting-started/user-management/amazon-cognito.md)
  * [Github](getting-started/user-management/docker-desktop.md)

## 📓 User Guide

* [Organizations](user-guide/organizations/README.md)
  * [Creating an Organization](user-guide/organizations/organizations.md)
  * [Team Management](user-guide/organizations/team-management.md)
  * [Templates](user-guide/organizations/templates.md)
  * [SSH](user-guide/organizations/ssh.md)
* [VCS Providers](user-guide/vcs-providers/README.md)
  * [Github](user-guide/vcs-providers/github.com.md)
  * [Azure DevOps](user-guide/vcs-providers/azure-devops.md)
  * [Bitbucket](user-guide/vcs-providers/bitbucket.com.md)
  * [GitLab](user-guide/vcs-providers/gitlab.com.md)
* [Open Policy Agent](user-guide/open-policy-agent.md)

## 🎓 Learn

* [What is Terrakube](learn/what-is-terrakube/README.md)
  * [Section Overview](learn/what-is-terrakube/section-overview.md)
  * [Terraform in a Nutshell](learn/what-is-terrakube/terraform-in-a-nutshell.md)
  * [Terraform Challenges at Enterprise Level](learn/what-is-terrakube/terraform-challenges-at-enterprise-level.md)
  * [Introducing Terrakube](learn/what-is-terrakube/introducing-terrakube.md)
  * [Summary and Up Next](learn/what-is-terrakube/summary-and-up-next.md)
* [Deploying using Terrakube](learn/deploying-using-terrakube.md)
* [Using Terrakube Private Registry](learn/using-terrakube-private-registry.md)

## 💻 CLI

* [🌟 Getting started](cli/getting-started.md)
* [💿 Installation](cli/install.md)
* [ℹ Commands](cli/commands/README.md)
  * [terrakube login](cli/commands/azb-login.md)
  * [terrakube logout](cli/commands/terrakube-logout.md)
  * [terrakube organization](cli/commands/azb-organization/README.md)
    * [organization list](cli/commands/azb-organization/list.md)
    * [organization create](cli/commands/azb-organization/create.md)
    * [organization update](cli/commands/azb-organization/update.md)
    * [organization delete](cli/commands/azb-organization/delete.md)
  * [terrakube team](cli/commands/terrakube-team/README.md)
    * [team list](cli/commands/terrakube-team/team-list.md)
    * [team create](cli/commands/terrakube-team/team-create.md)
    * [team update](cli/commands/terrakube-team/team-update.md)
    * [team delete](cli/commands/terrakube-team/team-delete.md)
  * [terrakube workspace](cli/commands/terrakube-workspace/README.md)
    * [workspace list](cli/commands/terrakube-workspace/workspace-list.md)
    * [workspace create](cli/commands/terrakube-workspace/workspace-create.md)
    * [workspace update](cli/commands/terrakube-workspace/workspace-update.md)
    * [workspace delete](cli/commands/terrakube-workspace/workspace-delete.md)
    * [workspace variable](cli/commands/terrakube-workspace/workspace-variable/README.md)
      * [variable list](cli/commands/terrakube-workspace/workspace-variable/variable-list.md)
  * [terrakube variable](cli/commands/terrakube-variable/README.md)
    * [variable update](cli/commands/terrakube-variable/variable-update.md)
    * [variable delete](cli/commands/terrakube-variable/variable-delete.md)
    * [variable create](cli/commands/terrakube-variable/variable-create.md)
  * [terrakube job](cli/commands/terrakube-job/README.md)
    * [job list](cli/commands/terrakube-job/job-list.md)
    * [job create](cli/commands/terrakube-job/job-create.md)
  * [terrakube module](cli/commands/azb-module/README.md)
    * [module list](cli/commands/azb-module/list.md)
    * [module create](cli/commands/azb-module/create.md)
    * [module update](cli/commands/azb-module/module-update.md)
    * [module delete](cli/commands/azb-module/module-delete.md)

## 📖 API

* [🌟 Getting started](api/getting-started.md)
* [⚙ Methods](api/methods/README.md)
  * [Globalvar](api/methods/globalvar.md)
  * [Organization](api/methods/organization.md)
  * [Teams](api/methods/teams.md)
  * [Workspace](api/methods/workspace.md)
  * [Variables](api/methods/variables.md)
  * [History](api/methods/history.md)
  * [Jobs](api/methods/jobs.md)
  * [Template](api/methods/template.md)
  * [Schedule](api/methods/schedule.md)
  * [Step](api/methods/step.md)
  * [Module](api/methods/module.md)
  * [Vcs](api/methods/vcs.md)
  * [Provider](api/methods/provider.md)
  * [Personal Access Token](api/methods/personal-access-token.md)
  * [SSH Key](api/methods/ssh-key.md)

## 🏗 CI/CD Integration

* [Bitbucket](ci-cd-integration/bitbucket.md)
