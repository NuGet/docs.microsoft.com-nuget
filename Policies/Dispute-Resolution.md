# NuGet Dispute Resolution

This document is a recommended dispute resolution process for community members to help resolve disputes with other NuGet publishers.  

## Process

1) Contact the owners of the package you have the dispute with using the ‘Contact Owners’ link on the package details page.  Explain your issue in a kind and direct manner.

2) Send a copy of your message to [support@nuget.org](mailto:support@nuget.org) so that NuGet and the .NET Foundation are aware of your dispute.

3) Wait a maximum of 30 days and if you don’t have resolution notify [support@nuget.org](mailto:support@nuget.org) again.  The NuGet.org support team will get involved and try to work this out.

## Prohibited Use

The following things are not allowed on the public NuGet gallery and will be immediately removed without discussion.  Package owners will be notified if their package is removed.

•	Malware, adware, or any kind of spyware;

•	Packages that are designed to harm a developer’s workstation or their organization;

•	Copyright infringement or license violation;

•	Illegal content;

•	“Squatting” on package names that you plan to use but are not actually using. This also includes publishing packages that have zero productive content. Either publish code and get going or concede the name to someone who actually has a product to ship; and

•	Attempting to make the gallery do something that it is not explicitly designed to do.


If you find a package that is in violation of any of these items, click the ‘Report Abuse’ link on the package details page and submit a report.


## Summary

The growth of the NuGet ecosystem has been amazing.  There are a lot of packages out there now, and more software companies are getting involved and publishing packages.  This sometimes brings about disputes similar to the following scenarios (fictitious names used):

1) Northwind Traders makes a CRM system and they provide client drivers as a downloadable MSI from their website.  Alan wants to make it easier to use Northwind’s client library, and turns it into a NuGet package called “NorthwindTraders.Client”  Northwind would like to maintain an official NuGet package for their client library, and files a dispute against Alan.

2) Nancy is a customer of the AdventureWorks web service and wrote a library to make programmatic usage of their service easier.  She bundled this library as a NuGet package and published it as “Nancy.AdventureWorks.WebClient”  AdventureWorks now has an official client library available and would like to direct developers to that library instead of the tool that Nancy previously published.

In both scenarios Alan and Nancy do not appear to be acting with bad intentions, rather they are supporting these tools by contributing their own time and code to make improvements and help other customers of Northwind and AdventureWorks. 

To date, every dispute brought to NuGet's attention has been resolved between the individual parties without NuGet administrators needing to get involved.  Many of these disputes have been resolved between organizations and developers because they share the same goal: make a developers job easier.

The NuGet ecosystem is moving at a fast moving pace, and everyone wants their projects to “just work”.  When people collaborate to find the best solution, it usually works out much better than if a third party gets involved and makes a judgement.  We know there are very good lawyers at many organizations but it is our belief that when developers communicate with other developers a better technical solution for the community can be achieved.

## Changes

This policy is subject to change at the discretion of the NuGet administrators and the .NET Foundation.  It is available on [GitHub in source form](https://github.com/NuGet/NuGetDocs/blob/master/NuGet.Docs/Consume/Dispute-Resolution.md) with a detailed change history.
