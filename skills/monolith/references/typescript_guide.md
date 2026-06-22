# Google TYPESCRIPT Style Guide Details

```text
Google TypeScript Style Guide 

 Google TypeScript Style Guide 

 This guide is based on the internal Google TypeScript style guide, but it has
been slightly adjusted to remove Google-internal sections. Google's internal
environment has different constraints on TypeScript than you might find outside
of Google. The advice here is specifically useful for people authoring code they
intend to import into Google, but otherwise may not apply in your external
environment. 

 There is no automatic deployment process for this version as it's pushed
on-demand by volunteers. 

 Introduction 

 Terminology notes 

 This Style Guide uses RFC 2119 
terminology when using the phrases must , must not , should , should not ,
and may . The terms prefer and avoid correspond to should and should
not , respectively. Imperative and declarative statements are prescriptive and
correspond to must . 

 Guide notes 

 All examples given are non-normative and serve only to illustrate the
normative language of the style guide. That is, while the examples are in Google
Style, they may not illustrate the only stylish way to represent the code.
Optional formatting choices made in examples must not be enforced as rules. 

 Source file basics 

 File encoding: UTF-8 

 Source files are encoded in UTF-8 . 

 Whitespace characters 

 Aside from the line terminator sequence, the ASCII horizontal space character
(0x20) is the only whitespace character that appears anywhere in a source file.
This implies that all other whitespace characters in string literals are
escaped. 

 Special escape sequences 

 For any character that has a special escape sequence ( \' , \" , \\ , \b ,
 \f , \n , \r , \t , \v ), that sequence is used rather than the
corresponding numeric escape (e.g \x0a , \u000a , or \u{a} ). Legacy octal
escapes are never used. 

 Non-ASCII characters 

 For the remaining non-ASCII characters, use the actual Unicode character (e.g.
 ∞ ). For non-printable characters, the equivalent hex or Unicode escapes (e.g.
 \u221e ) can be used along with an explanatory comment. 

 // Perfectly clear, even without a comment.
const units = 'μs';

// Use escapes for non-printable characters.
const output = '\ufeff' + content; // byte order mark

 // Hard to read and prone to mistakes, even with the comment.
const units = '\u03bcs'; // Greek letter mu, 's'

// The reader has no idea what this is.
const output = '\ufeff' + content;

 Source file structure 

 Files consist of the following, in order : 

 Copyright information, if present 
 JSDoc with @fileoverview , if present 
 Imports, if present 
 The file’s implementation 

 Exactly one blank line separates each section that is present. 

 Copyright information 

 If license or copyright information is necessary in a file, add it in a JSDoc at
the top of the file. 

 @fileoverview JSDoc 

 A file may have a top-level @fileoverview JSDoc. If present, it may provide a
description of the file's content, its uses, or information about its
dependencies. Wrapped lines are not indented. 

 Example: 

 /**
 * @fileoverview Description of file. Lorem ipsum dolor sit amet, consectetur
 * adipiscing elit, sed do eiusmod tempor incididunt.
 */

 Imports 

 There are four variants of import statements in ES6 and TypeScript: 

 Import type 
 Example 
 Use for 

 module[ module_import] 

 import * as foo from
'...'; 
 TypeScript imports

 named[ destructuring_import] 

 import {SomeThing}
from '...'; 
 TypeScript imports

 default

 import SomeThing
from '...'; 

 Only for other
external code that
requires them 

 side-effect

 import '...'; 

 Only to import
libraries for their
side-effects on load
(such as custom
elements) 

 // Good: choose between two options as appropriate (see below).
import * as ng from '@angular/core';
import {Foo} from './foo';

// Only when needed: default imports.
import Button from 'Button';

// Sometimes needed to import libraries for their side effects:
import 'jasmine';
import '@polymer/paper-button';

 Import paths 

 TypeScript code must use paths to import other TypeScript code. Paths may be
relative, i.e. starting with . or .. ,
 or rooted at the base directory, e.g.
 root/path/to/file . 

 Code should use relative imports ( ./foo ) rather than absolute imports
 path/to/foo when referring to files within the same (logical) project as this
allows to move the project around without introducing changes in these imports. 

 Consider limiting the number of parent steps ( ../../../ ) as those can make
module and path structures hard to understand. 

 import {Symbol1} from 'path/from/root';
import {Symbol2} from '../parent/file';
import {Symbol3} from './sibling';

 Namespace versus named imports 

 Both namespace and named imports can be used. 

 Prefer named imports for symbols used frequently in a file or for symbols that
have clear names, for example Jasmine's describe and it . Named imports can
be aliased to clearer names as needed with as . 

 Prefer namespace imports when using many different symbols from large APIs. A
namespace import, despite using the * character, is not comparable to a
 wildcard import as seen in other languages. Instead, namespace imports give a
name to all the exports of a module, and each exported symbol from the module
becomes a property on the module name. Namespace imports can aid readability for
exported symbols that have common names like Model or Controller without the
need to declare aliases. 

 // Bad: overlong import statement of needlessly namespaced names.
import {Item as TableviewItem, Header as TableviewHeader, Row as TableviewRow,
 Model as TableviewModel, Renderer as TableviewRenderer} from './tableview';

let item: TableviewItem|undefined;

 // Better: use the module for namespacing.
import * as tableview from './tableview';

let item: tableview.Item|undefined;

 import * as testing from './testing';

// Bad: The module name does not improve readability.
testing.describe('foo', () => {
 testing.it('bar', () => {
 testing.expect(null).toBeNull();
 testing.expect(undefined).toBeUndefined();
 });
});

 // Better: give local names for these common functions.
import {describe, it, expect} from './testing';

describe('foo', () => {
 it('bar', () => {
 expect(null).toBeNull();
 expect(undefined).toBeUndefined();
 });
});

 Special case: Apps JSPB protos 

 Apps JSPB protos must use named imports, even when it leads to long import
lines. 

 This rule exists to aid in build performance and dead code elimination since
often .proto files contain many message s that are not all needed together.
By leveraging destructured imports the build system can create finer grained
dependencies on Apps JSPB messages while preserving the ergonomics of path based
imports. 

 // Good: import the exact set of symbols you need from the proto file.
import {Foo, Bar} from './foo.proto';

function copyFooBar(foo: Foo, bar: Bar) {...}

 Renaming imports 

 Code should fix name collisions by using a namespace import or renaming the
exports themselves. Code may rename imports ( import {SomeThing as
SomeOtherThing} ) if needed. 

 Three examples where renaming can be helpful: 

 If it's necessary to avoid collisions with other imported symbols. 
 If the imported symbol name is generated. 
 If importing symbols whose names are unclear by themselves, renaming can
improve code clarity. For example, when using RxJS the from function might
be more readable when renamed to observableFrom . 

 Exports 

 Use named exports in all code: 

 // Use named exports:
export class Foo { ... }

 Do not use default exports. This ensures that all imports follow a uniform
pattern. 

 // Do not use default exports:
export default class Foo { ... } // BAD!

 Why? 

 Default exports provide no canonical name, which makes central maintenance
difficult with relatively little benefit to code owners, including potentially
decreased readability: 

 import Foo from './bar'; // Legal.
import Bar from './bar'; // Also legal.

 Named exports have the benefit of erroring when import statements try to import
something that hasn't been declared. In foo.ts : 

 const foo = 'blah';
export default foo;

 And in bar.ts : 

 import {fizz} from './foo';

 Results in error TS2614: Module '"./foo"' has no exported member 'fizz'. While
 bar.ts : 

 import fizz from './foo';

 Results in fizz === foo , which is probably unexpected and difficult to debug. 

 Additionally, default exports encourage people to put everything into one big
object to namespace it all together: 

 export default class Foo {
 static SOME_CONSTANT = ...
 static someHelpfulFunction() { ... }
 ...
}

 With the above pattern, we have file scope, which can be used as a namespace. We
also have a perhaps needless second scope (the class Foo ) that can be
ambiguously used as both a type and a value in other files. 

 Instead, prefer use of file scope for namespacing, as well as named exports: 

 export const SOME_CONSTANT = ...
export function someHelpfulFunction()
export class Foo {
 // only class stuff here
}

 Export visibility 

 TypeScript does not support restricting the visibility for exported symbols.
Only export symbols that are used outside of the module. Generally minimize the
exported API surface of modules. 

 Mutable exports 

 Regardless of technical support, mutable exports can create hard to understand
and debug code, in particular with re-exports across multiple modules. One way
to paraphrase this style point is that export let is not allowed. 

 export let foo = 3;
// In pure ES6, foo is mutable and importers will observe the value change after a second.
// In TS, if foo is re-exported by a second file, importers will not see the value change.
window.setTimeout(() => {
 foo = 4;
}, 1000 /* ms */);

 If one needs to support externally accessible and mutable bindings, they
 should instead use explicit getter functions. 

 let foo = 3;
window.setTimeout(() => {
 foo = 4;
}, 1000 /* ms */);
// Use an explicit getter to access the mutable export.
export function getFoo() { return foo; };

 For the common pattern of conditionally exporting either of two values, first do
the conditional check, then the export. Make sure that all exports are final
after the module's body has executed. 

 function pickApi() {
 if (useOtherApi()) return OtherApi;
 return RegularApi;
}
export const SomeApi = pickApi();

 Container classes 

 Do not create container classes with static methods or properties for the sake
of namespacing. 

 export class Container {
 static FOO = 1;
 static bar() { return 1; }
}

 Instead, export individual constants and functions: 

 export const FOO = 1;
export function bar() { return 1; }

 Import and export type 

 Import type 

 You may use import type {...} when you use the imported symbol only as a type.
Use regular imports for values: 

 import type {Foo} from './foo';
import {Bar} from './foo';

import {type Foo, Bar} from './foo';

 Why? 

 The TypeScript compiler automatically handles the distinction and does not
insert runtime loads for type references. So why annotate type imports? 

 The TypeScript compiler can run in 2 modes: 

 In development mode, we typically want quick iteration loops. The compiler
transpiles to JavaScript without full type information. This is much faster,
but requires import type in certain cases. 
 In production mode, we want correctness. The compiler type checks everything
and ensures import type is used correctly. 

 Note: If you need to force a runtime load for side effects, use import '...'; .
See 

 Export type 

 Use export type when re-exporting a type, e.g.: 

 export type {AnInterface} from './foo';

 Why? 

 export type is useful to allow type re-exports in file-by-file transpilation.
See
 isolatedModules docs . 

 export type might also seem useful to avoid ever exporting a value symbol for
an API. However it does not give guarantees, either: downstream code might still
import an API through a different path. A better way to split & guarantee type
vs value usages of an API is to actually split the symbols into e.g.
 UserService and AjaxUserService . This is less error prone and also better
communicates intent. 

 Use modules not namespaces 

 TypeScript supports two methods to organize code: namespaces and modules ,
but namespaces are disallowed. That
is, your code must refer to code in other files using imports and exports of
the form import {foo} from 'bar'; 

 Your code must not use the namespace Foo { ... } construct. namespace s
 may only be used when required to interface with external, third party code.
To semantically namespace your code, use separate files. 

 Code must not use require (as in import x = require('...'); ) for imports.
Use ES6 module syntax. 

 // Bad: do not use namespaces:
namespace Rocket {
 function launch() { ... }
}

// Bad: do not use <reference>
/// <reference path="..."/>

// Bad: do not use require()
import x = require('mydep');

 NB: TypeScript namespace s used to be called internal modules and used to use
the module keyword in the form module Foo { ... } . Don't use that either.
Always use ES6 imports. 

 Language features 

 This section delineates which features may or may not be used, and any
additional constraints on their use. 

 Language features which are not discussed in this style guide may be used with
no recommendations of their usage. 

 Local variable declarations 

 Use const and let 

 Always use const or let to declare variables. Use const by default, unless
a variable needs to be reassigned. Never use var . 

 const foo = otherValue; // Use if "foo" never changes.
let bar = someValue; // Use if "bar" is ever assigned into later on.

 const and let are block scoped, like variables in most other languages.
 var in JavaScript is function scoped, which can cause difficult to understand
bugs. Don't use it. 

 var foo = someValue; // Don't use - var scoping is complex and causes bugs.

 Variables must not be used before their declaration. 

 One variable per declaration 

 Every local variable declaration declares only one variable: declarations such
as let a = 1, b = 2; are not used. 

 Array literals 

 Do not use the Array constructor 

 Do not use the Array() constructor, with or without new . It has confusing
and contradictory usage: 

 const a = new Array(2); // [undefined, undefined]
const b = new Array(2, 3); // [2, 3];

 Instead, always use bracket notation to initialize arrays, or from to
initialize an Array with a certain size: 

 const a = [2];
const b = [2, 3];

// Equivalent to Array(2):
const c = [];
c.length = 2;

// [0, 0, 0, 0, 0]
Array.from<number>({length: 5}).fill(0);

 Do not define properties on arrays 

 Do not define or use non-numeric properties on an array (other than length ).
Use a Map (or Object ) instead. 

 Using spread syntax 

 Using spread syntax [...foo]; is a convenient shorthand for shallow-copying or
concatenating iterables. 

 const foo = [
 1,
];

const foo2 = [
 ...foo,
 6,
 7,
];

const foo3 = [
 5,
 ...foo,
];

foo2[1] === 6;
foo3[1] === 1;

 When using spread syntax, the value being spread must match what is being
created. When creating an array, only spread iterables. Primitives (including
 null and undefined ) must not be spread. 

 const foo = [7];
const bar = [5, ...(shouldUseFoo && foo)]; // might be undefined

// Creates {0: 'a', 1: 'b', 2: 'c'} but has no length
const fooStrings = ['a', 'b', 'c'];
const ids = {...fooStrings};

 const foo = shouldUseFoo ? [7] : [];
const bar = [5, ...foo];
const fooStrings = ['a', 'b', 'c'];
const ids = [...fooStrings, 'd', 'e'];

 Array destructuring 

 Array literals may be used on the left-hand side of an assignment to perform
destructuring (such as when unpacking multiple values from a single array or
iterable). A final rest element may be included (with no space between the
 ... and the variable name). Elements should be omitted if they are unused. 

 const [a, b, c, ...rest] = generateResults();
let [, b,, d] = someArray;

 Destructuring may also be used for function parameters. Always specify [] as
the default value if a destructured array parameter is optional, and provide
default values on the left hand side: 

 function destructured([a = 4, b = 2] = []) { … }

 Disallowed: 

 function badDestructuring([a, b] = [4, 2]) { … }

 Tip: For (un)packing multiple values into a function’s parameter or return,
prefer object destructuring to array destructuring when possible, as it allows
naming the individual elements and specifying a different type for each. 

 Object literals 

 Do not use the Object constructor 

 The Object constructor is disallowed. Use an object literal ( {} or {a: 0,
b: 1, c: 2} ) instead. 

 Iterating objects 

 Iterating objects with for (... in ...) is error prone. It will include
enumerable properties from the prototype chain. 

 Do not use unfiltered for (... in ...) statements: 

 for (const x in someObj) {
 // x could come from some parent prototype!
}

 Either filter values explicitly with an if statement, or use for (... of
Object.keys(...)) . 

 for (const x in someObj) {
 if (!someObj.hasOwnProperty(x)) continue;
 // now x was definitely defined on someObj
}
for (const x of Object.keys(someObj)) { // note: for _of_!
 // now x was definitely defined on someObj
}
for (const [key, value] of Object.entries(someObj)) { // note: for _of_!
 // now key was definitely defined on someObj
}

 Using spread syntax 

 Using spread syntax {...bar} is a convenient shorthand for creating a shallow
copy of an object. When using spread syntax in object initialization, later
values replace earlier values at the same key. 

 const foo = {
 num: 1,
};

const foo2 = {
 ...foo,
 num: 5,
};

const foo3 = {
 num: 5,
 ...foo,
}

foo2.num === 5;
foo3.num === 1;

 When using spread syntax, the value being spread must match what is being
created. That is, when creating an object, only objects may be spread; arrays
and primitives (including null and undefined ) must not be spread. Avoid
spreading objects that have prototypes other than the Object prototype (e.g.
class definitions, class instances, functions) as the behavior is unintuitive
(only enumerable non-prototype properties are shallow-copied). 

 const foo = {num: 7};
const bar = {num: 5, ...(shouldUseFoo && foo)}; // might be undefined

// Creates {0: 'a', 1: 'b', 2: 'c'} but has no length
const fooStrings = ['a', 'b', 'c'];
const ids = {...fooStrings};

 const foo = shouldUseFoo ? {num: 7} : {};
const bar = {num: 5, ...foo};

 Computed property names 

 Computed property names (e.g. {['key' + foo()]: 42} ) are allowed, and are
considered dict-style (quoted) keys (i.e., must not be mixed with non-quoted
keys) unless the computed property is a
 symbol 
(e.g. [Symbol.iterator] ). 

 Object destructuring 

 Object destructuring patterns may be used on the left-hand side of an assignment
to perform destructuring and unpack multiple values from a single object. 

 Destructured objects may also be used as function parameters, but should be kept
as simple as possible: a single level of unquoted shorthand properties. Deeper
levels of nesting and computed properties may not be used in parameter
destructuring. Specify any default values in the left-hand-side of the
destructured parameter ( {str = 'some default'} = {} , rather than
 {str} = {str: 'some default'} ), and if a
destructured object is itself optional, it must default to {} . 

 Example: 

 interface Options {
 /** The number of times to do something. */
 num?: number;

 /** A string to do stuff to. */
 str?: string;
}

function destructured({num, str = 'default'}: Options = {}) {}

 Disallowed: 

 function nestedTooDeeply({x: {num, str}}: {x: Options}) {}
function nontrivialDefault({num, str}: Options = {num: 42, str: 'default'}) {}

 Classes 

 Class declarations 

 Class declarations must not be terminated with semicolons: 

 class Foo {
}

 class Foo {
}; // Unnecessary semicolon

 In contrast, statements that contain class expressions must be terminated with
a semicolon: 

 export const Baz = class extends Bar {
 method(): number {
 return this.x;
 }
}; // Semicolon here as this is a statement, not a declaration

 exports const Baz = class extends Bar {
 method(): number {
 return this.x;
 }
}

 It is neither encouraged nor discouraged to have blank lines separating class
declaration braces from other class content: 

 // No spaces around braces - fine.
class Baz {
 method(): number {
 return this.x;
 }
}

// A single space around both braces - also fine.
class Foo {

 method(): number {
 return this.x;
 }

}

 Class method declarations 

 Class method declarations must not use a semicolon to separate individual
method declarations: 

 class Foo {
 doThing() {
 console.log("A");
 }
}

 class Foo {
 doThing() {
 console.log("A");
 }; // <-- unnecessary
}

 Method declarations should be separated from surrounding code by a single blank
line: 

 class Foo {
 doThing() {
 console.log("A");
 }

 getOtherThing(): number {
 return 4;
 }
}

 class Foo {
 doThing() {
 console.log("A");
 }
 getOtherThing(): number {
 return 4;
 }
}

 Overriding toString 

 The toString method may be overridden, but must always succeed and never have
visible side effects. 

 Tip: Beware, in particular, of calling other methods from toString, since
exceptional conditions could lead to infinite loops. 

 Static methods 

 Avoid private static methods 

 Where it does not interfere with readability, prefer module-local functions over
private static methods. 

 Do not rely on dynamic dispatch 

 Code should not rely on dynamic dispatch of static
methods. Static methods should only be called on the base class
itself (which defines it directly). Static methods should not be called on
variables containing a dynamic instance that may be either the constructor or a
subclass constructor (and must be defined with @nocollapse if this is done),
and must not be called directly on a subclass that doesn’t define the method
itself. 

 Disallowed: 

 // Context for the examples below (this class is okay by itself)
class Base {
 /** @nocollapse */ static foo() {}
}
class Sub extends Base {}

// Discouraged: don't call static methods dynamically
function callFoo(cls: typeof Base) {
 cls.foo();
}

// Disallowed: don't call static methods on subclasses that don't define it themselves
Sub.foo();

// Disallowed: don't access this in static methods.
class MyClass {
 static foo() {
 return this.staticField;
 }
}
MyClass.staticField = 1;

 Avoid static this references 

 Code must not use this in a static context. 

 JavaScript allows accessing static fields through this . Different from other
languages, static fields are also inherited. 

 class ShoeStore {
 static storage: Storage = ...;

 static isAvailable(s: Shoe) {
 // Bad: do not use `this` in a static method.
 return this.storage.has(s.id);
 }
}

class EmptyShoeStore extends ShoeStore {
 static storage: Storage = EMPTY_STORE; // overrides storage from ShoeStore
}

 Why? 

 This code is generally surprising: authors might not expect that static fields
can be accessed through the this pointer, and might be surprised to find that
they can be overridden - this feature is not commonly used. 

 This code also encourages an anti-pattern of having substantial static state,
which causes problems with testability. 

 Constructors 

 Constructor calls must use parentheses, even when no arguments are passed: 

 const x = new Foo;

 const x = new Foo();

 Omitting parentheses can lead to subtle mistakes. These two lines are not
equivalent: 

 new Foo().Bar();
new Foo.Bar();

 It is unnecessary to provide an empty constructor or one that simply delegates
into its parent class because ES2015 provides a default class constructor if one
is not specified. However constructors with parameter properties, visibility
modifiers or parameter decorators should not be omitted even if the body of
the constructor is empty. 

 class UnnecessaryConstructor {
 constructor() {}
}

 class UnnecessaryConstructorOverride extends Base {
 constructor(value: number) {
 super(value);
 }
}

 class DefaultConstructor {
}

class ParameterProperties {
 constructor(private myService) {}
}

class ParameterDecorators {
 constructor(@SideEffectDecorator myService) {}
}

class NoInstantiation {
 private constructor() {}
}

 The constructor should be separated from surrounding code both above and below
by a single blank line: 

 class Foo {
 myField = 10;

 constructor(private readonly ctorParam) {}

 doThing() {
 console.log(ctorParam.getThing() + myField);
 }
}

 class Foo {
 myField = 10;
 constructor(private readonly ctorParam) {}
 doThing() {
 console.log(ctorParam.getThing() + myField);
 }
}

 Class members 

 No #private fields 

 Do not use private fields (also known as private identifiers): 

 class Clazz {
 #ident = 1;
}

 Instead, use TypeScript's visibility annotations: 

 class Clazz {
 private ident = 1;
}

 Why? 

 Private identifiers cause substantial emit size and
performance regressions when down-leveled by TypeScript, and are unsupported
before ES2015. They can only be downleveled to ES2015, not lower. At the same
time, they do not offer substantial benefits when static type checking is used
to enforce visibility. 

 Use readonly 

 Mark properties that are never reassigned outside of the constructor with the
 readonly modifier (these need not be deeply immutable). 

 Parameter properties 

 Rather than plumbing an obvious initializer through to a class member, use a
TypeScript
 parameter property . 

 class Foo {
 private readonly barService: BarService;

 constructor(barService: BarService) {
 this.barService = barService;
 }
}

 class Foo {
 constructor(private readonly barService: BarService) {}
}

 If the parameter property needs documentation,
 use an @param JSDoc tag . 

 Field initializers 

 If a class member is not a parameter, initialize it where it's declared, which
sometimes lets you drop the constructor entirely. 

 class Foo {
 private readonly userList: string[];

 constructor() {
 this.userList = [];
 }
}

 class Foo {
 private readonly userList: string[] = [];
}

 Tip: Properties should never be added to or removed from an instance after the
constructor is finished, since it significantly hinders VMs’ ability to optimize
classes' shape . Optional fields that may be filled in later should be
explicitly initialized to undefined to prevent later shape changes. 

 Properties used outside of class lexical scope 

 Properties used from outside the lexical scope of their containing class, such
as an Angular component's properties used from a template, must not use
 private visibility, as they are used outside of the lexical scope of their
containing class. 

 Use either protected or public as appropriate to the property in question.
Angular and AngularJS template properties should use protected , but Polymer
should use public . 

 TypeScript code must not use obj['foo'] to bypass the visibility of a
property. 

 Why? 

 When a property is private , you are declaring to both automated systems and
humans that the property accesses are scoped to the methods of the declaring
class, and they will rely on that. For example, a check for unused code will
flag a private property that appears to be unused, even if some other file
manages to bypass the visibility restriction. 

 Though it might appear that obj['foo'] can bypass visibility in the TypeScript
compiler, this pattern can be broken by rearranging the build rules, and also
violates optimization compatibility . 

 Getters and setters 

 Getters and setters, also known as accessors, for class members may be used.
The getter method must be a
 pure function (i.e., result is
consistent and has no side effects: getters must not change observable state).
They are also useful as a means of restricting the visibility of internal or
verbose implementation details (shown below). 

 class Foo {
 constructor(private readonly someService: SomeService) {}

 get someMember(): string {
 return this.someService.someVariable;
 }

 set someMember(newValue: string) {
 this.someService.someVariable = newValue;
 }
}

 class Foo {
 nextId = 0;
 get next() {
 return this.nextId++; // Bad: getter changes observable state
 }
}

 If an accessor is used to hide a class property, the hidden property may be
prefixed or suffixed with any whole word, like internal or wrapped . When
using these private properties, access the value through the accessor whenever
possible. At least one accessor for a property must be non-trivial: do not
define pass-through accessors only for the purpose of hiding a property.
Instead, make the property public (or consider making it readonly rather than
just defining a getter with no setter). 

 class Foo {
 private wrappedBar = '';
 get bar() {
 return this.wrappedBar || 'bar';
 }

 set bar(wrapped: string) {
 this.wrappedBar = wrapped.trim();
 }
}

 class Bar {
 private barInternal = '';
 // Neither of these accessors have logic, so just make bar public.
 get bar() {
 return this.barInternal;
 }

 set bar(value: string) {
 this.barInternal = value;
 }
}

 Getters and setters must not be defined using Object.defineProperty , since
this interferes with property renaming. 

 Computed properties 

 Computed properties may only be used in classes when the property is a symbol.
Dict-style properties (that is, quoted or computed non-symbol keys) are not
allowed (see
 rationale for not mixing key types . A
 [Symbol.iterator] method should be defined for any classes that are logically
iterable. Beyond this, Symbol should be used sparingly. 

 Tip: be careful of using any other built-in symbols (e.g.
 Symbol.isConcatSpreadable ) as they are not polyfilled by the compiler and will
therefore not work in older browsers. 

 Visibility 

 Restricting visibility of properties, methods, and entire types helps with
keeping code decoupled. 

 Limit symbol visibility as much as possible. 
 Consider converting private methods to non-exported functions within the
same file but outside of any class, and moving private properties into a
separate, non-exported class. 
 TypeScript symbols are public by default. Never use the public modifier
except when declaring non-readonly public parameter properties (in
constructors). 

 class Foo {
 public bar = new Bar(); // BAD: public modifier not needed

 constructor(public readonly baz: Baz) {} // BAD: readonly implies it's a property which defaults to public
}

 class Foo {
 bar = new Bar(); // GOOD: public modifier not needed

 constructor(public baz: Baz) {} // public modifier allowed
}

 See also export visibility . 

 Disallowed class patterns 

 Do not manipulate prototype s directly 

 The class keyword allows clearer and more readable class definitions than
defining prototype properties. Ordinary implementation code has no business
manipulating these objects. Mixins and modifying the prototypes of builtin
objects are explicitly forbidden. 

 Exception : Framework code (such as Polymer, or Angular) may need to use prototype s, and should not resort
to even-worse workarounds to avoid doing so. 

 Functions 

 Terminology 

 There are many different types of functions, with subtle distinctions between
them. This guide uses the following terminology, which aligns with
 MDN : 

 function declaration : a declaration (i.e. not an expression) using the
 function keyword 
 function expression : an expression, typically used in an assignment or
passed as a parameter, using the function keyword 
 arrow function : an expression using the => syntax 
 block body : right hand side of an arrow function with braces 
 concise body : right hand side of an arrow function without braces 

 Methods and classes/constructors are not covered in this section. 

 Prefer function declarations for named functions 

 Prefer function declarations over arrow functions or function expressions when
defining named functions. 

 function foo() {
 return 42;
}

 const foo = () => 42;

 Arrow functions may be used, for example, when an explicit type annotation is
required. 

 interface SearchFunction {
 (source: string, subString: string): boolean;
}

const fooSearch: SearchFunction = (source, subString) => { ... };

 Nested functions 

 Functions nested within other methods or functions may use function
declarations or arrow functions, as appropriate. In method bodies in particular,
arrow functions are preferred because they have access to the outer this . 

 Do not use function expressions 

 Do not use function expressions. Use arrow functions instead. 

 bar(() => { this.doSomething(); })

 bar(function() { ... })

 Exception: Function expressions may be used only if code has to
dynamically rebind this (but this is discouraged ), or for
generator functions (which do not have an arrow syntax). 

 Arrow function bodies 

 Use arrow functions with concise bodies (i.e. expressions) or block bodies as
appropriate. 

 // Top level functions use function declarations.
function someFunction() {
 // Block bodies are fine:
 const receipts = books.map((b: Book) => {
 const receipt = payMoney(b.price);
 recordTransaction(receipt);
 return receipt;
 });

 // Concise bodies are fine, too, if the return value is used:
 const longThings = myValues.filter(v => v.length > 1000).map(v => String(v));

 function payMoney(amount: number) {
 // function declarations are fine, but must not access `this`.
 }

 // Nested arrow functions may be assigned to a const.
 const computeTax = (amount: number) => amount * 0.12;
}

 Only use a concise body if the return value of the function is actually used.
The block body makes sure the return type is void then and prevents potential
side effects. 

 // BAD: use a block body if the return value of the function is not used.
myPromise.then(v => console.log(v));
// BAD: this typechecks, but the return value still leaks.
let f: () => void;
f = () => 1;

 // GOOD: return value is unused, use a block body.
myPromise.then(v => {
 console.log(v);
});
// GOOD: code may use blocks for readability.
const transformed = [1, 2, 3].map(v => {
 const intermediate = someComplicatedExpr(v);
 const more = acrossManyLines(intermediate);
 return worthWrapping(more);
});
// GOOD: explicit `void` ensures no leaked return value
myPromise.then(v => void console.log(v));

 Tip: The void operator can be used to ensure an arrow function with an
expression body returns undefined when the result is unused. 

 Rebinding this 

 Function expressions and function declarations must not use this unless they
specifically exist to rebind the this pointer. Rebinding this can in most
cases be avoided by using arrow functions or explicit parameters. 

 function clickHandler() {
 // Bad: what's `this` in this context?
 this.textContent = 'Hello';
}
// Bad: the `this` pointer reference is implicitly set to document.body.
document.body.onclick = clickHandler;

 // Good: explicitly reference the object from an arrow function.
document.body.onclick = () => { document.body.textContent = 'hello'; };
// Alternatively: take an explicit parameter
const setTextFn = (e: HTMLElement) => { e.textContent = 'hello'; };
document.body.onclick = setTextFn.bind(null, document.body);

 Prefer arrow functions over other approaches to binding this , such as
 f.bind(this) , goog.bind(f, this) , or const self = this . 

 Prefer passing arrow functions as callbacks 

 Callbacks can be invoked with unexpected arguments that can pass a type check
but still result in logical errors. 

 Avoid passing a named callback to a higher-order function, unless you are sure
of the stability of both functions' call signatures. Beware, in particular, of
less-commonly-used optional parameters. 

 // BAD: Arguments are not explicitly passed, leading to unintended behavior
// when the optional `radix` argument gets the array indices 0, 1, and 2.
const numbers = ['11', '5', '10'].map(parseInt);
// > [11, NaN, 2];

 Instead, prefer passing an arrow-function that explicitly forwards parameters to
the named callback. 

 // GOOD: Arguments are explicitly passed to the callback
const numbers = ['11', '5', '3'].map((n) => parseInt(n));
// > [11, 5, 3]

// GOOD: Function is locally defined and is designed to be used as a callback
function dayFilter(element: string|null|undefined) {
 return element != null && element.endsWith('day');
}

const days = ['tuesday', undefined, 'juice', 'wednesday'].filter(dayFilter);

 Arrow functions as properties 

 Classes usually should not contain properties initialized to arrow functions.
Arrow function properties require the calling function to understand that the
callee's this is already bound, which increases confusion about what this 
is, and call sites and references using such handlers look broken (i.e. require
non-local knowledge to determine that they are correct). Code should always
use arrow functions to call instance methods ( const handler = (x) => {
this.listener(x); }; ), and should not obtain or pass references to instance
methods ( const handler = this.listener; handler(x); ). 

 Note: in some specific situations, e.g. when binding functions in a template,
arrow functions as properties are useful and create much more readable code.
Use judgement with this rule. Also, see the
 Event Handlers section below. 

 class DelayHandler {
 constructor() {
 // Problem: `this` is not preserved in the callback. `this` in the callback
 // will not be an instance of DelayHandler.
 setTimeout(this.patienceTracker, 5000);
 }
 private patienceTracker() {
 this.waitedPatiently = true;
 }
}

 // Arrow functions usually should not be properties.
class DelayHandler {
 constructor() {
 // Bad: this code looks like it forgot to bind `this`.
 setTimeout(this.patienceTracker, 5000);
 }
 private patienceTracker = () => {
 this.waitedPatiently = true;
 }
}

 // Explicitly manage `this` at call time.
class DelayHandler {
 constructor() {
 // Use anonymous functions if possible.
 setTimeout(() => {
 this.patienceTracker();
 }, 5000);
 }
 private patienceTracker() {
 this.waitedPatiently = true;
 }
}

 Event handlers 

 Event handlers may use arrow functions when there is no need to uninstall the
handler (for example, if the event is emitted by the class itself). If the
handler requires uninstallation, arrow function properties are the right
approach, because they automatically capture this and provide a stable
reference to uninstall. 

 // Event handlers may be anonymous functions or arrow function properties.
class Component {
 onAttached() {
 // The event is emitted by this class, no need to uninstall.
 this.addEventListener('click', () => {
 this.listener();
 });
 // this.listener is a stable reference, we can uninstall it later.
 window.addEventListener('onbeforeunload', this.listener);
 }
 onDetached() {
 // The event is emitted by window. If we don't uninstall, this.listener will
 // keep a reference to `this` because it's bound, causing a memory leak.
 window.removeEventListener('onbeforeunload', this.listener);
 }
 // An arrow function stored in a property is bound to `this` automatically.
 private listener = () => {
 confirm('Do you want to exit the page?');
 }
}

 Do not use bind in the expression that installs an event handler, because it
creates a temporary reference that can't be uninstalled. 

 // Binding listeners creates a temporary reference that prevents uninstalling.
class Component {
 onAttached() {
 // This creates a temporary reference that we won't be able to uninstall
 window.addEventListener('onbeforeunload', this.listener.bind(this));
 }
 onDetached() {
 // This bind creates a different reference, so this line does nothing.
 window.removeEventListener('onbeforeunload', this.listener.bind(this));
 }
 private listener() {
 confirm('Do you want to exit the page?');
 }
}

 Parameter initializers 

 Optional function parameters may be given a default initializer to use when
the argument is omitted. Initializers must not have any observable side
effects. Initializers should be kept as simple as possible. 

 function process(name: string, extraContext: string[] = []) {}
function activate(index = 0) {}

 // BAD: side effect of incrementing the counter
let globalCounter = 0;
function newId(index = globalCounter++) {}

// BAD: exposes shared mutable state, which can introduce unintended coupling
// between function calls
class Foo {
 private readonly defaultPaths: string[];
 frobnicate(paths = defaultPaths) {}
}

 Use default parameters sparingly. Prefer
 destructuring to create readable APIs when
there are more than a small handful of optional parameters that do not have a
natural order. 

 Prefer rest and spread when appropriate 

 Use a rest parameter instead of accessing arguments . Never name a local
variable or parameter arguments , which confusingly shadows the built-in name. 

 function variadic(array: string[], ...numbers: number[]) {}

 Use function spread syntax instead of Function.prototype.apply . 

 Formatting functions 

 Blank lines at the start or end of the function body are not allowed. 

 A single blank line may be used within function bodies sparingly to create
 logical groupings of statements. 

 Generators should attach the * to the function and yield keywords, as in
 function* foo() and yield* iter , rather than function *foo() or
 yield *iter . 

 Parentheses around the left-hand side of a single-argument arrow function are
recommended but not required. 

 Do not put a space after the ... in rest or spread syntax. 

 function myFunction(...elements: number[]) {}
myFunction(...array, ...iterable, ...generator());

 this 

 Only use this in class constructors and methods, functions that have an
explicit this type declared (e.g. function func(this: ThisType, ...) ), or in
arrow functions defined in a scope where this may be used. 

 Never use this to refer to the global object, the context of an eval , the
target of an event, or unnecessarily call() ed or apply() ed functions. 

 this.alert('Hello');

 Interfaces 

 Primitive literals 

 String literals 

 Use single quotes 

 Ordinary string literals are delimited with single quotes ( ' ), rather than
double quotes ( " ). 

 Tip: if a string contains a single quote character, consider using a template
string to avoid having to escape the quote. 

 No line continuations 

 Do not use line continuations (that is, ending a line inside a string literal
with a backslash) in either ordinary or template string literals. Even though
ES5 allows this, it can lead to tricky errors if any trailing whitespace comes
after the slash, and is less obvious to readers. 

 Disallowed: 

 const LONG_STRING = 'This is a very very very very very very very long string. \
 It inadvertently contains long stretches of spaces due to how the \
 continued lines are indented.';

 Instead, write 

 const LONG_STRING = 'This is a very very very very very very long string. ' +
 'It does not contain long stretches of spaces because it uses ' +
 'concatenated strings.';
const SINGLE_STRING =
 'http://it.is.also/acceptable_to_use_a_single_long_string_when_breaking_would_hinder_search_discoverability';

 Template literals 

 Use template literals (delimited with ` ) over complex string
