# Hosting compatible with Node

## Managed

Managed providers provide a simplified "Node Appliance" solution. Node and NPM will already be set up for you, and deploys are typically done via git push or similar method. You will have less control of your server, but everything will be set up for you.

Name | Node Version | Web Sockets | IP/Hostname | IRC | Repository | Free Plan | Paid Plans | Notes |
:-----------|:------------|:-------------|:-------------|:-------------|:-------------|:-------------|:-------------|:-------------------------------------|
[App Engine](https://cloud.google.com/appengine/) | 4.x, 6.x, 8.x, \*.\* | No | Yes | | [google-cloud-node](https://github.com/GoogleCloudPlatform/google-cloud-node) | Free Trial | Yes | General Availability
[appfog](http://appfog.com/) | 0.4.12, 0.6.17, 0.8.14, 0.10.22 | No | Yes, custom domains in paid plan only | | [appfog](https://github.com/appfog) | Free Signups suspended | Yes – monthly subscriptions and enterprise support available | General Availability
[Azure](http://azure.microsoft.com/en-us/develop/nodejs/) | 0.6+ |Yes | Yes || [Azure-Sdk-for-Node](https://github.com/Azure/azure-sdk-for-node) |3 month free trial 10 free web sites forever | Yes ||
[Baidu App Engine](http://developer.baidu.com/cloud/rt) | 0.10.21 | Yes (with Port Service enabled) | Yes | | 
[Clever Cloud](http://www.clever-cloud.com/) | version you need | Yes | Yes, custom domains in paid plan only | [#clevercloud on irc.freenode.net](irc://irc.freenode.net:6667/clevercloud) | [Clever Cloud on github](https://github.com/clevercloud) | No, just a free trial | Yes – pay as you go model | General Availability, launch of [nodejs-cloud](http://nodejs-cloud.com)
[MangoRaft](http://mangoraft.ca/) | 0.2.x - 0.12.x | Yes | Yes | | https://github.com/MangoRaft | Yes - Up to 5 projects | Yes | In Closed Alpha
[Cloud Foundry](http://www.cloudfoundry.com) | 0.4.12, 0.6.8, 0.8.2 | No | No | | [cloudfoundry](https://github.com/cloudfoundry) | Yes - Only during beta. | | Beta, accepting signups
[Cloudnode](http://cloudno.de) | 0.4.12, 0.6.17, 0.8.25 | Yes | Yes | | | Yes - Up to 3 VMs, 25 MB CouchDB space, 250,000 requests/month. | | Beta (accepting signups) - powered by Nodester 
[DotCloud](http://www.dotcloud.com) | 0.4.10 | Yes | Paid plan | #dotcloud | [dotcloud](https://github.com/dotcloud) |  | Pro - $99/month, 4 services. Enterprise - Unlimited services. | 
[EvenNode](http://www.evennode.com) | 0.8.x, 0.9.x, 0.10.x, 0.11.x | Yes | Yes |  |  |  | Starting at $8/month | Runs on AWS 
[exoscale apps](https://www.exoscale.ch/open-cloud/apps/) | 0.8.x, 0.10.x | Yes | Yes | | [exoscale](https://github.com/exoscale) | Yes, free credit on sign-up | Yes, pre-paid credit, post-paid option | General Availability 
[Heroku](http://heroku.com) | [0.4.x, 0.6.x, 0.8.x, 0.10.x](http://heroku-buildpack-nodejs.s3.amazonaws.com/manifest.nodejs) | Yes | Yes | #heroku | [heroku](http://github.com/heroku) | Yes - 1 Dyno (512 MB Ram) | $0.05/hour/dyno  | 
[IBM Bluemix](http://bluemix.net) | [v4.x, v6.x](https://www.ng.bluemix.net/docs/#starters/nodejs/index.html#deploynodejsapp) | Yes | Yes | | [IBM-Bluemix](https://github.com/IBM-Bluemix) | Yes | Yes | Based on Cloud Foundry |
[iClickAndHost](https://iclickandhost.com/website-hosting/) | v0.10.18 | Yes | Yes | | | No, but 30-day money back guarantee | Yes | SSH, free VPN, RemoteSQL, Daily Backups, MySQL/PostgreSQL |
[Modulus](http://modulus.io) | 0.2.x - 0.11.x | Yes | Yes | #modulus | [OnModulus](https://github.com/onmodulus) | $15 free credits | $0.02/hour per instance | Live on AWS
[Nodejitsu](http://nodejitsu.com) | 0.4.12, 0.6.x, 0.8.x, 0.10.x | Yes | Yes | #nodejitsu | [nodejitsu](http://github.com/nodejitsu) | 30 days sandbox | Yes | now with Joyent
[JSApp.US](http://jsapp.us) | | | No | | [node-host](https://github.com/matthewfl/node-host) | | | Open signup, web editing/npm command
[NAE(CN)](http://cnodejs.net) | 0.6.2 | Yes | Just allow *.cnodejs.net or custom domain | | | Yes - You can create 10 apps. And you can request a MongoDB for every app. | | Open (with invite)
[opeNode](https://www.openode.io/) | all versions | Yes | sub-domain |  | https://github.com/martinlevesque/openode-cli |  |  | 
[OpenShift](https://openshift.redhat.com/) | 0.6.20 [or any other](https://openshift.redhat.com/community/blogs/any-version-of-nodejs-you-want-in-the-cloud-openshift-does-it-paas-style)| Yes (preview) | Yes | #openshift | [openshift](https://github.com/openshift) | Yes | | Open
[Pogoapp](http://pogoapp.com) | 0.6.x, 0.8.x, 0.10.x | Yes | Yes | #pogoapp | [pogoapp](http://github.com/pogoapp) | Yes - Trial | Paid Beta | Private Beta
[RoseHosting](http://www.rosehosting.com) | 0.6+ | Yes | Yes | | | | Yes - Fully Managed VPS Hosting Plans - 24×7 EPIC Support ||
[Scalingo](https://scalingo.com) | [0.4.x, 0.6.x, 0.8.x, 0.10.x, 0.12.x](http://doc.scalingo.com/languages/javascript/nodejs/) | Yes | Yes |  | [scalingo](http://github.com/Scalingo) | Yes, 30-days free trial, allows you to run 1 app using a maximum of 5 Containers | €0.02/hour/container  | General Availability, Free SSL, MongoDB/Redis/Elasticsearch/MySQL/PotsgreSQL integration ||
[Qt Cloud Services](http://qtcloudservices.com/) | | Yes | Yes, custom domains in paid plan only | | [qtcloudservices](https://github.com/qtcloudservices) | Yes | Yes | |	

Empty cells mean we weren't sure what to put there. 

##Self-Managed Preconfigured

"Node Appliance" solution where Node and NPM are already set up for you, and deploys are typically done via git push or similar method. You will have complete control over and responsibility for your servers, but everything will be set up for you.

Name | Node Version | Web Sockets | IP/Hostname | IRC | Repository | Free Plan | Paid Plans | Notes |
:-----------|:------------|:-------------|:-------------|:-------------|:-------------|:-------------|:-------------|:-------------------------------------|
[Cure](http://cure.willsave.me) | 0.6.2 | Yes | Yes | | | Yes - One week trial. (Up to 1GB outgoing b/w, $0.18 per GB after.) | $12.95/month per server. | Uses Rackspace rather than Amazon EC2 |
[Joyent](http://www.joyentcloud.com/products/appliances/nodejs-smartmachine/) | 0.10.21 | yes | yes | | | Yes - [Free trial available](https://my.joyentcloud.com/landing/signup/70180000000ShEu) | Plans starting from $0.008/hour, up to $2.56/hour, depending on instance size | Custom SmartOS template, running on the [Joyent Public Cloud](http://www.joyent.com/) |


## Self-Managed

VPS providers, which often require you to set everything up yourself, including the operating system.
* [Host Stage](http://www.host-stage.net/linuxvps.php)
* [A2 Hosting](http://www.a2hosting.com/web-development/nodejs-hosting)
* [Amazon EC2](http://aws.amazon.com/ec2)
* [BuyVM](http://www.buyvm.net)
* [Digital Ocean](https://www.digitalocean.com/) - $5/mo. SSD VPS, New York and Amsterdam
* [DreamHostVPS](http://www.dreamhost.com/hosting/vps/) - Need a total reconfiguration of virtual machines
* [exoscale](https://exoscale.ch)
* [Gandi](http://en.gandi.net/hosting)
* [Google Compute Engine](https://cloud.google.com/products/compute-engine/)
* [GleSYS](http://glesys.com/vps.php)
* [HP Cloud] (https://www.hpcloud.com)
* [Host Europe](http://www.hosteurope.de)
* [Host Gator] (http://www.hostgatortalk.com)
* [Joyent](http://www.joyentcloud.com/products/appliances/nodejs-smartmachine/)
* [iClickAndHost](https://iclickandhost.com/openvz-vps-web-hosting-3/)
* [Linode](http://www.linode.com)
* [Microsoft Azure](http://azure.microsoft.com/en-us/documentation/articles/cloud-services-nodejs-develop-deploy-app/)
* [MilesWeb](https://www.milesweb.com/self-managed-vps.php) - VPS in India, UK, US and Romania from $12/mo.
* [Oversun Scalaxy](http://www.scalaxy.ru) - Site in Russian (the site says "Company closed" - November 16, 2013)
* [Prgmr](http://prgmr.com)
* [Rackspace Cloud](http://www.rackspacecloud.com)
* [rootbox.com](http://rootbox.com) - SSD Cloud Servers
* [ServerGrove](http://servergrove.com) [also has MongoDB hosting]
* [VPS.Net](https://www.vps.net/vps-signup)
* [W2Servers](http://w2servers.com)
* [Webbynode](http://www.webbynode.com) - [setup guide](http://blog.dtrejo.com/nodejs-for-server-newbs) - [deployment guide](http://guides.webbynode.com/articles/rapidapps/nodejs.html) - [screencast](http://vimeo.com/15406437)
* [WebFaction](http://webfaction.com) - [setup guide](http://davestevens.us/articles/setting-up-nodejs-on-webfaction-revised)
* [Wirehive](http://www.wirehive.net/hosting/managed-hosting)
* [Xentos VPS Germany](http://www.xentos.de/)
* [Zerigo](http://www.zerigo.com/)

## DIY Platforms

Node.JS hosting platforms that allow you to host Node.JS apps on your own servers or clouds.

* [Nodester](http://nodester.com/) - Open source Node.JS hosting platform and services
* [CloudFoundry](https://github.com/cloudfoundry) - Open source PaaS with support for NodeJS
* [OpenShift](https://openshift.redhat.com/community/open-source) - Open source polyglot PaaS with native support for Node.js
* [Nodejitsu](http://github.com/nodejitsu)
  * [haibu](http://github.com/nodejitsu/haibu) - Open-source Node.js Application Server
  * [node-http-proxy](http://github.com/nodejitsu/node-http-proxy) - Proxy / Load Balancing
  * [jitsu](http://github.com/nodejitsu/jitsu) - CLI Deployment tool
  * [forever](http://github.com/nodejitsu/forever) - Process Monitoring
  * [winston](http://github.com/indexzero/winston) - Multi-transport Logging
  * [node-cloudservers](http://github.com/nodejitsu/node-cloudservers) Rackspace Cloud Server Provisioning
  * [node-cloudfiles](http://github.com/nodejitsu/node-cloudfiles) Rackspace Cloud Files
* [Stagecoach](http://github.com/punkave/stagecoach) - Simple staging and production deployment with node-http-proxy and forever; good for deploying many node apps and non-node sites to one VPS
* [awsbox](https://github.com/mozilla/awsbox) - A featherweight PaaS on top of Amazon EC2 for deploying node apps
* [paastor](https://paastor.com) - Server setup and app management from a UI and CLI.