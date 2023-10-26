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
Save it in your system, You can open notepad or IDE paste the html code and save it as index.html.
Go back to S3, click open the bucket name


### Step 7
With a successful upload the interesting thing here with the static website hosting, if we go to properties and we go all the way
down to the bottom again it should have produced a link and that is the static website hosting link. Go ahead and open it up by clicking on it. We should get a 403. So the reason we got a 403 is that by default our bucket is not public so if we go back over to S3 this is a security feature over here if we go all the way to the top and we go to permissions, we are blocking Public Access so this is not publicly accessible, another thing is that even if we have this turned on we need a bucket policy to make it accessible to the internet. You're allowed to create a bucket policy now what permissions are allowed there? So a 403 is a forbidden it means you do not have access, you're not allowed to get to it and it's totally okay. So those are two things that we would need. In order to create that so there are a few ways we can do this we could make this publicly accessible but what I'd
rather do is, I'd rather go create a cloudfront distribution, because that is the way that we should really serve our bucket. Cloudfront is a CDN and what it does is, it takes a copy of your website and caches it to a bunch of computers around the world so that when somebody, let's say in India is downloading my website in England it will serve up that HTML file to a computer that's nearby if that makes sense.

So coming back over to our diagram we have our bucket, we need our cloudfront distribution and when we set this up we'll hopefully set up our origin access controls and our bucket policy.

Well we don't want to make it public we want to keep it private because the thing is that we can make it public but we want everything to be forced through cloudfront as well because cloudfront can also attach additional Security Services like AWS firewall or whatever and that's just the the way it's going to work. 

To be clear this is the process that most people do in professional organizations in production level websites using cloudfront distribution or aquamire or something like that is the process for serving out web pages

### Step 8 
so what we'll do is we'll go back over to AWS and we're going to search for cloudfront, click on it, and create a new distribution Thing we need to do is choose our origin domain, so if we drop this down we have a few options now sometimes they'll say
point it to the S3 bucket and sometimes they'll say point it to the static website hosting URL.
I want to choose the correct bucket so I'm going to just scroll on down and look for it and go and select your origin domain notice right away it says this S3 bucket has static web hosting enabled if you plan to use the distribution as a website we recommend using S3 website endpoint rather than the bucket endpoint and this is very conditional based on your use case, I wish this had a little bit better description in terms of what use case you would use one over the other but I think that we're supposed to actually keep it this way. So it looks like the origin here is it actually is the website bucket domain name so it actually is the domain name so we'll go back here I was totally wrong and up here I'm going to actually use the website endpoint because that's what it wants so make sure you press that button it's using the static website hosting it's not always the case if you're using origin access identities the old one it might want the S3 bucket or it will change the way you do it one three two in order that's probably a good order two is bad it's bad but it's good right anyway so let's scroll on down and we have our HTTP only so we're gonna leave that to Port 80. because that's the protocol it's going to send over to that we'll scroll on down here it says origin path enter the URL path to append to the origin domain name we're going to leave that blank because we don't need to change that it has entered the name of this origin we aren't going to change that we'll keep going down here advanced settings we'll leave those alone HTTP and HTTPS is fine but I probably would want to set to HTTPS only, we'll leave this alone here restrict your access if you restrict access the viewer must use cloudfront sign URLs or cookies to access the content no we're going to leave that alone we have caching policies so cash policy caching optimize we're going to leave that alone am I ever going to change any of these options here we'll leave these alone for now I believe we do set these uh later. I'm gonna go to additional settings these look fine this looks fine just leave this alone that's expensive this is fine we'll go over this twice I realize there's a lot of options
we're not using C names because we're not using custom domain names we do that in the intermediate bootcamp um custom SSL no that'd probably be expensive or we'd use it from um one for our domain so we'll leave that alone okay we actually have something we can enter in default root object index.html

we'll go over this twice here um yeah I think that's that's it so we'll go all the way back to the top okay so yeah all we did is we had that static website hosting here and then we literally scrolled on down down to the ground and we put an index.html in here now there was one thing that I wanted to know is I wanted to know if it was going to use the SS SSL certificate for cloudfront and I think actually in terraform we actually have to tell it explicitly but here we don't I just want to tell you that when you use terraform or when you use cloud formation or any IAC tool just understand that the apis are not always the same as the uis so sometimes the UI does like three four things for you that you don't see but when you do encoding you actually have to know more about how the apis work and that's why I always encourage people learning cloud to start looking at the API sooner so you have a better idea of what that is but I think it's important to look at both the UI and the apis and the clis and the code to understand and try to get a mapping of where things are different if that makes sense I know that's a big mess anyway one other thing I want to do is put a description in because I found out with cloudfront they just really don't help you understand what these things are for, so I'm just going to go here and say Tarahouse example CDN Please fill in a description. Go ahead and create that distribution, and by the way this takes a little bit of time to create and that's the perfect opportunity to go take a break

