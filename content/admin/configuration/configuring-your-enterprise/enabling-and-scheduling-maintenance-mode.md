---
title: Enabling and scheduling maintenance mode
intro: 'Some standard maintenance procedures, such as upgrading {% data variables.product.product_location %} or restoring backups, require the instance to be taken offline for normal use.'
redirect_from:
  - /enterprise/admin/maintenance-mode
  - /enterprise/admin/categories/maintenance-mode
  - /enterprise/admin/articles/maintenance-mode
  - /enterprise/admin/articles/enabling-maintenance-mode
  - /enterprise/admin/articles/disabling-maintenance-mode
  - /enterprise/admin/guides/installation/maintenance-mode
  - /enterprise/admin/installation/enabling-and-scheduling-maintenance-mode
  - /enterprise/admin/configuration/enabling-and-scheduling-maintenance-mode
  - /admin/configuration/enabling-and-scheduling-maintenance-mode
versions:
  ghes: '*'
type: how_to
topics:
  - Enterprise
  - Fundamentals
  - Maintenance
  - Upgrades
shortTitle: Configure maintenance mode
---
## About maintenance mode

Some types of operations require that you take {% data variables.product.product_location %} offline and put it into maintenance mode:
- Upgrading to a new version of {% data variables.product.prodname_ghe_server %}
- Increasing CPU, memory, or storage resources allocated to the virtual machine
- Migrating data from one virtual machine to another
- Restoring data from a {% data variables.product.prodname_enterprise_backup_utilities %} snapshot
- Troubleshooting certain types of critical application issues

We recommend that you schedule a maintenance window for at least 30 minutes in the future to give users time to prepare. When a maintenance window is scheduled, all users will see a banner when accessing the site.



![End user banner about scheduled maintenance](/assets/images/enterprise/maintenance/maintenance-scheduled.png)

When the instance is in maintenance mode, all normal HTTP and Git access is refused. Git fetch, clone, and push operations are also rejected with an error message indicating that the site is temporarily unavailable. In high availability configurations, Git replication will be paused. GitHub Actions jobs will not be executed. Visiting the site in a browser results in a maintenance page.

![The maintenance mode splash screen](/assets/images/enterprise/maintenance/maintenance-mode-maintenance-page.png)

{% ifversion ip-exception-list %}

You can perform initial validation of your maintenance operation by configuring an IP exception list to allow access to {% data variables.product.product_location %} from only the IP addresses and ranges provided. Attempts to access {% data variables.product.product_location %} from IP addresses not specified on the IP exception list will receive a response consistent with those sent when the instance is in maintenance mode. 

{% endif %}

## Enabling maintenance mode immediately or scheduling a maintenance window for a later time

{% data reusables.enterprise_site_admin_settings.access-settings %}
{% data reusables.enterprise_site_admin_settings.management-console %}
2. At the top of the {% data variables.enterprise.management_console %}, click **Maintenance**.
  ![Maintenance tab](/assets/images/enterprise/management-console/maintenance-tab.png)
3. Under "Enable and schedule", decide whether to enable maintenance mode immediately or to schedule a maintenance window for a future time.
    - To enable maintenance mode immediately, use the drop-down menu and click **now**.
    ![Drop-down menu with the option to enable maintenance mode now selected](/assets/images/enterprise/maintenance/enable-maintenance-mode-now.png)
    - To schedule a maintenance window for a future time, use the drop-down menu and click a start time.
    ![Drop-down menu with the option to schedule a maintenance window in two hours selected](/assets/images/enterprise/maintenance/schedule-maintenance-mode-two-hours.png)
4. Select **Enable maintenance mode**.
  ![Checkbox for enabling or scheduling maintenance mode](/assets/images/enterprise/maintenance/enable-maintenance-mode-checkbox.png)
{% data reusables.enterprise_management_console.save-settings %}

{% ifversion ip-exception-list %}

## Validating changes in maintenance mode using the IP exception list

The IP exception list provides controlled and restricted access to {% data variables.product.product_location %}, which is ideal for initial validation of server health following a maintenance operation. Once enabled, {% data variables.product.product_location %} will be taken out of maintenance mode and available only to the configured IP addresses. The maintenance mode checkbox will be updated to reflect the change in state.

If you re-enable maintenance mode, the IP exception list will be disabled and {% data variables.product.product_location %} will return to maintenance mode. If you just disable the IP exception list, {% data variables.product.product_location %} will return to normal operation.

You can also use a command-line utility to configure the IP exception list. For more information, see "[Command-line utilities](/admin/configuration/configuring-your-enterprise/command-line-utilities#ghe-maintenance)" and "[Accessing the administrative shell (SSH)](/admin/configuration/configuring-your-enterprise/accessing-the-administrative-shell-ssh)."

{% data reusables.enterprise_site_admin_settings.access-settings %}
{% data reusables.enterprise_site_admin_settings.management-console %}
1. At the top of the {% data variables.enterprise.management_console %}, click **Maintenance**, and confirm maintenance mode is already enabled.
  ![Maintenance tab](/assets/images/enterprise/management-console/maintenance-tab.png)
1. Select **Enable IP exception list**.
 ![Checkbox for enabling ip exception list](/assets/images/enterprise/maintenance/enable-ip-exception-list.png)
1. In the text box, type a valid list of space-separated IP addresses or CIDR blocks that should be allowed to access {% data variables.product.product_location %}.
 ![completed field for IP addresses](/assets/images/enterprise/maintenance/ip-exception-list-ip-addresses.png)
1. Click **Save**.
![after IP excetpion list has saved](/assets/images/enterprise/maintenance/ip-exception-save.png)

{% endif %}

## Scheduling maintenance mode with the {% data variables.product.prodname_enterprise_api %}

You can schedule maintenance for different times or dates with the {% data variables.product.prodname_enterprise_api %}. For more information, see "[Management Console](/enterprise/user/rest/reference/enterprise-admin#enable-or-disable-maintenance-mode)."

## Enabling or disabling maintenance mode for all nodes in a cluster

With the `ghe-cluster-maintenance` utility, you can set or unset maintenance mode for every node in a cluster.

```shell
$ ghe-cluster-maintenance -h
# Shows options
$ ghe-cluster-maintenance -q
# Queries the current mode
$ ghe-cluster-maintenance -s
# Sets maintenance mode
$ ghe-cluster-maintenance -u
# Unsets maintenance mode
```
