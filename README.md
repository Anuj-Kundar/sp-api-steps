# sp-api-setps

Requirements:
===================
1. Aws Free Tier account for configuring IAM Roles.
2. [Professional-level Amazon Seller account](https://sellercentral.amazon.com/)

Access Amazon's Selling Partner API 
===================
## Complete Developer Application in Seller Central:
1. apply for a Amazon Marketplace developer account! Go to [sellercentral.amazon.com](https://sellercentral.amazon.com)
2. then click `Partner Network > Develop Apps`.

3. then Click `Proceed to Developer Profile` will land you on form with a few different sections. Fill `Contact Information` which is straightforward, 
4. next is  `Data Access`:
The first dropdown asks whether you're planning to use the seller APIs to make applications for your own Seller account, or applications that will be distributed to other Selling Partners. This totally depends on your use case. If you're not planning on distributing the tools you make, choose the first option. Otherwise, choose the second.
5. Next is a list of roles, each corresponding to a set of resources and operations that are available via the Selling Partner API. As noted on the application form, some of the roles grant access to Personally Identifiable Information (PII), so tighter security requirements are placed on them. I recommend not selecting any of these roles unless you absolutely need to – they will slow down the application process and require you to implement complex data security me asures, and you can always request access to them later if necessary.
6. Next move on to the `Use Cases` section. This is highly dependent on what you plan to build, but in your answers, I recommend restating their questions...For some reason, they seem to react well to that. For example, when answering this prompt:

> Describe the application or feature(s) you intend to build using the functionality in the requested roles.

Start your answer something like this:

> The application we plan to build using the roles we selected above will...

7. The "correct" answers to the `Security Controls` questions can be derived from what I wrote above about the policies around handling Amazon data. I recommend complying with their policies, because they can shut you down whenever they want if you don't.

8. Once you're done with the whole form, click `Register`. Then hurry up and wait! It could take Amazon anywhere from a few minutes to a few weeks to approve the application, but it will probably be somewhere in the middle. In the meantime, let's get started registering your (future) Selling Partner API application.

Registering your Selling Partner API application
================================================

Overview:
1.  Login to AWS account.
2.  Create an IAM user that will eventually be connected to your Selling Partner API developer credentials.
3.  Create an inline policy on the IAM user that grants the user access to the SP API (this is where I'm telling you to do something different from what Amazon's instructions say—I'll explain why below).
4.  Register your new SP API application! (Your developer application has to be approved first.)

For steps 1 and 2, follow this guides->[step 1](https://youtu.be/EAJOsBNRIQA),[step 2](https://youtu.be/KSjPTqNBlGc) 

Once you've completed Step 2, navigate to the IAM user you just created by clicking on its username in the list of IAM users. Then to grant the user access to the SP API:

1.  Click **Add inline policy** in the upper right
2.  Select the **JSON** tab
3.  Paste the following JSON into the editor:
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
    
4.  Click **Review Policy**, and name your policy .
5.  Click **Create policy**

 Here we are Done configuring IAM and can ignore steps 3-5 of Amazon's guide on registering our SP API application.

Connecting to Selling Partner API
================================================
Use this PHP client library [docs](https://github.com/jlevers/selling-partner-api#setup)

[access sp api](https://www.highsidelabs.co/blog/selling-partner-api-access)

[How to use Selling Partner API PHP library ](https://www.highsidelabs.co/blog/spapi-php-library)








<!-- Go to `Developer central` and  Create new app.
 ![image](https://user-images.githubusercontent.com/89484481/217552820-109fa024-4819-42a7-8eb6-eeffdc8b91ec.png) -->


