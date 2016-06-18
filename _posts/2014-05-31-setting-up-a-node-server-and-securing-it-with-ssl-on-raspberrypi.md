---
layout: post
title: Setting up a Node Server and Securing it with SSL on RaspberryPi
date: 2014-05-31 20:02:38.000000000 +02:00
type: post
published: true
status: publish
categories:
tags: [js]
meta:
  _edit_last: '54045106'
  geo_public: '0'
  _publicize_pending: '1'
  _oembed_3f8370509bc992b412d9a2b99316a489: "{{unknown}}"
  _oembed_e29453bf7ea39e15da8d01f65c58ee14: "{{unknown}}"
  _oembed_cf85e4e4cbcea161afc4adfde7280c07: "{{unknown}}"
  _oembed_1b4a01090aad5f1c877ce08903569047: "{{unknown}}"
  _oembed_fa7dbbdc64c56615e95750d7ea7eb2a3: "{{unknown}}"
  _oembed_19a1a822b5fdfddf0a54c70cc269ffa2: "{{unknown}}"
  _oembed_6db199cf2dc2ad2886881c1c4c2cb044: "{{unknown}}"
  _oembed_510a950842ac87f96e602ba1526f0d28: "{{unknown}}"
  _oembed_947cb8b908111e9068f078b481149e84: "{{unknown}}"
  _oembed_34e6232837cbcb38a2b23cb05154bbaf: "{{unknown}}"
  _oembed_10f217aa6337d968e9b51a58b166fb49: "{{unknown}}"
  _oembed_f81dd7662451cb0e4dc574ae29034f0e: "{{unknown}}"
  _oembed_89a04b3ea02262454832f385d95f7cbc: "{{unknown}}"
  _oembed_eeaa5a22b4f476b0f93fff39447b6f2d: "{{unknown}}"
  _oembed_eb90c42f14ed100f664902f6575fd02b: "{{unknown}}"
  _oembed_ea209a1ea13e0bb9bc1046afb07a5ca4: "{{unknown}}"
  _oembed_e5a273bb0e070ed1610bb66f2bc5dd01: "{{unknown}}"
  _oembed_94f15e3788940333552c42c232a95b92: "{{unknown}}"
  _oembed_1004b95767badc700a7bf12a61494d45: "{{unknown}}"
  _oembed_c5720595b28bba10d5677ce6827150f7: "{{unknown}}"
  _oembed_eff23e8709d154ad6dac81dd0526b0d0: "{{unknown}}"
  _oembed_9504c70f30545f817a651b996b793a24: "{{unknown}}"
  _oembed_a3c6fe0443fa4f776004a70d0666ecd7: "{{unknown}}"
  _oembed_15d7ad96554fa5c8b50a41f5ff1e3eb6: "{{unknown}}"
  _oembed_7e5cf1282649ed2662944e136293db40: "{{unknown}}"
  _oembed_aba703849ed89ccb74c6a6494bbb55ed: "{{unknown}}"
  _oembed_2041cb8fcaee18be9fe1e9ac29be55ac: "{{unknown}}"
  _oembed_dbf48cd61d9384f599e90d589a4e56fa: "{{unknown}}"
  _oembed_5a140edf93b54cc814f4780e3e1ab02e: "{{unknown}}"
  _oembed_b0afd48ec860260ebab2d9d4b21a0e65: "{{unknown}}"
  _oembed_c3f36d16da3ffade5b94aa053cc635f9: "{{unknown}}"
  _oembed_b99d0227417d7abf9c9d22f103b6012b: "{{unknown}}"
  _oembed_57f6ad421d898964f47b9fa78ec07fa9: "{{unknown}}"
  _oembed_0c3a0265cfb26daab55eb6b55e388725: "{{unknown}}"
  _oembed_29dfa26b8fb2ca9aaad7a2b490fb7ac3: "{{unknown}}"
  _oembed_15bc870872771c866ad5ad2cbd74f150: "{{unknown}}"
  _oembed_f014864bf6e135a49b861a1f67273e72: "{{unknown}}"
  _oembed_100fe17016aa9c801d2d9dfc52bfae0b: "{{unknown}}"
  _oembed_f41b182049c4ca01156eaebe68c40015: "{{unknown}}"
  _oembed_9961b8ade4153d28db0691ed238a5f39: "{{unknown}}"
  _oembed_5c75b37239c20674b3d17c82693f9abd: "{{unknown}}"
  _oembed_4ca661c2281ee0ed4e03f2e0dfac5798: "{{unknown}}"
  _oembed_c32f2784dbf8b0dd9d15318b6687a207: "{{unknown}}"
  _oembed_668cd41aa2eeb7b0d30bad800a88f24b: "{{unknown}}"
  _oembed_550e05293adc02124003e4d9d6a21222: "{{unknown}}"
  _oembed_20b0a40398fa80259340f0ab361f6419: "{{unknown}}"
  _oembed_f073873ab4ac03e58cea87ae073d9326: "{{unknown}}"
  _oembed_461391211fd71047394987165930e074: "{{unknown}}"
  _oembed_f3a56eb32c567526d2c76648439b7e72: "{{unknown}}"
  _oembed_b28d34bf3bed22999383282e9b1fd0a2: "{{unknown}}"
  _oembed_5d9935051d70a72493b33ec3929ec174: "{{unknown}}"
  _oembed_c1efe28c425f72270748ec5265306ef0: "{{unknown}}"
  _oembed_7664f0ac524aa76720ffc342c2005b87: "{{unknown}}"
  _oembed_ba1d93fd89bf545954755111c06775d8: "{{unknown}}"
  _oembed_afe6660d57f9e608d33aee40b90a32ed: "{{unknown}}"
  _oembed_b0a25be23b2976aa3fbce0c8e8c4958a: "{{unknown}}"
  _oembed_4a4ae06f667c7284028f33de5336f25b: "{{unknown}}"
  _oembed_27b89e8c15627f26ba5e6009594b03f7: "{{unknown}}"
  _oembed_6a8bd366f6ea771054fa6a3831896e91: "{{unknown}}"
  _oembed_b3a2ec6c8c6b933ac32b5857c3272b0d: "{{unknown}}"
  _oembed_7a08c21e32e66863fd3e405c8d1c9909: "{{unknown}}"
  _oembed_5111877749ae97360050e9ffa3dedcde: "{{unknown}}"
  _oembed_60d9f0d3cc711daeb2f6b0246fbf5427: "{{unknown}}"
  _oembed_c81ca64871e77a17a35b505705cd1486: "{{unknown}}"
  _oembed_7770c8df98f24c41a84e20a80efbc3d4: "{{unknown}}"
  _oembed_f83345b865ea2cb67cfe977c1609d50b: "{{unknown}}"
  _oembed_462c6a0f9fe32e81082982cc7a0f0c77: "{{unknown}}"
  _oembed_1d84bfdf6a5c785587d64174124abb54: "{{unknown}}"
  _oembed_52795fe4686ecc7d3ee4844d69e7c152: "{{unknown}}"
  _oembed_b523126ea844e56aa521d573fe54d3c9: "{{unknown}}"
  _oembed_3bd4a5e133a53107c48dfdde6400ee00: "{{unknown}}"
  _oembed_0387b33ccbc6eac420e78af49bb9bff4: "{{unknown}}"
  _oembed_2416f9c8b5987c9bb96e0d16ffc93b34: "{{unknown}}"
  _oembed_1a00e51b6d35ced292410fd206df1ebb: "{{unknown}}"
  _oembed_e31cd491c387da4a63d2aa729f35a200: "{{unknown}}"
  _oembed_03503cc47b93d5ac7c0a1264efd606d2: "{{unknown}}"
  _oembed_b3a58bf2a21a188d663540e37be44ea1: "{{unknown}}"
  _oembed_e528f5c6a67bbd40c1bb6850ab6070a5: "{{unknown}}"
  _oembed_16b58155b53b4f895399ef083efc7928: "{{unknown}}"
  _oembed_4728e3461bae108a99eb9058b204059e: "{{unknown}}"
  _oembed_5493b6c16664936715a7d47c9a39b152: "{{unknown}}"
  _oembed_ccf8321e1135f52988401fbe4b5063f2: "{{unknown}}"
  _oembed_429f8a2b3df37a7371a3b8713bf51b35: "{{unknown}}"
  _oembed_16c60bf3e6952813520d8346ec0aa31a: "{{unknown}}"
  _oembed_c591284dc01f1482f73f05d6695d3f4f: "{{unknown}}"
  _oembed_a6033b97d3477cd8304fefb3a3ce2c38: "{{unknown}}"
  _oembed_adb318f23800fc74f6ece26c314b81cf: "{{unknown}}"
  _oembed_bd213053ff3edfe1d1a6df1015f7a8b3: "{{unknown}}"
  _oembed_d7b37ac9a64c6e5729b5760d83fb6616: "{{unknown}}"
  _oembed_cfc37a0ad4945828c8ba9b65c79c3849: "{{unknown}}"
  _oembed_f1ffc6e24f3961cbd4d35bf38449995f: "{{unknown}}"
  _oembed_7dc2f014bf2a82e4794bf27980039331: "{{unknown}}"
  _oembed_19854ae50f5cc81199775c9250315245: "{{unknown}}"
  _oembed_f955b5de9dcb37b1ca7fd8b2ffa75919: "{{unknown}}"
  _oembed_e8b0240ecc03f7398ffa046d92b16054: "{{unknown}}"
  _oembed_6b346a9cb2305ed8655913397a7b0742: "{{unknown}}"
  _oembed_6124f914cb4574d93e850a8016418b27: "{{unknown}}"
  _oembed_d8c39804b1b5d8143f5b8041a65e3bb9: "{{unknown}}"
  _oembed_c560464d09735af9b69a976bd2731947: "{{unknown}}"
  _oembed_cf853f805a5fdc2ccfd5e227d3258a79: "{{unknown}}"
  _oembed_b9213a6d5a776328d62633f0915e86b6: "{{unknown}}"
  _oembed_9e85c0cd8f4f43b862055b3446dffcc0: "{{unknown}}"
  _oembed_c61366d5e9209bbc0309c8dd48b587a8: "{{unknown}}"
  _oembed_f50a84330ad7ca8275cd427208f6fba8: "{{unknown}}"
  _oembed_e6888ad49151ba97f9d9fd69110399ef: "{{unknown}}"
  _oembed_2dd9ba799f06602778be8f9f90e4784f: "{{unknown}}"
  _oembed_11a7662a98b2a44eac1ff58a23f7cfc8: "{{unknown}}"
  _oembed_180745f65fa80d72176920e7dc1adc70: "{{unknown}}"
  _oembed_43387d3b16c081ae513514d1b6e144d4: "{{unknown}}"
  _oembed_56b1af673227bdb3d92282854ea2377a: "{{unknown}}"
  _oembed_a32310cfee13736fd33ecba47b38455f: "{{unknown}}"
  _oembed_e39613f6a4f5250f6bb96b95f20d7c56: "{{unknown}}"
  _oembed_f87e0c0a775428c860120afd1f4ba556: "{{unknown}}"
  _oembed_8263f6d4fd5e44140f1b325aa1780710: "{{unknown}}"
  _oembed_1c196f7125ab8f5d9cd977a10b47a1c8: "{{unknown}}"
  _oembed_34d219c3570b6b94d8c67353de78b446: "{{unknown}}"
  _oembed_3aed46a52121be5586cf9e8a18bf4ee9: "{{unknown}}"
  _oembed_9c106cce280772419f62b1a7870b3312: "{{unknown}}"
  _oembed_3601eeeac18111842b2d8b1d04b46362: "{{unknown}}"
  _oembed_c3996dac45c4cb042ddd3183228abe73: "{{unknown}}"
  _oembed_30a462b4a32d79c55cb5edd4a94c3953: "{{unknown}}"
  _oembed_9bda31b274deeed95c38ef2105b05200: "{{unknown}}"
  _oembed_4b0909c91c9e256259e73c3712041388: "{{unknown}}"
  _oembed_1aa000e4a59b2801e81ac83a79bb0800: "{{unknown}}"
  _oembed_e68244669efe3e43cb4cfed9d8e6f53d: "{{unknown}}"
  _oembed_f6c81f8ca50173f407c77dbf0e3fa4ea: "{{unknown}}"
  _oembed_2f66bac61801a11b368c2b234ec01b64: "{{unknown}}"
  _oembed_2a1f4cf6c2e8bb714cf7ae5ee8d59e4c: "{{unknown}}"
  _oembed_4747924e9ae3c1d532f0c23fff1eb567: "{{unknown}}"
  _oembed_5691a8e671086ea50de3748c4100eaa3: "{{unknown}}"
  _oembed_b7f2a803bc3eab9bc66eef42000a96d9: "{{unknown}}"
  _oembed_d30f7455893eaf07bcfeecd345b0d8bd: "{{unknown}}"
  _oembed_b28baa51ff3fa8845d134e7262743c46: "{{unknown}}"
  _oembed_696bf454d9f97587dd280fa4212b9a20: "{{unknown}}"
  _oembed_c5bf520446288f4f8132c2a7f99b9626: "{{unknown}}"
  _oembed_148547a45475b9e23c8984c787dfeba6: "{{unknown}}"
  _oembed_b213efc5d7f882f9c783ac3114528783: "{{unknown}}"
  _oembed_412a7303823bba779300a5e4839d39dc: "{{unknown}}"
  _oembed_35c01871eda5565b3ed708f3b338ca58: "{{unknown}}"
  _oembed_e137b62f2a80ca812ab47e91d1f29395: "{{unknown}}"
  _oembed_4363401e40b90e4fd89cb8bff4d9424a: "{{unknown}}"
  _oembed_9d496fe97fa87d450200ff7b70e8e289: "{{unknown}}"
  _oembed_f3bde0c10ac3d874efe436cdf1a47655: "{{unknown}}"
  _oembed_70d4f4404ca3fa1baa71f436045c828a: "{{unknown}}"
  _oembed_c812a32bfdcb073c3a4b99d8436a4dc1: "{{unknown}}"
  _oembed_271321b23c21c3a13f6191dbf92b388c: "{{unknown}}"
  _oembed_81001aa985efbb18e8f669517b81cd56: "{{unknown}}"
  _oembed_5940057ffcf20204992521087326c68c: "{{unknown}}"
  _oembed_0098d069c59e26e94e679046e4a3ca7b: "{{unknown}}"
  _oembed_82583484ce198eca638fb471b44e32b8: "{{unknown}}"
  _oembed_80c736607ef9e650b0966afaedcdb4b0: "{{unknown}}"
  _oembed_fd5b417a80a54161bf5e34aa89a64be2: "{{unknown}}"
