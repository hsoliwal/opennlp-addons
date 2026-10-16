<!-- SPDX-License-Identifier: Apache-2.0 -->

# Fork documentation and contribution map

This guide describes the public `hsoliwal/opennlp-addons` fork at [`51960cb6bef8`](https://github.com/hsoliwal/opennlp-addons/tree/51960cb6bef8b1e1392b98a08341c7f2ab8d9e35), inspected on 2026-10-14. Its upstream is [apache/opennlp-addons](https://github.com/apache/opennlp-addons). The existing [README](README.md) and DocBook manual remain authoritative.

## Current implementation boundary

The checked-in reactor contains a parent POM and one documentation module. It currently ships **no executable add-on modules and no Java source packages**. This is explicitly stated in the manual's [module catalog](opennlp-addons-docs/src/docbkx/modules.xml). The generic dependency examples in the manual describe the intended add-on structure; their placeholder artifact names are not installable components.

Neither the parent POM nor the manual JAR supplies an NLP runtime. Use a concrete, published OpenNLP core artifact when the goal is to process text. Do not infer that former add-ons still exist here from repository history or the project name.

## Complete project and module inventory

| Path | Artifact / packaging | Purpose and entry point |
| --- | --- | --- |
| [pom.xml](pom.xml) | `org.apache.opennlp.addons:opennlp-addons:3.0.0-SNAPSHOT`, POM | Reactor, shared dependency/plugin policy, version properties and verification configuration; not an application library |
| [opennlp-addons-docs](opennlp-addons-docs/pom.xml) | `opennlp-addons-docs`, JAR | Builds the user/contributor manual. HTML and PDF profiles bind DocBook generation to `package`; Surefire tests are skipped for this documentation module |

The complete recursive tree was inspected without truncation: two POMs, one child module, zero Java files and zero Java package directories. Resource folders and DocBook chapters are documentation/build inputs, not Java modules.

## Manual source map

Start with [opennlp-addons.xml](opennlp-addons-docs/src/docbkx/opennlp-addons.xml), which includes these five chapters in order:

1. [overview.xml](opennlp-addons-docs/src/docbkx/overview.xml): optional-component boundary and relationship to OpenNLP core
2. [using-addons.xml](opennlp-addons-docs/src/docbkx/using-addons.xml): artifact/version selection and externally supplied model or dictionary resources
3. [modules.xml](opennlp-addons-docs/src/docbkx/modules.xml): current component inventory, including the absence of executable add-ons
4. [building.xml](opennlp-addons-docs/src/docbkx/building.xml): build requirements, verification, manual generation and isolated local core snapshots
5. [addon-guidelines.xml](opennlp-addons-docs/src/docbkx/addon-guidelines.xml): where an add-on belongs, naming, compatibility and release expectations

[CSS and images](opennlp-addons-docs/src/docbkx) control presentation; [XSL resources](opennlp-addons-docs/src/main/resources/xsl) customize HTML/PDF output. Editing those inputs is separate from adding an executable add-on.

## Build and verification

The inspected [parent POM](pom.xml) and [building chapter](opennlp-addons-docs/src/docbkx/building.xml) require JDK 21+ and Maven 3.9.6+. There is no Maven wrapper in this snapshot. Run commands from the repository root.

- Generate both manuals: `mvn -pl opennlp-addons-docs -am clean package -Dopennlp.forkCount=1`
- Full configured verification: `mvn clean verify -Dopennlp.forkCount=1`
- Install local artifacts after verification, if needed: `mvn install`

The manual outputs are `opennlp-addons-docs/target/docbkx/html/opennlp-addons.html` and `opennlp-addons-docs/target/docbkx/pdf/opennlp-addons.pdf`. Inspect both outputs after changing DocBook.

`package`, `verify`, and `install` have different purposes. Building this manual is not evidence that an NLP add-on works; the only child module explicitly skips Surefire. Parent verification configuration includes style, forbidden-API, license and dependency-license checks, but there are no Java implementation packages to exercise in this snapshot. This documentation change did not execute any commands or generate the manual.

## Proposed M3 integration, not implemented

A future M3 adapter could be considered here only when it uses a public OpenNLP interface and needs a distinct model format, external resource or optional third-party dependency. There is no M3 adapter, runtime package, service or compatibility test in the inspected tree.

Before proposing one:

1. Identify the exact public OpenNLP interface and supported core version. The parent currently selects `opennlp.version=3.0.0-M6`; this is a build input, not a claim about the latest public release
2. Specify input/output types, model/resource licenses, loading failures and ownership of mutable state
3. Add an isolated module only after its scope is agreed, with focused tests and usage examples
4. Follow [add-on guidelines](opennlp-addons-docs/src/docbkx/addon-guidelines.xml) and the existing [contribution guide](.github/CONTRIBUTING.md)

## Coverage and maintenance

All current projects, modules and manual entry points are indexed above. Java package documentation is not applicable because no Java source is present. Future modules require real API documentation, examples and tests; the existence of this inventory does not satisfy those requirements.

On a new upstream snapshot, recheck the parent `<modules>`, every child POM, Java source roots and the manual catalog together. Record the new commit and explain any disagreement between the README and actual source.

## Troubleshooting the manual build

- **Maven is unavailable:** this snapshot has no wrapper. Use an installed Maven
  matching the building chapter, and check its selected JDK with `mvn -version`.
- **Only one output format appears:** the documentation POM uses default HTML and
  PDF profiles. Explicitly selecting one documentation profile deactivates the
  other default profile; use the default manual command above to request both.
- **A chapter or cross-reference is missing:** inspect the includes in
  `opennlp-addons.xml`, XML namespaces, and chapter IDs, then rebuild cleanly and
  inspect the generated output.
- **A dependency placeholder does not resolve:** choose a real implementation
  artifact from the module inventory. This snapshot contains only documentation,
  so the generic add-on dependency example is not an installable NLP component.
