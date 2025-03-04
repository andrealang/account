---

copyright:

  years: 2017, 2022

lastupdated: "2022-04-27"

keywords: resource access, assign access, IAM access policy, access to resource groups, edit access, remove access 

subcollection: account

---

{{site.data.keyword.attribute-definition-list}}

# Managing access to resources
{: #assign-access-resources}

To manage access for users or service IDs by using IAM policies, you must be the account owner or have the correct access assigned. To assign user's access to resources you must be an administrator on all services in the account, or the assigned administrator for the particular service or service instance. To assign access to a service ID, you must be administrator on the identity service or the specific service ID.
{: shortdesc}

## Assigning access to resources
{: #assign-new-access}

You can assign access to resources by using two types of policies:

* Access to resources in the account, including the option for just one type or all types
* Access to resources within a resource group, including the option for just one resource or all

If you delete or edit an existing policy for a service ID currently being used, it might cause service interruption.
{: note}

If you want to enable a user full administrator access to complete [account management tasks](/docs/account?topic=account-account-services#account-management-access), such as inviting and removing users, viewing billing and usage, managing service IDs, managing access groups, managing user access, and access to all IAM-enabled resources, you must assign a user the following access:
* A policy for **All Identity and Access enabled services** within the **Account** with the Administrator and Manager roles.
* A policy with Administrator role on **All Account Management Services**.

You can also set access management tags to manage access. For more information, see [Controlling access to resources by using tags](/docs/account?topic=account-access-tags-tutorial).
{: tip}

### Assigning access to resources in the console
{: #access-resources-console}
{: ui}
{: help}
{: support}

To assign access to an individual resource in the account or access to all resources in the account, complete the following steps:

1. In the {{site.data.keyword.cloud_notm}} console, click **Manage** > **Access (IAM)** and select **Users** or **Service IDs**, depending on which identity you want to assign access.
2. From the row for the user or service ID that you want to assign access, click the **Actions** icon ![List of actions icon](../icons/action-menu-icon.svg) > **Assign access**.
3. Add the user or service ID to an access group by selecting **Access groups**. 
4. Select the access group or groups that you want the user or service ID to belong to and click **Add**.
5. (Optional) Assign access by using an individual access policy.
    1. Select **Access policy**.
    2. Select group of services or a specific service, then click **Next**. 
    3. Select any combination of roles to assign the user or service ID, and click **Finish** > **Add** > **Assign**.

If a user doesn't have a role on the resource group that contains the resources, they can see the resources, but can't access the resources by going to the Resource list page in the account to start working with them. Assign the Viewer role or higher on the resource group itself to ensure that a user can access the resource.
{: note}

### Assigning access within a resource group in the console
{: #access-to-resources-console}
{: ui}

To assign access to all resources in a resource group or to just one service within a resource group, complete the following steps:

1. In the {{site.data.keyword.cloud_notm}} console, click **Manage** > **Access (IAM)** and select **Users** or **Service IDs**, depending on which identity you want to assign access.
2. From the row for the user or service ID that you want to assign access, click the **Actions** icon ![Actions icon](../icons/action-menu-icon.svg "Actions") > **Assign access**.
3. Add the user or service ID to an access group by selecting **Access groups**. 
4. Select the access group or groups that you want the user or service ID to belong to and click **Add**.
5. (Optional) Assign access by using an individual access policy.
    1. Select **Access policy**.
    2. Select group of services or a specific service, then click **Next**. 
    3. Scope the acccess to **Resources based on selected attributes**, then select the **Resource group** attribute. By selecting a resource group, you can select roles for access to manage the resource group as well.
    4. Select any combination of roles to assign, and click **Add** > **Assign**.

### Assigning access to resources by using the CLI
{: #access-resources-cli}
{: cli}

1. Log in to {{site.data.keyword.cloud}} CLI. If you have multiple accounts, you are prompted to select which account to use. If you do not specify a region with the `-r` flag, you must also select a region.
   ```bash
   ibmcloud login
   ```
   {: codeblock}

   If your credentials are rejected, you might be using a federated ID. To log in with a federated ID, use the `--sso` flag. See [Logging in with a federated ID](/docs/account?topic=account-federated_id) for more details.
   {: tip}
    
   If it's your first time using the {{site.data.keyword.cloud_notm}} CLI, check out the [getting started tutorial](/docs/cli?topic=cli-getting-started).

2. Create an access policy and assign it to a user or a service ID.
   * To assign access to an individual resource in the account or access to all resources in the account, enter the [**`ibmcloud iam user-policy-create`**](/docs/cli?topic=cli-ibmcloud_commands_iam#ibmcloud_iam_user_policy_create) command. This example gives `name@example.com` `Administrator` role for all instances of `sample-service` service:
    ```bash
   ibmcloud iam user-policy-create name@example.com --roles Administrator --service-name sample-service
   ```
   {: codeblock}

   * To assign access to a service ID or to more services in the account, enter the [**`ibmcloud iam service-policy-create`**](/docs/cli?topic=cli-ibmcloud_commands_iam#ibmcloud_iam_service_policy_create) command. This example grants service `test` the `Administrator` role for all account management services:
    ```bash
    ibmcloud iam service-policy-create test --roles Administrator --account-management
    ```
    {: codeblock}

### Assigning access within a resource group by using the CLI
{: #access-resourcegroups-cli}
{: cli}

Enter the [**`ibmcloud user-policy-create`**](/docs/cli?topic=cli-ibmcloud_commands_iam#ibmcloud_iam_user_policy_create) command to assign access to all resources in a resource group or to just one service within a resource group. This example gives `name@example.com` `Operator` role for resource group with ID `dda27e49d2a1efca58083a01dfde18f6`:

```bash
ibmcloud iam user-policy-create name@example.com --roles Operator --resource-type resource-group --resource dda27e49d2a1efca58083a01dfde18f6
```
{: codeblock}

Enter the [**`ibmcloud iam service-policy-create`**](/docs/cli?topic=cli-ibmcloud_commands_iam#ibmcloud_iam_service_policy_create) command to assign access to all resources in a resource group or to just one service within a resource group. This example gives service `test` `Administrator` role for resource group called `sample-resource-group`:

```bash
ibmcloud iam service-policy-create test --roles Administrator --resource-group-name sample-resource-group
```
{: codeblock}

### Assigning access to resources by using the API
{: #access-resources-api}
{: api}

You can assign access to an individual resource in the account or access to a list of resources in the account by calling the [{{site.data.keyword.iamlong}} (IAM) Policy Management API](/apidocs/iam-policy-management#create-policy){: external} as shown in the following sample request. The sample request gives `Administrator` role access for an instance of a service:

```bash
curl -X POST 'https://iam.cloud.ibm.com/v1/policies' -H 'Authorization: Bearer $TOKEN' \
-H 'Content-Type: application/json' -d '{
  "type": "access",
  "description": "Administrator role for SERVICE_NAME's RESOURCE_NAME",
  "subjects": [
    {
      "attributes": [
        {
          "name": "iam_id",
          "value": "IBMid-123453user"
        }
      ]
    }'
  ],
  "roles":[
    {
      "role_id": "crn:v1:bluemix:public:iam::::role:Administrator"
    }
  ],
  "resources":[
    {
      "attributes": [
        {
          "name": "accountId",
          "value": "$ACCOUNT_ID"
        },
        {
          "name": "serviceName",
          "value": "$SERVICE_NAME"
        },
        {
          "name": "resource",
          "value": "$RESOURCE_NAME",
          "operator": "stringEquals"
        }
      ]
    }
  ]
}'
```
{: curl}
{: codeblock}
   
```java
SubjectAttribute subjectAttribute = new SubjectAttribute.Builder()
      .name("iam_id")
      .value("EXAMPLE_USER_ID")
      .build();

PolicySubject policySubjects = new PolicySubject.Builder()
      .addAttributes(subjectAttribute)
      .build();

PolicyRole policyRoles = new PolicyRole.Builder()
      .roleId("crn:v1:bluemix:public:iam::::role:Administrator")
      .build();

ResourceAttribute accountIdResourceAttribute = new ResourceAttribute.Builder()
      .name("accountId")
      .value(exampleAccountId)
      .operator("stringEquals")
      .build();

ResourceAttribute serviceNameResourceAttribute = new ResourceAttribute.Builder()
      .name("serviceName")
      .value("service")
      .operator("stringEquals")
      .build();

PolicyResource policyResources = new PolicyResource.Builder()
      .addAttributes(accountIdResourceAttribute)
      .addAttributes(serviceNameResourceAttribute)
      .build();

CreatePolicyOptions options = new CreatePolicyOptions.Builder()
      .type("access")
      .subjects(Arrays.asList(policySubjects))
      .roles(Arrays.asList(policyRoles))
      .resources(Arrays.asList(policyResources))
      .build();

Response<Policy> response = service.createPolicy(options).execute();
Policy policy = response.getResult();

System.out.println(policy);
```
{: java}
{: codeblock}
   
```javascript
const policySubjects = [
  {
    attributes: [
      {
        name: 'iam_id',
        value: 'exampleUserId',
      },
    ],
  },
];
const policyRoles = [
  {
    role_id: 'crn:v1:bluemix:public:iam::::role:Administrator',
  },
];
const accountIdResourceAttribute = {
  name: 'accountId',
  value: exampleAccountId,
  operator: 'stringEquals',
};
const serviceNameResourceAttribute = {
  name: 'serviceName',
  value: 'service',
  operator: 'stringEquals',
};
const policyResources = [
  {
    attributes: [accountIdResourceAttribute, serviceNameResourceAttribute]
  },
];
const params = {
  type: 'access',
  subjects: policySubjects,
  roles: policyRoles,
  resources: policyResources,
};

iamPolicyManagementService.createPolicy(params)
  .then(res => {
    examplePolicyId = res.result.id;
    console.log(JSON.stringify(res.result, null, 2));
  })
  .catch(err => {
    console.warn(err)
  });
```
{: javascript}
{: codeblock}
   
```python
policy_subjects = PolicySubject(
  attributes=[SubjectAttribute(name='iam_id', value='example_user_id')])
policy_roles = PolicyRole(
  role_id='crn:v1:bluemix:public:iam::::role:Administrator')
account_id_resource_attribute = ResourceAttribute(
  name='accountId', value=example_account_id)
service_name_resource_attribute = ResourceAttribute(
  name='serviceName', value='service')
policy_resources = PolicyResource(
  attributes=[account_id_resource_attribute,
        service_name_resource_attribute])

policy = iam_policy_management_service.create_policy(
  type='access',
  subjects=[policy_subjects],
  roles=[policy_roles],
  resources=[policy_resources]
).get_result()

print(json.dumps(policy, indent=2))
```
{: python}
{: codeblock}
   
```go
subjectAttribute := &iampolicymanagementv1.SubjectAttribute{
  Name:  core.StringPtr("iam_id"),
  Value: core.StringPtr("exampleUserID"),
}
policySubjects := &iampolicymanagementv1.PolicySubject{
  Attributes: []iampolicymanagementv1.SubjectAttribute{*subjectAttribute},
}
policyRoles := &iampolicymanagementv1.PolicyRole{
  RoleID: core.StringPtr("crn:v1:bluemix:public:iam::::role:Administrator"),
}
accountIDResourceAttribute := &iampolicymanagementv1.ResourceAttribute{
  Name:     core.StringPtr("accountId"),
  Value:    core.StringPtr(exampleAccountID),
  Operator: core.StringPtr("stringEquals"),
}
serviceNameResourceAttribute := &iampolicymanagementv1.ResourceAttribute{
  Name:     core.StringPtr("serviceName"),
  Value:    core.StringPtr("service"),
  Operator: core.StringPtr("stringEquals"),
}
policyResources := &iampolicymanagementv1.PolicyResource{
  Attributes: []iampolicymanagementv1.ResourceAttribute{
    *accountIDResourceAttribute, *serviceNameResourceAttribute}
}

options := iamPolicyManagementService.NewCreatePolicyOptions(
  "access",
  []iampolicymanagementv1.PolicySubject{*policySubjects},
  []iampolicymanagementv1.PolicyRole{*policyRoles},
  []iampolicymanagementv1.PolicyResource{*policyResources},
)

policy, response, err := iamPolicyManagementService.CreatePolicy(options)
if err != nil {
  panic(err)
}
b, _ := json.MarshalIndent(policy, "", "  ")
fmt.Println(string(b))
```
{: go}
{: codeblock}

### Assigning access within a resource group by using the API
{: #access-resourcegroups-api}
{: api}

This action can be done only through the UI or CLI. To see the steps, switch to the UI or CLI instructions.

### Assigning access to resources by using Terraform
{: #access-resources-terraform}
{: terraform}

You can assign access to resources by using Terraform. 

1. To install the Terraform CLI and configure the {{site.data.keyword.cloud_notm}} Provider plug-in for Terraform, follow the tutorial for [Getting started with Terraform on {{site.data.keyword.cloud}}](/docs/ibm-cloud-provider-for-terraform?topic=ibm-cloud-provider-for-terraform-getting-started). The plug-in abstracts the {{site.data.keyword.cloud_notm}} APIs that are used to complete this task.

2. Create a Terraform configuration file that is named `main.tf`. In this file, you add the configuration to assign access to resources by using HashiCorp Configuration Language. For more information, see the [Terraform documentation](https://www.terraform.io/docs/language/index.html){: external}.

      The following example gives `test@in.ibm.com` `Viewer` role for all instances of `kms` service. 
    
   ```terraform
   resource "ibm_iam_user_policy" "policy" {
    ibm_id = "test@in.ibm.com"
    roles  = ["Viewer"]

   resources {
    service = "kms"
   }
   }
   ```
   {: codeblock}

   You can specify the name of the service for which you want to assign access to on the `service` option. For more information, see the argument reference details on the [Terraform Identity and Access Management (IAM)](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/resources/iam_user_policy){: external} page.
  
3. Initialize the Terraform CLI.

   ```bash
   terraform init
   ```
   {: pre}
   
4. Create a Terraform execution plan. The Terraform execution plan summarizes all the actions that need to be run to assign access to a resource.

   ```bash
   terraform plan
   ```
   {: pre}

5. Assign access to the resource.

   ```bash
   terraform apply
   ```
   {: pre}

### Assigning access within a resource group by using Terraform
{: #access-to-resources-terraform}
{: terraform}

You can assign access within a resource group by using Terraform. 

1. To install the Terraform CLI and configure the {{site.data.keyword.cloud_notm}} Provider plug-in for Terraform, follow the tutorial for [Getting started with Terraform on {{site.data.keyword.cloud}}](/docs/ibm-cloud-provider-for-terraform?topic=ibm-cloud-provider-for-terraform-getting-started). The plug-in abstracts the {{site.data.keyword.cloud_notm}} APIs that are used to complete this task.

2. Create a Terraform configuration file that is named `main.tf`. In this file, you add the configuration to assign access within a resource group by using HashiCorp Configuration Language. For more information, see the [Terraform documentation](https://www.terraform.io/docs/language/index.html){: external}.

      The following example gives `test@in.ibm.com` `Viewer` role for resource group with ID `data.ibm_resource_group.group.id`.
          
   ```terraform
   data "ibm_resource_group" "group" {
    name = "default"
   }

   resource "ibm_iam_user_policy" "policy" {
    ibm_id = "test@in.ibm.com"
    roles  = ["Viewer"]

   resources {
    service           = "containers-kubernetes"
    resource_group_id = data.ibm_resource_group.group.id
   }
   }
   ```
   {: codeblock}

   You can specify the ID of the resource group within you want to assign access to on the `resource_group_id` option. For more information, see the argument reference details on the [Terraform Identity and Access Management (IAM)](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/resources/iam_user_policy){: external} page.
  
3. Initialize the Terraform CLI.

   ```bash
   terraform init
   ```
   {: pre}
   
4. Create a Terraform execution plan. The Terraform execution plan summarizes all the actions that need to be run to assign access within a resource group.

   ```bash
   terraform plan
   ```
   {: pre}

5. Assign access within the resource group.

   ```bash
   terraform apply
   ```
   {: pre}
   
## Removing access in the console
{: #removing-access-console}
{: ui}

Removing access for a user or service ID can take up to 10 minutes to take effect.

1. In the {{site.data.keyword.cloud_notm}} console, click **Manage** > **Access (IAM)**, and select **Users** or **Service IDs**, depending on which identity you want to assign access.
2. Select the user's name or service ID that you want to remove access for.
3. Go to **Access** and click the **Actions** icon ![Actions icon](../icons/action-menu-icon.svg "Actions") > **Remove** on the row for the policy you want to remove.
4. Review the policy details that you're removing, and confirm by clicking **Remove**.

You can also remove users and service IDs from access groups by selecting the checkbox for the user or service ID that you want to remove, and click **Remove**. Then, click **Remove** again to approve the process. 
{: tip}

## Removing access by using the CLI
{: #removing-access-cli}
{: cli}

To remove a user policy by using the CLI, you can use the [**`ibmcloud iam user-policy-delete`**](/docs/cli?topic=cli-ibmcloud_commands_iam#ibmcloud_iam_user_policy_delete) command.

```bash
ibmcloud iam user-policy-delete USER_ID POLICY_ID [-f, --force]
```
{: codeblock}

To remove a service ID policy by using the CLI, you can use the [**`ibmcloud iam service-policy-delete`**](/docs/cli?topic=cli-ibmcloud_commands_iam#ibmcloud_iam_service_policy_delete) command.

```bash
ibmcloud iam service-policy-delete SERVICE_ID POLICY_ID [-f, --force]
```
{: codeblock}

## Removing access by using the API
{: #remove-access-api}
{: api}

Delete a policy by providing a policy ID and calling the [{{site.data.keyword.iamlong}} (IAM) Policy Management API](/apidocs/iam-policy-management#delete-policy){: external} as shown in the following sample request:

```bash
curl -X DELETE 'https://iam.cloud.ibm.com/v1/policies/$POLICY_ID' \
-H 'Authorization: Bearer $TOKEN' \
-H 'Content-Type: application/json'
```
{: curl}
{: codeblock}

```java
DeletePolicyOptions options = new DeletePolicyOptions.Builder()
      .policyId(examplePolicyId)
      .build();

service.deletePolicy(options).execute();
```
{: java}
{: codeblock}

```javascript
const params = {
  policyId: examplePolicyId,
};

iamPolicyManagementService.deletePolicy(params)
.then(res => {
  console.log(JSON.stringify(res, null, 2));
})
.catch(err => {
  console.warn(err)
});
```
{: javascript}
{: codeblock}

```python
response = iam_policy_management_service.delete_policy(
  policy_id=example_policy_id
).get_result()

print(json.dumps(response, indent=2))
```
{: python}
{: codeblock}

```go
options := iamPolicyManagementService.NewDeletePolicyOptions(
  examplePolicyID,
)

response, err := iamPolicyManagementService.DeletePolicy(options)
if err != nil {
  panic(err)
}
```
{: go}
{: codeblock}

A policy cannot be deleted if the subject ID contains a locked service ID.
{: note}

## Reviewing assigned access in the console
{: #review-your-access-console}
{: ui}

If you need to review your assigned access in an account that you've been added to, complete the following steps:

1. In the {{site.data.keyword.cloud_notm}} console, click **Manage** > **Access (IAM)**, and select **Users** or **Service IDs**, depending on which identity you want to review.
2. Select your name or the service ID.
3. Review the assigned access in the **Access policies** section.

If you need more access, you must contact the account owner to update your access or contact the administrator for the service or service instance to update the access policy.
{: tip}

## Reviewing assigned access by using the CLI
{: #review-your-access-cli}
{: cli}

If you need to review your assigned access in an account that you've been added to, you can use the [**`ibmcloud iam user-policies`**](/docs/cli?topic=cli-ibmcloud_commands_iam#ibmcloud_iam_user_policies) command. This example lists policies of user `name@example.com`:

```bash
ibmcloud iam user-policies name@example.com
```
{: codeblock} 

## Reviewing assigned access by using the API
{: #review-your-access-api}
{: api}

By using the API, you can only retrieve all policies in the account and filter by attribute values. You can check your assigned access in an account by going to **Manage** > **Users** > **your_name** > **Access policies** in the {{site.data.keyword.cloud_notm}} console. To retrieve policies, call the [{{site.data.keyword.iamlong}} (IAM) Policy Management API](/apidocs/iam-policy-management#list-policies){: external} as shown in the following sample request:

```bash
curl -X GET 'https://iam.cloud.ibm.com/v1/policies?account_id=$ACCOUNT_ID' \
-H 'Authorization: Bearer $TOKEN' \
-H 'Content-Type: application/json'
```
{: curl} 
{: codeblock}

```java
ListPoliciesOptions options = new ListPoliciesOptions.Builder()
    .accountId(exampleAccountId)
    .iamId(EXAMPLE_USER_ID)
    .build();

Response<PolicyList> response = service.listPolicies(options).execute();
PolicyList policyList = response.getResult();

System.out.println(policyList);
```
{: java} 
{: codeblock}

```javascript
const params = {
  accountId: exampleAccountId,
  iamId: exampleUserId
};

iamPolicyManagementService.listPolicies(params)
 .then(res => {
   console.log(JSON.stringify(res.result, null, 2));
 })
 .catch(err => {
   console.warn(err)
 });
```
{: javascript} 
{: codeblock} 

```python
policy_list = iam_policy_management_service.list_policies(
 account_id=example_account_id, iam_id=example_user_id
).get_result()

print(json.dumps(policy_list, indent=2))
```
{: python} 
{: codeblock} 

```go
options := iamPolicyManagementService.NewListPoliciesOptions(
 exampleAccountID,
)
options.SetIamID(exampleUserID)

policyList, response, err := iamPolicyManagementService.ListPolicies(options)
if err != nil {
 panic(err)
}
b, _ := json.MarshalIndent(policyList, "", "  ")
fmt.Println(string(b))
```
{: go} 
{: codeblock}
