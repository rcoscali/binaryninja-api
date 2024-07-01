# api-docs

In order to build these docs, you'll need:
 - Install sphinx
 - Install the binaryninja python package
    - it can be done through the <BinaryNinjaInstallDir>/scripts/linux-setup.sh that must be run through sudo (sudo and not sudo invocations can be executed for having .desktop files in root & your HOME). Take care if you have an i18n distrib to the name of the Desktop dir ... translate it in the shell script.
 - Install the sphinxcontrib python packages
    - On some distrib you can install through package manager. On others use PIP.

Once all done, you can use 'make html'. I didn't try yet other formats, but you will obsviously need other tools for generated these formats...
Do not hesitate to document more here if needed ;-)