concatenation, particularly if multiple string literals are involved. Template
literals may span multiple lines. 

 If a template literal spans multiple lines, it does not need to follow the
indentation of the enclosing block, though it may if the added whitespace does
not matter. 

 Example: 

 function arithmetic(a: number, b: number) {
 return `Here is a table of arithmetic operations:
${a} + ${b} = ${a + b}
${a} - ${b} = ${a - b}
${a} * ${b} = ${a * b}
${a} / ${b} = ${a / b}`;
}

 Number literals 

 Numbers may be specified in decimal, hex, octal, or binary. Use exactly 0x ,
 0o , and 0b prefixes, with lowercase letters, for hex, octal, and binary,
respectively. Never include a leading zero unless it is immediately followed by
 x , o , or b . 

 Type coercion 

 TypeScript code may use the String() and Boolean() (note: no new !)
functions, string template literals, or !! to coerce types. 

 const bool = Boolean(false);
const str = String(aNumber);
const bool2 = !!str;
const str2 = `result: ${bool2}`;

 Values of enum types (including unions of enum types and other types) must not 
be converted to booleans with Boolean() or !! , and must instead be compared
explicitly with comparison operators. 

 enum SupportLevel {
 NONE,
 BASIC,
 ADVANCED,
}

const level: SupportLevel = ...;
let enabled = Boolean(level);

