(appendix-a-schema-format-details-anchor)=

# A. Schema format details

This appendix augments the discussion of HED schema formats presented in [Chapter 3: HED formats](./03_HED_formats.md) of the HED specification. The appendix presents additional details on the rules with examples for standard HED schema and HED library schema in `.mediawiki`, `.xml`, `.json`, and `.tsv` formats.

## A.1. Auxiliary schema sections

This section gives information about how the various auxiliary sections of the HED schema are used to specify the behavior of the schema elements.

### A.1.1. Unit classes and units

Unit classes allow annotators to express the units of values in a consistent way. The plurals of the various units are not explicitly listed, but are allowed as HED tools uses standard pluralize functions to expand the list of allowed units.

Units corresponding to unit symbols (i.e., have a `unitSymbol` attribute) represent abbreviated versions of units and cannot be pluralized.

Elements with the `SIUnit` modifier may be prefixed with a multiple or a sub-multiple modifier. If the SI unit does not also have the `unitSymbol` attribute, then multiples and sub-multiples with the `SIUnitModifier` attribute are used for the expansion.

On the other hand, units with both `SIUnit` and `unitSymbol` attributes are expanded using multiples and sub-multiples having the `SIUnitSymbolModifier` attribute.

Note that some units such as byte are designated as SI units, although they are not part of the SI standard. However, they follow the same rules for unit modifiers as do SI units.

```{list-table} Unit classes and units in HED 8.5.0 (* indicates unit symbol).
---
widths: 20 10 40
header-rows: 1
---
* - Unit class
  - Default units
  - Units
* - accelerationUnits
  - m-per-s^2 
  - m-per-s^2*
* - angleUnits
  - radian 
  - radian, rad*, degree
* - areaUnits
  - m^2 
  - m^2*
* - currencyUnits
  - $ 
  - dollar, $, euro, point
* - electricPotentialUnits
  - uV 
  - V*, uV, volt
* - frequencyUnits
  - Hz 
  - hertz, Hz*
* - intensityUnits
  - dB 
  - dB, candela, cd*
* - jerkUnits
  - m-per-s^3 
  - m-per-s^3*
* - magneticFieldUnits
  - T 
  - tesla, T*
* - memorySizeUnits
  - B 
  - byte, B*
* - physicalLengthUnits
  - m 
  - foot, inch, meter, metre, m*, mile   
* - speedUnits
  - m-per-s 
  - m-per-s*, mph, kph   
* - temperatureUnits
  - degree-Celsius
  - degree-Celsius, oC*    
* - timeUnits
  - s 
  - second, s*, day, month, minute, hour, year
* - volumeUnits
  - m^3 
  - m^3*  
* - weightUnits
  - g 
  - gram, g*, pound, lb   
```

### A.1.2. Unit modifiers

A unit modifier can be applied to SI base units to indicate a multiple or sub-multiple of the unit. Unit symbols are modified by unit symbol modifiers, whereas SI units that are not unit symbols are modified by unit modifiers.

```{list-table} SI unit modifiers
---
widths: 20 20 50
header-rows: 1
---
* - Modifier
  - Symbol modifier
  - Description
* - deca
  - da
  - Multiple representing 10 to power 1
* - hecto
  - h
  - Multiple representing 10 to power 2
* - kilo
  - k
  - Multiple representing 10 to power 3
* - mega
  - M
  - Multiple representing 10 to power 6
* - giga
  - G
  - Multiple representing 10 to power 9
* - tera
  - T
  - Multiple representing 10 to power 12
* - peta
  - P
  - Multiple representing 10 to power 15
* - exa
  - E	
  - Multiple representing 10 to power 18
* - zetta
  - Z
  - Multiple representing 10 to power 21
* - yotta
  - Y
  - Multiple representing 10 to power 24
* - deci
  - d
  - Submultiple representing 10 to power −1
* - centi
  - c
  - Submultiple representing 10 to power -2
* - milli
  - m
  - Submultiple representing 10 to power -3
* - micro
  - u
  - Submultiple representing 10 to power -6
* - nano
  - n
  - Submultiple representing 10 to power −9
* - pico
  - p
  - Submultiple representing 10 to power −12
* - femto
  - f
  - Submultiple representing 10 to power −15
* - atto
  - a
  - Submultiple representing 10 to power −18
* - zepto
  - z
  - Submultiple representing 10 to power −21
* - yocto
  - y
  - Submultiple representing 10 to power −24
```

### A.1.3. Value classes

HED has very strict rules about what characters are allowed in various elements of the HED schema, HED tags, and the substitutions made for `#` placeholders. These rules are encoded in the schema using value classes. When a node name extension or placeholder substitution is given a particular value class, that name or substituted value can only contain the characters allowed for by that value class.

```{Warning}
**Note**: A placeholder `#` specification may include multiple value class attributes.

Tools check the value in question against the union of an element's `valueClass` allowed
characters and any additional characters allowed by a particular unit type.

```

The allowed characters for a value class are specified in the definition of each value class. The HED validator and other HED tools may hardcode information about behavior of certain value classes (for example the `numericClass` value class).

(value-class-character-table-anchor)=

```{list-table} Allowed characters for value classes.
---
widths: 20 50
header-rows: 1
---
* - Value class
  - Allowed characters
* - dateTimeClass
  - `digits`, `T`, `hyphen`, `colon`
* - nameClass
  - `letters`, `digits`, `hyphen`, `underscore` 
* - numericClass
  - `digits`, `E`, `e`, `plus`, `hyphen`, `period`
* - posixPath
  - `digits`, `letters`, `slash`, `colon`
* - textClass
  - `text` (printable characters 32 ≤ ASCII < 127 excluding comma, square bracket, and curly braces, plus non-ASCII characters with ASCII codes > 127).
```

See [2.2 Character sets and restrictions](./02_Terminology.md#22-character-sets-and-restrictions) for definitions of the various character class definitions.

````{admonition} Notes on rules for allowed characters in the HED schema.
---
class: tip
---
1. Commas or single quotes are not allowed in any values with the exception of
the Prologue, Epilogue, term descriptions in the HED schema, and in tsv column values
declared to be of type "list". The latter must be handled specially by tools.
2. Date-times should conform to ISO8601 date-time format "YYYY-MM-DDThh:mm:ss[.000000][Z]".
A BIDS regular expression for this is:
```text
[0-9]{4}-[0-9]{2}-[0-9]{2}T(?:2[0-3]|[01][0-9]):[0-5][0-9]:[0-5][0-9](\.[0-9]{1,6})?([A-Z]{2,4})?
```
3. Any variation on the full form of ISO8601 date-time is allowed.
4. The `nameClass` is for schema nodes.
5. Values of `numericClass` must be equivalent to a valid floating point value.
6. Scientific notation is supported with the `numericClass`.
7. The `textClass` is for descriptions, mainly for use with the `Description` tag or schema element descriptions.
8. The `posixPath` class allows digits, letters, forward slash, and colon characters for POSIX path specifications.

````

### A.1.4. Schema attributes

The type of schema element that a schema attribute may apply to is indicated by its schema type property values. Tools hardcode processing based on the schema attribute name. Only the schema attributes listed in the following table can be handled by current HED tools.

```{list-table} Schema attributes.
---
widths: 21 15 12 40
header-rows: 1
---
* - Attribute
  - Domain
  - Range
  - Description  
