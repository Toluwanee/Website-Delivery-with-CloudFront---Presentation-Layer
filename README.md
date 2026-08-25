<img src="https://cdn.prod.website-files.com/677c400686e724409a5a7409/6790ad949cf622dc8dcd9fe4_nextwork-logo-leather.svg" alt="NextWork" width="300" />

# Website Delivery with CloudFront

**Project Link:** [View Project](http://nextwork.ai/projects/aws-networks-cloudfront)

**Author:** Toluwalope Oni  
**Email:** toluwalope9@gmail.com

---

## Website Delivery with CloudFront

![Image](http://nextwork.ai/compassionate_orange_mysterious_tuke/uploads/aws-networks-cloudfront_1dddddwe)

---

## Introducing Today's Project!

In this project, I will demonstrate how to deploy a Website with Content Delivery Network like Cloudfront. I'm doing this project to learn how to work with content delivery networks, and understand how I can optimize static and dynamic websites to have fast loading speed.

### Tools and concepts

In this project, I utilized Amazon S3 for secure, static asset hosting and integrated Amazon CloudFront to distribute the website globally. 

I mastered core content delivery concepts, including global caching at edge locations, configuring a default root object for seamless homepage routing, and securing origins using Origin Access Control (OAC). 

Additionally, I gained a critical understanding of AWS IAM and bucket permissions: disabling 'Block Public Access' is only a gateway step; to actually serve content, an explicit S3 bucket policy must be applied to define and allow access.

### Project reflection

This project took me approximately 4hrs. The most challenging part was loading the homepage after the OAC was enabled on S3 buckets, somehow index.html could not be accessed. I had to access via /index.html, then I. later discovered the CDN site became directly accessible after I doing this. 

 CloudFront Console -> Distributions -> select your distribution -> General -> Edit. Set Default Root Object to index.html

It was most rewarding to see that the time difference between CDN load time and direct S3 accessible site was significant.

I chose to do this project today because I wanted to learn about how content delivery works, and it met my need. Something that would make learning with NextWork even better is that nextwork will guide you through the simplest of steps, and that really builds understanding

---

## Set Up S3 and Website Files

I started the project by first creating an S3 bucket to hold my website files for accessibility. I can't use CloudFront for this task - holding and storing website files - because CloudFront is a Content Delivery Network that only hosts content that is stored somewhere else.

The three files that make up my website are; 
index.html, which is the main file for a website. It's where you organise the text, pictures, and everything that makes up your webpage,
style.css, which is where you write down the visual appearance of your website's HTML elements,
and script.js, which refers to a JavaScript file that adds interaction to your website. It's where you would write the instructions for making things on your website move or change when you click a button or submit a form.

I validated that my website files work by opening the index.html, style.css, and script.js with my preferred browser.

![Image](http://nextwork.ai/compassionate_orange_mysterious_tuke/uploads/aws-networks-cloudfront_qgo7wcd3)

---

## Exploring Amazon CloudFront

Amazon CloudFront is a content delivery network, which means it speeds up the caching and distribution of my static and dynamic web content such as .html, .css, .js. Businesses and developers use CloudFront because it caches website content in multiple servers around the world. When a user requests content that you're serving with CloudFront, the request is routed to the edge location that provides the lowest latency (time delay), so content is delivered with the best possible performance.

To use Amazon CloudFront, you set up distributions, which are sets of instruction that tells cloudfront how to deliver my content, it specifies where my website content is stored - the origin, how it should be called and cached, and who has permissions to access it. I set up a distribution for my frontend files - index.html, style.css, and script.js. The origin is the storage location of my files, which for this setup is Amazon S3 bucket.

My CloudFront distribution's default root object is the index.html in the amazon S3 bucket. This means that when a user requests the root URL of my website, CloudFront will automatically fetch and display the index,html file from S3, ensuring they see the homepage instead of receiving an access error.

![Image](http://nextwork.ai/compassionate_orange_mysterious_tuke/uploads/aws-networks-cloudfront_qgo7wcdt)

---

## Handling Access Issues

When I tried visiting my distributed website, I ran into an access denied error because I have not explicitly grant CloudFront the necessary permissions to access my S3 bucket...

My distribution's origin access settings were already set to Origin Access Control (OAC). This is because AWS automatically configured OAC for security when I selected the Single Website or App setup wizard, preventing the origin from being public. The reason for access denied error was because access was only enabled in the cloudfront distribution, but it has not yet been allowed in the S3 bucket policy. S3 buckets allow private access by default.

To resolve the error, I configured Origin Access Control (OAC) within the Origin settings of my CloudFront distribution. OAC serves as a secure, dedicated identity for CloudFront. By using OAC, I can keep my S3 bucket and its objects completely private from the public internet. However, setup is a two-part process: after enabling OAC in CloudFront, I also had to update the S3 bucket policy to explicitly trust this OAC, ensuring a secure and restricted handshake between both services.

![Image](http://nextwork.ai/compassionate_orange_mysterious_tuke/uploads/aws-networks-cloudfront_egrhntyu)

---

## Updating S3 Permissions

Once I set up my OAC, I still needed to update my bucket policy because the S3 bucket's policy still needs to explicitly grant the OAC permission to the bucket's contents.

Creating an OAC automatically gives me a policy I could copy to the S3 bucket permissions policy, which grants OAC access to the files in the S3 buckets.

![Image](http://nextwork.ai/compassionate_orange_mysterious_tuke/uploads/aws-networks-cloudfront_eg98ntyu)

---

## S3 vs CloudFront for Hosting

For my project extension, I'm comparing Compare S3 vs CloudFront based on the hosted website's URLs and permissions. I initially had an error with static website hosting because permissions to allow public access to the S3 buckets has not been given by the S3 buckets, permissions were only given to the CloudFront's OAC

I tried resolving this by enabling public access to the S3 buckets but it still failed, enabling public access doesn't grant permission to access the objects. It simply stops blocking all public access attempts. I still ran into an error because I still need a bucket policy to explicitly grant permissions. When I set up a bucket policy that allows public read access,I'm telling AWS to let anyone on the internet to read the files in the bucket.

I could finally see my S3 hosted website when I added this statement to my S3 policy
 {
    "Sid": "PublicReadGetObject",
    "Effect": "Allow",
    "Principal": "*",
    "Action": "s3:GetObject",
    "Resource": "arn:aws:s3:::nextwork-three-tier-enter your name-random characters/*"
}


This worked because the json statetemt explicitly enabled public access in S3 bucket permission policy, beyond just disallowing it in the Block public access settings

Compared to the permission settings for my CloudFront distribution, using S3 meant I had to make my entire storage bucket publicly accessible to the internet. I preferred the CloudFront approach because it keeps my S3 bucket completely private and secures my files, allowing only authenticated traffic through the CDN.



---

## S3 vs CloudFront Load Times

Load time means how quickly the content on a website loads. The load times for the CloudFront site were faster than the S3 site because CloudFront's CDN caches content closer to users globally, while S3 static website hosting serves files directly from a single region.

...

A business would prefer CloudFront when faster delivery is necessary and the business wants to the storage bucket to stay private. S3 static website hosting might be sufficient when speed is not important, they need quick setup, and can allow the bucket's contents to be completely public 

![Image](http://nextwork.ai/compassionate_orange_mysterious_tuke/uploads/aws-networks-cloudfront_12verpuh)

---

---
