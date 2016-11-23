Release Management GuideBook
============================

The Release Management GuideBook (Release Management for short)
contains a set of rules and guidelines for managing software releases.

This guide can be used for both libraries and complete applications.
It's designed mainly for open-source/free software but can also be used
for closed (or proprietary) software.

## Rules

These listed rules are to be followed strictly, without exception.

* Information about the release and steps for upgrading to a new release
  must be provided in a human readable format (eg. Markdown or rest-doc).
* Use [Semantic versioning] for versing numbering and public API stability.
* Write stability `ALPHA,BETA,RC` always in uppercase, and full (not a or b.)
* Omit `STABLE` within the release name and version number.
* Keep a changelog as described in [keepachangelog.com] - 
  (and see additional notes below).
* Don't increase Platform requirements within minor/patch release,
  unless this requirement is widely available.
* Keep upgrade instructions in `UPGRADE.md` (or `UPGRADE-x.y.md` when the file
  becomes to big otherwise). And see additional notes below.
* Include `[SECURITY RELEASE]` when the release contains security patches
  Eg. `Release v1.0.0-BETA2 [SECURITY RELEASE]`.
* Update the release title of broken release with `[YANKED]`.
  Eg. `Release v1.0.0-BETA2 [YANKED]`.
* Suffix (full) version numbers always with `v` (in lowercase).
  Eg. `v1.0.0-BETA1`, `v1.0.0` (see notes below).

[Semantic versioning]: http://semver.org/
[keepachangelog.com]: http://keepachangelog.com/en/0.3.0/

### Broken releases

A release is considered broken when it's impossible to it use at all
or when meta-data is wrong (eg. forgot to update the version number).

A broken release must be followed-up with a new release.
**Don't reuse the old version number!**

A broken release includes (but is not limited to):

* Syntax/linting errors.
* The bundled Installer is broken.
* Forgot to remove temporary debugging information.
* Versioning information in the code was not updated.
  or wrongfully changed.
* Violation of the BC promise
  (eg. bumped platform requirements before major release).
* Security bug introduced (by locked dependency).

### Versioning suffix

All (full) version numbers must always be suffixed with `v` (in lowercase).
Eg. `v1.0.0-BETA1`, `v1.0.0`.

There is some discussion about using, or omitting the `v` suffix.
For all clarity this sections was included.

The maintainers of the Git project recommend to always suffix version numbers
with `v` (for Git tags) to make it explicit it's a version number.
And to prevent confusion with branch-names.

Some developers however advise to omit the suffix, as it's redundant,
but this number of users is decreasing.

So for all feature releases, the version number must be suffix with
a lowercase `v`.

## Upgrade format

TBD.

* Was/becomes examples
* Use FQCN only once per section to save on excessive text block

## Changelog format

Use format as described in [keepachangelog.com].

Keep changelog per major version, or minor (when the file
becomes to big otherwise).

*It's advised (but not required) to use a software tool to generate
the changelog. Maintainers can use [HubKit] to generate a changelog.*

**Note:** The `Security` group MUST be placed first,
so users cannot mis important security fixes.

## Guidelines

The following set is recommended but not mandatory, project maintainers
are advised to follow them, or define there own set or rules.

### Plan ahead

A good release is planned, contains only that what was envisioned.
And is delivered on time. Don't be afraid to reject features for the next
release when they are not good enough yet.

Be watchful for big releases, the more changes the greater the chance
something is broken. As with software development itself: don't do anything
more then what is needed to get the job done.

### Release often

Release as often as possible, don't wait endlessly with a new release.
A good roadmap and planning will help to streamline this and prevent
broken releases.

*It's better to have an early beta release with missing functionality
then a stable release with half working functionality.*

Even when development is still going on and not everything is stable
yet, it will help early adapters to start testing and lock to a specific
version without the risk of pulling-in new changes.

*A release is more then a presentation of features, it's a promise to
users that nothing changes for this release (it's written in stone).*

When support for older minor versions is provided, make a new patch release,
at least once a month. Eg. at the end or start of the month.
Users now have an indication when a fix for a bug is available,
without having to update every day/week or waiting for to long.

**Note:** Proper automated tests and continues integration ensures
a release is functional. Delaying a patch release because you are
not sure it's stable enough is the indication of a bigger problem.

### Maintenance period

A release must be maintained for a period of time, that is
the maintainers provide and accept patches to fix problems.
And make new patch releases.

How long this maintenance period is, heavily dependents on your team size,
complexity of the software and actual usage of older versions.

**Note:**

> Any old version you maintain is a burden, it's a choice
> wether you "can" provide support for these older versions.
> Don't feel bad when you can't maintain all versions.

Maintenance is divided into two types: minor and major.

* A Minor release is usually maintained until the next Minor release
  or 6 months for bug fixes and at least 1 year in total for security fixes.
* A Major release is usually supported for 18 months or two 2 years
  for bug fixes, plus an additional 6 months for security fixes.

  When possible paid, extended support may be provided for commercial users.
  This is however mandatory.

**Caution:** Don't make the maintenance period for security fixes to short,
as this risks leaving users vulnerable to attacks. 6 months minimum should
be enough.

## Stability

Each release follows a clear path of stability. Maintainers should not ignore
this path as it will make things harder when an unexpected changes are required.

* `ALPHA` indicates that work is still going on,
  things may be broken or undecided. Only for developers.
* `BETA` is usable for testing and early adapters, some features
  may be removed when they remain unstable.
* `RC` is for bug fixes only, no new features can be introduced
  unless required to fix a Security problem.

## Major release type

There three distinct major release types, with a clear stability
path to follow. The time period of this path is undefined, any release
may follow shortly after the previous one, but should have at least one
day in between.

A Stability between `[]` is considered optional and can be skipped
when development is stable enough.

### Pre-1.0

This is the initial development phase, no (API) stability
should be expected at this point. This provides for receiving
early feedback and allows for experimentation.

To prevent delays in development, the support cycle for minor releases
should be short (no more then 2 months or 2 minor releases a time),
or support for older versions can be omitted completely.

Stability path: ALPHA -> BETA -> RC -> STABLE.

* Make a minor release directly after the completion of any (planned)
  refactoring (with BC breaks), don't delay.
* Don't break the API between patch releases, use patch releases only
  for bug fixes.
* Use `0.x` instead of `1.0.0-ALPHA1`

### Transitional release

This is the preferred major release type, it provides a smooth
transition for the users and implementors.

The (last) minor version of the current Major contains a list of
deprecations, that will be removed in "this" new Major release.

Once the user has taken care of all deprecations (warnings), and
upgraded to the new API it's possible to switch to the new version
by simply installing the new version.

Stability path: BETA -> [RC] -> STABLE.

The RC stability can be skipped, but this should be carefully considered
for each release plan.

### Refactor release

This is a more radical release plan as no smooth transition
is provided for the users and implementors.

It should only be used to solve problems that cannot be done cleanly by
deprecating first. This may include switching to a Platform version
(which requires syntax changes) or upgrades of dependencies that
cannot be provided with an BC adapter.

Stability path: ALPHA -> BETA -> RC -> STABLE.

The number of this release type should be limited, eg.
should have at least 2 stable major releases in between.

## Credits

Created by [Sebastiaan Stok](https://github.com/sstok) provided under the
terms of [Creative Commons Attribution Non Commercial Share Alike 3.0](https://creativecommons.org/licenses/by-nc-sa/3.0/legalcode)

:heart: Contributions are welcome.
