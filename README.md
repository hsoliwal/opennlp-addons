<!--
Licensed to the Apache Software Foundation (ASF) under one or more
contributor license agreements. See the NOTICE file distributed with
this work for additional information regarding copyright ownership.
The ASF licenses this file to You under the Apache License, Version 2.0
(the "License"); you may not use this file except in compliance with
the License. You may obtain a copy of the License at

    http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.
-->

<!-- fork-homepage:start -->
# OpenNLP Add-ons: understand the extension boundary

**A home for optional OpenNLP integrations, currently containing the add-ons manual rather than executable add-ons.** This public `hsoliwal` fork of [apache/opennlp-addons](https://github.com/apache/opennlp-addons) helps readers understand where an integration belongs, how its dependencies should be isolated, and how to build the documentation that describes that design.

## What is implemented in this snapshot?

The checked-in [parent POM](pom.xml) has one child, [opennlp-addons-docs](opennlp-addons-docs/pom.xml). The [module catalog](opennlp-addons-docs/src/docbkx/modules.xml) confirms that there are currently no executable add-on modules. There are no Java source packages in the inspected tree.

The useful deliverable here is the manual. The parent artifact uses `pom` packaging and the manual module packages documentation; neither is an NLP runtime to add to an application. Placeholder coordinates in design examples are not available integrations. For actual text processing, choose the appropriate core component from [Apache OpenNLP](https://github.com/apache/opennlp).

This fork adds a source-backed guide and this detailed landing page. It does not implement a new adapter, model, or M3 integration. The inherited Apache introduction and license notice remain below; their badges link upstream and are not verification of this fork.

## Concepts and repository map

| Concept / location | Meaning here |
| --- | --- |
| Add-on | Optional implementation using public OpenNLP contracts, with its own resource format or third-party dependency |
| Core API | Shared contracts that integrations should use rather than duplicate |
| Artifact | A separately versioned Maven package; the parent POM is configuration, not an all-in-one runtime JAR |
| Reactor | The root project and the child projects it builds together |
| DocBook | Structured XML used as the source for HTML and PDF manuals |
| `package` / `verify` / `install` | Produce artifacts / run configured verification / place artifacts in the local Maven repository |
| [Manual entry point](opennlp-addons-docs/src/docbkx/opennlp-addons.xml) | Includes the overview, usage, module catalog, building, and add-on guidelines chapters |

## Quick start: build a readable manual

You need JDK 21+, Maven 3.9.6+, and access to dependency repositories. This snapshot does not include a Maven wrapper.

```sh
git clone https://github.com/hsoliwal/opennlp-addons.git
cd opennlp-addons
git rev-parse HEAD
java -version
mvn -version
mvn -pl opennlp-addons-docs -am clean package -Dopennlp.forkCount=1
```

With the default documentation profiles, the intended outputs are:

- HTML: `opennlp-addons-docs/target/docbkx/html/opennlp-addons.html`
- PDF: `opennlp-addons-docs/target/docbkx/pdf/opennlp-addons.pdf`

Open both after building and check navigation, code examples, and chapter cross-references. These paths and commands come from the documentation POM; rendering and build execution were not performed for this homepage update.

## Worked example: improve an integration explanation

Suppose a reader cannot tell whether a dependency example names a real add-on. Start in the [usage chapter](opennlp-addons-docs/src/docbkx/using-addons.xml), compare it with the [module catalog](opennlp-addons-docs/src/docbkx/modules.xml), then clarify the example as a pattern if no matching module exists. Rebuild the manual and inspect the generated page. This fixes the reader's uncertainty without inventing an implementation or changing public API contracts.

For a proposed new add-on, first describe the core API it uses, the optional dependency or data it introduces, supported versions, resource licensing, and failure behavior. The [add-on guidelines](opennlp-addons-docs/src/docbkx/addon-guidelines.xml) are the place to begin; adding a name to the manual does not create that module.

## Compact cheat sheet

| Goal | Command / source |
| --- | --- |
| Build HTML and PDF | `mvn -pl opennlp-addons-docs -am clean package -Dopennlp.forkCount=1` |
| Build HTML only | `mvn -pl opennlp-addons-docs -am package -Pdoc-manual-html` |
| Run configured verification | `mvn clean verify -Dopennlp.forkCount=1` |
| Install locally when needed | `mvn install` |
| Check a pending patch | `git diff --check` |
| Locate all five chapters | [Manual source map](FORK_GUIDE.md) |

Selecting the HTML profile explicitly disables the other default documentation profile; that explains why a PDF may be absent. A successful documentation build is not an NLP accuracy or runtime test, and the documentation module skips Surefire tests.

**Continue reading:** [fork guide and troubleshooting](FORK_GUIDE.md) · [building chapter](opennlp-addons-docs/src/docbkx/building.xml) · [contribution guide](.github/CONTRIBUTING.md) · [license](LICENSE) · [notices](NOTICE).

## Preserved upstream project documentation

<!-- fork-homepage:end -->

Welcome to Apache OpenNLP Add-ons!
===========

[![GitHub license](https://img.shields.io/badge/license-Apache%202-blue.svg)](https://raw.githubusercontent.com/apache/opennlp-addons/main/LICENSE)
[![Build Status](https://github.com/apache/opennlp-addons/workflows/Java%20CI/badge.svg)](https://github.com/apache/opennlp-addons/actions)
[![Contributors](https://img.shields.io/github/contributors/apache/opennlp-addons)](https://github.com/apache/opennlp-addons/graphs/contributors)
[![GitHub pull requests](https://img.shields.io/github/issues-pr-raw/apache/opennlp-addons.svg)](https://github.com/apache/opennlp-addons/pulls)

Apache OpenNLP Add-ons is a set of optional components built on the Apache OpenNLP API.

The intended layout gives each add-on its own Maven artifact, so an application can take a single integration without taking the dependencies of the others. Shared contracts stay in OpenNLP core; an implementation belongs here when it needs a distinct model format, external data, or an optional third-party library.

## Useful Links

- [OpenNLP home page](https://opennlp.apache.org/)
- [OpenNLP documentation](https://opennlp.apache.org/docs/)
- [Mailing lists](https://opennlp.apache.org/mailing-lists.html)
- [OpenNLP questions on Stack Overflow](https://stackoverflow.com/questions/tagged/opennlp)

## Overview

Currently, the repository has the following modules:

* `opennlp-addons-docs` : The DocBook sources of the add-ons manual.

## Getting Started

The current reactor contains only `opennlp-addons-docs`; it does not contain
an add-on implementation to import into an application. The
[module catalog](opennlp-addons-docs/src/docbkx/modules.xml) explains that former
add-ons were removed because core OpenNLP or maintained replacements cover
their functionality.

The parent `opennlp-addons` has `pom` packaging. Building it locally builds the
manual and parent metadata, not a combined runtime toolkit. Consult the
[usage chapter](opennlp-addons-docs/src/docbkx/using-addons.xml) when choosing
an implementation; do not treat placeholder artifact names as available
Maven coordinates.

## Building OpenNLP Add-ons

At least JDK 21 and Maven 3.9.6 are required to build the add-ons.

After cloning the repository go into the destination directory and run:

```
mvn install
```

The `verify` phase (also included by `mvn install`) runs Checkstyle, forbidden API checks, the RAT license header check, and a dependency license report. Use `-Pjacoco` for coverage.

The manual is written in DocBook XML under [`opennlp-addons-docs/src/docbkx`](opennlp-addons-docs/src/docbkx) and builds with the rest of the reactor. The
[documentation POM](opennlp-addons-docs/pom.xml) enables `doc-manual-html` and
`doc-manual-pdf` by default and binds both generators to `package`.
For an HTML-only manual build from the repository root, select the HTML
profile explicitly:

```sh
mvn -pl opennlp-addons-docs -am package -Pdoc-manual-html
```

This is documentation generation, not an add-on runtime or training test.

## Contributing

Every contribution is welcome, from a documentation typo fix to a new add-on. To get involved, please follow the instructions [here](https://github.com/apache/opennlp/blob/main/.github/CONTRIBUTING.md)

## Working with this fork

See the [fork guide](FORK_GUIDE.md) for the current module inventory,
manual build commands, and the boundary between implemented and proposed features.