* - [`allowedCharacter`](#a141-allowedcharacter)  
  - unit<br/>unit modifier<br/>value class  
  - string
  - Specifies a character used in values of this class.  
* - [`annotation`](#a142-annotation)
  - element
  - string
  - Annotation link to an item in another ontology. (Added in version 8.3.0.)
* - [`conversionFactor`](#a143-conversionfactor)
  - unit<br/>unit modifier  
  - numeric  
  - Multiplicative factor to multiply by to convert to default units. (Added in version 8.1.0.)    
* - [`defaultUnits`](#a144-defaultunits)
  - unit class  
  - unit
  - Specifies units to use if placeholder value has no units.    
* - [`deprecatedFrom`](#a145-deprecatedfrom)
  - element
  - string
  - The latest schema version in which the element was not deprecated.  
* - [`extensionAllowed`](#a146-extensionallowed)  
  - node
  - boolean
  - Users can add unlimited levels of child nodes under this tag. This tag is propagated to child nodes with the exception of the hashtag placeholders.  
* - [`hedId`](#a147-hedid)
  - element
  - string
  - The unique identifier of this element in the HED namespace.  
* - [`inLibrary`](#a148-inlibrary)
  - element
  - string
  - This schema element is from the named library schema, not the standard schema. (Added/removed by tools.)  
* - [`isPartOf`](#a149-ispartof)
  - node
  - node
  - This tag is part of the indicated tag. (Added in version 8.3.0.)
* - [`relatedTag`](#a1410-relatedtag)
  - node
  - node
  - A HED tag closely related to this HED tag.  
* - [`requireChild`](#a1411-requirechild)
  - node
  - boolean  
  - A child of this node must be included in the HED tag.  
* - [`reserved`](#a1412-reserved)
  - node
  - boolean
  - This tag has special meaning and requires special handling by tools.  
* - [`rooted`](#a1413-rooted)
  - node
  - node  
  - A top-level library schema node should appear under this standard schema node when merged.  
* - [`SIUnit`](#a1414-siunit)
  - unit
  - boolean
  - This unit represents an SI unit and can be modified.  
* - [`SIUnitModifier`](#a1415-siunitmodifier)
  - unitModifier
  - boolean
  - Modifier applies to base units.  
* - [`SIUnitSymbolModifier`](#a1416-siunitsymbolmodifier)
  - unitModifier 
  - boolean   
  - Modifier applies to unit symbols.  
* - [`suggestedTag`](#a1417-suggestedtag)
  - node
  - node  
  - Tag could be included with this HED tag.  
* - [`tagGroup`](#a1418-taggroup)
  - node
  - boolean  
  - Tag can only appear inside a tag group.  
* - [`takesValue`](#a1419-takesvalue)
  - node
  - boolean 
  - Placeholder (#)should be replaced by a value.  
* - [`topLevelTagGroup`](#a1420-topleveltaggroup)
  - node
  - boolean     
  - Tag (or its descendants) can be in a top-level tag group.  
* - [`unique`](#a1421-unique)
  - node
  - boolean    
  - Tag or its descendants can only occur once in an event-level HED string.  
* - [`unitClass`](#a1422-unitclass)
  - node
  - unit class     
  - The unit class that the value of a placeholder node can belong to.  
* - [`unitPrefix`](#a1423-unitprefix)
  - unit
  - boolean       
  - Unit is a prefix (e.g., $ in the currency units).  
* - [`unitSymbol`](#a1424-unitsymbol)
  - unit
  - boolean    
  - An abbreviation representing a unit.  
* - [`valueClass`](#a1425-valueclass)
  - node
  - value class 
  - Type of value taken on by the value of a placeholder node.       
```

#### A.1.4.1. allowedCharacter

The `allowedCharacter` attribute should appear separately for each individual character to be allowed. However, the following group designations are allowed as values for this attribute:

- `letters` designates upper and lower case alphabetic characters.
- `blank` indicates a space is an allowed character.
- `digits` indicates the digits 0-9 may be used in the value.
- `alphanumeric` indicates `letters` and `digits`

#### A.1.4.2. annotation

#### A.1.4.3. conversionFactor

The value of `conversionFactor` is the factor to multiply by the current units to convert to default units. (Added in version 8.1.0.). The `conversionFactor` value must be numeric and positive.

#### A.1.4.4. defaultUnits

If placeholder (`#`) has a `unitClass`, but the replacement value for the placeholder does not have units, tools may assume the value has `defaultUnits` if the unit class has them. For example, the `timeUnits` has the attribute `defaultUnits=s` in HED versions. Tools may assume that tag `Duration/3` is equivalent to `Duration/3 s` because `Duration` has `defaultUnits` of `s`.

#### A.1.4.5. deprecatedFrom

The `deprecatedFrom` attribute value takes a value that must be an existing semantic schema version earlier than the current version. Since `deprecatedFrom` has the `elementProperty`, it may be applied to any element in the schema. It is an error for the schema to use an element that has the `deprecatedFrom` attribute in a non-deprecated context. For example, if a tag has the `deprecatedFrom` attribute, then that tag may not appear as a `suggestedTag` or `relatedTag`. Deprecated schema attributes, units, unit modifiers, or value classes cannot be applied except to elements that are deprecated. In addition, the children of a deprecated tag must be deprecated or moved to a parent that is not deprecated.

#### A.1.4.6. extensionAllowed

The `extensionAllowed` tag indicates that descendents of this node may be extended by annotators. However, any node that has a placeholder (`#`) child cannot be extended, regardless of the `extensionAllowed` attribute, since the node's single child is always interpreted as a user-supplied value.

#### A.1.4.7. hedId

#### A.1.4.8. inLibrary

#### A.1.4.9. isPartOf

#### A.1.4.10. relatedTag

#### A.1.4.11. requireChild

#### A.1.4.12. reserved

#### A.1.4.13. rooted

#### A.1.4.14. SIUnit

#### A.1.4.15. SIUnitModifier

#### A.1.4.16. SIUnitSymbolModifier

#### A.1.4.17. suggestedTag

#### A.1.4.18. tagGroup

#### A.1.4.19. takesValue

#### A.1.4.20. topLevelTagGroup

#### A.1.4.21. unique

#### A.1.4.22. unitClass

#### A.1.4.23. unitPrefix

#### A.1.4.24. unitSymbol

#### A.1.4.25. valueClass

#### A.1.4.x. Deprecated attributes

In addition to the attributes listed above, some schema attributes have been deprecated and are no longer supported in HED, although they are still present in earlier versions of the schema. The following table lists these.

```{list-table} Schema attributes deprecated for versions &ge; 8.0.0.
---
widths: 20 15 45
header-rows: 1
---
* - Schema attribute
  - Target
  - Description
* - `default`
  - node
  - A default value used if no value is provided. Removed in standard schema version 8.0.0. 
* - `position`
  - node    
  - Indicates where this tag should appear during display. Removed in standard schema version 8.0.0.  
* - `predicateType`
  - node   
  - Indicates the relationship of the node to its parent.  Removed standard schema version 8.0.0.  
* - `recommended`
  - node
  - Event-level HED strings should include this tag.  Removed in standard schema version 8.3.0.  
* - `required`
  - node      
  - Event-level HED string must include this tag. Removed in standard schema version 8.3.0.  
```

The `default` attribute was not implemented in existing tools. The attribute is not used in HED-3G. Only the `defaultUnits` for the unit class will be implemented going forward.

The `position` attribute was used to assist annotation tools, which sought to display required and recommend tags before others. The position attribute value is an integer and the order can start at 0 or 1. Required or recommended tags without this attribute or with negative position were to be shown after the others in canonical ordering. The tagging strategy of HED versions >= 8.0.0 using decomposition and definitions does not permit this type of ordering. The `position` attribute is not used for HED versions >= 8.0.0.

The `predicateType` attribute was introduced in HED-2G to facilitate mapping to OWL or RDF. It was needed because the HED-2G schema had a mixture of children that were properties and subclasses. The possible values of `predicateType` were `propertyOf`, `subclassOf`, or `passThrough` to indicate which role each child node had with respect to its parent. In HED versions >= 8.0.0, the parent-child relationship MUST be `subclassOf` to allow search generality. The attribute is ignored by tools.

### A.1.5. Schema properties

**The schema properties are qualifiers on the domains and ranges of schema attributes.** A property's presence implies the attribute has this property, while its absence implies it does not. Processing of `property` elements is hard-coded into the schema processors. The following is a list of schema attribute properties.

```{list-table} Summary of schema attribute properties for HED Version >= 8.0.0.
---
widths: 20 50
header-rows: 1
---
* - Property
  - Description
* - `annotationProperty`
  - This schema attribute is NOT inherited.<br/>Replaces `isInheritedProperty`.
* - `boolRange`
  - This schema attribute's value can be true or false.<br/>This property was formerly named `boolProperty`.  
* - `elementDomain`
  - This schema attribute can apply to any type<br/>of element (tag term, unit class, etc).<br/>This property was formerly named `elementProperty`. 
* - `isInheritedProperty`
  - **Deprecated from 8.2.0** in favor of `annotationProperty`.<br/>This schema attribute is inherited by child nodes.<br/>This property only applies to schema attributes for nodes.   
* - `tagDomain`
  - This schema attribute can apply to node (tag-term) elements.<br/>This was added so attributes could apply to multiple types of elements.<br/>This property was formerly named `nodeProperty`.   
* - `tagRange`
  - This schema attribute's value can be a node.<br/>This property was formerly named `nodeProperty`. 
* - `numericRange`
  - This schema attribute's value can be numeric.  
* - `stringRange`
  - This schema attribute's value can be a string.  
* - `unitClassDomain`
  - This schema attribute can apply to unit classes.<br/>This property was formerly named `unitClassProperty`. 
* - `unitClassRange`
  - This schema attribute's value can be a unit class.    
* - `unitModifierDomain`
  - This schema attribute can apply to unit modifiers.<br/>This property was formerly named `unitModifierProperty`. 
* - `unitDomain`
  - This schema attribute can apply to units.<br/>This property was formerly named `unitProperty`.   
* - `unitRange`
  - This schema attribute's value can be units.  
* - `valueClassDomain`
  - This schema attribute can apply to value classes.<br/>This property was formerly named `valueClassProperty`. 
* - `valueClassRange`
  - This schema attribute's value can be a value class.
```

Property names ending in `Range` designate the type of value a schema attribute has. Starting with HED standard schema version 8.3.0 the `boolProperty`, which indicates that a schema attribute value can be true or false, was renamed `boolRange`. In addition, `numericRange` and `stringRange` were added, since the `conversionFactor` schema attribute has a numeric value.

Property names ending in `Domain` indicate the type of schema element that a schema attribute applies to. String with HED standard schema version 8.3.0 the property names `elementProperty`, `nodeProperty`, `unitClassProperty`, `unitModifierProperty`, `unitModifierProperty`, `unitProperty`, and `valueClassProperty` were renamed as `elementDomain`, `tagDomain`, `unitClassDomain`, `unitModifierDomain`, `unitModifierDomain`, `unitDomain`, and `valueClassDomain` to better clarify their role and to facilitate mapping to the HED ontology.

```{admonition} Format for schema attributes with schema property values.
---
class: tip
---
**Attributes with boolean range** (`boolRange`):
  - In `.xml` the attribute appears as a `<name>` element with the property's name but no 
`<value>` in an `<attribute>` section of the schema element.
  - In `.mediawiki`, the attribute name appears in curly braces in the element's specification line.
  - In either case presence of the property indicates true, and absence indicates false.
<p></p>  

**Schema without a boolean range**:
  - In `.xml`, the attribute appears with both `<name>` and `<value>` in an `<attribute>` section of the schema element.
  - In `.mediawiki`, the schema element has the `{name =value}` in the element's specification line.
  - These schema attributes may appear multiple times in an element with different values if appropriate.

```

See [property example](#a255-schema-properties) for an example in MediaWiki format.

## A.2. MediaWiki file format

The rules for creating a valid `.mediawiki` specification of a HED schema are given below. The format is line-oriented, meaning that all information about an individual entity should be on a single line. Empty lines and lines containing only blanks are ignored.

### A.2.1. Overall file layout

````{admonition} Overall layout of a HED MEDIAWIKI schema file.

```moin
header
prologue
             . . .
!# start schema
schema-specification
!# end schema
unit-class-specification
unit-modifier-specification
value-class-specification
schema-attribute-specification
property-specification
!# end hed
epilogue
```
````

### A.2.2. MediaWiki header

The first line of the `.mediawiki` file should be a _header_ that starts with the keyword `HED` followed by a blank-separated list of name-value pairs.

```{eval-rst}
.. list-table:: Allowed HED schema header parameters
   :header-rows: 1
   :widths: 15 15 50

   * - Name
     - Level
     - Description
   * - library
     - optional
     - |
       | Name of library used in XML file names.
       | The value should only have lowercase alphabetic characters.
   * - version
     - required
     - A valid semantic version number of the schema.  
   * - xmlns
     - optional
     - xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance".
   * - xsi
     - optional
     - xsi:noNamespaceSchemaLocation points to an XSD file.
   * - withStandard
     - optional
     - |  
       | The version of the standard schema partner 
       | if this is a partnered library schema.
   * - unmerged
     - optional
     - | 
       | If true, this is an unmerged partnered library schema.
       | If omitted, assumed false.      
```

The following example gives a sample *header* for standard schema version 8.0.0 in `.mediawiki` format.

````{admonition} **Example:** Sample *header* for version 8.0.0 in .mediawiki format.

```moin
HED version="8.0.0"
```
````

The schema `.mediawiki` file specified in this example is named `HED8.0.0.mediawiki` and can be found in the [standard_schema/hedwiki](https://github.com/hed-standard/hed-schemas/tree/main/standard_schema/hedwiki) directory of the [hed-schemas](https://github.com/hed-standard/hed-schemas) GitHub repository.

The versions of the schema that use XSD validation to verify the format (versions 8.0.0 and above) have `xmlns:xsi` and `xsi:noNamespaceSchemaLocation` attributes. The `xsi` attribute is required if `xmlns:xsi` is given. The [XSD file](https://github.com/hed-standard/hed-schemas/blob/main/standard_schema/hedxml/HED8.0.0.xsd) allows validators to check the format of the `.xml` using standard XML validators.

The following example shows a sample *header* for `testlib` library schema version 1.0.2 in `.mediawiki` format.

````{admonition} **Example:** Sample *header* for testlib library version 1.0.2 in .mediawiki format.

```moin
HED library="testlib" version="1.0.2"
```
````

The `library` and `version` values are used to form the official file name `HED_testlib_1.0.2.mediawiki`. The file is found in [library_schemas/testlib/hedwiki](https://github.com/hed-standard/hed-schemas/tree/main/library_schemas/testlib/hedwiki) directory of the [hed-schemas](https://github.com/hed-standard/hed-schemas) GitHub repository.

A warning is generated when unknown header attributes are translated as attributes of the `HED` line during `.mediawiki` file validation.

### A.2.3. MediaWiki prologue and epilogue

The prologue is an optional paragraph of text appearing after the *header*. The prologue is used by tools for help and display purposes.

Early versions of HED use the prologue section to record a CHANGE_LOG as well as information about the syntax and rules. HED versions ≥ 8.0.0 include a separate change log file for released versions.

Similar to the prologue section, the epilogue is an optional paragraph of text, usually containing references and license information. The epilogue appears directly before the ending line of the file.

Both the prologue and epilogue may contain commas and new lines in addition to the characters specified by the [`textClass`](./Appendix_A.md#a13-value-classes).

### A.2.4. MediaWiki schema section

The beginning of the actual specification of the HED vocabulary is marked by the *start-line*:

```moin
!# start schema
```

The end of the main HED-specification is marked by the *end-line*:

```moin
!# end schema
```

A section separator is a line starting with `!#`. The section separator lines (`!# start schema`, `!# end schema`, `!# end hed`) must only appear once in the file and must appear in that order within the file.

The body of the HED specification is located between the `!# start schema` and `!# end schema` section separators. Each specification is a single line in the `.mediawiki` file.

The three types of lines in the main specification section are **top-nodes**, **normal-nodes**, and **placeholders**, respectively.

Empty lines or lines containing only blanks are ignored.

The basic format for a node-specification is:

```moin
node-name  <nowiki>{attributes}[description]</nowiki>
```

Top node names are enclosed in triple single quotes (e.g., `'''Event'''`), while other types of nodes have at least one preceding asterisk (\*) followed by a blank and then the name.

The number of asterisks indicates the level of the node in the subtree. The attributes are in curly braces (`{ }`) and the description is in square brackets (`[ ]`).

Node names in HED versions ≥ 8.0.0 can only contain alphanumeric characters, hyphens, and under-bars (i.e., they must be of type [`nameClass`](./Appendix_A.md#a13-value-classes)). They cannot contain blanks and must be unique.

HED versions < 8.0.0 allow blanks in node names and also have some duplicate node names. Use of HED versions < 8.0.0 is deprecated and validators no longer support their use.

For top nodes and normal nodes, everything after the node name must be contained within `<nowiki></nowiki>` tags. The `#` is included within the `<nowiki></nowiki>` tags in placeholder nodes.

````{admonition} **Example:** Different types of HED node specifications in .mediawiki format.

**Top node:**

```moin
'''Property''' <nowiki>{extensionAllowed} [Subtree of properties.]</nowiki>
```

**Normal node:**

```moin
***** Duration <nowiki>{requireChild} [Time extent of something.]</nowiki>
```

**Placeholder node:**

```moin
****** <nowiki># {takesValue, unitClass=time,valueClass=numericClass}</nowiki>
```
````

The `Duration` tag of this example is at the fifth level below the root (top node) of its subtree. The tag: `Property/Data-property/Data-value/Spatiotemporal-value/Temporal-value/Duration` is the long form. The placeholder in the example is the node directly below `Duration` in the hierarchy.

### A.2.5. MediaWiki auxiliary sections

After the line marking the end of the schema (`!# end schema`), the `.mediawiki` file contains the unit class definitions, unit modifier definitions, value class definitions, the schema attribute definitions, and property definitions. All of these sections are required starting with HED version 8.0.0 and must be given in this order.

#### A.2.5.1. Unit classes and units

Unit classes specify the types of units allowed to be used with a value substituted for a `#` placeholder.

The unit class specification section starts with `'''Unit classes'''` and lists the types of units (the unit classes) at the first level and the specific units corresponding to those unit classes at the second level.

````{admonition} **Example:** Part of the HED unit class for time in .mediawiki format.

```moin
'''Unit classes''' 
* time <nowiki>{defaultUnits=s}</nowiki> 
** second <nowiki>{SIUnit}</nowiki> 
** s <nowiki>{SIUnit, unitSymbol}</nowiki> 
```
````

#### A.2.5.2. Unit modifiers

The SI units can be modified by SI (International System Units) sub-multiples and multiples. All unit modifiers are at level 1 of the `.mediawiki` file.

````{admonition} **Example:** Part of the HED unit modifier in .mediawiki format.

```moin
'''Unit modifiers''' 
* deca <nowiki>{SIUnitModifier} [SI unit multiple for 10 raised to power 1]</nowiki> 
* da <nowiki>{SIUnitSymbolModifier} [SI unit multiple for 10 raised to power 1]</nowiki>
```
````

A unit must have the `SIUnit` attribute in order to be used with modifiers. If the unit has both the `SIUnit` and `unitSymbol` attributes, then it only can be used with `SIUnitSymbolModifier` modifiers. If the unit has only the `SIUnit` attribute, then it only can be used with the `SIUnitModifer`.

For example the unit `second` is an `SIUnit` but not a symbol, so `second`, `seconds`, `decasecond` and `decaseconds` are all valid units.

The unit `s` is both a `SIUnit` and a `unitSymbol`, so `s` and `das` are valid units. Note that rules about pluralization do not apply to unit symbols.

#### A.2.5.3. Value classes

Value classes give rules about what kind of value is allowed to be substituted for `#` placeholder tags.

````{admonition} **Example:** Part of the HED value class for date-time in .mediawiki format.

```moin
'''Value classes'''
* dateTimeClass <nowiki>{allowedCharacter=digits,allowedCharacter=T,allowedCharacter=-,allowedCharacter=:}[Should conform to ISO8601 date-time format YYYY-MM-DDThh:mm:ss.]</nowiki> 
```
````

#### A.2.5.4. Schema attributes

The schema attributes specify other characteristics about how particular tags may be used in annotation. These attributes allow validators and other tools to process tag strings based on the HED schema specification, thus avoiding hard-coding particular behavior.

````{admonition} **Example:** HED schema attributes allowedChaaracter and defaultUnits in .mediawiki format.

```moin
'''Schema attributes'''
* allowedCharacter <nowiki>{valueClassProperty}[Value may contain this character.]</nowiki>
* extensionAllowed <nowiki>{boolProperty}[This schema node may be extended.]</nowiki> 
```
````

The schema attributes, themselves, have attributes referred to as*schema properties*. These schema properties are listed in the `Properties` section of the schema. The example indicates that `allowedCharacter` is associated with value classes, while `defaultUnits` is associated with unit classes.

#### A.2.5.5. Schema properties

Properties apply only to schema attributes. The following example defines the `valueClassProperty` in `.mediawiki` format.

````{admonition} **Example:** HED schema property valueClassProperty in .mediawiki format.

```moin
'''Properties''' 
* valueClassProperty <nowiki>[Attribute is meant to be applied to value classes.]</nowiki> 
```
````

See [Schema properties](#a15-schema-properties) for a list of available schema properties.

## A.3. XML file format

This section describes details of the XML schema format.

### A.3.1. XML file layout

The XML schema file format has a header, prologue, main schema, definitions, and epilogue sections. The general layout is as follows:

````{admonition} XML layout of the HED schema.
```xml
<?xml version="1.0" ?>
<HED library="test" version="0.0.1" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="https://github.com/hed-standard/hed-specification/raw/master/hedxml/HED8.0.0-beta.3.xsd">
<prologue>unique optional text blob</prologue>
<schema>
         ...  schema specification  ... 
</schema>
<unitClassDefinitions>
   <unitClassDefinition> ... </unitClassDefinition>
                         ...
   <unitClassDefinition> ... </unitClassDefinition>
</unitClassDefinitions>
<unitModifierDefinitions>
   <unitModifierDefinition> ... </unitModifierDefinition>
                                ...
   <unitModifierDefinition> ... </unitModifierDefinition>
</unitModifierDefinitions>
    
<valueClassDefinitions>
    <valueClassDefinition> ... </valueClassDefinition>
                           ... 
    <valueClassDefinition> ... </valueClassDefinition>
</valueClassDefinitions>

<schemaAttributeDefinitions> 
   <schemaAttributeDefinition> ... </schemaAttributeDefinition>
                               ... 
   <schemaAttributeDefinition> ... </schemaAttributeDefinition>
</schemaAttributeDefinitions>

<propertyDefinitions>
    <propertyDefinition> ... </propertyDefinition>
                             ... 
    <propertyDefinition> ... </propertyDefinition>
</propertyDefinitions>

<epilogue>unique optional text blob</epilogue>
</HED>
```
````

### A.3.2. XML header

The `HED` node is the root node of the XML schema.

````{admonition} **Example:** Header for Version 8.0.0 of the standard HED XML schema.

```xml
<HED version="8.0.0">
```
````

The file name corresponding to this example is `HED8.0.0.xml`. The file is found in the [standard_schema/hedxml](https://github.com/hed-standard/hed-schemas/tree/main/standard_schema/hedxml) directory of the [hed-schemas](https://github.com/hed-standard/hed-schemas) GitHub repository.

Library schemas must include the `library` attribute with the library name in their header line as shown in the following example.

````{admonition} **Example:** Version 1.0.2 of HED testlib library schema in .xml format.
```xml
<HED library="testlib" version="1.0.2">
```
````

The `library` and `version` values are used to form the official xml file name `HED_testlib_1.0.2.xml`. The file is found in [library_schemas/testlib/hedxml](https://github.com/hed-standard/hed-schemas/tree/main/library_schemas/testlib/hedxml) directory of the [hed-schemas](https://github.com/hed-standard/hed-schemas) GitHub repository.

Unknown header attributes are translated as attributes of the `HED` root node of the `.xml` version, but a warning is issued when the `.mediawiki` file is validated.

The `library` and `version` values are used to form the official xml file name `HED_testlib_1.0.2.xml`.

Library schemas may also be partnered as is `HED_testlib_2.0.0.xml`.

````{admonition} **Example:** Partnered library schema testlib version 2.0.0 in .xml format.
```xml
<HED library="testlib" version="2.0.0" withStandard="8.2.0">
```
````

### A.3.3. XML prologue and epilogue

The `<prologue>...</prologue>` and `<epilogue>...</epilogue>` elements are meant to be treated as opaque as far as schema processing goes.

HED versions < 8.0.0 contained a Change Log for the HED schema in the prologue section as well as some basic documentation of syntax. The epilogue section contained additional metadata to be ignored during processing.

### A.3.4. XML schema section

The schema section of the HED XML document consists of an arbitrary number of `<node></node>` elements enclosed in a single `<schema></schema>` element.

````{admonition} Top-level XML layout of the HED schema.
```xml
<schema>
    <node> ... </node>
           ...
    <node> ... </node>
</schema>
```
````

A `<node>` element contains a required `<name>` child element, an optional `<description>` child element, and an optional number of additional `<attribute>` child elements:

````{admonition} XML layout HED node element.
```xml
<node>
    <name>xxx</name>
    <description>yyy</description>
    <attribute> ... </attribute>
    <attribute> ... </attribute>
    <attribute> ... </attribute>   
    <node> ... <node>
</node>
```
````

The `<name>` element text must conform to the rules for naming HED schema nodes. It corresponds to the _node-name_ in the `mediawiki` specification and must not be empty. A `#` value is used to represent value place-holder elements.

The `<description>` element has the text contained in the square brackets `[ ]` in the `.mediawiki` node specification. If the `.mediawiki` description is missing or has an empty `[ ]`, the `<description>` element is omitted.

The optional `<attribute>` elements are derived from the attribute list contained in curly braces `{ }` of the `.mediawiki` specification. An `<attribute>` element has a single non-empty `<name></name>` child element whose text value corresponds to the node-name of attribute in the corresponding `.mediawiki` file. If the attribute does not have the `boolProperty`, then the `<attribute>` element should also have one or more child `<value></value>` elements giving the value(s) of the attribute.

**Example:** The `requireChild` attribute represents a boolean value. In the `.mediawiki` representation this attribute appears as `{requireChild}` if present and is omitted if absent.

The format of the XML attributes was changed with HED versions &ge 8.0.0. Earlier versions of the schema have been deprecated and tools no longer support their validation.

````{admonition} The requireChild attribute represents a boolean value.

**Old xml if true:**     

```xml
<node requireChild="true"><name>xxx</name></node>
```

**New xml if true:**

```xml
<node>
    <name>xxx</name>
    <attribute>
       <name>requireChild</name>
    </attribute>
</node>
```
````

**Example:** The `suggestedTag` is a schema attribute that has a value. The attribute is meant to be used by tagging tools to suggest additional tags that a user might want to include. Notice that the `suggestedTag` values are valid HED tags in any form (short, long, or intermediate).

````{admonition} The suggestedTag old format.

**Old xml if present:**

```xml
<node suggestedTag="Sweet,Gustatory-attribute/Salty">
    <name>xxx</name>
</node>
```

**New xml if present:**

```xml
<node>
   <name>xxx</name>
   <attribute>
      <name>suggestedTag</name>
    	 <value>Sweet</value>
    	 <value>Gustatory-attribute/Salty</value>
   </attribute>
</node>
```
````

### A.3.5. XML auxiliary sections

The auxiliary sections define various aspects of behavior of various types of elements in the schema.

#### A.3.5.1. Unit classes

The unit classes are defined in the `<unitClassDefinitions>` section of the XML schema file, and the unit modifiers are defined in the `<unitModifierDefinitions>` section. These sections follow a format similar to the `<node>` element in the `<schema>` section.

The `<unitClassDefinition>` elements have a required `<name>`, an optional `<description>`, and an arbitrary number of additional `<attribute>` child elements. These `<attribute>` elements describe properties of the unit class rather than of individual unit types. In addition, `<unitClassDefinition>` elements may have an arbitrary number of `<unit>` child elements as shown in the following example.

````{admonition} Example XML layout of the unit class definitions.
```xml
<unitClassDefinition>
    <name>time</name>
    <description>Temporal values except date and time of day.</description>
    <attribute>
       <name>defaultUnits</name>
       <value>s</value>
    </attribute>
    <unit>
       <name>second</name>
       <description>SI unit second.</description>
       <attribute>
          <name>SIUnit</name>
       </attribute>
    </unit>
    <unit>
       <name>s</name>
       <description>SI unit second in abbreviated form.</description>
       <attribute>
          <name>SIUnit</name>
       </attribute>
       <attribute>
          <name>unitSymbol</name>
       </attribute>
    </unit>  
</unitClassDefinition>
```
````

#### A.3.5.2. Unit modifiers

Unit modifiers are defined in the `<unitModifierDefinitions>` section of the XML schema file. The following shows the layout of an example unit modifier definitions:

````{admonition} Example XML layout of the unit modifier definition
```xml

<unitModifierDefinitions>
    <unitModifierDefinition>
        <name>deca</name>
        <description>SI unit multiple representing 10^1.</description>
        <attribute>
           <name>SIUnitModifier</name>
        </attribute>
        <attribute>
           <name>conversionFactor</name>
           <value>10.0</value>
        </attribute>
    </unitModifierDefinition>
                . . .
</unitModifierDefinitions>  
```
````

#### A.3.5.3 Value classes

Value classes are defined in the `<valueClassDefinitions>` section of the XML schema file. These sections follow a format similar to the `<node>` element in the `<schema>`:

````{admonition} Example XML layout of the unit class definitions.
```xml
<valueClassDefinitions>
   <valueClassDefinition>
       <name>dateTimeClass</name>
       <description>Should conform to ISO8601 date-time format YYYY-MM-DDThh:mm:ss.</description>
       <attribute>
           <name>allowedCharacter</name>
           <value>digits</value>
           <value>T</value>
           <value>-</value>
           <value>:</value>
       </attribute>
   </valueClassDefinition>
</valueClassDefinitions>
```
````

#### A.3.5.4. Schema attributes

The `<schemaAttributeDefinitions>` section specifies the allowed attributes of the other elements including the `<node>`, `<unitClassDefinition>`, `<unitModifierDefinition>`, and `<valueClassDefinition>` elements. The specifications of individual attributes are given in `<schemaAttributeDefinition>` elements.

````{admonition} Example XML layout of the schema attribute definitions.
```xml
<schemaAttributeDefinitions>
    <schemaAttributeDefinition>
        <name>allowedCharacter</name>
        <description>Value may contain this character.</description>
        <property>
            <name>valueClassProperty</name>
        </property>
    </schemaAttributeDefinition>
    <schemaAttributeDefinition>
        <name>extensionAllowed</name>
        <description>This schema node may be extended.</description>
        <property>
            <name>boolProperty</name>
        </property>
    </schemaAttributeDefinition>
    . . .
</schemaAttributeDefinitions>    
```
````

#### A.3.5.5. Schema properties

The following is an example of the layout of the `valueClassProperty` in `.xml` format.

````{admonition} Example XML layout of the schema property definitions.
```xml

  <propertyDefinitions>
                  . . .
      <propertyDefinition>
         <name>valueClassProperty</name>
         <description>Indicates that the schema attribute is meant to be applied to value classes.</description>
      </propertyDefinition>
   </propertyDefinitions>
```
````

See [Schema properties](#a15-schema-properties) for a list of available schema properties.

## A.4. JSON file format

This section describes details of the JSON schema format.

### A.4.1. JSON file layout

The JSON schema file format is a nested structure representing the schema in JavaScript Object Notation. The general layout follows the standard JSON format with objects and arrays:

````{admonition} JSON layout of the HED schema.
```json
{
  "version": "8.5.0",
  "xmlns:xsi": "http://www.w3.org/2001/XMLSchema-instance",
  "xsi:noNamespaceSchemaLocation": "...",
  "prologue": "unique optional text blob",
  "epilogue": "unique optional text blob",
  "tags": {
    "tag-name": {
      "short_form": "...",
      "long_form": "...",
      "description": "...",
      "parent": "...",
      "children": [...],
      "attributes": {...},
      "explicitAttributes": {...}
    },
    ...
  },
  "unit_classes": {...},
  "unit_modifiers": {...},
  "value_classes": {...},
  "schema_attributes": {...},
  "properties": {...},
  "prefixes": [...],
  "external_annotations": [...]
}
```
````

### A.4.2. JSON header

The top-level JSON object contains metadata attributes including the schema version and XML schema location information.

````{admonition} Example JSON header for version 8.5.0 of the standard HED schema.
```json
{
  "version": "8.5.0",
  "xmlns:xsi": "http://www.w3.org/2001/XMLSchema-instance",
  "xsi:noNamespaceSchemaLocation": "https://raw.githubusercontent.com/hed-standard/hed-schemas/refs/heads/main/standard_schema/hedxml/HED8.4.0.xsd"
}
```
````

Library schemas include a `library` field with the library name:

````{admonition} Example JSON header for a library schema.
```json
{
  "library": "testlib",
  "version": "1.0.2"
}
```
````

Partnered library schemas also include `withStandard` and optionally `unmerged` fields:

````{admonition} Example JSON header for partnered library schema.
```json
{
  "library": "testlib",
  "version": "2.0.0",
  "withStandard": "8.2.0",
  "unmerged": "True"
}
```
````

### A.4.3. JSON prologue and epilogue

The `prologue` and `epilogue` fields contain optional text strings that provide documentation about the schema but are not processed during validation.

### A.4.4. JSON tags section

The `tags` section is an object where each key is a tag name and the value is an object containing:

- `short_form`: The tag name without ancestors
- `long_form`: The full path of the tag
- `description`: Human-readable description
- `parent`: The parent tag name (or null for top-level tags)
- `children`: Array of child tag names
- `attributes`: All attributes including inherited ones
- `explicitAttributes`: Only attributes explicitly assigned to this tag

````{admonition} Example JSON tag element.
```json
"Agent-action": {
  "short_form": "Agent-action",
  "long_form": "Event/Agent-action",
  "description": "Any action engaged in by an agent.",
  "parent": "Event",
  "children": [],
  "attributes": {
    "suggestedTag": ["Task-event-role", "Agent", "Task-property"],
    "hedId": "HED_0012003"
  },
  "explicitAttributes": {
    "suggestedTag": ["Task-event-role", "Agent"],
    "hedId": "HED_0012003"
  }
}
```
````

### A.4.5. JSON auxiliary sections

#### A.4.5.1. Unit classes

The `unit_classes` section defines unit classes as objects where each key is a unit class name:

````{admonition} Example JSON layout of unit classes.
```json
"unit_classes": {
  "timeUnits": {
    "description": "Temporal values except date and time of day.",
    "hedId": "HED_0011513",
    "units": ["second", "s", "day", "month", "minute", "hour", "year"],
    "default_units": "s"
  }
}
```
````

#### A.4.5.2. Unit modifiers

The `unit_modifiers` section defines unit modifiers with their properties:

````{admonition} Example JSON layout of unit modifiers.
```json
"unit_modifiers": {
  "deca": {
    "description": "SI unit multiple representing 10e1.",
    "hedId": "HED_0011400",
    "conversionFactor": "10.0",
    "SIUnitModifier": true
  },
  "da": {
    "description": "SI unit multiple representing 10e1.",
    "hedId": "HED_0011401",
    "conversionFactor": "10.0",
    "SIUnitSymbolModifier": true
  }
}
```
````

#### A.4.5.3. Value classes

The `value_classes` section defines value classes with their allowed characters:

````{admonition} Example JSON layout of value classes.
```json
"value_classes": {
  "dateTimeClass": {
    "description": "Date-times should conform to ISO8601...",
    "hedId": "HED_0011301",
    "allowed_characters": ["digits", "T", "hyphen", "colon"]
  },
  "numericClass": {
    "description": "Value must be a valid numerical value.",
    "hedId": "HED_0011303",
    "allowed_characters": ["digits", "E", "e", "plus", "hyphen", "period"]
  }
}
```
````

#### A.4.5.4. Schema attributes

The `schema_attributes` section defines the allowed schema attributes with their domain and range properties:

````{admonition} Example JSON layout of schema attributes.
```json
"schema_attributes": {
  "suggestedTag": {
    "description": "A tag that is often associated with this tag.",
    "hedId": "HED_0010106",
    "tagDomain": true,
    "tagRange": true
  },
  "takesValue": {
    "description": "This tag is a hashtag placeholder...",
    "hedId": "HED_0010503",
    "tagDomain": true,
    "boolRange": true,
    "annotationProperty": true
  }
}
```
````

#### A.4.5.5. Schema properties

The `properties` section defines schema attribute properties in JSON format:

````{admonition} Example JSON layout of schema properties.
```json
"properties": {
  "tagDomain": {
    "description": "This schema attribute can apply to node (tag-term) elements.",
    "hedId": "HED_0010704"
  },
  "boolRange": {
    "description": "This schema attribute's value can be true or false.",
    "hedId": "HED_0010702"
  }
}
```
````

See [Schema properties](#a15-schema-properties) for a list of available schema properties.

## A.5. TSV file format

This section describes details of the TSV (Tab-Separated Values) schema format. Unlike other HED schema formats that use a single file, the TSV format represents a schema as a collection of separate TSV files organized in a directory structure. This format is designed to facilitate ontology mapping and interoperability with semantic web technologies.

### A.5.1. TSV file organization

The TSV schema format consists of multiple files, each representing a different aspect of the schema:

````{admonition} TSV file organization for HED schema.
```text
HED8.5.0/
├── HED8.5.0_Structure.tsv
├── HED8.5.0_Tag.tsv
├── HED8.5.0_UnitClass.tsv
├── HED8.5.0_Unit.tsv
├── HED8.5.0_UnitModifier.tsv
├── HED8.5.0_ValueClass.tsv
├── HED8.5.0_DataProperty.tsv
├── HED8.5.0_ObjectProperty.tsv
├── HED8.5.0_AnnotationProperty.tsv
├── HED8.5.0_AnnotationPropertyExternal.tsv
├── HED8.5.0_AttributeProperty.tsv
├── HED8.5.0_Prefixes.tsv
└── HED8.5.0_Sources.tsv
```
````

### A.5.2. TSV header formats

Each TSV file begins with a header row defining the column names. The specific columns vary by file type but commonly include:

- `hedId`: Unique identifier for the element
- `rdfs:label`: The element name/label
- `omn:SubClassOf`: The parent class or type
- `Attributes`: Tab-separated list of attribute assignments
- `dc:description`: Human-readable description
- Additional domain-specific columns

### A.5.3. TSV structure file

The Structure file contains metadata about the schema including header, prologue, and epilogue:

````{admonition} Example TSV Structure file layout.
```tsv
hedId	rdfs:label	Attributes	omn:SubClassOf	dc:description
HED_0010010	StandardHeader	version="8.5.0", xmlns:xsi="..."	HedHeader	
HED_0010011	StandardPrologue		HedPrologue	The HED standard schema is...
HED_0010012	StandardEpilogue		HedEpilogue	This schema is released under...
```
````

### A.5.4. TSV tag file

The Tag file contains all HED tags with their hierarchy and attributes:

````{admonition} Example TSV Tag file layout.
```tsv
hedId	Level	rdfs:label	omn:SubClassOf	Attributes	dc:description
HED_0012001	0	Event	HedTag	suggestedTag=Task-property, annotation=ncit:C25499	Something that happens...
HED_0012002	1	Sensory-event	Event	suggestedTag=Task-event-role	Something perceivable...
```
````

The `Level` column indicates the depth in the hierarchy (0 for top-level tags, 1 for children, etc.).

### A.5.5. TSV unit class file

The UnitClass file defines all unit classes:

````{admonition} Example TSV UnitClass file layout.
```tsv
hedId	rdfs:label	omn:SubClassOf	Attributes	dc:description
HED_0011513	timeUnits	StandardUnitClass	defaultUnits=s	
HED_0011510	physicalLengthUnits	StandardUnitClass	defaultUnits=m	
```
````

### A.5.6. TSV unit file

The Unit file contains all units with their attributes and unit class associations:

````{admonition} Example TSV Unit file layout.
```tsv
hedId	rdfs:label	omn:SubClassOf	Attributes	dc:description	hasUnitClass
HED_0011633	second	StandardUnit	SIUnit, conversionFactor=1.0		timeUnits
HED_0011634	s	StandardUnit	SIUnit, unitSymbol, conversionFactor=1.0		timeUnits
HED_0011635	day	StandardUnit	conversionFactor=86400		timeUnits
```
````

The `hasUnitClass` column indicates which unit class the unit belongs to.

### A.5.7. TSV unit modifier file

The UnitModifier file defines unit modifiers (SI prefixes):

````{admonition} Example TSV UnitModifier file layout.
```tsv
hedId	rdfs:label	omn:SubClassOf	Attributes	dc:description
HED_0011424	milli	StandardUnitModifier	SIUnitModifier, conversionFactor=0.001	SI unit submultiple...
HED_0011425	m	StandardUnitModifier	SIUnitSymbolModifier, conversionFactor=0.001	SI unit submultiple...
```
````

### A.5.8. TSV value class file

The ValueClass file defines value classes and their allowed characters:

````{admonition} Example TSV ValueClass file layout.
```tsv
hedId	rdfs:label	omn:SubClassOf	Attributes	dc:description
HED_0011301	dateTimeClass	StandardValueClass	allowedCharacter=digits, allowedCharacter=T	Date-times should conform...
HED_0011303	numericClass	StandardValueClass	allowedCharacter=digits, allowedCharacter=E	Value must be a valid...
```
````

### A.5.9. TSV property files

The schema includes three property files defining different types of properties:

#### A.5.9.1. DataProperty file

Defines properties with data type ranges (string, numeric, boolean):

````{admonition} Example TSV DataProperty file layout.
```tsv
hedId	rdfs:label	Type	omn:Domain	omn:Range	Properties	dc:description
HED_0010304	allowedCharacter	DataProperty	HedUnit or HedValueClass	string	unitDomain, valueClassDomain	A special character...
HED_0010311	SIUnit	DataProperty	HedUnit	boolean	unitDomain, boolRange	This unit element is an SI unit...
```
````

#### A.5.9.2. ObjectProperty file

Defines properties with object ranges (references to other schema elements):

````{admonition} Example TSV ObjectProperty file layout.
```tsv
hedId	rdfs:label	Type	omn:Domain	omn:Range	Properties	dc:description
HED_0010104	defaultUnits	ObjectProperty	HedUnitClass	HedUnit	unitClassDomain, unitRange	The default units...
HED_0010105	relatedTag	ObjectProperty	HedTag	HedTag	tagDomain, tagRange	A HED tag closely related...
```
````

#### A.5.9.3. AnnotationProperty file

Defines annotation properties that don't participate in reasoning:

````{admonition} Example TSV AnnotationProperty file layout.
```tsv
hedId	rdfs:label	Type	omn:Domain	omn:Range	Properties	dc:description
HED_0010500	hedId	AnnotationProperty	HedElement	string	elementDomain, stringRange	The unique identifier...
HED_0010501	requireChild	AnnotationProperty	HedTag	boolean	tagDomain, boolRange	This tag must have...
```
````

### A.5.10. TSV attribute property file

The AttributeProperty file defines the schema attribute properties (formerly called schema properties):

````{admonition} Example TSV AttributeProperty file layout.
```tsv
hedId	rdfs:label	Type	dc:description
HED_0010704	tagDomain	AnnotationProperty	This schema attribute can apply to node elements...
HED_0010702	boolRange	AnnotationProperty	This schema attribute's value can be true or false...
```
````

### A.5.11. TSV prefixes and annotations

#### A.5.11.1. Prefixes file

Defines namespace prefixes used for external ontology references:

````{admonition} Example TSV Prefixes file layout.
```tsv
hedId	rdfs:label	omn:Namespace	dc:description
HED_0010901	dc:	http://purl.org/dc/elements/1.1/	Dublin Core Metadata Element Set Version 1.1
HED_0010902	dcterms:	http://purl.org/dc/terms/	DCMI Metadata Terms
```
````

#### A.5.11.2. External annotations file

Defines external annotation properties that can be used:

````{admonition} Example TSV AnnotationPropertyExternal file layout.
```tsv
hedId	rdfs:label	dc:identifier	dc:description
HED_0010903	dc:creator	http://purl.org/dc/elements/1.1/creator	An entity primarily responsible...
HED_0010904	dc:description	http://purl.org/dc/elements/1.1/description	An account of the resource
```
````

### A.5.12. TSV format conventions

1. **Delimiters**: Fields are separated by tab characters (`\t`).
2. **Multiple values**: When a column contains multiple values (e.g., multiple attributes), they are separated by commas.
3. **Attribute format**: Attributes are specified as `name=value` for valued attributes or just `name` for boolean attributes.
4. **Encoding**: Files use UTF-8 encoding.
5. **Hierarchy**: Tag hierarchy is expressed through the `Level` column in the Tag file and through the `omn:SubClassOf` column in all files.
6. **Identifiers**: Each element has a unique `hedId` that persists across schema versions.

See [Schema properties](#a15-schema-properties) for a list of available schema properties.
