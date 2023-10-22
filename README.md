# Creating a Static Website Host: Manual vs. Automated (with Terraform)

Understanding the manual setup of a static website host is an essential first step before automating the entire process, especially if you're new to the cloud space. Just like how you learn basic math before using a calculator, getting hands-on experience by clicking through the cloud resources helps you comprehend how everything is connected. In this blog post, we'll explore the manual setup process (using ClickOps) and the automated approach with Terraform for creating a static website host.

## Resources
To create a static website host, we'll work with the following AWS resources:

- **S3 (Simple Storage Service):** S3 serves as our storage and content delivery solution.
- **CloudFront Distribution:** We'll use CloudFront to distribute our website content efficiently.
- **Origin Access Controls:** These controls help secure our resources and protect our content.

## Manual Setup (ClickOps)
In the manual setup, we'll navigate through the AWS Management Console to create an S3 bucket, configure static website hosting, and set up a CloudFront distribution. This hands-on approach provides valuable insights into how these components are interconnected.

### Step 1
Log in to your AWS Account and search for S3. Then click into S3
!()[] 

### Step 2
S3 buckets is a form of cloud storage in some sense it's kind of like but it's not but it's kind of like uh Google Drive it's kind of like Dropbox it's kind of like iCloud storage I'm not a I'm not a huge uh digital files in the cloud and you don't have to worry about the size of the drive you just dump things in there and you just pay for what you consume what you store and your downloads uh
like in traffic if that makes sense but the way it's different is that it's not as uh uh user friendly as something like Dropbox or Google Drive but it doesn't need to be because we're using it to integrate into our applications so it's great for developers it's great for cloud Engineers but for regular consumers you wouldn't use this as a Dropbox replacement um what you do is you probably build a layer on top of this and make your own Dropbox Google Drive is good you just drop things in and they go 

So you can see I already have some buckets here if this is a new account for you you should not have any buckets here
What we want to do is we want to go ahead and create a new bucket so go ahead and hit the nice big orange button
!()[]

### Step 3
So the first thing we want to do is name our bucket when we name our buckets our bucket names are within the global namespace what does that mean names are like domain names because they literally will produce a subdomain on a domain name so whatever the restrictions that you have for a domain name for naming are going to for a subdomain are going to apply for the bucket name the other part is that it has to be unique so the idea again is that these Global namespaces are Global so the idea is that I can name this whatever I want you can't name it the same thing.

S3 is a global Service but why do you choose your AWS region because it's both so the namespace is global but the storage that they put it in is regional so you have so you know where you have to pick the region so that you know which hard drives effectively it gets written to but because it's leveraging the global DNS service it has to be globally from a regional perspective

I'm going to choose us-east-1 you can choose your own prefered region and then we'll scroll on down we have we have some object ownership permissions uh We're Gonna Leave This disabled we do not need ACL this is for allowing people remote access using ACL controls into yours uh there hasn't been many use cases for me to use ACL in any project but I'm sure there is some case we have the block Public Access setting for this but this bucket this feature is really interesting because over the years AWS keeps trying to make sure people don't expose their buckets and they've made this feature they keep adding to it like I feel like there's gonna be like 10 check marks at one point being like are you sure you want to make it public we are going to keep our buckets um locked down we're never going to turn keep that check marked

we have bucket versioning generally it's a good idea to turn on bucket versioning we're not doing this on this blog, it's a good idea to do it but I also do find that it does increase the complexity when you are trying to get rid of things in your bucket but what bucket versioning does it allows you to add a every time you push to the same named key in your bucket it will create a new version on top of it that you'll access um kind of making like an onion if that makes sense but uh in order to delete I think you have to tear everything down it's quite the headaches so we're not going to do that here but this is really good if let's say somebody got into your account and they tried deleting things then they can only delete up to a certain point uh which is really good. so we've got bucket versioning disabled 

For encryption yeah it's encrypted by default so we have SE S3 was that always the case oh you can disable it I don't know why you'd want to no but I guess you could disable it if you'd like.

we'll go down here and we have some advanced settings uh there's object lock so I think that's really good yeah preventing object deletion so I was talking about this in versioning but I forgot to mention that I believe you have to have this turned on in order to have versioning in and so that was my thought is that versioning forces uh you to have to turn object lock on and that kind of helps there and it does say there down below object Lock Works only in version buckets and object lock automatically enables bucket versioning

We'll go ahead and create our bucket and we'll click that
!()[]

### Step 4
Check if the bucket is listed
!()[] 
Click the bucket name

### Step 5
So the idea is that we want to set up static website hosting. We're going to enable static website hosting. click on properties and we'll go all the way that down to the ground and we'll find static website hosting and go ahead and hit edit, we'll go here and we'll have it enabled. Fill the form to include index.html and error.html we have some redirection rules so if you'd like we're not going to there are ways to uh redirect things to other places also notice up here we have two options host a static website and redirect requests for an object I find that it's very common that when you're setting up static website hosting for like a domain that you own you will set up an S3 bucket with the domain name and then you'll set one up with www and you'll use redirect to redirect one bucket to another one we are not doing that because we are not using custom domains we're just going to use the domain that we're going to get from cloudfront. So going down below here there is this thing but all you need to do is set up the index.html the error.html and we'll save those changes.
!()[]

### Step 6
We need to upload some index.html code. Copy the code below or use your own html code
```html
```


## Automated Setup with Terraform
For the automated setup, we'll leverage Terraform, an infrastructure as code (IaC) tool. Terraform allows us to define our infrastructure in code, making it easy to manage, version, and replicate our static website hosting environment.

[Include Terraform code and instructions for setting up the environment automatically.]

By comparing the manual setup with ClickOps to the automated approach with Terraform, you'll gain a better understanding of the underlying infrastructure and see the benefits of automating repetitive tasks. This knowledge will be valuable as you delve deeper into cloud operations.