We gotta just say we don't want WAFF say no to this because this is an extra cost and we'll go ahead and create that distribution so I want folks to go back and click on their distribution and then scroll to the right you can see that note comes in handy here because the rest is really not helpful in terms of information so make sure that you can see your description and then scroll all the right and tell me if the status is enabled. So now the question is will this work if we click into a distribution we get a custom domain name so this is a different domain name from the one that the S3 bucket generated, and the idea is that we're hoping that when we use this domain name it's going to point to S3 bucket and work and it may not work and I have an idea if it doesn't but let's go and see if it works so I'm going to copy it bring it back up to the browser and we still have a 403 the reason we have a 403 is that we probably don't have a bucket policy so coming back to our diagram we're supposed to have an origin access controls and a bucket policy we need those two things in order for it to work and we didn't exactly configure one we didn't see those options when we were doing that but what we'll do is we will go back over to cloudfront

it was called control access settings let's go over here and try origin access and take a look oh look here control settings and identities so it's actually separate from it that's interesting I never knew that so now we know where it is and we'll go ahead and create a control setting So what I want you to do is go over to origin access in cloudfront right and click on this I'll get let's reset the counter we're going to do high with numbers here so we'll need to create a new uh control access I'm going to call this one Tara Town Tara house well hold on it says valid characters letters numbers multiple special characters and say Cara house example arrowhead's example well we don't need the description we'll go down below we say Do not sign requests sign requests we definitely want to sign requests we have an origin type of S3 so that sounds good I'm going to go ahead and create that so I've now created our origin Access Control. 

So how do we actually attach that to our cloudfront distribution in our distribution it looks like when we create our distribution we can do that over here origin access I didn't even notice that when we were doing that so I guess that's an option I wonder if when we created it we could have created it all on the spot I bet we could have eh but wherever that was I I didn't notice it so what we'll do is we'll go back over to our distribution at the top and we'll click into this one and we'll go to behaviors and we'll edit our Behavior because that's where it looked like it was under okay can we change that after the fact I don't think we can
let's go back to that where was that here create distribution in origin configuration in the origin drop down list you can optionally configure origin path to append the origin domain name I don't know if we can change that retroactively you should sign the request the reason you want the request signed is that is what's going to allow it to get access to the the private bucket because they're signed at least that's the way I understand it

I'm going to delete this distribution I know this takes forever but we're going to do it anyway we're going to delete it or sorry 

**disable it first** After disabling it then delete

I'm going to create a new distribution I'm gonna go and choose that uh that S3 address I'm going to ignore that option I'm going to go here instead I'm going to drop down this I'm going to go to here I'm going to scroll on down I'm going to say no to that I'm going to leave all this alone I'm going to set this to index.html and I'm going to say Tara house example two I'm going to create the distribution complete the distribution configuration by allowing read access to cloudfront so that's the last part of it was that even though we created the distribution with the access control policy we still have to have this bucket policy and that is this last part here if we go back to this one where did you go what we need to do is we need to copy it so I'm going to copy that policy and I'm going to go to my S3 bucket permissions right here to edit the bucket policy and paste. I'm going to go back over to cloudfront after it's deployed succesfully it worked. And the most impressive part of this is the fact that the S3 bucket is still
blocking all access that's the cool thing

I'm gonna just start cleaning this up so the way we clean this up you just saw that we we did that before how we deleted that so we'll go ahead and disable this one I think that we can't delete the bucket until well first we got to empty the bucket right the other part of it is that I don't know if it will let us delete the bucket if the the distributions points to it and we'll find that out here in just a moment so go ahead and empty the bucket
okay and I'm going to go and see I can delete the bucket now I'm going to see if it'll let me delete this bucket oh it did oh so I didn't know that so you can delete interesting that's very interesting right unless this one was done because it is disabling maybe it knows it's in a disabled flag but um yeah I would have not thought you'd been able to do that but also it's it's pointing at maybe if it's pointing at the domain name um it doesn't resolve the domain name and so it's not checking the S3 bucket right so what if I know that we set it as S3 bucket but because we're using a cross origin origin access control and when we look in the terraform code it actually uses the domain name




## Automated Setup with Terraform
For the automated setup, we'll leverage Terraform, an infrastructure as code (IaC) tool. Terraform allows us to define our infrastructure in code, making it easy to manage, version, and replicate our static website hosting environment.

[Include Terraform code and instructions for setting up the environment automatically.]

By comparing the manual setup with ClickOps to the automated approach with Terraform, you'll gain a better understanding of the underlying infrastructure and see the benefits of automating repetitive tasks. This knowledge will be valuable as you delve deeper into cloud operations.


