Heroku style Vagrant deployment demo for STHLM.js
=================================================
 
This is the demo by Christian Landgren, Iteam for STHLM.js in May 2013

## About deployment / devOps

Why use think deployment during development? Find problems faster, less time spending troubleshooting differences, integration testing. 

Get an instant production-like environment on your laptop. 
Set up all depenencies with chef. Create your projects deployable from start. 

## Demo heroku
 
    heroku create
    git init
    express
    node app.js

Edit app.js and make it listen to the local port 3000 if it isn't already set by environment

    app.listen(process.env.PORT || 3000);

Test your app in a browser http://localhost:3000

Add a Procfile

    touch Procfile

Specify how your app is going to be started

    web: node app.js

Finally push this to Heroku and watch the results

    git add .
    git commit -am "Initial commit"
    git push heroku
    heroku open


This is perfect for getting your app "up there" as soon as possible. But there are a few things you should know, Websockets are not supported (yet) and if you want to use SPDY or other direct ways of communicating you are out of luck. Also the big advantage with node is it's resource free way of handling multiple connections so we don't want too much things in the way of our app. Let's go in to the SSH world ;)

So how do we get a Heroku-style deployment scenario without Heroku to our own servers or virtual servers?


Set up heroku-vagrant
===================

    vagrant init heroku    

## Change machine settings and apply a local or global ip-adress in the Vagrantfile

    vagrant up     

## Set up SSH keys

    ssh vagrant@<server IP> "cat >> ~/.ssh/authorized_keys" < ~/.ssh/id_rsa.pub    
    git remote add origin vagrant@<server IP>:/home/vagrant/sthlm.js     

## Demo git-deploy

    gem install git-deploy     
    git deploy setup     
    git deploy init    

## Edit deploy script in /deploy/restart

    touch tmp/restart.txt     
    echo "restarting Node.JS app" 
    npm install    
    npm test     
    /opt/ruby/bin/foreman start    



