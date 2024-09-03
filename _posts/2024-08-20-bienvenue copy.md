---
layout: post  
title: "Why and How to Migrate Your WordPress from OVH to Static AWS S3"  
date: 2024-08-20  
tags: [WordPress, OVH, AWS S3, CloudFront, Staatic, Laragon]  
categories: english  
---

## Why Migrate Your WordPress to Static AWS?

Migrating a WordPress site to a static version hosted on AWS S3 offers numerous advantages.

It might seem a bit complex at first, but the benefits in terms of performance, security, and cost savings are undeniable. In this article, I'll explain how to make this migration and why it's a great idea.

### The Benefits

- **Speed and Performance**: Static sites are fast. Really fast. Unlike a traditional WordPress site, a static site loads instantly because the content is already prepared and doesn't need to query a database. If you're using AWS CloudFront to distribute your content, it's even faster.

- **Security and Maintenance**: By switching to a static site, you eliminate many common WordPress security vulnerabilities, such as SQL injection attacks or plugin vulnerabilities. And the best part? Less maintenance! No more dealing with frequent WordPress updates or plugin issues.

- **Cost Reduction**: Hosting a static site on AWS S3 is often much cheaper than hosting a dynamic WordPress site on a server. You only pay for the storage and bandwidth used, without the hassle of server management.

## Step-by-Step Migration Guide

### Context

I had a WordPress site hosted on OVH with several domain names, and I wanted to consolidate everything on AWS. So, I exported my WordPress site locally, then converted it into a static site using Staatic. After that, I uploaded the files to Amazon S3, enabled website mode, and used CloudFront to distribute my content.

### Tools Used

- [LocalWP](https://localwp.com/): A free tool for creating a WordPress development environment on your computer.
- [Staatic](https://staatic.com/): A WordPress plugin that converts your dynamic site into a static site, perfect for AWS S3.
- [Laragon](https://laragon.org/): A portable, fast, and powerful development environment for PHP, Node.js, Python, Java, Go, Ruby, etc.

### Issues Encountered

#### Local Export

1. **Avoid exporting Staatic directly from OVH**: The process is resource-intensive (RAM, CPU, storage), which can cause issues. It's better to perform this task on a local version of your site.

#### Blocked by Wordfence on the Live Version

When I tried to export the site directly from OVH, Wordfence, a security plugin, blocked my IP address, thinking it was suspicious activity.

**Error Details**:
- **Blocked IP Address**: XXXXXX
- **Reason**: "Exceeded the maximum global requests per minute for crawlers or humans."
- **Block Duration**: 1 month

To avoid this kind of problem, it is crucial to perform the Staatic export on a local version of your site.

### Action List

#### Step 1: Local Export

The first step is to get an offline version of your WordPress site. I used LocalWP by Flywheel, but since I encountered issues importing the site, I eventually switched to Laragon.

##### Procedure with Laragon

1. **Quick creation of a WordPress environment**:
   - Quick create -> WordPress
   - Access the generated local URL
   - Choose the language during WordPress installation

2. **Migrating files and database**:
   - Before setting up the WordPress username and password, copy the original "wp-content" folder (from the live site) and paste it into the new local WordPress folder.
   - Import the original SQL file via phpMyAdmin to replace the local database.

3. **Error resolution**:
   - **Enable SSL**: To fix 404 errors, enable SSL.
   - **Modify the `php.ini` file**: Increase the allocated memory by changing `memory_limit = 128M` to `memory_limit = 512M`.
   - **Install the `php_imagemagick` extension**: To handle images correctly. Edit the `php.ini` file and add `extension=php_imagemagick.dll`, then restart Apache.

4. **Disable unnecessary plugins**:
   - **Disable Wordfence**: Wordfence is not needed in offline mode. 
   - **Disable Autoptimize**: Autoptimize was injecting JavaScript and CSS, which caused issues.

#### Step 2: Static Export with Staatic

Once the local modifications were done, I used Staatic to export the site as a static version. I also created another folder in Laragon, for example `wordpress-static`, to test this static version before deploying it to AWS.

#### Step 3: Deployment on AWS S3 and CloudFront

After the successful static export, I uploaded the files to Amazon S3, enabled website mode, and configured CloudFront to distribute the content.

Staatic allows you to configure S3 directly from the interface, making it easy to automatically deploy your site.

## What's Next?

Migrating your WordPress site to a static version on AWS requires some preparation, but the benefits in terms of performance, security, and cost are undeniable.

Once the migration is done, you can focus on improving your site without worrying about WordPress updates or security threats. Also, explore other possibilities offered by AWS, such as deployment automation.

For me, the main advantage is no longer having to pay for OVH hosting, which represents a significant saving. In the end, I save money, and my site is faster, more secure, and more stable. I can focus on what really matters: creating content for my site.

## Useful Links

- [Import/Export a WordPress Site - Local (localwp.com)](https://localwp.com/help-docs/getting-started/how-to-import-a-wordpress-site-into-local/?utm_source=local-app&utm_medium=local-internal&utm_content=local-import-help-doc&utm_campaign=local)
- [Laragon Documentation](https://laragon.org/docs/quick-add)
- [PHP Imagick Setup](https://www.php.net/manual/en/imagick.setup.php)
- [Laragon Quick Add](https://laragon.org/docs/quick-add)
- [Staatic Documentation](https://staatic.com/documentation)
- [AMPPS](http://ampps.com/download) - Note: AMPPS is now paid and requires a premium version to manage WordPress, which is not ideal.

---

You can also follow me on social media to stay updated with the latest posts.

[LinkedIn](https://www.linkedin.com/in/gatienboquet/) | [GitHub](https://github.com/gatienboquet)
