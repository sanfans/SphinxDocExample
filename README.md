# SphinxDocExample
This repository is an example of how create project documentation with Sphinx. The process is pretty simple, but some guidelines must be followed. There are many web sites that gives you information about that, like:
- [Sphinx Issue](https://github.com/sphinx-doc/sphinx/issues/3382)
- [Sphinx on gh-pages (as here)](https://daler.github.io/sphinxdoc-test/includeme.html)
- [Sphinx on docs folder](https://jefflirion.github.io/sphinx-github-pages.html)
- [Another...](http://nikhilism.com/post/2012/automatic-github-pages-generation-from/)
- There are others. The list is too long to write here, so write on Google **"sphinx github pages"** and enjoy...

However, all this guides requires always some additional work to modify the documentation, as:
- Create a different folder for the generation of the documentation. This is something i don't really like, i usually want all things related to a project in the same directory. Moreover this imply additional work, like setting paths for building with Sphinx, etc. I don't want to start discussions about that, it is something personal. If your opinion is the same, go on reading this guide, otherwise...Bye Bye. Look for other guides, because this one will not fit your needings.
- Change folders name in order to avoid the creation of the /html directory, in which all the builds created by Sphinx are stored. There is no way i'm going to do that every time i make a change in the documentation...madness.
- Copy the html files from the build directory to the one required by GitHub pages or, better, create scripts to do it automatically. This is another good option, but i'm currently using Windows so i don't really know (and i think will never) how to create bash scripts and so on. Moreover this solution is not portable because you have to create a different script for every different OS...not a big deal but, if portable, is better no?

So, at the end, i decided to combine some of the hints found in this guides to define my own way to create documentation for my projects. This required:
- Sphinx build scripts included in the documentation folder
- Don't make any change of directory name, or setting build paths
- Do as less work as possible...lazy forever

I came out with this two solutions, one for each of the possibilities available at the moment:
- Create a **gh-pages** branch and put inside the html of your web-site
- Create a **docs** folder and put inside the html of your web-site
