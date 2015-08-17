### Web Deploy Package with ISAPI

A Web Deploy Package can easily be created by Visual Studio, if you are working with .NET or some other Microsoft technology, however, if you are working of other technologies, like Delphi, it may be a little tricky.

This repository is an example of a Web Deploy Package that contains only a simple ISAPI. It is basically some IIS config files packet in a predefined structure with the objetive of make the deploy on Azure/AWS more simple.

In fact, you can generate a package like this with the IIS itself. You just need to configure your ISAPI on IIS and use the Export Application option. I have to change some of the generated configs o make it work correctly in IIS 8.5 (I exported from IIS 7.5).

I tested this package on IIS 8.5 running on a Windows Server 2012 R2 Core.

### AWS Elastic Beanstalk Deploy Configs

In order to deploy this package on AWS Elastic Beanstalk I have to make some customizations.

#### IIS
I created the `.ebextensions\00 iis isapi.config` which changes some IIS configs in order to be possible to run ISAPI applications, by default, these applications are not allowed.

#### NewRelic
The `01_install_newrelic.config` will configure the [NewRelic Server Monitor](http://newrelic.com/server-monitoring). You just have to configure these environment variables:
- NEW_RELIC_APP_NAME
- NEW_RELIC_LICENSE_KEY
If you don't want that, simple delete the file.

### The ISAPI Application

The application is a dummy DataSnap REST server. It has some sample methods that can be tested accessing this paths:
- yourapp.elasticbeanstalk.com/datasnap/sample_isapi.dll/datasnap/rest/TServerMethods1/ReverseString/sample
- yourapp.elasticbeanstalk.com/datasnap/sample_isapi.dll/datasnap/rest/TServerMethods1/EchoString/sample

In order to customize this package for your ISAPI application, you may have to replace some configs on these files:
- archive.xml: Path properties
- parameter.xml: defaultValue, description, match
- 00_iis_isapi.config: ISAPI DLL name on isapi_restrictions command.
