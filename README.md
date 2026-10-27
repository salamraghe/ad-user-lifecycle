# AD User Lifecycle Lab

Hands-on lab, Sep 2026 (Mover step added Oct 2026).

This is the joiner, mover, leaver ticket flow every help desk and IAM team runs, done by hand in Active Directory on a Windows Server 2022 domain controller I built. A new hire gets an account and the right group. When they change roles, they get the new role's access and lose the old one. When they leave, the account gets disabled. That keeps access from piling up over time (privilege creep).

## What I built

- A VirtualBox VM **ADDC01** running Windows Server 2022, promoted to the first domain controller for **lab.local** (NetBIOS **LAB**) with AD DS and DNS.
- OU **LabUsers** with **testuser** (Test User) and **Leaver User**.
- **Joiner:** security group **Lab-Access-Group** (Global, Security) with testuser as a member.
- **Mover (19 Oct 2026):** created **Finance-Users**, added testuser, removed **Lab-Access-Group**. Member Of now shows only Domain Users and Finance-Users.
- **Leaver:** disabled Leaver User. AD confirmed "Object Leaver User has been disabled."

## What I learned

In my own words, on the Mover step: "testuser is a person in the company that's moving to a finance role. I changed his job permissions to finance and removed his old role access to Lab-Access-Group. He doesn't need those since he's not in that role anymore, so he only has access to what he's supposed to."

*Interview line:*

> "When testuser moved to Finance, I added him to Finance and removed his old Lab-Access-Group, so he only has access to what his new role needs."

## Key screenshots

![](key/01-domain-controller-lab-local.png)
Built a Windows Server 2022 domain controller for the lab.local domain.

![](key/02-joiner-testuser-in-labusers.png)
Joiner: created testuser inside the LabUsers organizational unit in AD.

![](key/03-group-members-testuser.png)
Added testuser to Lab-Access-Group to grant the starting access.

![](key/04-mover-groups-before.png)
Mover, before: testuser belongs to Domain Users and Lab-Access-Group.

![](key/05-finance-users-group-created.png)
Created the Finance-Users security group for testuser's new role.

![](key/06-mover-groups-after.png)
Mover, after: testuser is in Finance-Users and the old access is removed.

![](key/07-leaver-disabled.png)
Leaver: AD confirms the Leaver User account has been disabled.

## Environment

| Object | Value |
| --- | --- |
| Hypervisor | Oracle VirtualBox, VM **ADDC01** |
| OS | Windows Server 2022 Standard Evaluation, Desktop Experience |
| Domain | **lab.local** (NetBIOS **LAB**), AD DS + DNS |
| OU | **LabUsers** |
| Users | **testuser** (active), **Leaver User** (disabled) |
| Groups | **Lab-Access-Group**, **Finance-Users** (both Global, Security) |

## What broke / issues I fixed

- **Static IP warning** on the promote Prerequisites Check (the NIC was on DHCP). The lab kept going and DNS came up. A real DC should have a static address.
- **Computer name** stayed the auto-generated `WIN-OONUN715QQO` in the early promote and Local Server shots. The VM is named ADDC01 in VirtualBox.
- **Wrong tab / empty Add dialogs** while working Member Of. Those shots were skipped. The final proof is the Members tab and testuser's Member Of tab.

## Gaps

- **Password reset** on Test User was practiced but not screenshotted. Every capture would have shown a password field, so none was kept.
- The lab NIC is isolated (red network "x" on some shots). That's expected for an offline lab.
- Two Mover shots in the raw dump were exact duplicates and were left out.

<details><summary>Full proof (all screenshots)</summary>

![](full-proof/01-vbox-addc01-created.png)

![](full-proof/02-windows-server-2022-iso-download.png)

![](full-proof/03-review-options-lab-local.png)

![](full-proof/04-prerequisites-check-passed.png)

![](full-proof/06-aduc-lab-local.png)

![](full-proof/07-aduc-containers.png)

![](full-proof/08-server-manager-ad-ds-dns.png)

![](full-proof/09-ou-labusers.png)

![](full-proof/11-labusers-both-users.png)

![](full-proof/12-lab-access-group-general.png)

![](full-proof/15-testuser-member-of-lab-access-group.png)

![](full-proof/17-select-groups-finance-users.png)

</details>