const maybeLevel: SupportLevel|undefined = ...;
enabled = !!maybeLevel;

 enum SupportLevel {
 NONE,
 BASIC,
 ADVANCED,
}

const level: SupportLevel = ...;
let enabled = level !== SupportLevel.NONE;

const maybeLevel: SupportLevel|undefined = ...;
enabled = level !== undefined && level !== SupportLevel.NONE;

 Why? 

 For most purposes, it doesn't matter what number or string value an enum name is
mapped to at runtime, because values of enum types are referred to by name in
source code. Consequently, engineers are accustomed to not thinking about this,
and so situations where it does matter are undesirable because they will be
surprising. Such is the case with conversion of enums to booleans; in
particular, by default, the first declared enum value is falsy (because it is 0)
while the others are truthy, which is likely to be unexpected. Readers of code
that uses an enum value may not even know whether it's the first declared value
or not. 

 Using string concatenation to cast to string is discouraged, as we check that
operands to the plus operator are of matching types. 

 Code must use Number() to parse numeric values, and must check its return
for NaN values explicitly, unless failing to parse is impossible from context. 

 Note: Number('') , Number(' ') , and Number('\t') would return 0 instead
of NaN . Number('Infinity') and Number('-Infinity') would return Infinity 
and -Infinity respectively. Additionally, exponential notation such as
 Number('1e+309') and Number('-1e+309') can overflow into Infinity . These
