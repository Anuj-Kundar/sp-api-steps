# sp-api-setps

Requirements:
===================
1. Aws Free Tier account for configuring IAM Roles.
2. [Professional-level Amazon Seller account](https://sellercentral.amazon.com/)

[](#set-up-aws-iam-configuration)Set up AWS IAM configuration
===================

Overiew of the process looks like this:

1.  Create an AWS account, if you don't have one already.
2.  Create an IAM user that will eventually be connected to your Selling Partner API developer credentials.
3.  Create an IAM policy that grants access to the Selling Partner API to any resource it is attached to.
4.  Create an IAM role and attach the policy from the previous step to it.
5.  Add a policy to the user to allow it to assume the permissions of the role.
6.  Register your new SP API application! (Your developer application has to be approved first.)

Let's go through it.

#### [](#create-an-aws-account)Create an AWS account

This is relatively straightforward – if you don't already have an AWS account, go to [aws.amazon.com](https://aws.amazon.com) and follow the prompts to make an account. If you already have an account, log into it and go to Step 2.

#### [](#create-an-iam-user)Create an IAM user

Search for `IAM` in the top search bar, go to the result with the same name, click `Users` in the left sidebar, and then `Add Users`.

![image](https://user-images.githubusercontent.com/89484481/218149801-a9f73513-c3ba-45a6-84e6-4267beedd2e8.png)
</br>
![image](https://user-images.githubusercontent.com/89484481/218150193-9dafb239-ec10-4e38-9ed0-3c20f14772d5.png)


On the next screen, name your user whatever you'd like – I named mine `SPAPI`. You don't need to enable console access.

You can click `Next` through the permissions and tagging steps, and then click `Create` to finish making the user. You'll be redirected to a list showing your IAM users – click the name of the one you just made, and copy the 12-digit number from the role's ARN – we'll need it later. Then go to the `Security credentials` tab, scroll down to `Access keys`, and select `Create access key`. Choose the `Application running outside of AWS` use case, click `Next`, and then click `Create access key` one more time.

Download the CSV with your credentials, and make sure to save it somewhere safe – if you lose these credentials do, you'll have to create a new set.
![image](https://user-images.githubusercontent.com/89484481/218152185-87bab9fc-58db-428b-82d4-46a2146e863c.png)


#### [](#create-an-iam-policy)Create an IAM policy

Now we need to make an IAM policy, which is basically a ruleset that can be connected to other IAM resources to give them permission to do particular things – in this case, permission to make calls to the Selling Partner API.

Click `Policies` in the left sidebar

![image](https://user-images.githubusercontent.com/89484481/218152842-63d48a5e-b184-4e6c-bd7a-3a2b1942ac9b.png)


then `Create policy`. 
![image](https://user-images.githubusercontent.com/89484481/218153042-d73672b0-c36a-4ee1-8041-0ccd66dbd5dc.png)


Select the `JSON` tab, and replace what's there with this:
```json
    {
      "Version": "2012-10-17",
      "Statement": [
        {
          "Effect": "Allow",
          "Action": "execute-api:Invoke",
          "Resource": "arn:aws:execute-api:*:*:*"
        }
      ]
    }
```
    

Skip past the tagging step, and then name the policy whatever you'd like (I chose `ExecuteAPI`). Finally, click `Create policy` to finish this step.

#### [](#create-an-iam-role)Create an IAM role

If you think about IAM users as end users and IAM policies as permissions, IAM roles are the bridge between them. Permissions can be assigned to roles, and then users can assume the permissions granted to the role for a limited time via a short-lived token.

Let's create a role that will allow the IAM user to get access to the SP API via the policy we just created.

Click `Roles` in the sidebar, click `Create role`. Under `Select trusted entity`, choose `AWS account > This account`.

![select role trusted entity](https://user-images.githubusercontent.com/89484481/218148724-bd7fa5b1-4dfd-41c8-9129-a6acffed4715.png)

Click `Next` to the permissions page. There, select the policy you just created from the list of policies. Click `Next`, name your role (whatever name you want, you know the drill ;), and click `Create role`. Like when you created the user, select the role you just created from the list of all your IAM roles and save its ARN – we'll need it later.

#### [](#allow-the-user-to-assume-the-permissions-of-the-role)Allow the user to assume the permissions of the role

In order for our IAM user to be able to make use of the `ExecuteApi` policy on our role, we need to set up a policy so that the user can "assume" the role temporarily. This is done with Amazon's Security Token Service (STS). Open up the user's permissions by selecting it from the list of users on the `Users` page, and go to `Add permissions > Add inline policy`. Then select the `JSON` tab, and paste this in:

```json
    {
      "Version": "2012-10-17",
      "Statement": [
        {
          "Sid": "VisualEditor0",
          "Effect": "Allow",
          "Action": "sts:AssumeRole",
          "Resource": "arn:aws:iam::<id>:role/<role_name>"
        }
      ]
    }
```
    

Replace the `Resource` value with your role ARN from the last step. Click `Review policy`, name this whatever you want , and click `Create policy`.

Here we are done with IAM.

Access Amazon's Selling Partner API 
===================
## Complete Developer Application in Seller Central:
1. apply for a Amazon Marketplace developer account! Go to [sellercentral.amazon.com](https://sellercentral.amazon.com)
2. then click `Partner Network > Develop Apps`.
![image](https://user-images.githubusercontent.com/89484481/218136662-57146edf-25b9-46a0-b85d-f5dda18947a4.png)
![image](https://user-images.githubusercontent.com/89484481/218136791-8974ae04-7658-4adf-b446-a678e2d85edd.png)
![image](https://user-images.githubusercontent.com/89484481/218137154-9201c969-224b-48bd-8ebd-ac1f1c8d9025.png)

3.this will take you to `Developer central` here click on add new app.

 ![image](https://user-images.githubusercontent.com/89484481/217552820-109fa024-4819-42a7-8eb6-eeffdc8b91ec.png)
 this will take you to a registration  form.

![image](https://user-images.githubusercontent.com/89484481/218138206-941f2dd0-2a28-4751-b86b-5e03e09688c5.png)

Fill the form for reference watch this [video](https://youtu.be/KSjPTqNBlGc?list=PLyrrqKCT7jFKENJO9n_Y68-5o2GZLgLUU&t=235). 
Fill `Contact Information` which is straightforward, 


4. next is  `Data Access`:
The first dropdown asks whether you're planning to use the seller APIs to make applications for your own Seller account, or applications that will be distributed to other Selling Partners. This totally depends on your use case. If you're not planning on distributing the tools you make, choose the first option. Otherwise, choose the second.

5. Next is a list of roles, each corresponding to a set of resources and operations that are available via the Selling Partner API. As noted on the application form, some of the roles grant access to Personally Identifiable Information (PII), so tighter security requirements are placed on them. I recommend not selecting any of these roles unless you absolutely need to – they will slow down the application process and require you to implement complex data security me asures, and you can always request access to them later if necessary.

6. Next move on to the `Use Cases` section. This is highly dependent on what you plan to build, but in your answers, I recommend restating their questions...For some reason, they seem to react well to that. For example, when answering this prompt:

> Describe the application or feature(s) you intend to build using the functionality in the requested roles.

Start your answer something like this:

> The application we plan to build using the roles we selected above will...

7. The "correct" answers to the `Security Controls` questions can be derived from what I wrote above about the policies around handling Amazon data. I recommend complying with their policies, because they can shut you down whenever they want if you don't.

8. Once you're done with the whole form, click `Register`. Then  wait! It could take Amazon anywhere from a few minutes to a few weeks to approve the application, but it will probably be somewhere in the middle. In the meantime, let's get started registering your (future) Selling Partner API application.







