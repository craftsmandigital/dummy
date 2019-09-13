## Hugo modules for "dummies"
### Part 1. Using hugo modules for a Hugo theme
For the first time I have been able to use the **Hugo modules** feature. Thanks to @chreliot and his [post](https://discourse.gohugo.io/t/how-to-add-a-theme-using-modules-for-beginners/20665), I finally figured out how to use it. I'm not smart enough to figure it out out through the documentation that are available so far.

For other **Hugo modules** noobs that are fighting with **Hugo modules**. I have made an extremely detailed description how to install a theme with **Hugo modules**. I am not going in detail on anything. Just get a site up and running. [Here is the Link to the final Hugo site](https://github.com/craftsmandigital/hugo-test-modules). Just follow the steps carefully to create that Hugo site.

Here is what you have to do:

* Install latest version of **go** on your computer
* Prepare a test site to implement a theme as a **Hugo module**
* Finally you dive into the tasks and code to implement the **Hugo module**

## Install latest version of **go** on your computer

Make sure that you have installed a recent version of go on your computer. [Here is the link to the **go** install](https://golang.org/dl/). Follow the instructions carefully. [The **Hugo mod** commands](https://gohugo.io/commands/hugo_mod/) do not work without doing this. If you use the **Hugo mod** commands, without installing **go**, nothing happens. You don't get an error message as feedback.

## Prepare a test site to implement a theme as a **Hugo module**

The theme **[hugo-xmin](http://github.com/yihui/hugo-xmin)** are used as an example (yes that's exactly the same as @chreliot used in his [post](https://discourse.gohugo.io/t/how-to-add-a-theme-using-modules-for-beginners/20665))

First you have to prepare a Hugo site to test out the **[hugo-xmin](http://github.com/yihui/hugo-xmin)** theme as a **Hugo module**

1. Download the example site for the **hugo-xmin** theme:
You can [download the zip file here](https://github.com/yihui/hugo-xmin/archive/master.zip)
2. Extract the folder [exampleSite](https://github.com/yihui/hugo-xmin/tree/master/exampleSite) to your harddrive
3. Rename exampleSite to **hugo-test-modules**

Later on, you will add the **hugo-xmin** theme as a **Hugo module**.

## Finally you dive into the tasks and code to implement the **Hugo module**

There are different ways to use **Hugo modules** to add a theme to your Hugo site. This is one of them.

When you test your site in this stadium. You get an error message.

```bash
hugo serve
```
`Error: module "hugo-xmin" not found; ...`

That's because no theme is added to the Hugo site.

### For now it has only been bureaucracy. The fun part is starting now:

1. Comment out or delete the variable **theme** in **[config.toml](https://github.com/craftsmandigital/dummy/blob/master/config.toml)** file

   ```toml
   # theme = "hugo-xmin
   ```
   We no longer need this variable since we make use of **Hugo modules** (It is possible to use the **theme** variable to mount modules. For simplicity you use the new preferred method).

   

1. Add this to your **[config.toml](https://github.com/craftsmandigital/dummy/blob/master/config.toml)** to specify a theme as Hugo module:
   ```toml
   [module]
     [[module.imports]]
       path = "github.com/yihui/hugo-xmin"
   ```
   
   > You don't have to specify the folder to mount to, neither that it is a theme you are mounting. By default **Hugo modules** behave as it is a theme. Hugo mounts github.com/yihui/hugo-xmin in the Hugo theme folder.
   
1. Initialize project as **Hugo module**. Go to CLI and type in this command in your Hugo site:
   ```bash
   hugo mod init ugly-dummy
   ```
   YES IT WAS **ugly-dummy** (It really doesn't mater what the parameter is to the **hugo mod init** command, but there are some restrictions on ".", "/", etc.). I think it's more appropriate to name the module as your Hugo site name. In this case hugo-test-modules. but ugly-dummy is also fine.
   
   The command could output something like this:
   
   `go: creating new go.mod: module ugly-dummy`

   *information: a new file* **go.mod** *was created*
   
1. Test your site:
   ```bash
   hugo serve
   ```
   
   Your site should look exactly the same as [this site](https://xmin.yihui.name/)
   
   *information: a new file* **go.sum** *was created*

1. Now its time to upload your finished site to **GitHub**. 
   
   [Create a **GitHub** Repo](https://github.com/new) and name it **hugo-test-modules**
   
   Git commands to upload repo:

   ```bash
   git init
   git add -A && git commit -m "Initial Commit"
   git remote add origin https://github.com/< your username >/hugo-test-modules.git
   git push -u origin master
   ```
   
1. When you now clone your newly updated repo to your machine, there is no need for **git clone --recursive**. It's just plug and play. Just do a regular **git clone**
   ```bash
   git clone https://github.com/< your username >/hugo-test-modules.git
   ```


That was all “happy moduling”

### Part 2. Hugo modules for content
Now it is starting to be realy realy realy funn. Now you are finished with all the bureaucracy and our site is up and running with a Hugo module "The theme". Now lets add som markdownfiles to your content. You can mount folders with markdownfiles from any git repo in the world. The repo dont has to be a Hugo repo. You are going to mount a folder for markdownfiles from this [repo](https://github.com/craftsmandigital/markdown-content-repo). Check it out and be familiar with it. The repo contains a folder testing-hugo-modules that could be mounted, with these files:
* testing-hugo-modules/file-1.md
* testing-hugo-modules/file-2.md

Here is the conifguration that you could drop in your [config.toml](https://) file. I wil explane it later on. Add it just under the theme stuff, under [module] section:
```
[[module.imports]]
    path = "github.com/craftsmandigital/markdown-content-repo"
    disabled = false

    [[module.imports.mounts]]
    source = "testing-hugo-modules"
    target = "content/new-stuff"
```
* path describe the repo you vil mount content from
* source describe witch folder(from root) in mounted repo you could append to your Hugo site ([this is the actual folder](https://github.com/craftsmandigital/markdown-content-repo/tree/master/testing-hugo-modules))
* target describe witch folder(from root) in your Hugo site the mount could apear (content/new-stuff)

After uppdating [config.toml](https://link) there is nothing more to do. It's time to test your site.
```
hugo serve
```
Now you can see your two new markdown posts at the bottom of the start page
* 2019/09/12 file 1 for testing Hugo modules for content
* 2019/09/12 file 2 for testing Hugo modules for content

Clikk on one of the posts. Check out the URL adress
* http://localhost:1313/new-stuff/file-2/

Do you reckognice **new-stuff** from your config.toml file. That was your target for your munting point(content/new-stuff)

### You can use this to mount any kind of reshources to your hugo site. 
That was all, realy realy realy “happy moduling”