cases may require special handling. 

 const aNumber = Number('123');
if (!isFinite(aNumber)) throw new Error(...);

 Code must not use unary plus ( + ) to coerce strings to numbers. Parsing
numbers can fail, has surprising corner cases, and can be a code smell (parsing
at the wrong layer). A unary plus is too easy to miss in code reviews given
this. 

 const x = +y;

 Code also must not use parseInt or parseFloat to parse numbers, except for
non-base-10 strings (see below). Both of those functions ignore trailing
characters in the string, which can shadow error conditions (e.g. parsing 12
dwarves as 12 ). 

 const n = parseInt(someString, 10); // Error prone,
const f = parseFloat(someString); // regardless of passing a radix.

 Code that requires parsing with a radix must check that its input contains
only appropriate digits for that radix before calling into parseInt ; 

 if (!/^[a-fA-F0-9]+$/.test(someString)) throw new Error(...);
// Needed to parse hexadecimal.
// tslint:disable-next-line:ban
const n = parseInt(someString, 16); // Only allowed for radix != 10

 Use Number() followed by Math.floor or Math.trunc (where available) to
parse integer numbers: 

 let f = Number(someString);
if (isNaN(f)) handleError();
f = Math.floor(f);

 Implicit coercion 

 Do not use explicit boolean coercions in conditional clauses that have implicit
