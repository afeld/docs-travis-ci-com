---
title: Community-Supported Languages
layout: en
permalink: /user/languages/community-supported-languages/
---

There are many programming languages out there, and Travis CI would like
to support as many as possible.

However, the Travis CI team often lacks the expertise to make this
a reality.
This is where the community-supported languages come in.

## What does 'community-support' mean?

The community-supported lanugages are those programming languages the support
of which is provided by self-identified experts in the langugages'
respoective community.

Following the process described below, your favorite language could be
the next community-supported language on Travis CI!

## Process to add a new community-supported language

1. Gather a group of 3 or more volunteers versed in the desired language
  to provide support.
1. Create pull requests (details below).
1. Work with Travis CI team to get the PRs production ready.
1. Provide ongoing support for the issues involving the language.

A group of 3 is a minimum to support a language.
This allows redundancy in providing support when a member of
the support team is unavailable.

## Technical Details

### Code

There are a few repositories (only 1 is required) that realizes support
for builds in a new languages.

1. [travis-build](https://github.com/travis-ci/travis-build)

    This is the only repository required for the new language support.

    Create a new class, inheriting from `Travis::Build::Script`, that implements
    reasonable defaults for your language's build stages.

    Basic build follows these stages:

    `configure` → `setup` → `announce` → `install` → `script`

    There are other phases that can be customized for a particular language;
    we can work with you to identify and implement the customization
    if you think it is appropriate to do so.

	NOTE: The `configure` phase runs before `sudo` is disabled in the container builds,
	so if you need to use `sudo` to set up your language environment
	(e.g., install Ubuntu packages), you should do that here.

1. [travis-core](https://github.com/travis-ci/travis-core)

    If you want to support build matrix expansion based on various language
    versions (e.g., Ruby 2.2, 2.1, etc.), `travis-core` needs to know about it.

    `Build::Config::ENV_KEYS` defines which keys are possible matrix dimensions,
    and `Build::Config::EXPANSION_KEYS_LANGUAGE` defines which keys among
    the possible ones are actually expanded based on the `language` value.

1. [travis-web](https://github.com/travis-ci/travis-web)

    If the language does provide the build matrix expansion, it would be nice
    to have this information visible to the end user.

    To make this happen, you need to tell `travis-web` to pick up the value
    from the job's data, and display it.

    See [this PR](https://github.com/travis-ci/travis-web/pull/313) for an example.


It is important to note that languages are configured at the build time,
so that components are downloaded every time a job runs.

To save build time, you should limit your language resource usage to a minimum.

### Testing `travis-build` changes

Testing will be done in our staging environment, which, at the moment, is a shared
resource.
As such, testing the proposed changes could take some coordination between you and
the Travis CI team.

## List of community-supported languages

In alphabetical order, they are:

1. [C#](../csharp)
1. [D](../d)
1. [Dart](../dart)
1. [Haxe](../haxe)
1. [Julia](../julia)
1. [R](../r)

