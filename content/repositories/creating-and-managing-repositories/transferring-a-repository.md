---
title: Transferring a repository
intro: You can transfer repositories to other users or organization accounts.
redirect_from:
  - /articles/about-repository-transfers
  - /move-a-repo
  - /moving-a-repo
  - /articles/what-is-transferred-with-a-repository
  - /articles/what-is-transferred-with-a-repo
  - /articles/how-to-transfer-a-repo
  - /articles/how-to-transfer-a-repository
  - /articles/transferring-a-repository-owned-by-your-personal-account
  - /articles/transferring-a-repository-owned-by-your-organization
  - /articles/transferring-a-repository
  - /github/administering-a-repository/transferring-a-repository
  - /github/administering-a-repository/managing-repository-settings/transferring-a-repository
versions:
  fpt: '*'
  ghes: '*'
  ghae: '*'
  ghec: '*'
topics:
  - Repositories
---
## About repository transfers

When you transfer a repository to a new owner, they can immediately administer the repository's contents, issues, pull requests, releases, {% data variables.product.prodname_projects_v1 %}, and settings.

Prerequisites for repository transfers:
- When you transfer a repository that you own to another personal account, the new owner will receive a confirmation email.{% ifversion fpt or ghec %} The confirmation email includes instructions for accepting the transfer. If the new owner doesn't accept the transfer within one day, the invitation will expire.{% endif %}
- To transfer a repository that you own to an organization, you must have permission to create a repository in the target organization.
- The target account must not have a repository with the same name, or a fork in the same network.
- The original owner of the repository is added as a collaborator on the transferred repository. Other collaborators to the transferred repository remain intact.{% ifversion ghes < 3.7 or ghae %}
- Internal repositories can't be transferred.{% endif %}
- Private forks can't be transferred.

{% ifversion fpt or ghec %}If you transfer a private repository to a {% data variables.product.prodname_free_user %} user or organization account, the repository will lose access to features like protected branches and {% data variables.product.prodname_pages %}. {% data reusables.gated-features.more-info %}{% endif %}

### What's transferred with a repository?

When you transfer a repository, its issues, pull requests, wiki, stars, and watchers are also transferred. If the transferred repository contains webhooks, services, secrets, or deploy keys, they will remain associated after the transfer is complete. Git information about commits, including contributions, is preserved. In addition:

- If the transferred repository is a fork, then it remains associated with the upstream repository.
- If the transferred repository has any forks, then those forks will remain associated with the repository after the transfer is complete.
- If the transferred repository uses {% data variables.large_files.product_name_long %}, all {% data variables.large_files.product_name_short %} objects are automatically moved. This transfer occurs in the background, so if you have a large number of {% data variables.large_files.product_name_short %} objects or if the {% data variables.large_files.product_name_short %} objects themselves are large, it may take some time for the transfer to occur.{% ifversion fpt or ghec %} Before you transfer a repository that uses {% data variables.large_files.product_name_short %}, make sure the receiving account has enough data packs to store the {% data variables.large_files.product_name_short %} objects you'll be moving over. For more information on adding storage for personal accounts, see "[Upgrading {% data variables.large_files.product_name_long %}](/articles/upgrading-git-large-file-storage)."{% endif %}
- When a repository is transferred between two personal accounts, issue assignments are left intact. When you transfer a repository from a personal account to an organization, issues assigned to members in the organization remain intact, and all other issue assignees are cleared. Only owners in the organization are allowed to create new issue assignments. When you transfer a repository from an organization to a personal account, only issues assigned to the repository's owner are kept, and all other issue assignees are removed.
- If the transferred repository contains a {% data variables.product.prodname_pages %} site, then links to the Git repository on the Web and through Git activity are redirected. However, we don't redirect {% data variables.product.prodname_pages %} associated with the repository.
- All links to the previous repository location are automatically redirected to the new location. When you use `git clone`, `git fetch`, or `git push` on a transferred repository, these commands will redirect to the new repository location or URL. However, to avoid confusion, we strongly recommend updating any existing local clones to point to the new repository URL. You can do this by using `git remote` on the command line:

  ```shell
  $ git remote set-url origin NEW_URL
  ```

- When you transfer a repository from an organization to a personal account, the repository's read-only collaborators will not be transferred. This is because collaborators can't have read-only access to repositories owned by a personal account. For more information about repository permission levels, see "[Permission levels for a personal account repository](/github/setting-up-and-managing-your-github-user-account/permission-levels-for-a-user-account-repository)" and "[Repository roles for an organization](/organizations/managing-access-to-your-organizations-repositories/repository-roles-for-an-organization)."{% ifversion fpt or ghec %}
- Sponsors who have access to the repository through a sponsorship tier may be affected. For more information, see "[Adding a repository to a sponsorship tier](/sponsors/receiving-sponsorships-through-github-sponsors/managing-your-sponsorship-tiers#adding-a-repository-to-a-sponsorship-tier)".{% endif %}

For more information, see "[Managing remote repositories](/github/getting-started-with-github/managing-remote-repositories)."

### Repository transfers and organizations

To transfer repositories to an organization, you must have repository creation permissions in the receiving organization. If organization owners have disabled repository creation by organization members, only organization owners can transfer repositories out of or into the organization.

Once a repository is transferred to an organization, the organization's default repository permission settings and default membership privileges will apply to the transferred repository.

## Transferring a repository owned by your personal account

You can transfer your repository to any personal account that accepts your repository transfer. When a repository is transferred between two personal accounts, the original repository owner and collaborators are automatically added as collaborators to the new repository.

{% ifversion fpt or ghec %}If you published a {% data variables.product.prodname_pages %} site in a private repository and added a custom domain, before transferring the repository, you may want to remove or update your DNS records to avoid the risk of a domain takeover. For more information, see "[Managing a custom domain for your {% data variables.product.prodname_pages %} site](/articles/managing-a-custom-domain-for-your-github-pages-site)."{% endif %}

{% data reusables.repositories.navigate-to-repo %}
{% data reusables.repositories.sidebar-settings %}
{% data reusables.repositories.transfer-repository-steps %}

## Transferring a repository owned by your organization

If you have owner permissions in an organization or admin permissions to one of its repositories, you can transfer a repository owned by your organization to your personal account or to another organization.

1. Sign into your personal account that has admin or owner permissions in the organization that owns the repository.
{% data reusables.repositories.navigate-to-repo %}
{% data reusables.repositories.sidebar-settings %}
{% data reusables.repositories.transfer-repository-steps %}
