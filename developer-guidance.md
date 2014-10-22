## Developer Guidance and Standards
This document outlines the standards and guidance for developers working on the core OZONE Platform repositories, as well as other developers making contributions through plugins, addons, or additional functionality.

As a general rule, we use the [Google Javascript style guide](https://google-styleguide.googlecode.com/svn/trunk/javascriptguide.xml). The additional points below are for clarification and any differences from the Google style guide.

### Consistency
The biggest takeaway from the Google style guide is to BE CONSISTENT. When creating or editing source code, look at the surrounding code and make your code look like the existing code. 

### Formatting
1. Whitespace will be spaces only with indentation of four spaces. Where tabs are used in IDEs, these are to be converted to spaces.
2. No trailing EOLs will be left in a source code file.
3. Scope application (brackets/braces/etc.) will be applied on the same line:

    ```javascript
            for (var i = 0; i <= 25; i++) {
                // insert code here
            }
    ```
4. Internal source code documentation will be applied liberally thoughout the file via block comments (``` /* ... */ ```) and inline comments (``` // ... ```).
5. Each source code file will contain a block comment header with appropriate tags in the YUI standard (http://yui.github.io/yuidoc/)
6. Each source code file will include a ```@class``` tag defining the prototypal class in JavaScript.
7. User interface content (HTML) will apply similar whitespace and formatting as appropriate (no flat DOM design)
                                    
### Licensing
1. All repository root folders will contain a copy of the Apache v2 license in text format (https://www.apache.org/licenses/LICENSE-2.0).
2. All source code, configuration, and other relevant files will contain a header for the Apache v2 license (see https://www.apache.org/licenses/LICENSE-2.0, appendix)

### Convention
1. All JavaScript objects that summarize a domain object or Class-like structure will be capitalized (ex: ```ThisClass```).
2. All instantiation of objects will be represented in camel case (ex: ```var thisClass = new ThisClass()```)
3. Object-oriented design will be applied using prototypal inheritance (https://en.wikipedia.org/wiki/Prototype-based_programming). This is to be conducted with the use of prototype chaining between objects, not object and key/value extensions (see reference).

### Repository
1. When merging a branch (merge/pull request), delete the branch upon completion.

### Language Use
1. The core OZONE Platform service layer will be constructed in Node.js. Additional permutations of the software may exist as other implementations through external efforts or community pull requests.

### Dependencies
1. Dependency management for server-side components in Node.js should be implemented via AMD with CommonJS (```require``` style includes and injection). Other forms of AMD for server-side development should be ignored as they can easily cause conflicts with CommonJS harnesses and testing frameworks.
2. Dependency management for client-side components in Angular should be handled via the library itself (lazy-loading elements with directives, see http://docs.angularjs.org/guide/di).
3. Additional client-side libraries and components should be loaded after static comment (see http://developer.yahoo.com/performance/rules.html#js_bottom).
4. NPM modules should be limited to core libraries, not embedded functionality.
5. Project-based dependencies should be packaged as a common Git repository without submodules. In cases where external developers are building plugins or additional functionality, this case may be an exception and handled as a submodule where appropriate (see http://git-scm.com/book/en/Git-Tools-Submodules).
