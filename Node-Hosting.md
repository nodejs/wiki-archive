# Hosting compatible with Node

## Managed

Managed providers provide a simplified "Node Appliance" solution. Node and NPM will already be set up for you, and deploys are typically done via git push or simular method. You will have less control of your server, but everything will be set up for you.

Name | Node Version | Web Sockets | IP/Hostname | IRC | Repository | Free Plan | Paid Plans | Notes |
:-----------|:------------|:-------------|:-------------|:-------------|:-------------|:-------------|:-------------|:-------------------------------------|
[appfog](http://appfog.com/) | | | | | [cloudfoundry](https://github.com/cloudfoundry) | | | Private Beta (accepting signups)
[bejes.us](http://bejes.us) | | | | | | | | Beta Coming Soon (accepting signups) - powered by Nodester 
[Cloud Foundry](http://www.cloudfoundry.com) | | | No | | [cloudfoundry](https://github.com/cloudfoundry) | Yes - Only during beta. | | Beta, accepting signups
[Cloudnode](http://cloudno.de) | 0.4.12, 0.6.x | Yes | Yes | | | Yes - Up to 3 VMs, 25 MB CouchDB space, 250,000 requests/month. | | Beta (accepting signups) - powered by Nodester 
[Cure](http://cure.willsave.me) | 0.6.2 | Yes | Yes | | | Yes - One week trial. (Up to 1GB outgoing b/w, $0.18 per GB after.) | $12.95/month per server. | 
[DotCloud](http://www.dotcloud.com) | 0.4.10 | Yes | Paid plan | #dotcloud | [dotcloud](https://github.com/dotcloud) | Yes - 2 services. | Pro - $99/month, 4 services. Enterprise - Unlimited services. | Beta
[Heroku](http://heroku.com) | 0.4.7 | No | Yes | #heroku | [heroku](http://github.com/heroku) | Yes - 1 Dynamo (256 MB Ram) | | 
[no.de](http://no.de) | 0.4.11 | Paid plan | Paid plan | #joyent | [joyent](http://github.com/joyent) | Yes - 128 MB Ram | | 
[NodeSocket](http://www.nodesocket.com) | 0.4.12 | Yes | Yes | | [nodesocket](https://github.com/nodesocket) | | | In Private Beta
[Nodejitsu](http://nodejitsu.com) | 0.4.12, 0.6.x | Yes | Yes | #nodejitsu | [nodejitsu](http://github.com/nodejitsu) | Free - More info after beta | | In Private Beta
[Nodester](http://nodester.com) | 0.4.9, 0.4.12, 0.6.12 | Yes | Yes | #nodester |[nodester](https://github.com/nodester) | Yes - Unlimited | No | Open (with invite)
[JSApp.US](http://jsapp.us) | | | No | | [node-host](https://github.com/matthewfl/node-host) | | | Open signup, web editing/npm command
[Node Ninja(JP)](https://node-ninja.com) | 0.6.11 | Yes | Yes | | | | | In Private Beta
[NAE(CN)](http://cnodejs.net) | 0.6.2 | Yes | Just allow *.cnodejs.net or custom domain | | | Yes - You can create 10 apps. And you can request a MongoDB for every app. | | Open (with invite)

Empty cells mean we weren't sure what to put there. 

## Self-Managed

VPS providers, which often require you to set everything up yourself, including the operating system.

* [A2 Hosting](http://www.a2hosting.com/web-development/nodejs-hosting)
* [Amazon EC2](http://aws.amazon.com/ec2)
* [BuyVM](http://www.buyvm.net)
* [DreamHostPS](http://www.dreamhost.com/hosting-vps.html) - Need a total reconfiguration of virtual machines
* [Gandi](http://en.gandi.net/hosting)
* [GleSYS](http://glesys.com/vps.php)
* [Host Europe](http://www.hosteurope.de)
* [IPAP](http://ipap.co) - Consult the many comments at [lowendbox](http://www.lowendbox.com/) before considering these guys…
* [Joyent](http://www.joyentcloud.com/products/appliances/nodejs-smartmachine/)
* [Linode](http://www.linode.com)
* [Oversun Scalaxy](http://www.scalaxy.ru) - Site in Russian
* [Prgmr](http://prgmr.com)
* [Rackspace Cloud](http://www.rackspacecloud.com)
* [Slicehost](http://www.slicehost.com)
* [VPS.Net](https://www.vps.net/vps-signup)
* [W2Servers](http://w2servers.com)
* [Webbynode](http://www.webbynode.com) - [setup guide](http://blog.dtrejo.com/nodejs-for-server-newbs) - [deployment guide](http://guides.webbynode.com/articles/rapidapps/nodejs.html) - [screencast](http://vimeo.com/15406437)
* [WebFaction](http://webfaction.com) - [setup guide](http://davestevens.us/articles/setting-up-nodejs-on-webfaction-revised)
* [Zerigo](http://www.zerigo.com/)

## DIY Platforms

Node.JS hosting platforms that allow you to host Node.JS apps on your own servers or clouds.

* [Nodester](http://nodester.com/) - Open source Node.JS hosting platform and services
* [Nodejitsu](http://github.com/nodejitsu)
  * [haibu](http://github.com/nodejitsu/haibu) - Open-source Node.js Application Server
  * [node-http-proxy](http://github.com/nodejitsu/node-http-proxy) - Proxy / Load Balancing
  * [jitsu](http://github.com/nodejitsu/jitsu) - CLI Deployment tool
  * [forever](http://github.com/indexzero/forever) - Process Monitoring
  * [winston](http://github.com/indexzero/winston) - Multi-transport Logging
  * [node-cloudservers](http://github.com/nodejitsu/node-cloudservers) Rackspace Cloud Server Provisioning
  * [node-cloudfiles](http://github.com/nodejitsu/node-cloudfiles) Rackspace Cloud Files