Blue Clover Devices C/C++ Style Guide
====================================

This coding guideline is based on [Google C++ Style Guide] with some deviations
based on our requirements at BCD. This document goes over the places where we 
deviate from the original coding style. 

Pre-requisite reading
----------------------
[Google C++ Style Guide]

Introduction
------------

Although the google coding standard is a C++ standard, it easily adapts to a 
C environment. This document will try and stick to the order of topics found in the 
original coding style. It is suggested to read a section of the original coding style
and then refer to this document for the specific overrides to the section.


#### Header files
* *Header Guards:*  
   Google specifies using the path of the header file relative to the project folder
   as part of the header guard. Instead we will use only the name of the header file 
   with the leading and trailing double underscores. Logical words in the name are 
   separated with underscores. 

``` C
#ifndef __MY_HEADER_FILE_H__
#define __MY_HEADER_FILE_H__
// Header declarations.
#endif
```
* *Function parameter ordering:*  
  Stick with the specification as in the google style guide. 
  In addition, add const modifiers to all parameters that are strictly input.


#### Scoping
* *Local Variables:*  
   We will be assuming a C99 standard and so always declare variables in the
   most limited scope possible.

* *Static and Global Variables:*  
   The google standard disallows statics and globals and this should be strictly
   adhered to in general and specifically in C++ environments. 
   
   However for certain embedded applications the use of 
   globals and statics are significantly convenient. In environments 
   where heap allocation is not available, we may have to have statically created 
   memory objects. In these cases use plain old data types. For complex state, 
   make sure the allocated objects are static private and are not manipulated 
   directly by client code. 

``` C
        static int s_state = 0;
        
        inline int* GetState()  { return &s_state; }
        
        int DoSomethingWithState() 
        { 
           /// ... 
           return SUCCESS;
        };
```
When a **heap is available, do not use globals**. Create stateful objects using an 
initializer and then use interface functions to interract with the state. 

#### Naming
We restress the importance of descriptive names. Do not use acronymns, unless its
something known globally like LCD or USB. At BCD we take a different approach to 
naming.

* **File Names:** should use mixed case. eg `FileName.c` or 
    `FileName.cc` or `FileName.cpp`. Do not use filenames that come with library 
    and toolchain files. All source files should have a header (the exception 
    being main.c perhaps). 

* **Type Names:** use mixed case. eg. `MyDataType` , `MyClass`.

* **Variable Names:** use mixed case however,
    * the first word should start with a lowercaseletter of the name. 
    * Variables that are globals or statics will have a prefix of `g_` or `s_`. 
    * Member variables of classes will in addition, have a prefix of `m_`. 
    * Members of structures don't need a prefix.
    * Pointers and pointer-to-pointers should have a `p` or a `pp` as the first
      word of the name.
    * Constants will have the `k` as the fist word of the name.
   
   Examples  
    ``` C++
    static int g_globalVariable
    
    struct NewType {
        int myVal;
        char myVal2;
    };

    class MyClass {
    /// ....
    private:
        int m_dataItem;
        void *m_pObject;
    };
    
    void Func()
    {
        static int s_staticVariable;
        int localVariable = 1;
        void *pObject = NULL; 
        void **ppObject = NULL;
        const int kDaysInAWeek = 7;
    }
    ```
* **Function Names:** should also use mixed case. eg `AddTableEntry()`. Accessors
    and mutators should match the name of the variable they are getting and 
    setting.

* **Namespace Names:** should be all lower-case, and based on project names and
    possibly their director structure.

* **Enumerator Names:** Enumerators should be named like constants or like
    macros.

* **Macro Names:** should be named with all uppercase characters, with words 
    separated by underscore. eg `#define MACRO_DEFINITION`
    
#### Comments
Stick to the google coding style regarding comments. In addition make sure 
that all interface documentation uses comment extenstion that will allow them to
be parsed by Doxygen to allow for documenation generation.

#### Formatting
We shall stick to the 
Google Coding Style except for the list documented below:

* **Line Length:** try to keep the line length between 80 - 100 characters 
   as far as possible
* **Spaces vs Tabs:** Expand all tabs to spaces. Configure your editor to do so
    automatically.
* **Paranthesis:** The contents of all parentheses except the parentheses with
    no arguments, should always have a starting and trailing white space 
    chararcter. eg.
    
    ``` C++
    int Function( int param1, int param2 );
    int Foo()
    {
        int var1 = 0 , vart2 = 1;
        int ret = 0;
        ret = DoSomething( var1, var2 );
        if( ret < 1 ) {
            // Stuff
        }
        for( int i = 0; i < 5; ++i );
    }
    ```
* **Braces:** should be on the following line for function definitions 
    and prototypes. All other forms should include the opening brace on the 
    same line as the key word. eg.
    ``` C++
    int Function( int param1, int paramt2 )
    {
        int k = 0;
        //Function
        for( int i = 0; i < 5; ++i );
        if( k < 1 ) {
            // Do Stuff
        }
        while( 1 ) { 
            //Do More Stuff
        }
    }
    ```
* **Switch Statements:** You may use braces for blocks. Annotate non-trivial
    fall-through between cases. Indent case statements from switch. Indent blocks
    in code.

* **Indentation:** Set all indentation levels to two spaces. 

[Google C++ Style Guide]:[http://google-styleguide.googlecode.com/svn/trunk/cppguide.xml]
