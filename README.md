# quickblog

Sometimes it's nice to add a blog to a repo as a way to record changes
and provide updates. There are many static blog generators available,
so I thought it would be a simple task to add to a repo. After looking
at the most popular static site generators, I saw a lot to dislike,
including:

- dependencies, dependencies, and more dependencies
- requiring all files to be committed to a Git repo (!)
- configuration files
- metadata
- strong opinions hardcoded deep in the source code
- poor documentation combined with weird customization
- Javascript

All I really wanted was a way to turn a directory of markdown files into
a blog. No messing with configuration files, no metadata, no limits on
the ability to customize.

I'm sure such a system exists, but I couldn't find it, so I wrote one in
Bash and Perl. Not that these are my favorite languages, but that solves
the dependency issue, since they'll be on any system I use. The only real
dependency is Pandoc - for converting markdown to html - but that's not
a problem for me.

The resulting html files are plain. I'm okay with that. Customizing the
CSS is trivial (editing a Pandoc template). Sometimes all you need is
a way to get the job done.

# Installation

Here's how I "install". I clone this repo. I create symlinks from
`allposts`, `editpost`, `newpost`, `onepost`, and `viewposts` to ~/bin
(which is in my PATH). They should be executable, but if not, I do that.
Inside `onepost` and `allposts`, I change the variable `$templatedir` to 
the directory holding this repo (fully expanded, as you can't have a `~`
in the path).

# Usage

In the directory you want to hold your blog (or more accurately, your
journal), create the first post by typing

```
newpost foo 'Foo and Such'
```

That creates `index.md` if it doesn't exist. It adds a link to foo.md,
using the description "Foo and Such", plus the date. It creates the
file foo.md, adds the header "Foo and Such", and puts the date at the
bottom. It opens foo.md in Geany, my editor of choice, to add the content.

More posts can be added in the same way. Since index.md already exists,
the new post will be added at the top.

I can then build the blog:

```
allposts
```

There's an index.html file that can be opened in the browser for viewing - you
don't need a web server. That works fine if index.html is already open,
so that I need only to refresh the page. If it's not, I instead call

```
viewposts
```

That builds the blog and opens index.html in Firefox.

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

That recreates foo.html and nothing else.

All that needs to be done to make the pages publicly available is copy
the html files to the web server. If using Github, Bitbucket, or Gitlab
pages, copying them to the correct directory and pushing is all that is
needed. I rely mostly on Fossil, so it's a simple call to Fossil commit
and the new pages are available.

These are small, easy to read files. You can customize anything easily.
You're never required to stick with any decision I've made in order to
use quickblog.
