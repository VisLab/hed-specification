# 7. Library schemas

## 7.1. Why library schemas?

The variety and complexity of events in electrophysiological experiments make full documentation challenging. As more experiments move out of controlled laboratory environments and into less controlled virtual and real-world settings, the terminology required to adequately describe events has the potential to grow exponentially.

In addition, experiments in any given subfield can create pressures to add overly-specific terms and jargon to the schema hierarchy—for example, adding musical terms to tag events in music-based experiments, video markup terms for experiments involving movie viewing, traffic terms for experiments involving virtual driving, and so forth.

Clinical fields using neuroimaging also have their own specific vocabularies for describing data features of clinical interest (e.g., seizure, sleep stage IV). Including these discipline-specific terms quickly makes the standard HED schema unwieldy and less usable by the broader user community.

Third generation HED addressed the problem of vocabulary bloat by introducing **HED library schemas** to organize discipline-specific terminology. To use a programming analogy, when programmers write a Python module, the resulting code does not become part of the Python language or core libraries. Instead, the module becomes part of a library used in conjunction with core modules of the programming language.

A HED library schema contains the specialized vocabulary terms needed for event annotation in a specialized area. An example of such a library is the [HED SCORE schema](https://hed-schemas.readthedocs.io/en/latest/hed_score_schema.html) for annotation of EEG by clinicians.

## 7.2. Unpartnered schemas

## 7.3. Partnered schemas

HED library schemas were originally assumed to be **unpartnered** vocabularies, complete with all the needed schema attributes and properties. These unpartnered library schemas were usually used in conjunction with the HED standard schema, and the tags from the two different vocabularies were distinguished by prefixing the tags from one of the vocabularies with `xx:`. Here `xx:` is the **namespace prefix**: it places the schema's tags in the namespace `xx`, whose name is chosen by the annotator.

**Partnered library schemas** were introduced in HED specification version `3.2.0` and are supported by HED standard schema versions >= `8.2.0`.

A partnered library schema version is tied to a specific version of the HED standard schema as specified in its header. A given library schema version is either **partnered** or **unpartnered**.

### 7.3.1. Partnered files

The XML file corresponding to a partnered library schema is a single, unified schema containing the information from both the library and its standard schema partner and validated as an integrated whole.

This XML merged schema file is downloaded and used by tools. Downstream tools see a single schema and can process it with no special handling. The following example shows the XML header for merged TESTLIB library version `2.0.0`.

````{admonition} XML header for TESTLIB library 2.0.0 partnered with 8.4.0 (merged).
```xml
<?xml version="1.0" ?>
<HED library="testlib" version="2.0.0" withStandard="8.4.0">

```
````

The canonical filename for this `.xml` file is `HED_testlib_2.0.0.xml`. This file is always stored in the libraries `hedxml` directory in the [hed-schemas](https://github.com/hed-standard/hed-schemas) GitHub repository. For the above example, the directory is [library_schemas/testlib/hedxml](https://github.com/hed-standard/hed-schemas/tree/main/library_schemas/testlib/hedxml).

As with any HED schema, schema builders develop and maintain their schema in MediaWiki mark-down format and use tools to convert to XML. The schema developer's version is unmerged, containing only the information specific to the library schema. The following example shows the header for the `.mediawiki` developer's version of a partnered library schema.

````{admonition} MediaWiki header for TESTLIB library 2.0.0 partnered with 8.4.0 (unmerged).
```html
HED library="testlib" version="2.0.0" withStandard="8.4.0" unmerged="true"
```
````

The canonical filename for this `.mediawiki` file is `HED_testlib_2.0.0_unmerged.mediawiki`.

Tools also support an alternative form of the `.mediawiki` library schema containing all the information in the merged schema (a mirror to the XML), which may be useful for debugging, but is usually not explicitly created.

The following table summarizes the different partnered library schema formats and their uses. File names and link examples are specifically for the TESTLIB library. For other libraries, substitute the library name for the word *testlib*.

| Format    | Merged<br/>status | Canonical filename                     | Handling                                                                                                                                                                 |
| --------- | ----------------- | -------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| XML       | merged            | `HED_testlib_2.0.0.xml`                | Stored in library [hedxml](https://github.com/hed-standard/hed-schemas/tree/main/library_schemas/testlib/hedxml).<br/>Used by tools.                                     |
| XML       | unmerged          | `HED_testlib_2.0.0_unmerged.xml`       | Can be generated but is never<br/> stored on [hed-schemas](https://github.com/hed-standard/hed-schemas).<br/>Not used, but available for completeness.                   |
| MediaWiki | merged            | `HED_testlib_2.0.0.mediawiki`          | Usually not stored in [hedwiki](https://github.com/hed-standard/hed-schemas/tree/main/library_schemas/testlib/hedwiki).<br/>Possibly used during<br/>schema development. |
| MediaWiki | unmerged          | `HED_testlib_2.0.0_unmerged.mediawiki` | Working format for developers<br/>Should be stored in [hedwiki](https://github.com/hed-standard/hed-schemas/tree/main/library_schemas/testlib/hedwiki).                  |

### 7.3.2. Partnered formats

There are four significant differences between merged and unmerged MediaWiki formats:

1. The unmerged version has the `unmerged="true"` attribute in its header line. <br/> <br/>
2. The unmerged version should only include the auxiliary sections (e.g., unit classes, unit modifiers, value classes, schema attributes, and schema properties) that it explicitly extends. <br/> <br/>
3. In the unmerged schema, nodes with the `rooted=XXX` schema attribute must be top-level tags, and `XXX` must correspond to a node in the standard schema. In the merged schema, nodes with the `rooted=XXX` schema attribute are placed directly under the standard schema node `XXX`. <br/> <br/>
4. Nodes in the unmerged version cannot have the `inLibrary` attribute. In contrast, nodes from the library schema are given the `inLibrary==YYY` attribute during the merging process. Here `YYY` is the library schema name.

The following excerpt from an unmerged TESTLIB library schema in MediaWiki format shows a library schema node (`Data-mode`) rooted to `Statistical-value` in the standard schema.

````{admonition} Example of a rooted node in an unmerged schema in MediaWiki format.
```html
                      . . .
'''Data-mode''' <nowiki>{rooted=Statistical-value}[A value that occurs most often in data.]</nowiki>
* <nowiki># {takesValue, valueClass=numericClass}</nowiki>
                      . . .
```
````

Notice that the indentation asterisks (\*) indicate that the node's children are at the first level. In the merged schema, these are adjusted accordingly as shown in the following:

````{admonition} When merged with the standard schema, the indentation levels are adjusted.
```html
                      . . .
*** Statistical-value <nowiki>{extensionAllowed}[A value based on or employing the principles of statistics.]</nowiki>
                       . . .
**** Data-minimum <nowiki>[The smallest possible quantity.]</nowiki>
***** <nowiki># {takesValue, valueClass=numericClass}</nowiki>
**** Data-mode <nowiki>{inLibrary=testlib, rooted}[A value that occurs most often in data.]</nowiki>
***** <nowiki># {takesValue, valueClass=numericClass, inLibrary=testlib}</nowiki>
**** Probability <nowiki> [A measure of the expectation of the occurrence of a particular event.]</nowiki>
***** <nowiki># {takesValue, valueClass=numericClass}</nowiki>
                      . . .
```
````

Similar differences occur between the merged and unmerged XML formats, but only the merged XML format is useful.

### 7.3.3. Auxiliary sections

The unmerged version of a partnered library schema **must** have prologue and epilogue sections that appropriately explain the purpose of the library schema. The contents of these prologue and epilogue sections become the prologue and epilogue, respectively, in the merged schema.

All the other auxiliary sections of the corresponding partner standard schema are inherited by the merged schema. Most unmerged partnered library schemas will not contain any additional auxiliary sections.

**Auxiliary section items that do not appear in a standard schema are unlikely to be supported by the HED infrastructure if they require special handling.** Thus, adding items to the auxiliary library schema sections is discouraged.

Library schema developers who need to add an item, such as a unit class to an auxiliary section, should first contact the HED Working Group to determine whether this item could be appropriately added to the standard schema. If a new item must be added, only that item and its corresponding auxiliary section should appear in the unmerged schema.

Library schema additions of units, unit classes, unit modifiers, value classes, and schema attributes are permitted, though not encouraged. **Library schemas cannot add information to the property definitions section of the schema.**

### 7.3.4. Partnered attributes

To support partnered library schema the following items were introduced in HED standard schema `8.2.0`:

| Name           | Type              | Role                                                                                                                                                                                                                                                                                                                                                                                     |
| -------------- | ----------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `withStandard` | Header attribute  | <ul><li>Indicates that this is a partnered library schema.</li><li>Its value is the version of its standard schema partner.</li></ul>                                                                                                                                                                                                                                                    |
| `unmerged`     | Header attribute  | <ul><li>Indicates that this schema contains only library information.</li><li>Its value is either "true" or "false.</li><li>If "false", the attribute should be omitted.</li></ul>                                                                                                                                                                                                       |
| `inLibrary`    | Element attribute | <ul><li>Indicates that this element is in the library schema.</li><li>Its value is the library name in lowercase.</li><li>The attribute appears only in merged schemas.</li><li>An element contributed by more than one library in a merge group carries one `inLibrary` value per contributing library, and the attribute is excluded from element compatibility comparisons.</li></ul> |
| `rooted=XXX`   | Node attribute    | <ul><li>Indicates that this node is to appear directly under<br/> standard schema node `XXX` in the merged schema.</li><li>A node with the `rooted` attribute must be<br/>a top-level node in the unmerged schema.</li></ul>                                                                                                                                                             |
| `reserved`     | Node attribute    | <ul><li>Indicates that this node has special meaning or function.</li><li>**Can only appear in standard schemas.**</li></ul>.                                                                                                                                                                                                                                                            |

### 7.3.5. Motivation for partners

Starting with HED specification version `3.2.0` and HED standard schema version `8.2.0`, **partnered library schemas** have become the recommended form for library schemas. This section describes the motivation for this preference.

#### 7.3.5.1. Auxiliary consistency

An unpartnered library schema must duplicate the [auxiliary schema sections](https://www.hedtags.org/hed-specification/Appendix_A.html#a1-auxiliary-schema-sections) appearing in standard schemas, introducing the possibility of inconsistency in usage or definition between the library schema and standard schemas.

Partnered library schema automatically inherit the partner standard schema's auxiliary attributes, this assuring consistent handling by tools and preventing the introduction of inconsistently handled attributes.

Although unpartnered library schemas may add additional items to the auxiliary sections, HED tools only guarantee support of standard schema auxiliary items requiring special handling. **Thus, addition of items in the auxiliary sections of a library schema is discouraged.**

#### 7.3.5.2. Reserved tag handling

Several tags in the standard schema such as `Definition`, `Onset`, and `Offset` define the structure of events and the data. By partnering with a standard schema, a library schema is assured of having HED support for key features such as events of temporal extent and definitions.

Developers of partnered library schemas should release new versions whenever HED updates its standard schema. This ensures that the partnered library schema benefits from the latest updates to HED features and tools.

If the update can be done without conflict, this update may be initiated as part of the release mechanism by the maintainers of the HED repositories.

#### 7.3.5.3. Annotation conciseness

The most common use case for library schemas in annotation requires tags from both a standard schema and a library schema, thus requiring that a `xx:` be assigned to tags from one of the schemas when unpartnered library schemas are used.

Because a partnered library schema is merged with a standard schema to form a single, unified schema, users can annotate data without the `xx:` namespace prefix. The `xx:` is still needed if more than one library schema is used.

#### 7.3.5.4. Library searches

The subtrees appearing in the library schemas are often elaborations of a particular term in the standard schema. However, if the library schema terms are not in appropriate standard schema hierarchy, HED search can not be leveraged to find these elaborations by searching for a more general standard schema term.

#### 7.3.5.5. Suggested tags

Unpartnered library schemas cannot use the `suggestedTag` or `relatedTag` attributes to suggest using particular tags from the standard schema, since the values of the tags must be in the schemas themselves. However, with partnered library schemas, validation is only performed on the merged versions of the schema, so tags from the standard schema can be used as `suggestTag` or `relatedTag` values.

### 7.3.6. Lazy partnering

HED allows multiple partnered schemas to be loaded into the default namespace and used without prefixes provided that there are no conflicts. We refer to this process as **lazy merging**. Conflicting schemas can always be used together if all but one are in distinct namespaces. A merge is attempted within each namespace, including the default namespace.

In the following example, all the library schemas are partnered with standard schema `8.4.0`. Library schemas `liba_1.0.0` and `libc_4.3.2` are merged in the default namespace, and library schemas `ac:libb_2.8.1` and `ac:exam_2.3.2` are merged in the `ac` namespace. The schema `sc:test_1.3.2` stays the same, and listing `8.4.0` adds nothing, since it is already the partner of `liba_1.0.0` and `libc_4.3.2`. If there are any conflicts during the merging process, an error is raised.

````{admonition} Example: Merging of multiple schemas.

```
     ['liba_1.0.0', 'ac:libb_2.8.1', 'libc_4.3.2', '8.4.0', 'sc:test_1.3.2', 'ac:exam_2.3.2']
```
````

The full set of rules governing merge groups and partnered combination is given in the rule table of [3.1.2.2. Rules for partnered combination](./03_HED_formats.md#3122-rules-for-partnered-combination). The subsections below give worked examples of version specifications that load and version specifications that fail, organized by the row groups of that table.

If an incompatible list of schemas is given, a [SCHEMA_LOAD_FAILED](./Appendix_B.md#schema_load_failed) error is generated.

```{admonition} Avoid new auxiliary section entries in library schemas.
---
class: warning
---
Auxiliary sections have information for HED tools, and new entries may require modification to
schema validation and analysis tools.

With the possible (and rare) exception of new `unitClasses` and `units`, partnered library schemas
usually have empty auxiliary sections except for the `prologue` and `epilogue`.

While new value class and schema attribute definitions are allowed in library schemas, this is strongly discouraged. If a new entry is needed, contact the HED Working Group (hed.maintainers@gmail.com) to see if the
entry might be added to the standard schema instead.
```

#### 7.3.6.1. Schemas used in the examples

The worked examples in the following subsections use three test-only library schemas: **testconflict**, **testclash**, and **testminimal**. These schemas are maintained in the [hed-tests](https://github.com/hed-standard/hed-tests) repository (under `json_test_data/test_schemas/`) and are not released HED schemas. Their contents are fully controlled, so each example exercises exactly one rule of the rule table in [3.1.2.2. Rules for partnered combination](./03_HED_formats.md#3122-rules-for-partnered-combination). Each example mirrors a test case in that repository's `json_test_data/validation_test_data/SCHEMA_LOAD_FAILED.json`, keeping the specification and the validator test suites in sync.

| Schema versions                                                  | Standard schema partner (`withStandard`) |
| ---------------------------------------------------------------- | ---------------------------------------- |
| `testconflict_1.0.0`, `testconflict_1.1.0`, `testconflict_1.1.2` | None (unpartnered)                       |
| `testconflict_2.0.0`, `testconflict_2.1.0`, `testconflict_2.1.1` | `8.5.0`                                  |
| `testclash_1.0.0` through `testclash_12.0.0`                     | `8.5.0`                                  |
| `testminimal_1.0.0`                                              | None (unpartnered)                       |
| `testminimal_2.0.0`                                              | `8.4.0`                                  |
| `testminimal_2.1.0`                                              | `8.5.0`                                  |

The examples rely on the following facts about these libraries:

- `testconflict_2.1.0` is `testconflict_2.0.0` plus one added tag, so every element the two versions share is identical. `testconflict_2.1.1` differs from `testconflict_2.1.0` only in one revised tag description (a patch-level change).
- `testconflict_2.0.0` declares the top-level tags `Shared-item`, `Attribute-item` (with `suggestedTag=Object-one`), `Description-item`, `Anchor-item` (with child `Nested-item`), `Placeholder-item` (with a `#` child), and `Rooted-tag` (with `rooted=Event`), whose children are `Rooted-one` (with child `Deep-one`) and `Rooted-two`.
- Each `testclash` version declares its own `Clash-tag` subtree plus at most one probe element also declared by `testconflict_2.0.0`, identical except for at most one controlled difference. The probe differences are tabulated in [7.3.6.5. Element compatibility examples](#7365-element-compatibility-examples).
- `testminimal` declares only its own `Mini-tag` subtree, which is disjoint from the vocabularies of the other two libraries.

#### 7.3.6.2. Merge group examples

| Version specification                                            | Loads | Explanation                                                                                                                                             |
| ---------------------------------------------------------------- | ----- | ------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `['testconflict_2.1.0', 'testclash_1.0.0', 'testminimal_2.1.0']` | Yes   | All three libraries are partnered with `8.5.0`, and their elements are either disjoint or identical, so they merge into a single unprefixed vocabulary. |
| `['8.4.0', 'sc:testconflict_2.1.0', 'ts:testminimal_1.0.0']`     | Yes   | Three merge groups (unprefixed, `sc:`, `ts:`) resolve independently; the standard schema partners of different groups do not have to agree.             |
| `['8.5.0', 'sc:8.4.0']`                                          | Yes   | A standard schema in its own namespace forms its own merge group, so the two standard versions do not conflict.                                         |
| `['testconflict_2.0.0', 'testconflict_2.1.0']`                   | Yes   | Different versions of one library may share a merge group; these two share the `8.5.0` partner and every shared element is identical.                   |
| `['testconflict_2.0.0', 'testconflict_2.0.0']`                   | No    | A merge group cannot have multiple copies of the same schema (the same name and version).                                                               |
| `['8.5.0', 'sc:testconflict_2.0.0', 'sc:testminimal_2.0.0']`     | No    | The rules apply inside each namespace: the `sc` namespace has conflicting partners (`8.5.0` versus `8.4.0`), even though the default namespace is fine. |

#### 7.3.6.3. Standard schema partner examples

| Version specification                         | Loads | Explanation                                                                                                                                                                       |
| --------------------------------------------- | ----- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `['testconflict_2.0.0']`                      | Yes   | A partnered library schema automatically includes its standard schema partner (`8.5.0`), so standard schema tags such as `Red` validate without the standard schema being listed. |
| `['8.5.0', 'testconflict_2.0.0']`             | Yes   | A standard schema matching the group partner is allowed and adds nothing to the merged result.                                                                                    |
| `['8.4.0', 'testconflict_2.0.0']`             | No    | A standard schema in a merge group must match the group partner; `testconflict_2.0.0` is partnered with `8.5.0`, not `8.4.0`.                                                     |
| `['testconflict_2.0.0', 'testminimal_2.0.0']` | No    | Partnered library schemas in a merge group must have the same partner; here the partners are `8.5.0` and `8.4.0`.                                                                 |
| `['8.4.0', '8.5.0']`                          | No    | Two bare standard schema versions give the unprefixed group two conflicting partners.                                                                                             |

#### 7.3.6.4. Unpartnered schema examples

| Version specification                                        | Loads | Explanation                                                                                                            |
| ------------------------------------------------------------ | ----- | ---------------------------------------------------------------------------------------------------------------------- |
| `['8.4.0', 'sc:testconflict_2.1.0', 'ts:testminimal_1.0.0']` | Yes   | The unpartnered `testminimal_1.0.0` is alone in its own `ts:` namespace, which is where an unpartnered schema must be. |
| `['8.5.0', 'testconflict_1.1.0']`                            | No    | The unpartnered `testconflict_1.1.0` cannot share the unprefixed merge group with another schema.                      |
| `['ts:testconflict_1.1.2', 'ts:testminimal_1.0.0']`          | No    | Two unpartnered library schemas cannot share a namespace; each needs its own.                                          |

#### 7.3.6.5. Element compatibility examples

Each example in this subsection loads `testconflict_2.0.0` together with one version of `testclash`. Every `testclash` version carries at most one probe element shared with `testconflict_2.0.0`, so each pair isolates a single element compatibility rule:

| Version specification                        | Probe element        | Difference from `testconflict_2.0.0`                                                                                                                                                                                                                                  | Loads |
| -------------------------------------------- | -------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----- |
| `['testconflict_2.0.0', 'testclash_1.0.0']`  | `Shared-item`        | None: an identical copy. Identical shared elements are compatible, and the merged vocabulary keeps a single copy.                                                                                                                                                     | Yes   |
| `['testconflict_2.0.0', 'testclash_2.0.0']`  | `Attribute-item`     | The `suggestedTag` value is `Clash-one` instead of `Object-one`: the attribute values differ.                                                                                                                                                                         | No    |
| `['testconflict_2.0.0', 'testclash_3.0.0']`  | `Description-item`   | The description text differs.                                                                                                                                                                                                                                         | No    |
| `['testconflict_2.0.0', 'testclash_4.0.0']`  | `Nested-item`        | Declared at the top level instead of as a child of `Anchor-item`: the ancestor paths differ.                                                                                                                                                                          | No    |
| `['testconflict_2.0.0', 'testclash_5.0.0']`  | `Placeholder-item`   | Has no `#` child, while `testconflict`'s has one.                                                                                                                                                                                                                     | No    |
| `['testconflict_2.0.0', 'testclash_6.0.0']`  | `Rooted-tag`         | Declared as an ordinary top-level tag instead of with `rooted=Event`: the merged ancestor paths differ (`Rooted-tag` versus `Event/Rooted-tag`).                                                                                                                      | No    |
| `['testconflict_2.0.0', 'testclash_7.0.0']`  | `Rooted-tag`         | Declared with `rooted=Item` instead of `rooted=Event`: the same tag is anchored at different standard schema nodes.                                                                                                                                                   | No    |
| `['testconflict_2.0.0', 'testclash_8.0.0']`  | `Rooted-tag` subtree | `Rooted-tag` and its child `Rooted-one` are identical; `testclash` adds its own child `Rooted-three`. A shared element may have different non-`#` children, so the merged `Rooted-tag` has children `Rooted-one` (with `Deep-one`), `Rooted-two`, and `Rooted-three`. | Yes   |
| `['testconflict_2.0.0', 'testclash_9.0.0']`  | `Rooted-tag` subtree | The shared child `Rooted-one` carries a different description. The compatibility rules apply to shared children, so the changed shared child fails the merge even though the non-shared sibling `Rooted-three` would be allowed.                                      | No    |
| `['testconflict_2.0.0', 'testclash_10.0.0']` | `Rooted-tag`         | Identical, but its only child is `testclash`'s own `Rooted-four`: a shared element's children may be entirely disjoint.                                                                                                                                               | Yes   |
| `['testconflict_2.0.0', 'testclash_11.0.0']` | `Rooted-tag` subtree | The chain `Rooted-tag`/`Rooted-one` is identical; below it `testclash` declares `Deep-two` where `testconflict` declares `Deep-one`. The differing grandchildren are not shared, so the merged `Rooted-one` carries both.                                             | Yes   |
| `['testconflict_2.0.0', 'testclash_12.0.0']` | `Rooted-tag` subtree | The shared grandchild `Deep-one` carries a different description. Compatibility applies at every depth of a shared hierarchy.                                                                                                                                         | No    |

The same rules govern two versions of one library. The specification `['testconflict_2.1.0', 'testconflict_2.1.1']` fails to load even though the versions differ only in one description (a patch-level change): versions of one library merge only when their shared elements are identical, exactly like different libraries.

Any failing pair in this subsection can still be used together by giving one schema its own namespace. For example, `['testconflict_2.0.0', 'cl:testclash_2.0.0']` loads because the two `Attribute-item` declarations are in different merge groups and are never compared.

## 7.4. Library schema design

Library schema should be developed and maintained in MediaWiki format for readability. Developers should always validate the schema before converting to XML. Only validated versions of the schema should be uploaded to the GitHub [hed-schemas](https://github.com/hed-standard/hed-schemas) repository. More information about the development process is contained in the HED [Schema developers guide](https://www.hedtags.org/hed-schemas/developer_guide.html).

### 7.4.1. General design rules

This section summarizes the general design rules for all library schema.

```{admonition} General design rules for HED library schema.
---
class: tip
---
1. **Follow naming conventions**:<br/>
A library schema must be given a name containing only alphabetic characters.
This name must appear in the schema header line in the required format.
<br>&nbsp;</br>
2. **Use semantic versioning**:<br/>
A library library must use semantic versioning and follow the versioning update rules used by
the HED standard schema as specified in [3.1.3. Version updates](./03_HED_formats.md#313-version-updates).
<br>&nbsp;</br>
3. **Tag uniqueness**:<br/>
Every term must be unique within the library schema and must
conform to the rules for HED schema terms.
<br>&nbsp;</br>
4. **Have a meaningful prologue**:<br/>
The schema should include a prologue section giving an overview, purpose and scope
of the library schema.
<br>&nbsp;</br>
5. **Have a meaningful epilogue**:<br/>
The schema should include an epilogue section containing reference, citation, and license information.
<br>&nbsp;</br>
6. **Be understandable**:<br/>
Schema terms should be readily understood by most users. The terms should not be ambiguous and
should be meaningful in themselves without reference to their position in the schema hierarchy.
<br>&nbsp;</br>
7. **Be well-organized**:<br/>
If possible, no schema sub-tree should have more than 7 direct subordinate sub-trees.
<br>&nbsp;</br>
8. **Maintain subtree orthogonality**:<br/>
Terms that are used independently of one another should be in different sub-trees (orthogonality).
<br>&nbsp;</br>
9. **Enforce is-a relationship between child nodes and their parents**:<br/>
Every node in a HED hierarchy must be a subclass of its parent node.
This is required for HED search generalizability.

```

Rules 1 through 5 are enforced by validators, while rules 6 through 9 are the responsibility of the schema designers and review committees.

In general, library schema developers should avoid adding schema terms that duplicate those found in the latest HED standard schema at the time of release. For partnered library schemas this is a strict requirement: the unmerged library schema cannot contain a tag that appears in its standard schema partner ([SCHEMA_LIBRARY_INVALID](./Appendix_B.md#schema_library_invalid)); the merged form necessarily contains all of its partner's tags. Library schema developers should also try to avoid overlap of terms found in other schema libraries.

All HED schemas, including library schemas, must use [semantic versions](https://semver.org/) and adhere to the rules specified in [3.1.3. Version updates](./03_HED_formats.md#313-version-updates).

Unpartnered library schema developers must include the auxiliary schema classes from the standard HED schema including the schema attributes, unit classes, unit modifiers, value classes, and schema properties. No changes should be made to these sections since HED tools support the special auxiliary classes from the standard schema, but in general do not support special handling of added classes beyond basic verification.

If your application requires schema classes that are not available in the standard HED schema and would like these classes to be supported, please make a request using the [issues](https://github.com/hed-standard/hed-schemas/issues) forum of the [hed-schemas](https://github.com/hed-standard/hed-schemas) GitHub repository.

### 7.4.2. Unpartnered design rules

The following design rules are specifically meant for unpartnered library schemas.

```{admonition} Design rules specific to unpartnered HED library schemas.
---
class: tip
---
1. **Avoid tag duplication**:<br/>
The terms in the library schema should not overlap terms present in the latest
version of the HED schema at the time of its release.
<br>&nbsp;</br>
2. **Do not modify the special auxiliary sections**:<br/>
The unpartnered library schema should exactly duplicate the special auxiliary sections
of the HED standard schema that was the latest version when this schema version was released.
The special sections include:  schema attributes, unit classes,
unit modifiers, value classes, and schema properties.
<br>&nbsp;</br>
3. **Avoid adding special auxiliary items**:<br/>
A library schema may not modify any of the items in the special sections
of the HED standard schema.
<br>&nbsp;</br>
4. **Obtain the appropriate reviews early**:<br/>
Any additions to the special sections must be reviewed by the HED Working Group to
determine what requirements the additions would impose on downstream tools.
This should be done as early in the process as possible.

```

Unpartnered library schemas are no longer recommended because of the difficulty in enforcing conflict rules with HED standard schemas.

### 7.4.3. Partnered design rules

Partnered library schemas are now the recommended format for the reasons listed in [Motivation for partners](./07_Library_schemas.md#735-motivation-for-partners). The following design rules are specifically meant for partnered library schemas.

```{admonition} Design rules specific to partnered HED library schemas.
---
class: tip
---
1. **Check for overlap**:<br/>
The terms in the partnered library schema must not overlap with terms present in its partnered
standard schema.
<br>&nbsp;</br>
2. **Use the latest released version of the standard schema**:<br/>
A partnered library schema should always use the latest version of the HED schema
available at the time of its release.
<br>&nbsp;</br>
3. **Do not put any auxiliary sections**:<br/>
A partnered library schema should not contain the special auxiliary sections (e.g., schema attributes, unit classes, unit modifiers, value classes, and schema properties),
unless a new item is added to the section, in which only that item should appear.
<br>&nbsp;</br>
4. **Seek reviews early in the process**:<br/>
Any additions to the special sections must be reviewed by the HED Working Group to
determine what requirements the additions would impose on downstream tools.

```

It is recognized that HED standard and library schemas will both evolve and that additions or tag reorganizations may cause conflicts. These conflicts must be resolved as they occur. In general the standard schema takes precedence over any library schema in resolving these conflicts.

### 7.4.4. Schema namespaces

As part of the HED annotation process, users must associate one or more HED schemas with their datasets. Since it would be impossible to avoid naming conflicts across schema libraries built in parallel by different user communities, HED supports schema library namespaces to facilitate the use of multiple schemas in annotating a datasets.

If multiple schemas are used, users must define a local namespace for each additional schema and prefix the tags from each of these additional schemas with their respective namespace prefixes in annotations. The local names should be strictly alphabetic with no blanks or punctuation. If a tag namespace prefix is invalid in the version specification, a schema loading error occurs.

````{admonition} **Example:** Driving library schema example tags.

```
dp:Drive-action/Change-lanes
dp:Drive/Change-lanes
dp:Change-lanes
```
````

A colon (`:`) is used to separate the qualifying local name from the remainder of the tag.

The introduction of partnered library schemas has greatly reduced the need for namespaces, since the most common use case is a library schema used with a standard schema.

## 7.5. Library schemas in BIDS

The most common use case (for 99.9% of the HED users) is to tag events using a standard HED schema (preferably the latest one) available in the `standard_schema/hedxml` directory of the `hed-schemas` repository of the `hed-standard` organization on GitHub. The standard schemas are available at: [https://github.com/hed-standard/hed-schemas/tree/main/standard_schema](https://github.com/hed-standard/hed-schemas/tree/main/standard_schema).

The **official library schemas** are available at [https://github.com/hed-standard/hed-schemas/tree/main/library_schemas](https://github.com/hed-standard/hed-schemas/tree/main/library_schemas).

Standard schemas are referenced by their version number (e.g., `8.1.0`), while library schema are referenced by a combination of library name and version number (e.g., `score_1.0.0`).

For BIDS datasets, the versions of the HED schema are specified by the `HEDVersion` field of the BIDS `dataset_description.json` file. The following example specifies that version `8.1.0` of the standard HED schema is to be used in addition to `score` library schema version `1.0.0`.

````{admonition} Illustration of using the namespace prefix for tagging.
---
class: tip
---
The `dataset_description.json` file contains:

```json
{
  "Name": "A great experiment",
  "BIDSVersion": "1.8.0",
  "HEDVersion": ["8.1.0", "sc:score_1.0.0"]
}
```

A typical annotation is:

```text
"Data-feature, sc:Photomyogenic-response, sc:Wicket-spikes"
````

Based on the above description tools will download:

1. The standard HED schema:\
   [https://raw.githubusercontent.com/hed-standard/hed-schemas/main/standard_schema/hedxml/HED8.1.0.xml](https://raw.githubusercontent.com/hed-standard/hed-schemas/main/standard_schema/hedxml/HED8.1.0.xml).
2. The HED `score` library schema version `1.0.0`:\
   [https://raw.githubusercontent.com/hed-standard/hed-schemas/main/library_schemas/score/hedxml/HED_score_1.0.0.xml](https://raw.githubusercontent.com/hed-standard/hed-schemas/main/library_schemas/score/hedxml/HED_score_1.0.0.xml).

In the dataset annotations for the above example, tags drawn from the score schema would be prefixed with `sc:`, where `sc` is a local name used to distinguish tags from the additional schema.

The array specification of the schema versions in BIDS can have at most one version appearing without a namespace prefix.

SCORE version `1.0.0` is not partnered, so the HED version specification had to include both the library and standard schema versions. In contrast, SCORE version `1.1.0` is partnered with HED standard schema `8.2.0`, so no namespace prefixes are needed as shown in the following example:

````{admonition} **Example:** An example specification of HED version for a partnered schema.
---
class: tip
---
The `dataset_description.json` file contains:

```json
{
  "Name": "A great experiment",
  "BIDSVersion": "1.8.0",
  "HEDVersion": "score_1.1.0"
}
```

A typical annotation is:

```text
"Data-feature, Photomyogenic-response, Wicket-spikes"
````