author:
  login: sirars
  email: sirars@gmail.com
  display_name: Sirar Salih
  first_name: ''
  last_name: ''
---
<p><a href="http://www.raspberrypi.org/" title="RaspberryPi">RaspberryPis</a> are awesome little things that you can do a lot of cool stuff with. Basically, a RaspberryPi (RasPi) is a 35 dollar, mini computer. It has LAN, USB, audio, RCA, HDMI and power ports as well as an SD card input. The A model has a 700 MHz processor and 256 MB of RAM. An SD card is flashed with an operating system and used on the device, there are <a href="http://www.raspberrypi.org/downloads/">lots of operating systems</a> to choose from, I personally prefer <a href="http://www.debian.org/releases/stable/">Debian Wheezy</a>. I purchased my RasPi some time last year and have had quite some fun with it since. When I first got it, I set up <a href="http://xbmc.org/">XBMC</a> on it and used it as a humble <a href="http://en.wikipedia.org/wiki/Home_theater_PC">HTPC</a> or at least as a video media outlet in my apartment. </p>
<p><a href="https://sirars.files.wordpress.com/2014/05/wp_20140531_19_52_36_pro.jpg"><img src="http://sirars.files.wordpress.com/2014/05/wp_20140531_19_52_36_pro.jpg" alt="WP_20140531_19_52_36_Pro" width="440" height="247" class="alignnone size-full wp-image-484" /></a></p>
<p>Recently, I decided to use my RasPi for something far more awesome. I decided to set up a <a href="https://raspi.sirars.com">publicly secure node server on it</a> and have it control an <a href="http://ardrone2.parrot.com/">AR Drone</a> in my apartment through REST calls (I don't have any drone just yet though). In the following tutorial, I will show you how to set up a secure node server on your own RasPi and make it accessible from anywhere in the world.</p>
<p><strong>First things first: Install Node</strong></p>
<p>The first step is to install <a href="http://nodejs.org/download/">node</a> on your RasPi. Power up your RasPi, <i>log in as root user</i> and then simply type the following commands:</p>
<pre>wget http://nodejs.org/dist/v0.10.2/node-v0.10.2-linux-arm-pi.tar.gz
tar -xvzf node-v0.10.2-linux-arm-pi.tar.gz</pre>
<p>This will download node and unzip it for you. If you then type <code>ls</code> you'll see that <code>node-v0.10.2-linux-arm-pi</code> was unzipped successfully in your current folder. The next step is to set node in your global path so you can start it from anywhere. Type the following:</p>
<pre>NODE_JS_HOME=/home/pi/node-v0.10.2-linux-arm-pi 
PATH=$PATH:$NODE_JS_HOME/bin</pre>
<p>The node packet manager (npm) comes bundled with node, we will use that to install external modules. Node and npm should now be accessible from any location in your RasPi.</p>
<p><strong>Install External Node Modules</strong></p>
<p>You may need to install node's native build tool for future use, to do that, simply type:</p>
<pre>npm install -g node-gyp</pre>
<p>The <code>node-gyp</code> build tool is now installed and located at:</p>
<pre>%HOME%/node-v0.10.2-linux-arm-pi/lib/node_modules</pre>
<p>You will find all future installed modules located in this folder. Next up is to install <code>express</code> (we will use this module to create our secure node server later on):</p>
<pre>npm install -g express</pre>
<p>It may be a good idea to install the <code>socket.io</code> module as well for future use:</p>
<pre>npm install -g socket.io</pre>
<p>We now have all the external modules that are needed.</p>
<p><strong>Change from Dynamic to Static Local IP for the RaspberryPi</strong></p>
<p>Since the aim is getting the RasPi accessible from the outside world, you need to have a static IP for it. Chances are you are using a router, and thus a dynamic IP is provided to your RasPi by the router. In this case, you need to make the dynamic IP static. You can start by typing the following command:</p>
<pre>ifconfig</pre>
<p>Note the fields <code>inet addr</code>, <code>Bcast</code> and <code>Mask</code>. Here is an example:</p>
<pre>
inet addr:192.168.1.110  
Bcast:192.168.1.255  
Mask:255.255.255.0
</pre>
<p>Now type the following command:</p>
<pre>route -nee</pre>
<p>Note the field <code>Gateway</code>, which is the IP of your router: </p>
<pre>
Gateway: 192.168.1.1
</pre>
<p>Now type the following command:</p>
<pre>nano /etc/network/interfaces</pre>
<p>Change the line <code>iface eth0 inet dhcp</code> to <code>iface eth0 inet <strong>static</strong></code>. Now, change the information to include the following:</p>
<pre>
iface eth0 inet static
address 192.168.1.110
netmask 255.255.255.0
network 192.168.1.0
broadcast 192.168.1.255
gateway 192.168.1.1
</pre>
<p>Save changes made to this file and then exit (<code>CTRL+X</code>, <code>Y</code> and <code>Enter</code>). Now reboot your RasPi in order for the changes to take effect:</p>
<pre>
reboot
</pre>
<p><i>Log in as root user</i>. That's it, your RasPi now has a static local IP. In this example, the IP is <strong>192.168.1.110</strong>. The next step is to port forward your global IP to this IP.</p>
<p><strong>Port Forward Global IP to the RaspberryPi Static Local IP</strong></p>
<p>In order to access the RasPi from the outside, we need to port forward our global IP to the RasPi static local IP. I prefer to use the router UI for this, I use a <a href="http://support.linksys.com/en-us/support/routers/WRT54G">Linksys WRT54G</a> router, here is a screenshot of how I do the port forwarding using this router:</p>
<p><a href="https://sirars.files.wordpress.com/2014/05/capture1.png"><img src="http://sirars.files.wordpress.com/2014/05/capture1.png" alt="Capture" width="440" height="343" class="alignnone size-full wp-image-482" /></a></p>
<p>Once you've done port forwarding, the RasPi should now be accessible from the outside.</p>
<p><strong>Generate a Certificate Signing Request (CSR) for SSL</strong></p>
<p>We need to generate a Certificate Signing Request (CSR) to sign the SSL certificate that we will use for the RasPi node server. We will use <em>*cough*</em> OpenSSL <em>*cough*</em> to generate the CSR, if OpenSSL is not already installed in your RasPi then simply install it by typing the following command:</p>
<pre>apt-get install openssl</pre>
<p>Now generate the CSR like so:</p>
<pre>openssl req -nodes -newkey rsa:2048 -keyout private.key -out server.csr</pre>
<p><strong>Note:</strong> <code>private.key</code> is the private key which is for your eyes only, make sure to move it to a secure location. We will use this key later along with the SSL certificate to create the node server. </p>
<p>You will now get some questions that you need to answer, here is an example:</p>
<pre>
Country Name (2 letter code) [AU]: NO
State or Province Name (full name) [Some-State]: Oslo
Locality Name (eg, city) []: Oslo
Organization Name (eg, company) [Internet Widgits Pty Ltd]: Salih AS
Organizational Unit Name (eg, section) []: IT
Common Name (eg, YOUR name) []: mysubdomain.mydomain.com
Email Address []:
Please enter the following 'extra' attributes to be sent with your certificate request

A challenge password []: 
An optional company name []:
</pre>
<p><strong>Note:</strong> <code>Email address</code>, <code>challenge password</code> and <code>optional company name</code> can be left blank. The most important field here is <code>Common Name</code>, you need to type in the domain that you want to use for your RasPi. Setting up SSL requires a domain (unless you're going for a mighty expensive and special SSL certificate), therefore you need to have one (in my case, I use <a href="https://raspi.sirars.com">https://raspi.sirars.com</a>). You can purchase a domain at a good price from providers such as <a href="http://godaddy.com/">GoDaddy</a>.</p>
<p><strong>Generate the SSL Certificate</strong></p>
<p>We will now generate an SSL certificate using the CSR we got in the previous step. An SSL certificate costs money. I personally prefer <a href="http://www.ssls.com/lp/4.99-ssl-offer.html?gclid=CI7fzoblqr4CFUfLtAodtl0AhA">SSLs.com</a> for their cheap price range, fast delivery and good documentation. I recommend that you order your SSL certificate from them. Once you've done that, edit the <code>server.csr</code> by typing the following command:</p>
<pre>
nano server.csr
</pre>
<p>Copy the entire content. Now <a href="http://www.ssls.com/knowledgebase-article.html?article_id=657&amp;category_id=58">go to this link</a> and follow the steps thoroughly. When you're done, you'll get an e-mail from <a href="http://www.ssls.com/">SSLs.com</a> containing your SSL certificate (which has the name format <i>mysubdomain_mydomain_com.crt</i>).</p>
<p><strong>Point Your Domain to the Global IP of the RaspberryPi</strong></p>
<p>The next step is to point your domain to the global IP. Say that you own the domain <code>http://raspi.domain.com</code>, in that case you need to point this domain to the now global IP of your RasPi. I purchased <a href="https://raspi.sirars.com">my domain</a> from <a href="http://godaddy.com/">GoDaddy</a>, here is a screenshot showing how I pointed my subdomain to the global IP address of my RasPi using GoDaddy's domain management panel:</p>
<p><a href="https://sirars.files.wordpress.com/2014/05/capture.png"><img src="http://sirars.files.wordpress.com/2014/05/capture.png" alt="Capture" width="440" height="189" class="alignnone size-full wp-image-478" /></a></p>
<p><code>@</code> points the whole domain, while subdomain <code>raspi</code> points to a specific IP. For security reasons, my IP addresses are censored in the screenshot. Once you've pointed either your domain or subdomain to the global IP of the RasPi, it will take around 10 hours or so to take effect depending on the domain provider.</p>
<p><strong>Create Node Server Using the SSL Certificate</strong></p>
<p>The last step, is to create the node server itself with the generated SSL certificate. Start editing the JavaScript file by typing:</p>
<pre>
nano node_server.js
</pre>
<p>Now create the secure server by inserting the following JavaScript code (make sure to use absolute file paths):</p>

```javascript
var fs = require('fs');
var https = require('https');
var privateKey = fs.readFileSync('private.key', 'utf8');
var certificate = fs.readFileSync('mysubdomain_mydomain_com.crt', 'utf8');
var credentials = {key: privateKey, cert: certificate};
var express = require('%HOME%/node-v0.10.2-linux-arm-pi/lib/node_modules/express');
var app = express();
var ip = "192.168.1.110";
var port = 443;          //HTTPS

app.get('/', function(request, response){
    response.send("Welcome to my RaspberryPi Node Server!");
});

var httpsServer = https.createServer(credentials, app);
httpsServer.listen(port, ip);
console.log('Node express server started on port %s', port);
```

<p>This code simply creates the node server for you using the SSL certificate that was generated earlier. Note the usage of the static local IP. We also created here a simple GET request on "/" that returns a welcome a message to the user, so when the user types your domain name in the browser (which is basically doing a GET request, using HTTPS) he/she will be greeted by this message.</p>
<p>To start the node server, simply type the command:</p>
<pre>
node node_server.js
</pre>
<p>Test out your secure node server by visiting your domain using HTTPS, it should work by returning a welcome message. You can find <a href="https://raspi.sirars.com">my own server here.</a></p>
<p><strong>Create a Script to Automatically Start Node Server on Boot (optional)</strong></p>
<p>This step is optional but highly recommendable. Once you've finished setting up your secure node server, the next thing you want to do is to create a simple script that will automatically start up the server every time your RasPi is rebooted. Start editing the script file by typing the command:</p>
<pre>
nano startup_script.sh
</pre>
<p>Insert the following lines in the script:</p>
<pre>
NODE_JS_HOME=/home/pi/node-v0.10.2-linux-arm-pi
PATH=$PATH:$NODE_JS_HOME/bin
node /home/pi/node_server.js
</pre>
<p>Save the file and then exit. Make the file executable by typing the following command:</p>
<pre>
chmod +x startup_script.sh
</pre>
<p>Now, you need to move this script to the <code>/etc/init.d</code> folder:</p>
<pre>
mv startup_script.sh /etc/init.d
</pre>
<p>Reboot your RasPi, and the node server should now start automatically.</p>
<p>Hope you enjoyed this tutorial! I will do a second part soon explaining how this secure RasPi node server will control an <a href="http://ardrone2.parrot.com/">AR Drone</a> through REST calls. :)</p>
