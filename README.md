## Overview

Escope ([escope](http://github.com/Constellation/escope)) is
[ECMAScript](http://www.ecma-international.org/publications/standards/Ecma-262.htm)
scope analyzer extracted from [esmangle project](http://github.com/Constellation/esmangle).

It finds references to variables, ```node.type == 'Identifier'```, determining:
* Unused function arguments
* Undefined variables
* Where variables are declared
* Where and how variables are used (assigned, read, or updated)

## Examples

### Find variables that are used in multiple functions

### Find functions containing undefined variables

### Find out where a variable is declared, defined, used, or updated

### Find changes to specific variables (window.history, window.location, etc..)

## API
* ```escope.analyze(ast)``` returns __ScopeManager__

## Process

* ```escope.analyze(ast)``` returns a __ScopeManager__, let's call it ```sm```
	* Traverses ```ast```, let's call the node being visited ```v```
		* Collects information into ```sm.scopes```, a list; a __Scope__ consists of:
			* List of ```scope.references``` to Identifier nodes; a __Reference__ consists of:
				* ```reference.id_node[type = Identifier]```
				* ```reference[flag = WRITE | READ | READ_WRITE]``` indicates how a scope uses ```reference.id_node``` &mdash; whether it alters ```reference.id_node```, only reads the value, or both
				* ```reference.scope```, the referencing scope
				* Furthermore, a __Reference__:
					* Can eventually be ```reference.tainted``` if ???TODO???
					* Can eventually be assigned its ```reference.resolved_value```, otherwise ```reference.scope``` references an undefined identifier
			* Furthermore, a __Scope__:
				* Is created only if ```v[type = Program | FunctionDeclaration | FunctionExpression | WithStatement | CatchClause]```
				* Can contain child scopes, ```scope.upper``` is a reference to the parent scope
				* Has an ancestor ```scope.variableScope``` &mdash; points to itself if ```scope``` is one that can hold variables
				* Is considered dynamic if ```v[type = Program | WithStatement]``` or if the scope (or any child scope) contains a call to ```eval```
					* Otherwise, ```scope``` is considered static


## Details

### ```reference.flag```
* WRITE
	* AssignmentStatement


* READ
	* general access, example: ```binaryOp.left.type = 'Identifer'```
	

* READ_WRITE
	* UpdateExpression


## References
* AST structure
	* SpiderMonkey Parser API https://developer.mozilla.org/en-US/docs/SpiderMonkey/Parser_API



### License

Copyright (C) 2012 [Yusuke Suzuki](http://github.com/Constellation)
 (twitter: [@Constellation](http://twitter.com/Constellation)) and other contributors.

Redistribution and use in source and binary forms, with or without
modification, are permitted provided that the following conditions are met:

  * Redistributions of source code must retain the above copyright
    notice, this list of conditions and the following disclaimer.

  * Redistributions in binary form must reproduce the above copyright
    notice, this list of conditions and the following disclaimer in the
    documentation and/or other materials provided with the distribution.

THIS SOFTWARE IS PROVIDED BY THE COPYRIGHT HOLDERS AND CONTRIBUTORS "AS IS"
AND ANY EXPRESS OR IMPLIED WARRANTIES, INCLUDING, BUT NOT LIMITED TO, THE
IMPLIED WARRANTIES OF MERCHANTABILITY AND FITNESS FOR A PARTICULAR PURPOSE
ARE DISCLAIMED. IN NO EVENT SHALL <COPYRIGHT HOLDER> BE LIABLE FOR ANY
DIRECT, INDIRECT, INCIDENTAL, SPECIAL, EXEMPLARY, OR CONSEQUENTIAL DAMAGES
(INCLUDING, BUT NOT LIMITED TO, PROCUREMENT OF SUBSTITUTE GOODS OR SERVICES;
LOSS OF USE, DATA, OR PROFITS; OR BUSINESS INTERRUPTION) HOWEVER CAUSED AND
ON ANY THEORY OF LIABILITY, WHETHER IN CONTRACT, STRICT LIABILITY, OR TORT
(INCLUDING NEGLIGENCE OR OTHERWISE) ARISING IN ANY WAY OUT OF THE USE OF
THIS SOFTWARE, EVEN IF ADVISED OF THE POSSIBILITY OF SUCH DAMAGE.
