---
title: How to deploy your Django application to Railway with a custom Domain
date: '2023-01-25'
tags: ['Django', 'Python', 'Railway', 'Deployment','railway-app']
draft: false
summary: 'A step-by-step guide on how to deploy your Django application to Railway with a custom Domain'
---

Originally posted on [Hashnode.](https://meekkaran.hashnode.dev/how-to-deploy-your-django-application-to-railway-with-a-custom-domain)


By the time you are reading this, I believe you already have a website running on a development environment, Whether it's a Django application or react application running on your localhost.

Before you can host a website externally you're first going to have to:

- Make a few changes to your project settings.

- Choose an environment for hosting the Django app. In our case, we are using Railway

- Choose an environment for hosting any static files.

- Set up a production-level infrastructure for serving your website.

Expose your app to your custom domain. In our case, we are getting a domain from Cloudflare and exposing our app there.

I will be providing some guidance on your options for choosing a hosting site, a brief overview of what you need to do in order to get your app/website ready for production, a working example of how to install the app/website onto the Railway cloud hosting service and deploying it to your domain on cloudflare.

For clear demonstration, I will be publishing a simple Ecommerce Website built in Django to railway.

## Railway
### Why Railway
Railway has a free starter plan which makes it so affordable for developers to deploy their sites.

Railway takes care of most of the infrastructure so you don't have to. You do not have to worry about servers, load balancers, reverse proxies, and so on.

Railway has a faster and softer learning curve than many other alternatives due to its focus on developer experience for both development and deployment.

It is very reliable with predictable pricing and easy scaling of your app.

### How it works
Web applications are each run in their own isolated and independent virtualized container.

Railway has to set up the appropriate environment and dependencies and understand launching to execute your application.

since our example is a Django app, this information is provided in text files:

**runtime.txt:** states the programming language and version to use.

**requirements.txt:** lists the Python dependencies needed for your site, including Django.

**Procfile:** A list of processes to be executed to start the web application. For Django, this will usually be the Gunicorn web application server (with a .wsgi script).

**wsgi.py:** WSGI configuration to call our Django application in the Railway environment.

Once the application is running it can configure itself using the information provided in environment variables. For example, an application that uses a database can get the address using the variable DATABASE_URL. The database service itself may be hosted by Railway or some other provider.

Railway has a site that developers interact with using a special Command Line Interface (CLI) tool which allows you to associate a local GitHub repository with a railway project, upload the repository from the local branch to the live site, inspect the logs of the running process, set and get configuration variables and much more.

Another useful feature is that you can use the CLI to run your local project with the same environment variables as the live project.

To get the application working on Railway:

1. Put your application into a git repository

2. For our Django web application, we'll need to add the files above

3. Make changes to handle static files

4. Set up a Railway account and deploy your website.

## Deploying
1. Creating the application repository on GitHub
Commit your files to your local GitHub account. Railway is integrated with GitHub and the git version control system therefore, you can configure Railway to automatically deploy changes to a specific repo or branch on Git Hub.

2. Update the app for Railway
Some changes need to be made to your application to get it to work on Railway.

Some files have to be added:

*Procfile*
A Procfile is the web application "entry point". It lists the commands that will be executed by Railway to start your site.

Create the file Procfile (with no file extension) in the root of your project folder and copy/paste the following text:

*web: gunicorn ecommerce.wsgi --log-file -*

![Alt Text](/static/images/blog/deploy/procfile.JPG)

The web: prefix tells Railway that this is a web process and can be sent HTTP traffic.

We then start the gunicorn process, a popular web application server, passing its configuration information into the module ecommerce.wsgi (created with our application skeleton: /ecommerce/wsgi.py).

*Gunicorn*
Gunicorn is a pure-Python HTTP server that is commonly used for serving Django WSGI applications on Railway (as referenced in the Procfile above).

We'll install gunicorn locally so that it becomes part of our requirements for Railway to set up on the remote server.

Install Gunicorn locally on the command line using pip:

pip install gunicorn

3. Make changes to handle static files
changes need to be made to handle static files since during development, both Django and Django development web server is used to serve both static files(CSS, Javascript, etc and dynamic HTML files making it inefficient for static files since requests must pass through Django as much as Django has nothing to do with them. All of these would have a performance impact on production.

To avoid problems, we separate the static files from the Django web application In the production environment, making it easier to serve them directly from the web server or a content delivery network (CDN).

The important setting variables are:

STATIC_URL: This is the base URL location from which static files will be served, for example on a CDN.

STATIC_ROOT: This is the absolute path to a directory where Django's collectstatic tool will gather any static files referenced in our templates. Once collected, these can then be uploaded as a group to wherever the files are to be hosted.

STATICFILES_DIRS lists additional directories that Django's collectstatic tool should search for static files.

Django templates refer to static file locations relative to a static tag which in turn maps to the STATIC_URL setting. Static files can therefore be uploaded to any host and you can update your application to find them using this setting.

The collectstatic tool is used to collect static files into the folder defined by the STATIC_ROOT project setting. It is called with the following command:

python manage.py collectstatic

For this tutorial, collectstatic is run automatically by Railway before the application is uploaded, copying all the static files in the application to the location specified in STATIC_ROOT. Whitenoise then finds the files from the location defined by STATIC_ROOT (by default) and serves them at the base URL defined by STATIC_URL.

In your settings.py file, open /ecommerce/settings.py and copy the following configuration into the bottom of the file.

![Alt Text](/static/images/blog/deploy/settings.JPG)

Whitenoise
Whitenoise is one of the easiest methods for serving static assets directly from Gunicorn in production.

Railway automatically calls collectstatic to prepare your static files for use by WhiteNoise after it uploads your application.

Install WhiteNoise locally using the following command:

pip install whitenoise

To install WhiteNoise into your Django application, open your settings.py file and go to /ecommerce/settings.py, find the MIDDLEWARE setting and add WhiteNoiseMiddleware near the top of the list, just below the SecurityMiddleware:

![Alt Text](/static/images/blog/deploy/middleware.JPG)

To reduce the size of the static files when they are served, add the following to the bottom of /ecommerce/settings.py:

![Alt Text](/static/images/blog/deploy/settings.JPG)

Requirements
The Python requirements of your web application must be stored in a file requirements.txt at the root of your repository. Railway will then install these automatically when it rebuilds your environment. You can create this file using pip on the command line (run the following in the repo root):

pip freeze > requirements.txt

After installing all the different dependencies above, your requirements.txt file should have at least these items listed (though the version numbers may be different).

![Alt Text](/static/images/blog/deploy/requirements.JPG)

Runtime
The runtime.txt file, if defined, tells Railway which version of Python to use. Create the file in the root of the repo and add the following text:

python-3.10.7 depending on our current python version.

Once you get here, all configurations have been done. Test the site again locally to make sure the changes made did not break anything. As usual, run the development server and check your browser.

4. Set up a Railway account and deploy your application.
To set up the Railway account, go to railway.app and Login using your GitHub credentials and you'll be logged in to the Railway.app dashboard

the next step is to deploy from GitHub. On the top menu, choose the Dashboard option then select the New Project button:

![Alt Text](/static/images/blog/deploy/deploy1.JPG)

Railway will display a list of options for the new project. Select Deploy from GitHub repo option.

![Alt Text](/static/images/blog/deploy/deploy2.JPG)

All projects in the GitHub repos will be displayed. Select the GitHub repository that you want to deploy. In our case, it is the e-commerce site :
`{/* project name. i.e meekkaran/Django_Ecommerce  */}

![Alt Text](/static/images/blog/deploy/site.JPG)

Select Deploy Now to confirm your deployment
![Alt Text](/static/images/blog/deploy/deploy3.JPG)
Railway will then load and deploy your project, displaying progress on the deployments tab. When deployment completes, you'll see a screen like the one below.

Once your deployment is running correctly, navigate to the settings on your dashboard to generate a domain and your application will be exposed to the internet.
![Alt Text](/static/images/blog/deploy/railwaydone.JPG)

this is the domain provided for our Django -e-commerce website:

djangolibrarycatalogue-production.up.railway.app

## Deploying to your Custom Domain
This is where we connect our deployment from Railway to my Cloudflare meekkaran.com domain.

The best thing about Railway is it only requires a CNAME pointing "@" to your railway app djangolibrarycatalogue-production.up.railway.app. You only need to set up a Cloudflare proxy to set the CNAME record.

steps:

### Create a new site on Cloudflare

### Create a new CNAME Record pointing to your railway.app link.

1. Create a new site on CloudFlare
Create an account on Cloudflare

If you do not have a domain yet, click on Domain registration to get a domain name.

![Alt Text](/static/images/blog/deploy/cloudflare1.JPG)

On the websites, hit the "add a site" button and add your domain name.

![Alt Text](/static/images/blog/deploy/cloudflare2.JPG)

2. Create a new CNAME Record
Navigate to DNS and click on the "Add Record' button to create a new CNAME record that is pointing to your railway app link.

![Alt Text](/static/images/blog/deploy/cloudflare3.JPG)

The name should be CNAME and the target is the railway.app link

Go back to your settings tab on Railway and click on "Add domain" to add your custom domain from Cloudflare.
![Alt Text](/static/images/blog/deploy/adddomain.JPG)

Set up the name and the value of your CNAME and wait for Railway to pick your DNS record.
![Alt Text](/static/images/blog/deploy/dns.JPG)

And your new domain name will be displayed right below the Railway's provided domain name.

Within a few seconds, your Railway app should be up and running on your domain. On clicking the link it will be displayed on the website.
![Alt Text](/static/images/blog/deploy/done.JPG)

## Conclusion
And that's it. that's how to deploy your application to your custom Domain using Railway App and Cloudflare.
![Alt Text](/static/images/blog/deploy/success.JPG)

