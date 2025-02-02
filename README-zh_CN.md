# PostgREST Documentation https://postgrest.org
# PostgREST 文档（简体中文） https://postgrest.org/zh_CN/stable

PostgREST docs use the reStructuredText format, check this [cheatsheet](https://github.com/ralsina/rst-cheatsheet/blob/master/rst-cheatsheet.rst) to get acquainted with it.
<br />
PostgREST 文档使用 reStructuredText 格式，可查看 [速查表](https://github.com/ralsina/rst-cheatsheet/blob/master/rst-cheatsheet.rst) 来熟悉它。

## Install Dependencies / 安装依赖
```bash
# brew unlink python@3.9 && brew link python@3.10

python3 --version
# Python 3.10.1
pip3 --version
# pip 21.3.1 from /usr/local/lib/python3.10/site-packages/pip (python 3.10)

brew install sphinx-doc
# FOR ZSH USER
echo 'export PATH="/usr/local/opt/sphinx-doc/bin:$PATH"' >> ~/.zshrc
# FOR BASH USER
echo 'export PATH="/usr/local/opt/sphinx-doc/bin:$PATH"' >> ~/.bash_profile
sphinx-build --version
# sphinx-build 4.3.2

# Could not import extension sphinx_tabs.tabs (exception: No module named 'sphinx_tabs')
pip3 install sphinx-tabs sphinx_copybutton sphinx-rtd-theme

# TO AVOID COMMAND NOT FOUND ERROR
pip3 install sphinx-intl sphinx-autobuild
which sphinx-intl
# /usr/local/bin/sphinx-intl
which sphinx-autobuild
# /usr/local/bin/sphinx-autobuild
# If NOTICE: sphinx-intl not found
pip3 install -I sphinx-intl sphinx-autobuild
```

## Setup Sphinx-intl / 设置 Sphinx-intl
https://www.sphinx-doc.org/en/master/usage/advanced/intl.html
```bash
# The -v option for sphinx-build command is useful to check the locale_dirs config works as expected. 
# It emits debug messages if message catalog directory not found.
# https://www.sphinx-doc.org/en/master/usage/configuration.html#confval-locale_dirs

sphinx-build -v -b html docs _build
# Running Sphinx v4.3.2
# build succeeded.
# The HTML pages are in _build.

sphinx-build -v -b gettext docs _build/gettext
# GENERATE OR UPDATE POT FILES

sphinx-intl update -p _build/gettext -l zh_CN
# GENERATE OR UPDATE ZH_CN PO FILES FROM POT
# Update: locales/zh_CN/LC_MESSAGES/tutorials.po +3, -3
# Update: locales/zh_CN/LC_MESSAGES/api.po +7, -5
# Update: locales/zh_CN/LC_MESSAGES/admin.po +8, -0
# Update: locales/zh_CN/LC_MESSAGES/how-tos.po +9, -1
# Not Changed: locales/zh_CN/LC_MESSAGES/ecosystem.po
# Not Changed: locales/zh_CN/LC_MESSAGES/schema_structure.po
# Update: locales/zh_CN/LC_MESSAGES/releases.po +5, -5
# Update: locales/zh_CN/LC_MESSAGES/configuration.po +25, -14
# Update: locales/zh_CN/LC_MESSAGES/index.po +1, -0
# Update: locales/zh_CN/LC_MESSAGES/install.po +1, -0
# Update: locales/zh_CN/LC_MESSAGES/auth.po +3, -1
# Update: locales/zh_CN/LC_MESSAGES/schema_cache.po +2, -0


# find . -name \*.po -execdir sh -c 'msgfmt "$0" -o `basename $0 .po`.mo' '{}' \;
# MANUALLY MAKE ALL PO FILES TO MO FILES
```
## Live Preview / 实时预览
```bash
# python3 livereload_docs.py livereload
# Serving on http://127.0.0.1:5500
# NOT RELOAD WHEN MO FILES UPDATED

# FOR HTML AUTO REBUILD
# sphinx-autobuild -b html docs _build

# FOR zh_CN AUTO REBUILD
# SHOULD CHANGE conf.py: language = 'zh_CN'
# SINCE THE rst FILES MOVED TO docs, NEED WATCH locales
# OR po/mo UPDATED, live-reload NOT WORKS.
# https://github.com/coding-to-music/coding-to-music.github.io/issues/188 
sphinx-autobuild -v docs _build --watch locales
# Running Sphinx v4.4.0
# loading translations [zh_CN]... done
# Serving on http://127.0.0.1:8000
# WORKS PERFECTLY, AUTO REFRESH BROWSER WHEN MO FILES UPDATED.
```
