Tycho Metadata Repository Aggregation
-------------------------------------

<b> Management Summary:</b> This plug-in uses Tycho generated p2-metadata to aggregate the p2-repository. Works for Plug-Ins and Features. Experimental.

I created a fork of nexus-p2-repository-plugin and pushed my modifications to it. It is highly untested and hackish, can surely be optimized and will only work if you build your bundles and features with Tycho. It does not work for artifacts of classifier "binary". 

Build the bundle zip, install it by the side of nexus-p2-repository-plugin, nexus-p2-bridge-plugin and nexus-capabilities-plugin (all in version 2.0, get the bundled version from repository.sonatype.org) to the plugin-repository. Start Nexus and add the new "P2 Tycho Repository Aggregator capability" and a new task of type "Rebuild P2 repository (from Tycho Metadata)" to the repository where your Tycho builds live. I suggest to have distinct repositories for plain jars and Tycho build artifacts (so it is assured that the normal nexus-p2-repository-plugin will not produce metadata for Tycho build OSGi bundles, which could result in doubled or broken p2 repository entries).    


Some comments on this plugin:

File Names: 

The nexus-p2-repository-plugin generates

{artifactId}p2Artifacts.xml and {artifactId}p2Content.xml

Tycho is uploading this ones

{artifactId}p2artifacts.xml and {artifactId}p2metadata.xml

Unfortunately, the files generated by Tycho do not frame their content in a proper <repository> header, and so the p2 runtime called by the plug-in refuses to do the merge job. So I had to add the missing lines to both files before merging.

Afterwards I had to find a new approach to create the links from the /plugins and /features to the physical jars. This was quite difficult and the solution I found is pretty hackish (but works for me). 

Thats it. 