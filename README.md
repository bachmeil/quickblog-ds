# quickblog

Sometimes I want to add a blog to a repo as a way to record changes
and provide updates. Given the number of static blog generators out there,
adding a static blog to a repo should be a simple task. After looking
at the most popular static site generators, I saw a lot to dislike,
including:

- dependencies, dependencies, and more dependencies
- requiring all files to be committed to a Git repo (!)
- configuration files
- metadata
- strong opinions hardcoded deep in the source code
- poorly documented weird customization
- Javascript
- Nonstandard ways of handling equations, or not handling them at all

All I really want to do is take a directory of markdown files and turn 
it into a blog. That's a straightforward matter, so I shouldn't have to
mess with configuration files or metadata, have limits on my ability to
customize, or set up a Git repo.

I'm sure such a system exists, but that certainly doesn't describe the
most populat static site generators, so I wrote one in Bash and Perl in
a couple of hours. Not that these are my favorite languages, but that solves
the dependency issue, since they'll be on any system I use. The only true
dependency is Pandoc - for converting markdown to html - but that's not
a problem for me.

By default, the html files are plain. I'm okay with that, because as a
way to keep a project journal in a repo, all I need is readability. 
Customizing the CSS is trivial (you edit a Pandoc template).

# Easy Installation

If you want a default installation, clone this repo and use the included
Bash script:

```
bash install
```

If for some reason that doesn't work:

```
chmod a+x install
./install
```

The only thing to be careful about with the default installation is that
pulling from upstream can kill any customizations you've made.

# Manual Installation

Here's how I "install". I clone this repo. I create symlinks from
`allposts`, `editpost`, `newpost`, `onepost`, and `viewposts` to `~/bin`
(which is in my PATH). They should be executable, but if not, I `chmod a+x`
all five of them first:

```
chmod a+x allposts editpost newpost onepost viewposts
ln -s $(pwd)/allposts $HOME/bin
ln -s $(pwd)/editpost $HOME/bin
ln -s $(pwd)/newpost $HOME/bin
ln -s $(pwd)/onepost $HOME/bin
ln -s $(pwd)/viewposts $HOME/bin
```

I store the default template `template2.html` in `~/.quickblog`:

```
mkdir -p $TEMPLATEDIR
ln -s $(pwd)/template2.html $HOME/.quickblog
```

# Customizing the Installation

There are two things you might want to customize during installation: 
the installation directory and the template directory.

## Installation directory

Open `install` and change `INSTALLDIR` to the directory you want to install
the scripts. It needs to be a directory in your PATH.

## Template directory

Open `install` and change `TEMPLATEDIR` to the directory you want to store
your Pandoc html templates. It does not need to be a directory in your
PATH. Open `allposts` and `onepost` and change the `$TEMPLATE` variable
as needed.

# Other Customizations

## Template file

You can use any html Pandoc template file you want. Edit the `$TEMPLATE`
variable in `allposts` and `onepost` if you don't want to use the default.

## Text editor

The default is Geany, but that can be changed by editing `EDITOR` in
`newpost` and `editpost`.

## Browser

The default browser is Firefox, but that can be changed by editing
`BROWSER` in `viewposts`.

## Anything else

These are all small, easy to read files. It's easy to customize anything.
Read the files and make the necessary changes (stackoverflow.com will
probably be helpful). You are in no way stuck with any decisions I've
made.

# Usage

In the directory you want to hold your blog (or perhaps more accurately, 
your journal), create the first post by typing

```
newpost foo 'Foo and Such'
```

That creates `index.md` if it doesn't exist. It adds a link to foo.md,
using the description "Foo and Such", and the date. It creates the
file foo.md, adds the header "Foo and Such", and adds the date at the
bottom. It opens foo.md in Geany, my editor of choice, so I can add 
content.

More posts can be added in the same way. Since index.md already exists,
all new posts will be added to the top of the index.

Then build the blog and open `index.html` in Firefox:

```
viewposts
```

If I make some changes to the files and don't want to open index.html in
a new tab, I do

```
allposts
```

That rebuilds the blog. I can refresh the browser to see the changes.
Of course, you can always open index.html (or any other files) in your 
browser manually if you want. All content is static, so a web server is
not needed.

If I want to update a post, which means I don't want to mess with index.md,
I type

```
editpost foo
```

which opens foo.md in Geany, as well as index.md, just in case I want to
update the date or do something else.

Finally, if for some reason I want to build only one post (which is only
useful if I have thousands of posts in the directory, which is beyond the
scope of this app) I can call

```
onepost foo
```

That rebuilds foo.html and nothing else.

All that needs to be done to make the pages publicly available is copy
the html files to the web server. If using Github, Bitbucket, or Gitlab
pages, copying them to the correct directory and pushing is all that is
needed. I rely mostly on Fossil, so I do a Fossil commit and the new 
pages are automatically available.

