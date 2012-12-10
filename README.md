Escope ([escope](http://github.com/Constellation/escope)) is
[ECMAScript](http://www.ecma-international.org/publications/standards/Ecma-262.htm)
scope analyzer extracted from [esmangle project](http://github.com/Constellation/esmangle).

## Overview
escope.analyze takes ```ast```, returns a ```ScopeManager``` called ```sm```
	Traverses ```ast``` (let's call the node being visited ```v```)
		Collects information into ```sm.scopes```,
			```sm.scopes``` is a list; a ```Scope``` consists of:
				List of ```scope.references``` to Identifier nodes; a ```Reference``` consists of:
					```reference.id_node, type = 'Identifier'```
					```reference.flag, WRITE | READ | READ_WRITE``` indicates how a scope uses ```reference.id_node``` (whether it alters ```reference.id_node```, only reads the value, or both)
					```reference.scope``` is the referencing scope
					Furthermore, a ```Reference```:
						Can eventually be ```reference.tainted``` if ???TODO???
						Can eventually be assigned its ```reference.resolved_value```, otherwise ```reference.scope``` references an undefined identifier
				Furthermore, a ```Scope```:
					Is created only if ```v, type = Program | FunctionDeclaration | FunctionExpression | WithStatement | CatchClause```

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
