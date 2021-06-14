Title: OpenCMIS Release Process

# OpenCMIS Release Process

This is a rough description of the OpenCMIS release process.  
In its current state it covers all steps, but not all details. It should be refined and updated with every release. Parts of could even be scripted.
 

## Preparation 

1. Make sure you have the following pre-requisite software.
   - Java 8
   - Apache Maven 3.x
   - SVN 1.9 or above
   - gpg2 (e.g. GnuPG or GPG Tools for Mac)

1. Pre-requisite credentials.
   - Your PGP Key is public and cross-signed by Apache members on the Web of Trust (see all details at [https://www.apache.org/dev/release-publishing.html](https://www.apache.org/dev/release-publishing.html)).
   - Credentials to deploy to `org/apache/chemistry` in the Maven Repository (all committers should have those).
   - Put your credentials into your `<home>/.m2/settings.xml` [file](release-settings.xml).

1. Prepare JIRA.
   - Create the next version in [JIRA](https://issues.apache.org/jira/plugins/servlet/project-config/CMIS/versions) if it not already exists.
   - Check all open issues in [JIRA](https://issues.apache.org/jira/browse/CMIS) and either fix or postpone them.

1. Get a fresh check out of the source code: `svn co https://svn.apache.org/repos/asf/chemistry/opencmis/trunk/`

1. Run build and tests.
   - Run `mvn clean install`
   - Run `mvn apache-rat:check`
   - Run `mvn site:site -Papache-release`

1. Prepare relase.
   - Run `mvn release:prepare`
     - Select next DEV version (e.g. 1.3.0)
     - The TAG should always be in the form of `chemistry-opencmis-<versionNumber>-RC1` for release candidates. The tag will then have to be renamed upon successful vote.

1. Perform release.
   - Run `mvn release:perform`
   - Close the repository for voting here: [https://repository.apache.org/#stagingRepositories](https://repository.apache.org/#stagingRepositories)
   - Gather the `<stagingRepoRelativeDir>` (e.g. orgapachechemistry-1009/org/apache/chemistry/opencmis)

1. Build and deploy site.
   - Build site.
     - Go to the checkout directory `<opencmisReleaseCheckoutDir>/target/checkout`
     - Run `mvn site:site -Papache-release`
     - TODO: change staging URL to `<deployedSiteDir>` (file://...)
     - Run `mvn site:deploy -Papache-release`
   - Deploy site.
     - Check out web site: `svn co https://svn.apache.org/repos/asf/chemistry/site/trunk/`
     - Create new directory `<websiteDir>/content/java/<versionNumber>/maven`
     - Copy `<deployedSiteDir>` to `<websiteDir>/content/java/<versionNumber>/maven`
     - Add new directory: `svn add <versioNumber>`
     - Submit changes: `svn commit -m 'adding <versionNumber> site docs'`
     - The site should be live at `http://chemistry.staging.apache.org/java/<versioNumber>/maven/`

1. Create RC Dist packages.
   - Check out dist/dev: `svn co https://dist.apache.org/repos/dist/dev/chemistry/`
   - Create new directory: `mkdir chemistry-opencmis-<versionNumber>-RC1`
   - Download packages: `<chemistryReleaseCheckoutDir>/chemistry-dist/release-scripts/deploy-dist.sh <versionNumber> <stagingRepoRelativeDir>`
   - Add all packages: `svn add *`
   - Commit packages: `svn commit -m 'publishing packages for OpenCMIS <versionNumber>'`
   - Artifacts should be live at `https://dist.apache.org/repos/dist/dev/chemistry/chemistry-opencmis-<versionNumber>-RC1/` 


## Vote

1. Send release vote to <dev@chemistry.apache.org>:

		Subject: [VOTE] Release Apache Chemistry <versionNumber> - RC1
		
		Hi all,
		
		OpenCMIS <versionNumber> is ready for voting.
		
		<... add release highlights here ...>
		
		
		You can find the commodity packages release candidate artifacts (for final
		distribution at apache.org/dist) at [1].
		
		The full set of Maven artifacts (for distribution at repository.apache.org and
		Maven Central) is staged at [2].
		
		Sources tag can be found at [3].
		
		Maven generated javadoc/test reports are being deployed at [4].
		
		For detailed release notes check Jira at [5] (unresolved issues will be
		pushed to the next release).
				
		The vote is open for 72 hours and passes if a majority of at least three +1
		Chemistry PMC votes are cast.
		
		
		Please cast your votes!
		
		[ ] +1 Release packages as Apache Chemistry OpenCMIS <versionNumber>
		[ ] -1 Do not release this package because...
		
		Thanks for taking the time to vote!
		
		[1] https://dist.apache.org/repos/dist/dev/chemistry/chemistry-opencmis-<versionNumber>-RC1/
		[2] https://repository.apache.org/content/repositories/<path>/org/apache/chemistry/opencmis/
		[3] http://svn.apache.org/repos/asf/chemistry/opencmis/tags/chemistry-opencmis-<versionNumber>-RC1/
		[4] http://chemistry.staging.apache.org/java/<versionNumber>/maven/
		[5] https://issues.apache.org/jira/issues/?jql=project%20%3D%20CMIS%20AND%20status%20in%20(Resolved%2C%20Closed)%20AND%20fixVersion%20%3D%20%22OpenCMIS%201.0.0%22%20ORDER%20BY%20priority%20DESC

1. Tally and send `[VOTE][RESULT] - PASSED` or iterate from the beginnig until a successful vote is reached


## Publish

1. Promote repository at [https://repository.apache.org/#stagingRepositories](https://repository.apache.org/#stagingRepositories) so it will be synced to Maven Central.

1. Publish packages.
   - Check out dist/release: `svn co https://dist.apache.org/repos/dist/release/chemistry/opencmis`
   - Copy RC packages: `svn export https://dist.apache.org/repos/dist/dev/chemistry/chemistry-opencmis-<versionNumber>-RC1 <versionNumber>`
   - Add packages: `svn add <versionNumber>`
   - Check in: `svn commit -m 'added OpenCMIS <versionNumber> release to dist'`

1. Update the web site.
   - Check out web site: `svn co https://svn.apache.org/repos/asf/chemistry/site/trunk/`
   - Update the OpenCMIS page: `<websiteDir>/content/java/opencmis.mdtext`
   - Update the download page: `<websiteDir>/content/java/download.mdtext`
   - Update the homepage: `<websiteDir>/content/index.mdtext`
   - Maintain the javadoc link in `<websiteDir>/content/java`: `svn propset svn:externals '../java/<versionNumber>/maven/apidocs/ javadoc' .`
   - Check in: `svn commit -m 'updated web site for OpenCMIS <versionNumber>'`

1. Publish site.
   - Wait until the staging is complete!  
     Follow the build at [https://ci.apache.org/builders/chemistry-site-staging](https://ci.apache.org/builders/chemistry-site-staging).
   - Publish site: [https://cms.apache.org/chemistry/publish](https://cms.apache.org/chemistry/publish).

1. Remove old packages: `svn rm https://dist.apache.org/repos/dist/release/chemistry/opencmis/<versionNumber-1>  -m 'removed previous release'`

1. Close version in [JIRA](https://issues.apache.org/jira/plugins/servlet/project-config/CMIS/versions).

1. Rename tag: `svn mv https://svn.apache.org/repos/asf/chemistry/opencmis/tags/chemistry-opencmis-<versionNumber>-RC1 https://svn.apache.org/repos/asf/chemistry/opencmis/tags/chemistry-opencmis-<versionNumber> -m 'renamed tag after successful release'`.

1. Update DOAP file.

1. Wait 24 hours and then send email to <announce@apache.org> (with GPG signature):

		Subject: [ANNOUNCEMENT] Apache Chemistry OpenCMIS <versionNumber> released
		
				
		Hi,

		the Apache Chemistry OpenCMIS PMC is pleased to announce the release of
		Apache Chemistry OpenCMIS <versionNumber>.

		What is OpenCMIS?
		-----------------
		OpenCMIS is a collection of Java libraries, frameworks, and tools around
		the OASIS CMIS (Content Management Interoperability Services) specification [0].

		OpenCMIS <versionNumber>
		------------------------

		<... add release highlights here ...>

		Download OpenCMIS <versionNumber>
		---------------------------------
		OpenCMIS <versionNumber> download packages are available on the Apache mirrors [1]
		or you can get them via the ASF Maven repository [2] as described at [3].
		Refer to https://chemistry.apache.org for OpenCMIS documentation and code samples.
		
		
		Thanks,

		The Apache Chemistry OpenCMIS Dev team


		[0] https://www.oasis-open.org/committees/tc_home.php?wg_abbrev=cmis
		[1] https://chemistry.apache.org/java/download.html
		[2] https://repository.apache.org/index.html#nexus-search;gav~org.apache.chemistry.opencmis~
		[3] https://chemistry.apache.org/java/developing/dev-use-with-maven.html
