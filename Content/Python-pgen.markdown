---
draft: true
title: "pgen static site generator"
date: "2014-12-30T00:00:00Z"
tags:
- python
- projects
---

**_pgen_** converts a folder of template (jinja2) and markdown files to
html, while preserving paths.

<!--more-->

siteconfig
----------

_site.source_ and _site.output_ is specified in *_config.toml* or at the
*command-line*. _ArgumentParser_ sets up the required parameters.
*__cmdline* is a dictionary with _source_ and _output_.

``` python
parser = ArgumentParser("pgen static site generator")
parser.add_argument("-s", "--source", help="source directory", required=True)
parser.add_argument("-o", "--output", help="output directory", required=True)
__cmdline = vars(parser.parse_args())
```

Load *_config.toml* if it exists.

``` python
config_file = join(self.source,'_config.toml')
if exists(config_file):
  print("reading %s" % config_file)

  with open(config_file, 'r') as fh:
    _toml = fh.read()
  fh.close()

  __toml_config = toml.loads(_toml)
else:
  exit("Error: cannot find config file, _config.toml")
```

Merge *__cmdline* into *__toml_config* to create the _siteconfig_
dictionary.

``` python
siteconfig = dict(list(__toml_config.items()) + list(__cmdline.items()))
```

List of files
-------------

_source_files_ is a list of files to be converted or copied, without
hidden folders, dotfiles or partials. A *hidden folder* or a *dotfile*
begins with **'.'** and *partial* begins with an underscore **'\_'**

``` python
source_files = []
for root, dirs, files in walk(self.source):
  if files and not "/.git" in root:
    # remove hidden files or directories
    # http://stackoverflow.com/questions/13454164/os-walk-without-hidden-folders

    files = [f for f in files if not f[0] == '.']
    files = [f for f in files if not f.startswith('_')]
    dirs[:] = [d for d in dirs if not d[0] == '.']

    for srcfile in files:
      source_files.append(join(root,srcfile))
```

_output_path_ tells us where to save a file from _source_files_

``` python
def output_path(srcfile_path):
  prefix = commonprefix([self.source, srcfile_path])
  __outfile = relpath(srcfile_path, prefix)
  return join(siteconfig.output, __outfile)
```

We have a list of files to be processed and a location for saving them.

Templates
---------

Templates are written in [Jinja2](http://jinja.pocoo.org/docs/), content
in [Markdown](http://pythonhosted.org//Markdown/) and metadata is
specified in [toml](https://github.com/toml-lang/toml) as front-matter.

Templates contain `site` and `page` dictionaries.

`site` dictionary contains data from the command-line and
`_config.toml`. *source*, *output*, *title* and *date* are always
available. *title* defaults to **pgen static site** when it is not set.

`page` dictionary contains metadata for the current page. *title* and
*date* are always available. *title* defaults to filename with
underscores and hyphens converted to space (Mary-had-little-lamb.md is
converted to 'title' = 'Mary had a little lamb'). If filename contains
date *(2001-09-15-fleece.md)*, *date* defaults to it, otherwise
`site.date`.

`{{ markdown block }}` built-in tag is available in html templates and
renders html for the *markdown block*.

Markdown files can have template tags in them and is therefore processed
through Jinja2. `{{ markdown }}` is not available in markdown files.

Processing Templates
--------------------

1.  split into front matter and src\_content
2.  process front-matter through toml as `page`
3.  Convert raw\_text using **Jinja2** using siteconfig as `site` and
    toml as `page`

Split the source file into _front_matter_ and _src_content_ using the
regular expression `^-{3,}$`.

##### ref:

1.  [Python Regex to match YAML Front
    Matter](http://stackoverflow.com/questions/9756392/python-regex-to-match-yaml-front-matter)
2.  [github
    eyeseast/python-frontmatter](https://github.com/eyeseast/python-frontmatter)

``` python
with open(srcfile, 'r') as tmpl_fh:
  self.war_text = tmpl_fh.read()
tmpl_fh.close()

regex = re.compile(r'^-{3,}$', re.MULTILINE|re.DOTALL)
front_matter, src_content = regex.split(raw_text)[1:]

site = { 'site': vars(siteconfig) }
page = { 'page': front_matter }

__env = jinja2.Environment(loader=jinja2.FileSystemLoader(siteconfig.source))
__template = __env.from_string(src_content)
rendered_content = __template.render(site=site, page=page)
```

Markdown files are rendered first using python-markdown before
processing them as templates.

``` python
if markdown_file(srcfile):
  __template_text = Markdown().convert(src_content)
else:
  __template_text = src_content

...

__template = __env.from_string(__template_text)
```
