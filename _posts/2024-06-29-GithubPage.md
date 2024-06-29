---
title: GihubPage
date: 2024-06-29 14:17:00 +0000
categories: [TEMP, SUB_CATEGORIE]
tags: [jekyll]     # TAG names should always be lowercase
---

# Github page build record

## Theme choose
- [jekyll-theme-chirpy](https://chirpy.cotes.page/posts/getting-started/)
    ![alt text](/assets/img/test/image.png)  

## Installing Ruby and JekyllPermalink (Win)
```
Maybe you are wondering that what's the function of this part.--maintaining the pages.
After my first try, I got that we need these environment dependences which is Easy for you to upgrade, isolates irrelevant project files so you can focus on writing.
```

1. Download and install a Ruby+Devkit version from [RubyInstaller Downloads](https://rubyinstaller.org/downloads/). Use default options for installation. 
    ![alt text](/assets/img/test/image-1.png)

2. Run the `ridk install` step on the last stage of the installation wizard. This is needed for installing gems with native extensions. You can find additional information regarding this in the [RubyInstaller Documentation](https://github.com/oneclick/rubyinstaller2#using-the-installer-on-a-target-system). From the options choose `MSYS2 and MINGW development tool chain`.
    ![alt text](/assets/img/test/image-2.png)

3. Open a new command prompt window from the start menu, so that changes to the `PATH` environment variable becomes effective. Install Jekyll and Bundler using `gem install jekyll bundler`.
   ![alt text](/assets/img/test/image-3.png)

4. Check if Jekyll has been installed properly: `jekyll -v`.
   ![alt text](/assets/img/test/image-4.png)

## Using the Chirpy Starter to create a new site

Sign in to GitHub and browse to [Chirpy Starter](https://github.com/cotes2020/chirpy-starter), click the button `Use this template` > `Create a new repository`, and name the new repository USERNAME.github.io, where USERNAME represents your GitHub username.
![alt text](/assets/img/test/image-5.png)
![alt text](/assets/img/test/image-6.png)

