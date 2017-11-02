### Chapter 3 
## Debugging the AST(Abstract Syntax Tree) generator

Let us begin our debugging in the function _PyAST_FromNodeObject_ on _line number 1169_ which is a call to the function _PyAST_FromNode_ .

This function takes the root node of the parse tree as the input and produces the Abstract Syntax tree as the output. Let us step into this function which is defined in the file _Python/ast.c_ on _line number 216_. Let us insert a debug point on _line number 244_.

In this line it calls the function _sdl_seq_new_ to generate the Abstract Syntax Definition Language sequence for the generated parse tree.  I shall paste a piece of code for reference.


Before we understand this piece of code we need to understand some _macros_ useful for iterating parse trees, <br /> 

``` CHILD(node *, int) ``` 
Returns the nth child of the node using zero-offset indexing  <br /> 


``` RCHILD(node *, int) ```

Returns the nth child of the node from the right side. Index is negative number. <br />


``` NCH(node *) ```

Number of children the node has <br /> 
``` STR(node *) ```

String representation of the node; e.g., will return : for a COLON token  <br /> 

``` TYPE(node *) ```

The type of node as specified in _Include/graminit.h_  <br /> 

``` REQ(node *, TYPE) ```

Assert that the node is the type that is expected <br /> 

``` LINENO(node *) ```

Retrieve the line number of the source code that led to the creation of the parse rule ; defined in _Python/ast.c_  <br /> 

### Source Code: 
```
for (i = 0; i < NCH(n) - 1; i++) {
                ch = CHILD(n, i);
                if (TYPE(ch) == NEWLINE)
                    continue;
                REQ(ch, stmt);
                num = num_stmts(ch);
                if (num == 1) {
                    s = ast_for_stmt(&c, ch);
                    if (!s)
                        goto out;
                    asdl_seq_SET(stmts, k++, s);
                            }
                else {
                    ch = CHILD(ch, 0);
                    REQ(ch, simple_stmt);
                    for (j = 0; j < num; j++) {
                        s = ast_for_stmt(&c, CHILD(ch, j * 2));
                        if (!s)
                            goto out;
                        asdl_seq_SET(stmts, k++, s);
                    }
                }
            }
            res = Module(stmts, arena);
                    
```
In this piece of code we observe that the parse tree iterated and an _asdl sequence_ is constructed for the tree. Once the asdl sequence is generated an object of the type _module_ty_ is returned from which the compiler takes over to form the code generation.

Let us debug the function _ast_for_stmt_ which is defined in the file _Python/ast.c_ on _line number 3970_. Insert a breakpoint at this line and observe how the debugger gets trapped. Observe the flow of code and see that it detects that the type of the node is a small_stmt and it’s type is 270 which is an _expr_stmt_ . The codes for the individual statements of python is defined in the file Include/graminit.h. This intern calls the function _ast_for_expr_stmt_ which is defined for the asdl:
```
        expr_stmt: testlist (augassign (yield_expr|testlist)
                | ('=' (yield_expr|testlist))*)
       testlist: test (',' test)* [',']
       augassign: '+=' | '-=' | '*=' | '/=' | '%=' | '&=' | '|=' | '^='
                | '<<=' | '>>=' | '**=' | '//='
```

I would suggest you to debug this entire function to understand how the ASDL statement is generated. In _line number 2895_ we see that an Assign kind of statement  is returned. 
The generation of the Assign statement is defined in the file _Python/ast.c_ on _line number 1120_. Scroll through the file to observe how different kinds of statements are created. Print statement is generated on _line number 1173_.

I would suggest you to also debug this code flow for different programs. An example to state is with a program with a class or a function with a decorator and observe how the AST is generated for the same. Adios !!


