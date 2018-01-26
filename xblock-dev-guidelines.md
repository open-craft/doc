# XBlock Development

Information on XBlocks, how to develop them, write effective tests, and conform to coding standards.

## Introduction

XBlocks are python libraries used to provide custom course units for learners and instructors on the Open edX platform.

### Resources

Online resources include:

* [Open edX XBlock Tutorial](http://edx.readthedocs.io/projects/xblock-tutorial/en/latest/overview/index.html): the best source of information on XBlocks and how to develop them.  Particularly useful sections are:

    * [XBlock Overview](http://edx.readthedocs.io/projects/xblock-tutorial/en/latest/overview/index.html)

    * [Anatomy of an XBlock](http://edx.readthedocs.io/projects/xblock-tutorial/en/latest/anatomy/index.html)

    * [XBlock Concepts](http://edx.readthedocs.io/projects/xblock-tutorial/en/latest/concepts/index.html)

* [Open edX XBlocks Directory](https://openedx.atlassian.net/wiki/spaces/COMM/pages/43385346/XBlocks+Directory): list of existing XBlocks, their support status, and links to their code and maintainers.

* [Quote of the Day XBlock](https://github.com/open-craft/quote-of-the-day-xblock): example built for [Eugeny's XBlock tutorial for the Open edX Conference 2017](https://openedx.atlassian.net/wiki/spaces/COMM/pages/160365365/Open+edX+2017+Presentations#OpenedX2017Presentations-Tutorials,workshops,etc.).

* [Open edX Developer's Guide](http://edx.readthedocs.io/projects/edx-developer-guide): guidelines for developers on the Open edX platform.

### Prerequisites

XBlocks are written in python, and so [all the guides (including this one) assume](http://edx.readthedocs.io/projects/xblock-tutorial/en/latest/overview/introduction.html#prerequisites) that you have a basic understanding of how to write python, Javascript, HTML, and CSS.

You do not need to know everything about the Open edX platform to write XBlocks, or to start the tutorial, but some [basic understanding of the LMS and the Studio editor](http://edx.readthedocs.io/projects/xblock-tutorial/en/latest/front_matter/index.html) are useful.

We also use git for source code management, and to deploy XBlocks to Open edX environments.  See github's [Hello World](https://guides.github.com/activities/hello-world/) page for how to start using git.

## Creating a new XBlock

edX provides a template for creating new XBlocks: [https://github.com/edx/cookiecutter-xblock](https://github.com/edx/cookiecutter-xblock)

Follow the instructions there to create a new XBlock, and to set up a [docker](https://runnable.com/docker/getting-started/) XBlock development and testing environment.  If you don't wish to use [docker](https://runnable.com/docker/getting-started/), you can set up the[ XBlock SDK manually.](http://edx.readthedocs.io/projects/xblock-tutorial/en/latest/sdk/get_started_sdk.html)

Once you have an XBlock created, [use git to share it to a public repository](https://guides.github.com/activities/hello-world/) so that it can be used and installed on your Open edX deployments.

## Running XBlock in Open edX

If you have a [vagrant devstack](https://openedx.atlassian.net/wiki/spaces/OpenOPS/pages/60227787/Running+Vagrant-based+Devstack) or [docker devstack](https://github.com/edx/devstack) set up for Open edX, you can install your XBlock there and run it inside your LMS/Studio environment.

### Vagrant devstack

See [Deploy Your XBlock in Devstack](http://edx.readthedocs.io/projects/xblock-tutorial/en/latest/edx_platform/devstack.html) for instructions.  Your XBlock will be available under /edx/src/your-xblock.

Your devstack should already have advanced components enabled, but you can check this by viewing /edx/app/edxapp/*.env.json to see that:

"FEATURES": {

   …   "ALLOW_ALL_ADVANCED_COMPONENTS": true,

   …

},

### Docker devstack

The [XBlock Tutorial](http://edx.readthedocs.io/projects/xblock-tutorial/en/latest/edx_platform/devstack.html) has not yet been updated to cover development in docker devstacks, but the concepts are similar.

In docker however, you need to be sure to install the same version of your XBlock in both the LMS and Studio, since these services do not share virtual environments as they do in vagrant.

## Install in LMS

make lms-shell

pip install -e /path/to/your/xblock

## Install in Studio

make studio-shell

pip install -e /path/to/your/xblock

## Testing

Writing automated tests protects your XBlock's functionality against breaking changes as your project evolves.  It also provides examples of how you expect your XBlock to be used.

* [Quote of the Day XBlock](https://github.com/open-craft/quote-of-the-day-xblock): good example of how to write unit tests, with quality and code coverage.

* [XBlock-SDK: Testing](https://github.com/edx/xblock-sdk#testing): details how to run code coverage and automated tests in the SDK workbench.

* [Automated Tools](#automated-tools): section below provides information on how to measure and manage code quality.

## Translations and Internationalization

Whenever your XBlock displays text to a learner or instructor, use the [gettext](https://docs.python.org/2/library/gettext.html) utilities to mark the text, and to extract these text strings for translating.

See [XBlock Tutorial: Internationalization Support](http://edx.readthedocs.io/projects/xblock-tutorial/en/latest/edx_platform/edx_lms.html?internationalization-support) and the [Open edX Developer Resources: Internationalization Coding Guidelines](http://edx.readthedocs.io/projects/edx-developer-guide/en/latest/conventions/internationalization/i18n.html#internationalization-coding-guidelines) for details.

## Coding standards

A good XBlock will have:

* A README file which describes what the XBlock does, how to install it, and how to run its tests, and screenshots to display how the XBlock works.

* All python dependencies listed in [requirements files](https://pip.pypa.io/en/stable/user_guide/#requirements-files), e.g. [requirements.txt](https://github.com/edx/xblock-lti-consumer/blob/master/requirements.txt), [test_requirements.txt](https://github.com/edx/xblock-lti-consumer/blob/master/test_requirements.txt)

* A setup file to allow the XBlock to be packaged and installed by pip, e.g. [setup.py](https://github.com/edx/xblock-lti-consumer/blob/master/setup.py)

* A conf/locale directory containing extracted learner- or instructor-facing text for translation, e.g. [problem-builder/conf/locale](https://github.com/open-craft/problem-builder/tree/master/conf/locale)

* Automated tests to validate functionality.

* 100% quality report from [pylint](#pylint) and [pep8](#pep8).

* Minimum 80% code coverage.

### Writing good code

The [Open edX Developer's Guide](http://edx.readthedocs.io/projects/edx-developer-guide) contains a number of resources on this topic:

* [Writing Good Code](http://edx.readthedocs.io/projects/edx-developer-guide/en/latest/conventions/index.html): contains guides on [Accessibility](http://edx.readthedocs.io/projects/edx-developer-guide/en/latest/conventions/accessibility.html), [Internationalization](http://edx.readthedocs.io/projects/edx-developer-guide/en/latest/conventions/internationalization/index.html), and preventing [Cross-Site Scripting vulnerabilities](http://edx.readthedocs.io/projects/edx-developer-guide/en/latest/conventions/preventing_xss.html). 

* [Language Style Guidelines](http://edx.readthedocs.io/projects/edx-developer-guide/en/latest/style_guides/index.html): specifically the ones for [python](http://edx.readthedocs.io/projects/edx-developer-guide/en/latest/style_guides/python-guidelines.html) and [Javascript](http://edx.readthedocs.io/projects/edx-developer-guide/en/latest/style_guides/javascript-guidelines.html) are relevant for XBlock development

### Automated tools

Python provides a several tools to assist with maintaining coding standards, quality, and test coverage.

See [xblock-lti-consumer](https://github.com/edx/xblock-lti-consumer#running-tests) for an example of how to use these tools.

#### pylint

[pylint](https://pypi.python.org/pypi/pylint) is a tool which looks for programming errors and checks python coding standards.

See xblock-lti-consumer's [pylintrc](https://github.com/edx/xblock-lti-consumer/blob/master/pylintrc) for an example configuration file, and [quality.sh](https://github.com/edx/xblock-lti-consumer/blob/master/scripts/quality.sh) for an example of how to run pylint. The edx-platform [pylintrc](https://github.com/edx/edx-platform/blob/master/pylintrc) file is much more extensive, but is a good guide to use for more complex configuration.

#### pep8

[pep8](https://www.python.org/dev/peps/pep-0008/) is another tool for checking python coding standards.

See xblock-lti-consumer's [.pep8](https://github.com/edx/xblock-lti-consumer/blob/master/.pep8) for an example configuration file, and [quality.sh](https://github.com/edx/xblock-lti-consumer/blob/master/scripts/quality.sh) for an example of how to run pep8.

#### coverage

[coverage](https://coverage.readthedocs.io/en/coverage-4.4.2/) is a python tool which counts the lines of code that are run when a set of tests run, and provides a detailed report of what percentage of the code is tested, and which lines are missing.

See xblock-lti-consumer's [.coveragerc](https://github.com/edx/xblock-lti-consumer/blob/master/.coveragerc) for an example configuration file, and [test.sh](https://github.com/edx/xblock-lti-consumer/blob/master/scripts/test.sh) for an example of how to run python tests with coverage enabled.

## Deployment and Release management

### Installation

Because XBlocks are just python libraries, they can be installed using standard the standard pip install tool, using a github repository URL and revision.

* Standard XBlocks are listed in the [github requirements file](https://github.com/edx/edx-platform/blob/master/requirements/edx/github.txt), and installed during edxapp setup.

* Non-standard XBlocks are installed via configuration, by adding the pip requirement to the list of [EDXAPP_EXTRA_REQUIREMENTS](https://github.com/edx/configuration/blob/04b65e809e60f71d884f6f47d92129f4865dc7e3/playbooks/roles/edxapp/tasks/deploy.yml#L168-L180).

### Use pull requests

To update your XBlock, create a [pull request](https://help.github.com/articles/about-pull-requests/), so that your changes can be reviewed and tested before they are merged.

Each pull request should have:

* title: summary of the change

* Description of the change and its purpose

* Detailed instructions for how to test the change in the LMS/Studio.

* Updated automated tests to validate the code change.

To get your pull request reviewed, mention one or more developers who know about your project, and ask for a review.

### Use tags

The best way to manage releases of different versions of your XBlock is to mark each release with a [git tag](http://alblue.bandlem.com/2011/04/git-tip-of-week-tags.html).  For example, OpenCraft's [problem-builder uses tags](https://github.com/open-craft/problem-builder/tags) to mark the different versions available, and then installs them using [pip from a github URL using the specific tag](https://github.com/edx/edx-platform/blob/master/requirements/edx/edx-private.txt#L10).

