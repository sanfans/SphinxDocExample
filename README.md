# SphinxDocExample
This repository is an example of how create project documentation with Sphinx. The process is pretty simple, but some guidelines must be followed. There are many web sites that gives you information about that, like:
- [Sphinx Issue](https://github.com/sphinx-doc/sphinx/issues/3382)
- [Sphinx on gh-pages](https://daler.github.io/sphinxdoc-test/includeme.html)
- [Sphinx on docs folder](https://jefflirion.github.io/sphinx-github-pages.html)
- [Another...](http://nikhilism.com/post/2012/automatic-github-pages-generation-from/)
- There are others. The list is too long to write here, so write on Google **"sphinx github pages"** and enjoy...

However, all this guides requires always some additional work to modify the documentation, as:
- Create a different folder for the generation of the documentation. This is something i don't really like, i usually want all things related to a project in the same directory. Moreover this imply additional work, like setting paths for building with Sphinx, etc. I don't want to start discussions about that, it is something personal. If your opinion is the same, go on reading this guide, otherwise...Bye Bye. Look for other guides, because this one will not fit your needings.
- Change folders name in order to avoid the creation of the /html directory, in which all the builds created by Sphinx are stored. There is no way i'm going to do it every time i make a change in the documentation...madness.
- Copy the html files from the build directory to the one required by GitHub pages or, better, create scripts to do it automatically. This is another good option, but i'm currently using Windows so i don't really know (and i think will never) how to create bash scripts and so on. Moreover this solution is not portable because you have to create a different script for every different OS...not a big deal but, if portable, is better, no?

So, at the end, i decided to combine some of the hints found in this guides to define my own way to create documentation for my projects. This custom way required:
- Sphinx build scripts included in the documentation folder. In this way it is simple for other contributors to update the documentation.
- Don't make any change of directory name, or setting build paths (consider that they could break).
- Do as less work as possible for every documentation update...**FOREVERY LAZY**

I came out with a final solution that satisfy all the requirements. I don't use [gh-pages](https://daler.github.io/sphinxdoc-test/includeme.html) branch, the "old" way to do it, but i use the **/docs folder** approach. The reason for this choice is that gh-pages require that all the documentation files must be put in a separate branch from the source code. But Sphinx need source code to create automatic documentation...mmmmm...so both branches must be present at the same time...mmmmmm...too many folders or too many branch switch. I was too lazy for that.

Using the /docs folder, instead, we are still in the same branch of our main project. In the level above the folder i have all source code needed by Sphinx to create automatic documentation, while in folder i have all the documentation and the files to compile it with Sphinx. No branch switch is required, or multiple repository folders, just documentation compilation.

## The method in 3 steps
### Step 1
You have your beatiful folder with all the amazing source code you have created. But you are good guy...and you want other people understand the code you have wrote, because you know that people that don't do it...go to the hell. So you decide to create something revolutionary, called **DOCUMENTATION**. But you are lazy...you don't want to write a description of all the functions in your source code. Here comes the idea to use [Sphinx](https://www.youtube.com/watch?v=LQ6pFgQXQ0Q) and create automatic documentation from your code. I assume now that you spent some nights figuring out how to use Sphinx, and now you are expert on it. First step is to create a /docs folder inside your repository directory, and create your Sphinx documentation inside it. In my case i ended up with a structure like this (only important files are reported):
```
  repository
  |-- exampleFolder              <-- whatever your repository structure is
  |   |-- Example2.py
  |-- docs
  |   |-- _build                 <-- All builded htmls are here      
  |   |-- scripts
  |   |   |-- All .rst scripts of my docs
  |   |
  |   |-- Makefile
  |   |-- conf.py
  |   |-- index.rst
  |   |-- make.bat
  |
  |--- Example.py
```

### Step 2
If you push this repository on GitHub and [set GitHub pages](https://help.github.com/articles/configuring-a-publishing-source-for-github-pages/) to create the website from /docs folder, it won't work. The reason is that there is no index.html at the root of /docs folder. But, if we dig into \_build/html/, we find that also there is present an index.html...and that's actually the root html of our website!. So...how to cheat GitHub? we can do in this way: we create an index.html that contains an html redirect to our **real** index.html, as here below: 
```html
<meta http-equiv="refresh" content="0; url=./_build/html/index.html" />
```
What we are doing is saying: "hey GitHub, for more information on my website, go to /\build/html/index.html".

Now the folder structure would be like this:
```
  repository
  |-- exampleFolder              <-- whatever your repository structure is
  |   |-- Example2.py
  |-- docs
  |   |-- _build                 <-- All builded htmls are here      
  |   |-- scripts
  |   |   |-- All .rst scripts of my docs
  |   |
  |   |-- Makefile
  |   |-- conf.py
  |   |-- index.rst
  |   |-- index.html        <--------*************NEW FILE HERE************
  |   |-- make.bat
  |
  |--- Example.py
```
Now you can commit the changes and push to your remote repository.

### Step 3
Now you have a remote repository with a /docs folder and an index.html that cheats GitHub and redirect him to your actual website html source code in /\_build/html/ folder. This is still not enough. [Setting GitHub pages](https://help.github.com/articles/configuring-a-publishing-source-for-github-pages/) to create the website from /docs folder, it still won't work. The last trick it to do add a new file in our /docs folder, called .nojekyll. We can consider this as trying to cheat again GitHub; for more informations look at [Jekyll](https://help.github.com/articles/using-jekyll-as-a-static-site-generator-with-github-pages/). To do so, i just pressed "create new file" in my online repository and added to /docs folder a file with name .nojekyll. I commit the change and, magically, the website started working. You can see it in [my repository web site](https://sanfans.github.io/SphinxDocExample/_build/html/index.html). The final folder structure should be something like that:
```
  repository
  |-- exampleFolder              <-- whatever your repository structure is
  |   |-- Example2.py
  |-- docs
  |   |-- _build                 <-- All builded htmls are here      
  |   |-- scripts
  |   |   |-- All .rst scripts of my docs
  |   |
  |   |-- Makefile
  |   |-- conf.py
  |   |-- index.rst
  |   |-- index.html        <--------*************NEW FILE HERE************
  |   |-- .nojekyll         <--------************.nojekill FILE************
  |   |-- make.bat
  |
  |--- Example.py
```

## Important!!!!
The .nojekyll file must be added **AFTER** you have committed all your documentation files, in a separate commit. If you commit at the same time (or before)...unfortunately won't work. I don't know why, but is is what happened to me...it took me quite a while to understand what was wrong.  
