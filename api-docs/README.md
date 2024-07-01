# api-docs

In order to build these docs, you'll need:
 - Install sphinx
 - Install the binaryninja python package
    - it can be done through the <BinaryNinjaInstallDir>/scripts/linux-setup.sh that must be run through sudo (sudo and not sudo invocations can be executed for having .desktop files in root & your HOME). Take care if you have an i18n distrib to the name of the Desktop dir ... translate it in the shell script.
 - Install the sphinxcontrib python packages
    - On some distrib you can install through package manager. On others use PIP.
 - 
 - Set the BNLICENSE env var with the path of your license file:
    - $ export BNLICENSE=<LicenseDirectory>/license.txt
 - Set the BN_DISABLE_USER_PLUGINS env var to disable user plugins that are not of great interest for the api docs
    - $ export BN_DISABLE_USER_PLUGINS
Once all done, you can use 'make html'. I didn't try yet other formats, but you will obsviously need other tools for generated these formats...
Do not hesitate to document more here if needed ;-)

Edit: 
  I didn't succeed in using my license, then I started to disable the code for verifying license. Here is the patch I used ... and you can avoid setting the env vars BNLICENSE and BN_DISABLE_USER_PLUGINS

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
