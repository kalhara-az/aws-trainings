# AWS Organizations terminology and concepts

AWS Organizations is an account management service that enables you to consolidate multiple AWS accounts into an organization that you create and centrally manage. AWS Organizations includes account management and consolidated billing capabilities that enable you to better meet the budgetary, security, and compliance needs of your business. As an administrator of an organization, you can create accounts in your organization and invite existing accounts to join the organization

The following diagram shows a basic organization that consists of five accounts that are organized into four organizational units (OUs) under the root. The organization also has several policies that are attached to some of the OUs or directly to accounts. 

![image](images/AccountOuDiagram.png)

**Organization**
An entity that you create to consolidate your AWS accounts so that you can administer them as a single unit. You can use the AWS Organizations console to centrally view and manage all of your accounts within your organization. An organization has one management account along with zero or more member accounts. You can organize the accounts in a hierarchical, tree-like structure with a root at the top and organizational units nested under the root. Each account can be directly in the root, or placed in one of the OUs in the hierarchy. An organization has the functionality that is determined by the feature set that you enable.

**Root**
The parent container for all the accounts for your organization. If you apply a policy to the root, it applies to all organizational units (OUs) and accounts in the organization.

Currently, you can have only one root. AWS Organizations automatically creates it for you when you create an organization.

**Organizational unit (OU)**
A container for accounts within a root. An OU also can contain other OUs, enabling you to create a hierarchy that resembles an upside-down tree, with a root at the top and branches of OUs that reach down, ending in accounts that are the leaves of the tree. When you attach a policy to one of the nodes in the hierarchy, it flows down and affects all the branches (OUs) and leaves (accounts) beneath it. An OU can have exactly one parent, and currently each account can be a member of exactly one OU.

**Account**
An account in Organizations is a standard AWS account that contains your AWS resources and the identities that can access those resources.

There are two types of accounts in an organization: a single account that is designated as the *management account*, and one or more *member accounts*.

* **The management account** is the account that you use to create the organization. From the organization's management account, you can do the following:
   * Create accounts in the organization

   * Invite other existing accounts to the organization

    * Remove accounts from the organization

    * Manage invitations

    * Apply policies to entities (roots, OUs, or accounts) within the organization

    * Enable integration with supported AWS services to provide service functionality across all of the accounts in the organization.

The management account has the responsibilities of a payer account and is responsible for paying all charges that are accrued by the member accounts. You can't change an organization's management account.  

* **Member accounts** make up all of the rest of the accounts in an organization. An account can be a member of only one organization at a time. You can attach a policy to an account to apply controls to only that one account.

**Invitation**
The process of asking another account to join your organization. An invitation can be issued only by the organization's management account. The invitation is extended to either the account ID or the email address that is associated with the invited account. After the invited account accepts an invitation, it becomes a member account in the organization. Invitations also can be sent to all current member accounts when the organization needs all members to approve the change from supporting only consolidated billing features to supporting all features in the organization. Invitations work by accounts exchanging handshakes. You might not see handshakes when you work in the AWS Organizations console. But if you use the AWS CLI or AWS Organizations API, you must work directly with handshakes.

**Handshake**
A multi-step process of exchanging information between two parties. One of its primary uses in AWS Organizations is to serve as the underlying implementation for invitations. Handshake messages are passed between and responded to by the handshake initiator and the recipient. The messages are passed in a way that helps ensure that both parties know what the current status is. Handshakes also are used when changing the organization from supporting only consolidated billing features to supporting all features that AWS Organizations offers. You generally need to directly interact with handshakes only if you work with the AWS Organizations API or command line tools such as the AWS CLI.

**Available feature sets**

* All features – The default feature set that is available to AWS Organizations. It includes all the functionality of consolidated billing, plus advanced features that give you more control over accounts in your organization. For example, when all features are enabled the management account of the organization has full control over what member accounts can do. The management account can apply SCPs to restrict the services and actions that users (including the root user) and roles in an account can access. The management account can also prevent member accounts from leaving the organization. You can also enable integration with support AWS services to let those service provide functionality across all of the accounts in your organization.

You can create an organization with all features already enabled, or you can enable all features in an organization that originally supported only the consolidated billing features. To enable all features, all invited member accounts must approve the change by accepting the invitation that is sent when the management account starts the process.

* Consolidated billing – This feature set provides shared billing functionality, but does not include the more advanced features of AWS Organizations. For example, you can't enable other AWS services to integrate with your organization to work across all of its accounts, or use policies to restrict what users and roles in different accounts can do. To use the advanced AWS Organizations features, you must enable all features in your organization.

**Service control policy (SCP)**
A policy that specifies the services and actions that users and roles can use in the accounts that the SCP affects. SCPs are similar to IAM permissions policies except that they don't grant any permissions. Instead, SCPs specify the maximum permissions for an organization, organizational unit (OU), or account. When you attach an SCP to your organization root or an OU, the SCP limits permissions for entities in member accounts.

**Allow lists vs. deny lists**
Allow lists and deny lists are complementary strategies that you can use to apply SCPs to filter the permissions that are available to accounts.

* Allow list strategy – You explicitly specify the access that is allowed. All other access is implicitly blocked. By default, AWS Organizations attaches an AWS managed policy called FullAWSAccess to all roots, OUs, and accounts. This helps ensure that, as you build your organization, nothing is blocked until you want it to be. In other words, by default all permissions are allowed. When you are ready to restrict permissions, you replace the FullAWSAccess policy with one that allows only the more limited, desired set of permissions. Users and roles in the affected accounts can then exercise only that level of access, even if their IAM policies allow all actions. If you replace the default policy on the root, all accounts in the organization are affected by the restrictions. You can't add permissions back at a lower level in the hierarchy because an SCP never grants permissions; it only filters them.

* Deny list strategy – You explicitly specify the access that is not allowed. All other access is allowed. In this scenario, all permissions are allowed unless explicitly blocked. This is the default behavior of AWS Organizations. By default, AWS Organizations attaches an AWS managed policy called FullAWSAccess to all roots, OUs, and accounts. This allows any account to access any service or operation with no AWS Organizations–imposed restrictions. Unlike the allow list technique described above, when using deny lists, you leave the default FullAWSAccess policy in place (that allow "all"). But then you attach additional policies that explicitly deny access to the unwanted services and actions. Just as with IAM permission policies, an explicit deny of a service action overrides any allow of that action.

**Artificial intelligence (AI) services opt-out policy**
A type of policy that helps you standardize your opt-out settings for AWS AI services across all of the accounts in your organization. Certain AWS AI services can store and use customer content processed by those services for the development and continuous improvement of Amazon AI services and technologies. As an AWS customer, you can use AI service opt-out policies to choose to opt out of having your content stored or used for service improvements.

**Backup policy**
A type of policy that helps you standardize and implement a backup strategy for the resources across all of the accounts in your organization. In a backup policy, you can configure and deploy backup plans for your resources.

**Tag policy**
A type of policy that helps you standardize tags across resources across all of the accounts in your organization. In a tag policy, you can specify tagging rules for specific resources.