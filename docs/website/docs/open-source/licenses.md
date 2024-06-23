# Approved licenses

!!! info "Legal notice"
    This article describes what we consider best practices.

    We are not lawyers, and this is not legal advice.
    Always read the full license text before making a decision.

## Why does open source need licenses?

In many countries, author's rights and intellectual property law protect authors by forbidding unauthorized reproductions, modifications or even usage of their creations.

A license is a contract between the author and users, that describes what the author allows users to do with their work.

- Commercial licenses are usually very restrictive and are sold to users, often for limited periods of time.
- Open source licenses are usually publicly available directly in the repository they apply to, in a file called `LICENSE.txt` or similar.

**If you find a project that does not prominently mention under which license it may be used, do not assume you are allowed to use it any way!**
Even if the project is set to public, until a license specifically allows you to, you should assume copying anything from the project is forbidden. Even taking inspiration may be forbidden.

Be careful, the license may not always apply to the entirety of the project.
For example, at OpenSavvy, licenses never apply to logos or project names.
We may allow users to build a business using our projects, but this must be done under a different branding.

### Using well-known licenses

Licenses fall under contract law, and are therefore quite difficult to write correctly.
We recommend choosing a well-known license to ensure it takes edge cases into accounts, and to facilitate court proceedings if an issue ever happens.

A list of the most well-known licenses is maintained by Microsoft at [choosealicense.com](https://choosealicense.com/licenses/).
The Open Source Initiative maintains a large list of all licenses that fall under their definition of open source software, [available here](https://opensource.org/license), not all of them are well-known.

### Which licenses help the open source ecosystem?

We recognize the [Open Source Definition](https://opensource.org/osd) as declared by the Open Source Initiative (version 1.9).

To be useful to the ecosystem, a license **must consider all contributors to be equal**, meaning that everyone who contributed to the project must retain the same rights over their contribution than the original author has on theirs.

This protects everyone who contributed to the project: if the maintainers want to change the licensing terms, they must either have the approval of all contributors, or remove their contributions. This ensures a contribution can never be used in the future in a way the contributors didn't intend.

A license **must not be revocable or change in any way for already released versions of the project**. If the maintainers and contributors change the licensing terms in the future, users must be able to continue using already-released versions of the project under the exact terms that that specific version was released under.

### Open source doesn't have to be free

While the _distribution_ of open source software must be free, any other service doesn't have to be.
For example, we consider the following practices to be acceptable:

- Selling additional services (e.g. training courses, certifications…)
- Selling development itself
- Selling support (either to help users, or to backport fixes to previous versions)

In each of these examples, maintainers can be paid for their services, but users have the option to do the work themselves, or to pay someone else to do it instead.

!!! question ""
    This doesn't mean we are against closed source software.

    We believe open source is a committment to improving the ecosystem.
    We believe these restrictions ensure the ecosystem is actually improved.

    Closed-source software is created with the aim of benefitting its creators.
    By forbidding reproduction and modification, it locks users into a contract with a specific entity, blocking them from accessing potentially better services, and it stops the ecosystem from benefitting from any technological advancements made during development.
    
    As long as this is clear to all people involved, we have no issues with closed source software.

    However, **we believe it is hypocritical to advertise your project as open source, and later impose restrictions on your users after you have received their trust,** for example by changing to a source-available license.

Even if software is completely free, we still recommend users who have the financial possibility to do so, to sponsor the project. This ensures the project can continue existing in the future. This is especially true for closed source companies: today, virtually no closed source software exists that would work the same without open source software. Closed source companies should attempt to help open source projects in any way they can, financially or even simply by giving their employees a chance to upstream bugfixes and features.

## Our preferred licenses

### For libraries

For libraries, we encourage the use of the **Apache 2.0** license ([available here](https://www.apache.org/licenses/LICENSE-2.0)), because it allows to use the library for any reason, as long as your work is correctly attributed.

This is because libraries are meant to be a part of a larger product that provides features that are naturally different from the library.

### For end-user projects

For end-user projects, we encourage the use of the **GNU Affero General Public License 3.0** license ([available here](https://www.gnu.org/licenses/agpl-3.0.en.html)), because it allows to use the project for any reason (including commercially), but forces users to make publicly available any changes they make.

For example, if you plan to sell a SaaS product, and release it under the AGPL 3.0, other users are allowed to create a competitor based on your software, but they must:

- clearly communicate to their users that it is based on your software,
- publish any changes they make under the AGPL 3.0, meaning you can integrate them back into your version.
