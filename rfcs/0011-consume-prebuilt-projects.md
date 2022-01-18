- Start Date: 2022-04-15
- RFC PR: [#612](https://github.com/SAP/ui5-tooling/pull/612)
- Issue: *none*
- Affected components <!-- Check affected components by writing an "X" into the brackets -->
    + [x] [ui5-builder](https://github.com/SAP/ui5-builder)
    + [ ] [ui5-server](https://github.com/SAP/ui5-server)
    + [ ] [ui5-cli](https://github.com/SAP/ui5-cli)
    + [ ] [ui5-fs](https://github.com/SAP/ui5-fs)
    + [x] [ui5-project](https://github.com/SAP/ui5-project)
    + [ ] [ui5-logger](https://github.com/SAP/ui5-logger)


# RFC 0011 Reuse Build Results

## Summary
When building a project, it should be possible to consume build results of dependencies in order to not having to build them again.

Based on the requested build, UI5 Tooling should be able to identify which dependencies are required to be built. It should also be able to determine whether a provided build result of a dependency can be used, or whether a new build is required.

To be checked: Would it be necessary/helpful if users/projects define a set of "build-time" dependencies which always have to be built? Or can UI5 Tooling determine this by itself from the set of provided dependencies?

In a future enhancement, build results could be cached automatically. These could then also be used in ui5-server.

## Motivation

As described in [RFC 0010 UI5 Builder-Bundling Refactoring](https://github.com/SAP/ui5-tooling/blob/master/rfcs/0010-ui5-builder-bundling-refactoring.md#missing-minification-for-resources-from-dependencies):

> There is a gap in the minification process which causes a regression for some scenarios. JavaScript resources from other projects (dependencies) are not minified when not building the project with the `--all` flag. This is because the bundling task does not perform the minification but rather relies on this to be done by the minify task.
> 
> Requiring to always build a project with `--all` is not favorable due to performance reasons. Therefore, a solution is needed to still perform the needed minification steps for the mentioned resources.

## Detailed design

### Known tasks that require dependencies

1. generateVersionInfo (`ApplicationBuilder`)
    * Requires manifest.json of dependencies in order to list them in the generated `sap-ui-version.json`
1. generateResourcesJson (`ApplicationBuilder`)
    * Requires (built) runtime resources of dependencies in order to list them in the generated `resources.json`
1. JSDoc generation
    1. executeJsdocSdkTransformation (`LibraryBuilder`)
        * Requires `designtime/api.json` of dependencies
    1. generateJsdoc (`LibraryBuilder`)
        * Requires `designtime/api.json` of dependencies
    1. generateApiIndex (`ApplicationBuilder`)
1. buildThemes (`LibraryBuilder`, `ThemeLibraryBuilder`)
    * Requires theme resources of dependencies in order to build a library's theme. To be checked: Are the required resources already present in the dependency source? I.e. is it necessary to build them?
1. generateThemeDesignerResources
1. generateLibraryManifest
1. Bundling
    1. generateComponentPreload *(actually not necessary? See https://github.com/SAP/ui5-builder/pull/734)*
    1. generateBundle *(in case dependencies are bundled too)*
    1. generateLibraryPreload *(actually not necessary? See https://github.com/SAP/ui5-builder/pull/734)*
    1. generateStandaloneAppBundle *(to bundle required runtime resources)* - Note: `build --all` is already recommended for self-contained builds in order to generate required theme resources which are not part of the standalone app bundle.


<!-- This is the bulk of the RFC. Explain the design in enough detail for somebody familiar with the UI5 Tooling to understand, and for somebody familiar with the implementation to implement. This should get into specifics and corner-cases, and include examples of how the feature is used. Any new terminology should be defined here. -->

## How we teach this

<!--
What names and terminology work best for these concepts and why? How is this idea best presented?

Would the acceptance of this proposal mean the UI5 Tooling or any of its subcomponents documentation must be re-organized or altered?

How should this feature be introduced and taught to existing UI5 Tooling users?
-->

## Drawbacks

<!--
Why should we not do this? Please consider the impact on teaching people to use the UI5 Tooling, on the integration of this feature with existing and planned features, on the impact of churn on existing users.

There are tradeoffs to choosing any path, please attempt to identify them here.
-->

## Alternatives

<!--
What other designs have been considered? What is the impact of not doing this?
-->

## Unresolved Questions and Bikeshedding

<!--
*This section should be removed (i.e. resolved) before merging*

Optional, but suggested for first drafts. What parts of the design are still TBD? Are there any second priority decisions left to be made?
-->

Currently none.
