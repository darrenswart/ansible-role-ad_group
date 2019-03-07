Ansible Role: ad_group
=========

This role creates the specified Active Directory (AD) group in the Organizational Unit (OU) specified.  It can also add users and other groups to the group.  When used to add users/groups to an existing group it will only add the users/groups specified.  It will NOT remove users/groups currently in the group.  Therefore, this role can not be used to remove users/groups from a group but it can be used to delete the specified group.

Requirements
------------

This role requires that the Active Directory module for PowerShell be installed on a Windows server.  The server with the AD module installed also needs to have WinRM configured for use with Ansible.

Role Variables
--------------

| Name           | Default Value | Description                        |
| -------------- | ------------- | -----------------------------------|
| `ad_group_name` | None | Name of the group in Active Directory. Required|
| `ad_group_description` | None | Description of the group. Optional|
| `ad_group_ou` | None | OU that the group is located in.  Needs to be in Distinguished Name Format. For example OU=school-teachers,OU=1885,DC=fluxcapacitor,DC=com.  Required |
| `ad_group_membership_users` | [] | List of users to add to the group.  Specify users with their UPN (UserPrincipalName) which is usually their email address.  This should be specified as a list in yaml format.  Optional |
| `ad_group_membership_groups` | [] | List of groups to add the the group.  Groups need to be listed with their Distinguished Name.  This should be specified as a list in yaml format.  Optional |
| `ad_domain` | None | Domain to create the group in. |
| `ad_group_state` | present | If the group should be deleted specify **absent**.  For example `ad_group_state=absent` |
| `domain_username` | {{ ansible_user }} | By default the value of {{ansible_user}} which is the user connecting through WinRM will be used.  If a different user is required to create/modify/delete the group then specify that user here. |
| `domain_password` | {{ ansible_password }} | By default the value of {{ansible_password}} which is the user connecting through WinRM will be used.  If a different user is required to create/modify/delete the group then specify that user's password here. |

Example Playbook
----------------

```yaml
---
- hosts: all
  
  tasks:

  - name: Create the AD group travelers
    include_role:
      name: ad_group
    vars:
      ad_domain: fluxcapacitor.com
      ad_group_name: travelers
      ad_group_description: time travelers
      ad_group_ou: OU=school-teachers,OU=1885,DC=fluxcapacitor,DC=com
      ad_group_membership_users: ['emmett.brown@fluxcapacitor.com', 'marty.mcfly@fluxcapacitor.com']
```

**NOTE**
Due to the time it can take for replication to occur in Active Directory the role sets the fact (variable) `domain_controller` that is available after the role executes.  It is recommended to use this value in your playbook if wanting to interact with the newly created group in some way.  Since it can't be guaranteed that the group will be fully replicated throughout the domain but the time the role completes.

License
-------

BSD

Author Information
------------------

Darren Swart