boolean coercion. Those are the conditions in an if , for and while 
statements. 

 const foo: MyInterface|null = ...;
if (!!foo) {...}
while (!!foo) {...}

 const foo: MyInterface|null = ...;
if (foo) {...}
while (foo) {...}

 As with explicit conversions , values of enum types (including
unions of enum types and other types) must not be implicitly coerced to
booleans, and must instead be compared explicitly with comparison operators. 

 enum SupportLevel {
 NONE,
 BASIC,
 ADVANCED,
}

const level: SupportLevel = ...;
if (level) {...}

const maybeLevel: SupportLevel|undefined = ...;
if (level) {...}

 enum SupportLevel {
 NONE,
 BASIC,
 ADVANCED,
}

const level: SupportLevel = ...;
if (level !== SupportLevel.NONE) {...}

const maybeLevel: SupportLevel|undefined = ...;
if (level !== undefined && level !== SupportLevel.NONE) {...}

 Other types of values may be either implicitly coerced to booleans or compared
explicitly with comparison operators: 

 // Explicitly comparing > 0 is OK:
if (arr.length > 0) {...}
// so is relying on boolean coercion:
if (arr.length) {...}

 Control structures 

 Control flow statements and blocks 

 Control flow statements ( if , else , for , do , while , etc) always use
