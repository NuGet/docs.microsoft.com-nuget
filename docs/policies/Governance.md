---
title: NuGet Project Governance
description: The governance model for NuGet, including roles and responsibilities for committers, contributors, and users.
author: JonDouglas
ms.author: jodou
ms.date: 01/18/2018
ms.topic: article
---

# NuGet governance

> This document is based upon the [Benevolent Dictator Governance Model](http://www.oss-watch.ac.uk/resources/benevolentdictatorgovernancemodel) by the University of Oxford. It is licensed under a [Creative Commons Attribution-ShareAlike 2.0 UK: England & Wales License](http://creativecommons.org/licenses/by-sa/2.0/uk/).

The NuGet project is led by a Benevolent Dictator and managed by the community. That is, the community actively contributes to the day-to-day maintenance of the project, but the general strategic line is drawn by the benevolent dictator. In case of disagreement, the benevolent dictator has the last word.

It is the benevolent dictator’s job to resolve disputes within the community and to ensure that the project is able to progress in a coordinated way. In turn, it's the community’s job to guide the decisions of the benevolent dictator through active engagement and contribution.

## Roles and responsibilities

There are four roles described here: Benevolent Dictator, Committers, Contributors, and Users.

### Benevolent dictator

The NuGet core team is self-appointed as Benevolent Dictator or project lead. However, because the community always has the ability to fork, the team is fully answerable to the community. The project lead is expected to understand the community as a whole and strive to satisfy as many conflicting needs as possible, while ensuring that the project survives in the long term.

In many ways, the role of the benevolent dictator is less about dictatorship and more about diplomacy. The key is to ensure that, as the project expands, the right people are given influence over it and the community rallies behind the vision of the project lead. The lead’s job is then to ensure that the committers (see below) make the right decisions on behalf of the project. Generally speaking, as long as the committers are aligned with the project’s strategy, the project lead will allow them to proceed as they desire.

Additionally, .NET Foundation staff consider the project lead the primary or first point of contact for NuGet for purposes of business operations including domain registrations, and technical services (e.g. code-signing).

### Committers

Committers are contributors who have made sustained valuable contributions to NuGet and are appointed by the Benevolent Dictator. Once appointed, committers are relied upon to both write code directly to the repository and screen the contributions of others. Committers are often developers but can contribute in other ways.

Typically, a committer focuses on a specific aspect of the project, and brings a level of expertise and understanding that earns them the respect of the community and the project lead. The role of committer is not an official one, it's simply a position that influential members of the community assume as the project lead looks to them for guidance and support.

Committers have no authority where the overall direction of NuGet is concerned. However, they do have the ear of the project lead. It is a committer’s job to ensure that the lead is aware of the community’s needs and collective objectives, and to help develop or elicit appropriate contributions to the project. Often, committers are given informal control over their specific areas of responsibility, and are assigned rights to directly modify certain areas of the source code. That is, although committers do not have explicit decision-making authority, they will often find that their actions are synonymous with the decisions made by the lead.

### Contributors

Contributors are community members who submit patches to NuGet. These patches may be a one-time occurrence or occur over time. Expectations are that contributors submit patches that are small at first and grow larger when the contributor, committers, and the project lead have built confidence in the quality of a contributor's patches. Contributors are recognized in the associated product release notes document.

Before a contributor’s first patch is put into the repository, they must sign a [Contributor License Agreement](http://en.wikipedia.org/wiki/Contributor_License_Agreement) or an assignment agreement to the .NET Foundation. The patch can be submitted and discussed but it can’t actually be committed to the repository without the appropriate paperwork in place. To obtain a contributor license agreement, please send a request in email to [contributions@nuget.org](mailto:contributions@nuget.org).

To become a contributor, submit a pull request to one of the following repositories:

- [NuGet Client](https://github.com/NuGet/NuGet.Client)
- [NuGet Gallery](https://github.com/nuget/nugetgallery)
- [NuGet Docs](https://github.com/nuget/nugetdocs)

The detailed process for submitting a pull request varies by repository:

- [Contribution instructions for NuGet Client and NuGet Gallery](https://github.com/NuGet/Home/wiki/Contributing-to-NuGet)
- [Contribution instructions for NuGet Docs](https://github.com/NuGet/NuGetDocs/wiki/Contributing-to-NuGet-Documentation)

### Users

Users are community members who have a need for and use NuGet, as package consumers and/or authors. Users are the most important members of the community: without them, the project would have no purpose. Anyone can be a user; there are no specific requirements.

Users should be encouraged to participate in the life of NuGet and the community as much as possible. User contributions enable the project team to ensure that they are satisfying the needs of those users. Common user activities include but are not limited to the following:

- Advocating for use of the project
- Informing developers of project strengths and weaknesses from a new user’s perspective
- Providing moral support (a thank you goes a long way)
- Writing documentation and tutorials
- Filing bug reports and feature requests
- Participating in community events, such as bug bashes
- Participating on discussion boards or forums

Users who continue to engage with the project and its community will often find themselves becoming more and more involved. Such users may then go on to become contributors, as described above.

## Package succession under special circumstances

In the unfortunate situation where a NuGet account holder is incapacitated or deceased, we’ll work with the community to add appropriate owner/s to the package where the said account has sole ownership and the package is published under an [OSI approved license](https://opensource.org/licenses/alphabetical). To request ownership you must send us the following documents:

1. A photocopy of your government-issued photo ID.
1. One of the following documents proving the previous account holder’s status: 
    - An official, government-issued death certificate if the previous account holder is deceased, or,
    - A certified document such as a certificate signed by a medical professional in charge of the care of an incapacitated account holder.
1. One of the following documents proving your right to ownership: 
    - Marriage certificate showing that you are the surviving spouse of the account holder,
    - Signed power of attorney,
    - Copy of a will or trust document naming you as executor or beneficiary,
    - Birth certificate for the account holder, if you are their parent, or,
    - Guardianship paperwork if you are a legal guardian of the account holder.

If you find yourself in need of invoking this policy, please send us an email at [support@nuget.org](mailto:support@nuget.org) with the ID and version of the package.

## Transparency

Building community trust in the governance of an open-source project is vital to its success. To that end, decision making must be done in a transparent, open fashion. Discussion about the project’s direction must be done publicly. The community should never be caught off-guard by a decision by the Benevolent Dictator. Additionally, discussion about project decisions must be archived so that community members can understand the entire history of a decision and its context.
