# Scope Naming

Syntax definitions and color schemes in Sublime Text interact through the use of scope names. Scopes are dotted strings, specified from least-to-most specific. For example, the `if` keyword in PHP could be specified via the scope name `keyword.control.php`.

Sublime Text supports TextMate language grammars, and inherited its default syntaxes from various open-source bundles. The [TextMate language grammar documentation](https://www.sublimetext.com/docs/"https://manual.macromates.com/en/language_grammars#naming_conventions) provided a base set of scope names that have been slowly expanded and changed by the community.

This is a living document that attempts to document best practices for using scope names in syntax definitions and color schemes. All of the Sublime Text default packages strive to adhere to these recommendations.

- - [Syntax Definitions](https://www.sublimetext.com/docs/scope_naming.html#syntax-definitions)

    [Example Syntaxes](https://www.sublimetext.com/docs/scope_naming.html#example-syntaxes)[Alphabetical Reference](https://www.sublimetext.com/docs/scope_naming.html#alphabetical-reference)

- [Color Schemes](https://www.sublimetext.com/docs/scope_naming.html#color-schemes)

## Syntax Definitions

The scopes documented below are a recommended base set of scope names to use when creating a syntax definition.

In this documentation, the syntax name is omitted from the end of the dotted scope name. When writing a syntax, unless otherwise noted, the syntax name should be the final segment of a dotted name. For example, a control keyword in Ruby would be `keyword.control.ruby`, whereas in Python it would be `keyword.control.python`.

### EXAMPLE SYNTAXES

It is an on-going process to improve and expand upon the default syntaxes that are shipped with Sublime Text. As of early-2019, the following syntaxes have been recently re-worked, and may be used as a reference:

- [CSS](https://github.com/sublimehq/Packages/blob/master/CSS/CSS.sublime-syntax)
- [Python](https://github.com/sublimehq/Packages/blob/master/Python/Python.sublime-syntax)
- [Go](https://github.com/sublimehq/Packages/blob/master/Go/Go.sublime-syntax)
- [HTML](https://github.com/sublimehq/Packages/blob/master/HTML/HTML.sublime-syntax)
- [PHP](https://github.com/sublimehq/Packages/blob/master/PHP/PHP.sublime-syntax)
- [C#](https://github.com/sublimehq/Packages/blob/master/C#/C#.sublime-syntax)

### ALPHABETICAL REFERENCE

The following, top-level, list of scopes is sorted alphabetically. It is recommended to read through the entire list at least once before writing or modifying a syntax.

- [comment.](https://www.sublimetext.com/docs/scope_naming.html#comment)
- [constant.](https://www.sublimetext.com/docs/scope_naming.html#constant)
- [entity.](https://www.sublimetext.com/docs/scope_naming.html#entity)
- [invalid.](https://www.sublimetext.com/docs/scope_naming.html#invalid)
- [keyword.](https://www.sublimetext.com/docs/scope_naming.html#keyword)
- [markup.](https://www.sublimetext.com/docs/scope_naming.html#markup)
- [meta.](https://www.sublimetext.com/docs/scope_naming.html#meta)
- [punctuation.](https://www.sublimetext.com/docs/scope_naming.html#punctuation)
- [source.](https://www.sublimetext.com/docs/scope_naming.html#source)
- [storage.](https://www.sublimetext.com/docs/scope_naming.html#storage)
- [string.](https://www.sublimetext.com/docs/scope_naming.html#string)
- [support.](https://www.sublimetext.com/docs/scope_naming.html#support)
- [text.](https://www.sublimetext.com/docs/scope_naming.html#text)
- [variable.](https://www.sublimetext.com/docs/scope_naming.html#variable)

#### `comment.`

Single and multi-line comments should use, respectively:

- `comment.line`
- `comment.block`

Multi-line comments used as documentation, such as Javadoc or PhpDoc, should use:

- `comment.block.documentation`

Symbols that delineate a comment, e.g. `//` or `/*`, should additionally use:

- `punctuation.definition.comment`

Comments with special syntax that denote a section of code, should use the following scope on the text only. This will cause it to be shown in the symbol list.

- `meta.toc-list`

#### `constant.`

Numeric literals, including integers, floats, etc. should use one of:

- `constant.numeric`
- `constant.numeric.integer`
- `constant.numeric.integer.binary`
- `constant.numeric.integer.octal`
- `constant.numeric.integer.decimal`
- `constant.numeric.integer.hexadecimal`
- `constant.numeric.integer.other`
- `constant.numeric.float`
- `constant.numeric.float.binary`
- `constant.numeric.float.octal`
- `constant.numeric.float.decimal`
- `constant.numeric.float.hexadecimal`
- `constant.numeric.float.other`
- `constant.numeric.complex`
- `constant.numeric.complex.real`
- `constant.numeric.complex.imaginary`

Constants that are built into the language, such as booleans and null values, should use:

- `constant.language`

Character escapes in strings, e.g. `\n` and `\x20`, should use:

- `constant.character.escape`

Formatting placeholders, such as those used for `sprintf()`, e.g. `%s`, should use:

- `constant.other.placeholder`

Other language-specific constant values, such as symbols in Ruby, should use:

- `constant.other`

#### `entity.`

The entity scopes are generally assigned to the names of data structures, types and other uniquely-identifiable constructs in code and markup. The notable exceptions are `entity.name.tag` and `entity.other.attribute-name`, which are used in HTML and XML tags.

The names of data structures will use one of the following scopes, or a new sub-scope of `entity.name` – this list is not exhaustive. To provide rich semantic information, use the specific terminology for a given language construct.

*Avoid `entity.name.type.class` and `entity.name.type.struct` which unnecessarily nest scope labels under `type`.*

- `entity.name.class`
- `entity.name.struct`
- `entity.name.enum`
- `entity.name.union`
- `entity.name.trait`
- `entity.name.interface`
- `entity.name.impl`
- `entity.name.type`

`forward-decl` variants of the above are used in languages such as C and C++. Such scopes can be used to exclude identifiers from the symbol list and indexing.

- `entity.name.class.forward-decl`

Class, interface and trait names listed as an inherited class or implemented interface/trait should use:

- `entity.other.inherited-class`

Function names receive one of the following scopes. These are included in the symbol list and index.

- `entity.name.function`
- `entity.name.function.constructor`
- `entity.name.function.destructor`

Namespaces, packages and modules use the following scope. There are usually not multiple types of such constructs in a language, so this scope should suffice.

- `entity.name.namespace`

Constants should use the following scope or `variable.other.constant`, depending on the language semantics. This scope is often included in the symbol list and index.

- `entity.name.constant`

Labels for goto-like constructs should use:

- `entity.name.label`

Heading names in markup languages, such as Markdown and Textile, should use:

- `entity.name.section`

HTML and XML tags should use the following scope. This is the only `entity.name` scope that is applied to repeated constructs.

- `entity.name.tag`

HTML, CSS and XML use the following for tag attribute names:

- `entity.other.attribute-name`

#### `invalid.`

Elements that are illegal in a specific context should use the following scope. Overuse of this will likely lead to unpleasant highlighting for users as they edit code.

- `invalid.illegal`

Deprecated elements should be scoped using the following scope. This should be very rarely used, as users may be working with older versions of a language.

- `invalid.deprecated`

#### `keyword.`

Control keywords examples include `if`, `try`, `end` and `while`. Some syntaxes prefer to mark `if` and `else` with the `conditional` variant. The `import` variant is often used in appropriate situations.

- `keyword.control`
- `keyword.control.conditional`
- `keyword.control.import`

Keywords that contain punctuation, such as the `@` symbol in CSS, add the following scope to the symbols:

- `punctuation.definition.keyword`

All remaining non-operator keywords fall under the `other` variant:

- `keyword.other`

Operators are typically symbols, so the term `keyword` can seem somewhat contradictory. Specific variants are sometimes referenced based on the type of operator.

- `keyword.operator`
- `keyword.operator.assignment`
- `keyword.operator.arithmetic`
- `keyword.operator.bitwise`
- `keyword.operator.logical`

When the operator is a word, such as `and`, `or` or `not`, the following variant is used:

- `keyword.operator.word`

#### `markup.`

Markup scopes are used for content, as opposed to code. This includes syntaxes such as Markdown and Textile.

Section headings should use:

- `markup.heading`

Lists should use one of:

- `markup.list.unnumbered`
- `markup.list.numbered`

Basic text styling should use one of:

- `markup.bold`
- `markup.italic`
- `markup.underline`

Inserted and deleted content, such as with `diff` output, should use:

- `markup.inserted`
- `markup.deleted`

Links should use:

- `markup.underline.link`

Blockquotes and other quote styles should use:

- `markup.quote`

Inline and block literal quoting, often used for code, should use:

- `markup.raw.inline`
- `markup.raw.block`

Other markup, including constructs such as footnotes and tables, should use:

- `markup.other`

#### `meta.`

Meta scopes are used to scope larger sections of code or markup, generally containing multiple, more specific scopes. These are not intended to be styled by a color scheme, but used by preferences and plugins.

The complete contents of data structures should be scoped using one of the following scopes. Similar to `entity.name`, they should be customized per language to provide rich semantic information. They should include all elements, such as the name, inheritance details and body.

- `meta.class`
- `meta.struct`
- `meta.enum`
- `meta.union`
- `meta.trait`
- `meta.interface`
- `meta.impl`
- `meta.type`

The entire scope of a function should be covered by one of the following scopes. Each variant should be applied to a specific part, and not stacked. For example, `meta.function.php meta.function.parameters.php` should never occur, but instead the scopes should alternate between `meta.function.php` then `meta.function.parameters.php` and back to `meta.function.php`.

- `meta.function`
- `meta.function.parameters`
- `meta.function.return-type`

The entirety of a namespace, module or package should use:

- `meta.namespace`

Preprocessor statements in language such as C should use:

- `meta.preprocessor`

Annotations, attributes and decorator statements that are used to modify the behavior or implementation of a class, method or function should use one of the following `meta` scopes for each component of the annotation. That is to say, there should never be more than one `meta.annotation*` scope on the stack at any given time. See `variable.annotation` for scoping the identifier.

- `meta.annotation`
- `meta.annotation.identifier`
- `meta.annotation.parameters`
- `punctuation.definition.annotation`

Complete identifiers, including namespace names, should use the following scope. Such identifiers are the fully-qualified forms of variable, function and class names. For example, in C++ a path may look like `myns::myclass`, whereas in PHP it would appears such as `\MyNS\MyClass`.

- `meta.path`

Function names, including the full path, and all parameters should receive the following scope. The name of the function or method should be `variable.function`, unless the function is scoped with `support.function`.

- `meta.function-call`

Sections of code delineated by curly braces should use one the following `meta` scopes, based on appropriate semantics. The `{` and `}` characters should additionally use the `punctuation` scopes.

- `meta.block`
- `punctuation.section.block.begin`
- `punctuation.section.block.end`
- `meta.braces`
- `punctuation.section.braces.begin`
- `punctuation.section.braces.end`

Sections of code delineated by parentheses should use one the following `meta` scopes, based on appropriate semantics. The `(` and `)` characters should additionally use the `punctuation` scopes.

- `meta.group`
- `punctuation.section.group.begin`
- `punctuation.section.group.end`
- `meta.parens`
- `punctuation.section.parens.begin`
- `punctuation.section.parens.end`

Sections of code delineated by square brackets should use the following scope. The `[` and `]` characters should additionally use the `punctuation` scopes.

- `meta.brackets`
- `punctuation.section.brackets.begin`
- `punctuation.section.brackets.end`

Generic data type constructs should use the following scope. Any symbols that denote the beginning and end, such as `<` and `>`, should additionally use the `punctuation` scopes.

- `meta.generic`
- `punctuation.definition.generic.begin`
- `punctuation.definition.generic.end`

HTML and XML tags, including punctuation, names and attributes should use the following:

- `meta.tag`

Paragraphs in markup languages use:

- `meta.paragraph`

#### `punctuation.`

The following scopes are punctuation scopes that are not embedded within other scopes. For instance, the [string.](https://www.sublimetext.com/docs/scope_naming.html#string) section includes documentation about scopes for string punctuation.

Separators such as commas and colons should use:

- `punctuation.separator`

Semicolons or other statement terminators should use:

- `punctuation.terminator`

Line-continuation characters, such as in Python and R, should use:

- `punctuation.separator.continuation`

Member access, scope resolution, or similar constructs should use the following scope. For Python or JavaScript this would be `.`. In PHP this would be applied to `->` and `::`. In C++, this would be applied to all three.

- `punctuation.accessor`

#### `source.`

A language-specific variant of the following scope is typically applied to the entirety of a source code file:

- `source`

#### `storage.`

Types should use the following scope. Examples include `int`, `bool` and `char`.

- `storage.type`

Keywords that affect the storage of a variable, function or data structure should use the following scope. Examples include `static`, `inline`, `const`, `public` and `private`.

- `storage.modifier`

Keywords for functions or methods should use the following scopes. Example keywords include `func`, `function` and `def`. *This includes `storage.type` for backwards compatibility with older color schemes.*

- `storage.type.function keyword.declaration.function`

Keywords for classes, structs, interfaces, etc should use the following scopes – this list is not exhaustive. Example keywords include `class`, `struct`, `impl` and `typedef`. *This includes `storage.type` for backwards compatibility with older color schemes.*

- `storage.type.class keyword.declaration.class`
- `storage.type.struct keyword.declaration.struct`
- `storage.type.enum keyword.declaration.enum`
- `storage.type.union keyword.declaration.union`
- `storage.type.trait keyword.declaration.trait`
- `storage.type.interface keyword.declaration.interface`
- `storage.type.impl keyword.declaration.impl`
- `storage.type keyword.declaration.type`

#### `string.`

Basic strings use the one of the following scopes, based on the type of quotes used:

- `string.quoted.single`
- `string.quoted.double`
- `string.quoted.triple`

Strings that used unconventional quotes, such as `<` and `>` with C imports, should use:

- `string.quoted.other`

The entirety of a string, along with all punctuation, prefixes, suffixes and interpolations should use:

- `meta.string`

Punctuation at the beginning and end of strings should use:

- `punctuation.definition.string.begin`
- `punctuation.definition.string.end`

Unquoted strings, such as in Shell and Batch File, should use:

- `string.unquoted`

Regular expression literals should use:

- `string.regexp`

When a string contain interpolated code, such as a variable or expression, the `string.*` scope should be removed using `clear_scopes:`, and the following should be added to the entirety of the interpolation, including punctuation:

- `meta.interpolation`

The punctuation for an interpolated expression should be:

- `punctuation.section.interpolation.begin`
- `punctuation.section.interpolation.end`

Between the punctuation, the interpolated expression should get:

- `source.*language-suffix*.embedded`

#### `support.`

Elements provided by a base framework should use one of the following scopes. Examples include Cocoa within Objective-C, or the browser/Node within JavaScript.

- `support.constant`
- `support.function`
- `support.module`

While also used for base frameworks, many syntaxes apply these to scopes unrecognized classes and types, effectively scoping all user constructs.

- `support.type`
- `support.class`

#### `text.`

Programming languages use [source.](https://www.sublimetext.com/docs/scope_naming.html#source) as their base scope, whereas content uses `text.`. One of the biggest differences is the fact that many plugins and other dynamic functionality is disabled within `text.` scopes. [markup.](https://www.sublimetext.com/docs/scope_naming.html#markup) scopes are typically used within text.

HTML should use the following scope. Variants for this scope are different than other scopes, in that the variant is always added after the `.html`, such as `text.html.basic` or `text.html.markdown`.

- `text.html`

XML should use:

- `text.xml`

#### `variable.`

A generic variable should use the following scope. Some languages use the `readwrite` variant for contrast with the `constant` variant discussed below.

- `variable.other`
- `variable.other.readwrite`

Symbols that are part of the variable name, should additionally be applied the following scope. For example, the `$` in PHP and Shell.

- `punctuation.definition.variable`

Immutable variables, often via a `const` modifier, should receive the following scope. Depending on the language and semantics, `entity.name.constant` may be a better choice.

- `variable.other.constant`

Reserved variable names that are specified by the language, such as `this`, `self`, `super`, etc. should use:

- `variable.language`

Parameters to a function or methods should use the following scope. This may also be used for other parameter-like variables, such as receivers or named return values in Go.

- `variable.parameter`

Fields, properties, members and attributes of a class or other data structure should use:

- `variable.other.member`

Function and method names should be scoped using the following, but only when they are being invoked. When defined, they should use `entity.name.function`.

- `variable.function`

The final label in an identifier that is part of an annotation should use the following. For the entire identifier, the `meta.path` scope should be used. The entire annotation should get `meta.annotation`.

- `variable.annotation`

The leading symbol used to delineate an annotation should use:

- `punctuation.definition.annotation`

## Color Schemes

In general, when applying colors and styles to scopes in a color scheme, the most general form of a selector should be styled first. High-quality syntaxes utilizing the scopes outlined in the previous section should result is good user experience for end users.

### MINIMAL SCOPE COVERAGE

The following is a recommended minimal set of scopes to highlight. Adding extra may result in a slightly improved experience, however being too specific will result in a color scheme that often only looks good for one or two syntaxes.

- `entity.name`
- `entity.other.inherited-class`
- `entity.name.section`
- `entity.name.tag`
- `entity.other.attribute-name`
- `variable`
- `variable.language`
- `variable.parameter`
- `variable.function`
- `constant`
- `constant.numeric`
- `constant.language`
- `constant.character.escape`
- `storage.type`
- `storage.modifier`
- `support`
- `keyword`
- `keyword.control`
- `keyword.operator`
- `keyword.declaration`
- `string`
- `comment`
- `invalid`
- `invalid.deprecated`

### `meta.` COLORS

When styling scopes, resist the urge to directly style `meta` scopes. They are primarily intended to provide contextual information for preferences and plugins.

### `entity.name.` COLORS

Historically, many color schemes have provided one color for `entity.name.function` and `entity.name.type`, and often a different color for `entity.name.tag`. This leaves new `entity.name.*` scopes un-highlighted.

Color schemes should instead specify a color for `entity.name` that will be applied to classes, types, structs, interfaces and many other data structures. This color can be overridden for the two scopes `entity.name.tag` and `entity.name.section`, that are used for different types of constructs.