braced blocks for the containing code, even if the body contains only a single
statement. The first statement of a non-empty block must begin on its own line. 

 for (let i = 0; i < x; i++) {
 doSomethingWith(i);
}

if (x) {
 doSomethingWithALongMethodNameThatForcesANewLine(x);
}

 if (x)
 doSomethingWithALongMethodNameThatForcesANewLine(x);

for (let i = 0; i < x; i++) doSomethingWith(i);

 Exception: if statements fitting on one line may elide the block. 

 if (x) x.doFoo();

 Assignment in control statements 

 Prefer to avoid assignment of variables inside control statements. Assignment
can be easily mistaken for equality checks inside control statements. 

 if (x = someFunction()) {
 // Assignment easily mistaken with equality check
 // ...
}

 x = someFunction();
if (x) {
 // ...
}

 In cases where assignment inside the control statement is preferred, enclose the
assignment in additional parenthesis to indicate it is intentional. 

 while ((x = someFunction())) {
 // Double parenthesis shows assignment is intentional
 // ...
}

 Iterating containers 

 Prefer for (... of someArr) to iterate over arrays. Array.prototype.forEach and vanilla for 
loops are also allowed: 

 for (const x of someArr) {
 // x is a value of someArr.
}

for (let i = 0; i < someArr.length; i++) {
 // Explicitly count if the index i
```
