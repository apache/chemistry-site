Title: ObjectiveCMIS

# Welcome to Apache ObjectiveCMIS 


## Introduction

Apache Chemistry ObjectiveCMIS is a CMIS client library for Objective-C language.

The library is primarily targeted at iOS application development and aims to provide an interoperability API to CMIS based repositories.
However, as the base library is built on the Foundation.framework it can be used for developing Mac OSX applications as well. 

 The library is compatible with iOS v8.0 SDK and OS X v10.9 SDK or later. If your project needs to support older SDK versions, please download version 0.5 from the [Archives][archives].

### Where to get it from

#### Release 0.6 (2017-07-31)

[Full Download page](http://www.apache.org/dyn/closer.lua/chemistry/objectivecmis/0.6)

Package| zip
------|---
ObjectiveCMIS client source|[Download][srczip] ([md5][srcmd5], [sha1][srcsha], [pgp][srcpgp])
ObjectiveCMIS client binary|[Download][binzip] ([md5][binmd5], [sha1][binsha], [pgp][binpgp])


#### Older Releases

For older releases please browse the [Archives][archives].

#### Verify authenticity of Downloads package
While downloading the packages, make yourself familiar on how to verify their integrity, authenticity and provenience according to the Apache Software Foundation best practices. Please make sure you check the following resources:

* [Artifact verification](https://chemistry.apache.org/project/verification.html) details
* Developers and release managers PGP keys are publicly available [here](https://www.apache.org/dist/chemistry/KEYS)

### Get ObjectiveCMIS code

* binary distribution
* source code distribution


### What the library contains

The ObjectiveCMIS library is distributed as a ZIP file containing

* the library:  _libObjectiveCMIS.a_ 
* public header files: contained in the _ObjectiveCMIS_ folder
* documentation: provided as a docset (generated using appledocs).


### Minimum Requirements

The library is making use of Objective-C automated reference counting (ARC). Therefore the library is compatible with iOS SDK v5.x or later.

For development we recommend the latest available version of XCode and iOS/Mac OSX SDKs.


### How to include the library on your XCode project

The easiest way to include the ObjectiveCMIS library into your project is to unzip the ObjectiveCMIS ZIP file.

Then go to the 'File' menu in XCode and select the 'Add Files to…' option. Add the headers and library files contained in the ZIP distribution to your project:

* Make sure that the library is included in the list of frameworks/libraries. Select the build target and go to Build Phases. libObjectiveCMIS.a should be listed in the Link Binary with Libraries option. 
* The CMIS headers should be included in the 'Copy Headers' section of Build Phases for your build target.



## Getting Started


### Block based structure

ObjectiveCMIS calls to CMIS repositories are asynchronous. Callback handling is facilitated utilising _block_ structures available in Objective-C. Further information on the use of blocks in Objective-C can be found directly at [http://developer.apple.com/library/ios/#documentation/cocoa/Conceptual/Blocks/Articles/00_Introduction.html](http://developer.apple.com/library/ios/#documentation/cocoa/Conceptual/Blocks/Articles/00_Introduction.html).


### At the Beginning - There is a CMIS Session

It all starts with establishing a session to the CMIS repository. The class to facilitate this is 'CMISSession'.

The code snippet below demonstrates how to create a session with a minimum set of parameters (used for access/authentication)


    CMISSessionParameters *params = [[CMISSessionParameters alloc] initWithBindingType:CMISBindingTypeAtomPub];
    params.atomPubUrl = [NSURL URLWithString:cmisAtomPubLocation];
    params.username  = @"<yourusername>";
    params.password = @"<yourpassword>";
    params.repositoryId = self.repositoryId;

    //Connect to session

    [CMISSession connectWithSessionParameters:params completionBlock:^(CMISSession *session, NSError *error){
       if( nil == session )
       {
          // do your error handling here
       }
       else
       {
          self.session = session;
       }
    }];


#### How to get to the repository ID

In the code above you may have noticed that we used the repository ID in the session parameters. This is a required parameter to be passed into the 'connectWithSessionParameters:completionBlock:' method. CMISSession provides a convenience method to retrieve the repositories. This is demonstrated in the code snippet below. 


    __block CMISSessionParameters *params = [[CMISSessionParameters alloc] initWithBindingType:CMISBindingTypeAtomPub];
    params.atomPubUrl = [NSURL URLWithString:cmisAtomPubLocation];
    params.username  = @"<yourusername>";
    params.password = @"<yourpassword>";

    [CMISSession arrayOfRepositories:params completionBlock:^(NSArray *repos, NSError *error){
       if(nil == repos)
       {
          // error handling
       }
       else
       {
          CMISRepositoryInfo repoInfo = [repos objectAtIndex:0];
          params.repositoryId = repoInfo.identifier;
          [CMISSession connectWithSessionParameters:params completionBlock:^(CMISSession *session, NSError *sessionError){
             if(nil == session)
             {
                 // do your error handling here
             }
             else
             {
                self.session = session;
             }
          }];
       }
    }];


[srczip]: https://www.apache.org/dyn/closer.lua/chemistry/objectivecmis/0.6/chemistry-objectivecmis-0.6-src.zip
[srcmd5]: https://www.apache.org/dist/chemistry/objectivecmis/0.6/chemistry-objectivecmis-0.6-src.zip.md5
[srcsha]: https://www.apache.org/dist/chemistry/objectivecmis/0.6/chemistry-objectivecmis-0.6-src.zip.sha
[srcpgp]: https://www.apache.org/dist/chemistry/objectivecmis/0.6/chemistry-objectivecmis-0.6-src.zip.asc

[binzip]: https://www.apache.org/dyn/closer.lua/chemistry/objectivecmis/0.6/chemistry-objectivecmis-0.6-bin.zip
[binmd5]: https://www.apache.org/dist/chemistry/objectivecmis/0.6/chemistry-objectivecmis-0.6-bin.zip.md5
[binsha]: https://www.apache.org/dist/chemistry/objectivecmis/0.6/chemistry-objectivecmis-0.6-bin.zip.sha
[binpgp]: https://www.apache.org/dist/chemistry/objectivecmis/0.6/chemistry-objectivecmis-0.6-bin.zip.asc

[archives]: https://archive.apache.org/dist/chemistry/objectivecmis