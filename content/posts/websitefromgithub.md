+++ 
draft = false
date = 2018-10-28T19:39:11+01:00
title = "websitefromgithub"
slug = "" 
tags = []
categories = []
+++

This article is going to be about my utility named [websitefromgithub](https://github.com/worepix/websitefromgithub). You can have source code of your website on GitHub and using some framework for building websites. You want some how autimatically build your website from your source code of your GitHub repository if you commit changes. This little utility can solve your problem. It is easy to use and setup. Thanks to pm2 project it's running if you log out or reboot your server. I am using wfg on this website and it is awesome. If you will have any issue, use [issues](https://github.com/worepix/websitefromgithub/issues) on GitHub. I'm using [Hugo](https://gohugo.io) as static web generator, but if you are using different framework such as Jekyll, Next, etc... it is easy to set up. Just change command and exported folder at settings.json and that's it!

## Install
1. First make sure you have <a href="https://git-scm.com/" target="_blank">git</a> installed.
2. Add user wfg:
    
    ```
    adduser wfg
    ```

3. Add privileges to you website folder location. For example to folder /var/www/html
    
    ```
    sudo chown wfg:wfg -R /var/www/html
    ```

4. Make sure you have installed <a href="https://nodejs.org" target="_blank">Node.js</a> and <a href="http://pm2.keymetrics.io/" target="_blank">PM2</a>. If not - install them
   
    ```
    sudo npm install -g pm2
    ```

5. Change user to wfg and than cd to home directory
   
    ```
    su wfg
    ```
   
    ```
    cd $HOME
    ```

4. Clone this websitefromgithub repository
   
    ```
    git clone https://github.com/worepix/websitefromgithub.git
    ```

5. Go to websitefromgithub folder
    
    ```
    cd websitefromgithub
    ```

7. Edit settings.json file
    
    ```
    nano settings.json
    ```

    ```
    {
        "GitHub": "",
        "web_location": "/var/www/html",
        "exported_folder": "public",
        "command": "hugo -D",
        "frequency": "60"
    }
    ```

    **GitHub**: link to GitHub repository without .git in the end. Example:
    
    ```
    "GitHub": "https://github.com/worepix/radimkozak.com"
    ```

    **web_location**: folder where final exported website will be deployed, default <a href="https://httpd.apache.org/" target="_blank">Apache</a> webserver is /var/www/html. Example:
    
    ```
    "web_location": "/var/www/html"
    ```

    **exported_folder**: folder that makes your framework after build command with exported website. Default <a href="https://gohugo.io/" target="_blank">Hugo</a> folder is public. Example:
    
    ```
    "exported_folder": "public"
    ```

    **command**: Command for building website. For Hugo I use `hugo -D`. Example:
    
    ```
    "command": "hugo -D"
    ```

    **frequency**: In what frequency is websitefromgithub script checking if it's new version of source code on GitHub. The value is in minutes. Example:
   
    ```
    "frequency": "60"
    ```

8. Now just start script via pm2
   
    ```
    pm2 start websitefromgithub.js
    ```

9. Save pm2 settings. After restart will websitefromgithub automatically start
   
    ```
    pm2 save
    ```

# Conclusion
The script is in frequency you set checking changes by cloning just git folder by command `git clone --no-checkout <your repository>` and compares the `/.git/refs/heads/master` file to your fully downloaded repository in folder. If this hex isn't same, it will clone the hall website repository, build it with your specific command and move to your specific folder. If it is same, it just do nothing, wait to next specific time and do the all thing again.