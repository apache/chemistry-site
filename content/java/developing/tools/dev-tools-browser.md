Title: OpenCMIS Browser

# OpenCMIS Browser

The CMIS Browser is a simple web based tool to browse CMIS enabled
repositories that support the AtomPub binding. It sits between the web
browser of the end-user and the CMIS repository. It applies stylesheets to
the Atom entries and feeds that repository returns and creates HTML pages
that enable the end-user to navigate through the repository.

The CMIS Browser consists of a small WAR file that doesn't require any
configuration. Deploy it to a servlet engine and type
`http://<host>/<context>/browse` in your web browser. Enter the URL of
the AtomPub service document into the input box and start browsing.


## Build and Deploy the CMIS Browser

1. [Build OpenCMIS](../../how-to/how-to-build.html)
1. A ready-to-use WAR file should now exist in `/chemistry-opencmis-test/chemistry-opencmis-test-browser-app/target`.
1. Deploy the WAR file to your favorite servlet engine.


