# api-docs

In order to build these docs, you'll need:
 - Install sphinx
 - Install the binaryninja python package
    * it can be done through the <BinaryNinjaInstallDir>/scripts/linux-setup.sh that must be run through sudo (sudo and not sudo invocations can be executed for having .desktop files in root & your HOME). Take care if you have an i18n distrib to the name of the Desktop dir ... translate it in the shell script.
 - Install the sphinxcontrib python packages
    * On some distrib you can install through package manager. On others use PIP.
 - Set the BNLICENSE env var with the path of your license file:
    * $ export BNLICENSE=<LicenseDirectory>/license.txt
 - Set the BN_DISABLE_USER_PLUGINS env var to disable user plugins that are not of great interest for the api docs
    * $ export BN_DISABLE_USER_PLUGINS

Once all done, you can use 'make html'. I didn't try yet other formats, but you will obsviously need other tools for generated these formats...

```bash
rcoscali@ubuntu-lx-23-10:~/Sources/binaryninja-api/api-docs$ make
Please use `make <target>' where <target> is one of
  html       to make standalone HTML files
  dirhtml    to make HTML files named index.html in directories
  singlehtml to make a single large HTML file
  pickle     to make pickle files
  json       to make JSON files
  htmlhelp   to make HTML files and a HTML help project
  qthelp     to make HTML files and a qthelp project
  applehelp  to make an Apple Help Book
  devhelp    to make HTML files and a Devhelp project
  epub       to make an epub
  epub3      to make an epub3
  latex      to make LaTeX files, you can set PAPER=a4 or PAPER=letter
  latexpdf   to make LaTeX files and run them through pdflatex
  latexpdfja to make LaTeX files and run them through platex/dvipdfmx
  text       to make text files
  man        to make manual pages
  texinfo    to make Texinfo files
  info       to make Texinfo files and run them through makeinfo
  gettext    to make PO message catalogs
  changes    to make an overview of all changed/added/deprecated items
  xml        to make Docutils-native XML files
  pseudoxml  to make pseudoxml-XML files for display purposes
  dashdoc    to make a Dash docset
  linkcheck  to check all external links for integrity
  doctest    to run all doctests embedded in the documentation (if enabled)
  coverage   to run coverage check of the documentation (if enabled)
  dummy      to check syntax errors of document sources
```

The built documentation is then available in the build/<target> directory (build/html for 'make html', build/singlehtml for 'make singlehtml', etc ...)
Do not hesitate to document more here if needed ;-)

With a basic dev workstation setup from Ubuntu 24.04 I successfully built: 
 - html, dirhtml, singlehtml
 - man (you'll have to install `binaryninjaapi.1` manually in /opt/binaryninja/man/man1 and add dir /opt/binaryninja/man to your MANPATH)
 - latexpdf (with 'latexmk' debian package, a 711 pages `BinaryNinjaAPIDocumentation.pdf` was generated)

## Edit 
  I didn't succeed in using my license, then I started to disable the code for verifying license. Here is the patch I used ... and you can avoid setting the env vars BNLICENSE and BN_DISABLE_USER_PLUGINS. Of course adapt the `/opt/binaryninja/python/binaryninja/__init__.py` path to whereabout is your binaryninja install directory.

> [!CAUTION]
> ***Take care to revert your changes after having generated your files for avoiding to changes binary ninja behaviour***

```patch
+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
--- /opt/binaryninja/python/binaryninja/__init__.py.orig	2024-07-01 10:44:49.485119894 +0200
+++ /opt/binaryninja/python/binaryninja/__init__.py	2024-07-01 10:45:32.667703975 +0200
@@ -190,10 +190,7 @@
 		) and sys.stderr.isatty():
 			log_to_stderr(LogLevel[min_level])
 		core.BNInitRepoPlugins()
-	if core.BNIsLicenseValidated():
-		_plugin_init = True
-	else:
-		raise RuntimeError("License is not valid. Please supply a valid license.")
+	_plugin_init = True
 
 
 _destruct_callbacks = _DestructionCallbackHandler()
+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
```

> [!TIP]
> Perhaps there is a better way to be able to generate these docs but until someone found it ... it is better than nothing ;-)

# cppdocs

I successfully generated the cpp docs (the ones that are published on [BinaryNinja C++ API](https://api.binary.ninja/cpp/) by just using Doxygen
The makefile try to generate doc through poetry but a toml file is missing. I assumed it was a deprecated generation method.

```bash
$ doxygen Doxyfile-HTML
```


