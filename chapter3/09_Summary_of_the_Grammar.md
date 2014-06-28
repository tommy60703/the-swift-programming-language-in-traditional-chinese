> 翻譯：[stanzhai](https://github.com/stanzhai)
> 校對：[xielingwang](https://github.com/xielingwang)

# 語法總結
_________________

本頁包含內容：

* [語句（Statements）](#statements)
* [泛型參數（Generic Parameters and Arguments）](#generic_parameters_and_arguments)
* [宣告（Declarations）](#declarations)
* [模式（Patterns）](#patterns)
* [特性（Attributes）](#attributes)
* [表達式（Expressions）](#expressions)
* [詞法結構（Lexical Structure）](#lexical_structure)
* [型別（Types）](#types)

<a name="statements"></a>
## 語句

> 語句語法  
> *語句* → [*表達式*](..\chapter3\04_Expressions.html#expression) **;** _可選_  
> *語句* → [*宣告*](..\chapter3\05_Declarations.html#declaration) **;** _可選_  
> *語句* → [*迴圈語句*](..\chapter3\10_Statements.html#loop_statement) **;** _可選_  
> *語句* → [*分支語句*](..\chapter3\10_Statements.html#branch_statement) **;** _可選_  
> *語句* → [*標記語句(Labeled Statement)*](..\chapter3\10_Statements.html#labeled_statement)  
> *語句* → [*控制轉移語句*](..\chapter3\10_Statements.html#control_transfer_statement) **;** _可選_  
> *多條語句(Statements)* → [*語句*](..\chapter3\10_Statements.html#statement) [*多條語句(Statements)*](..\chapter3\10_Statements.html#statements) _可選_  

<!-- -->

> 迴圈語句語法  
> *迴圈語句* → [*for語句*](..\chapter3\10_Statements.html#for_statement)  
> *迴圈語句* → [*for-in語句*](..\chapter3\10_Statements.html#for_in_statement)  
> *迴圈語句* → [*while語句*](..\chapter3\10_Statements.html#wheetatype型別ile_statement)  
> *迴圈語句* → [*do-while語句*](..\chapter3\10_Statements.html#do_while_statement)  

<!-- -->

> For 迴圈語法  
> *for語句* → **for** [*for初始條件*](..\chapter3\10_Statements.html#for_init) _可選_ **;** [*表達式*](..\chapter3\04_Expressions.html#expression) _可選_ **;** [*表達式*](..\chapter3\04_Expressions.html#expression) _可選_ [*程式碼區塊*](..\chapter3\05_Declarations.html#code_block)  
> *for語句* → **for** **(** [*for初始條件*](..\chapter3\10_Statements.html#for_init) _可選_ **;** [*表達式*](..\chapter3\04_Expressions.html#expression) _可選_ **;** [*表達式*](..\chapter3\04_Expressions.html#expression) _可選_ **)** [*程式碼區塊*](..\chapter3\05_Declarations.html#code_block)  
> *for初始條件* → [*變數宣告*](..\chapter3\05_Declarations.html#variable_declaration) | [*表達式列表*](..\chapter3\04_Expressions.html#expression_list)  

<!-- -->

> For-In 迴圈語法  
> *for-in語句* → **for** [*模式*](..\chapter3\07_Patterns.html#pattern) **in** [*表達式*](..\chapter3\04_Expressions.html#expression) [*程式碼區塊*](..\chapter3\05_Declarations.html#code_block)  

<!-- -->

> While 迴圈語法  
> *while語句* → **while** [*while條件*](..\chapter3\10_Statements.html#while_condition) [*程式碼區塊*](..\chapter3\05_Declarations.html#code_block)  
> *while條件* → [*表達式*](..\chapter3\04_Expressions.html#expression) | [*宣告*](..\chapter3\05_Declarations.html#declaration)  

<!-- -->

> Do-While 迴圈語法  
> *do-while語句* → **do** [*程式碼區塊*](..\chapter3\05_Declarations.html#code_block) **while** [*while條件*](..\chapter3\10_Statements.html#while_condition)  

<!-- -->

> 分支語句語法  
> *分支語句* → [*if語句*](..\chapter3\10_Statements.html#if_statement)  
> *分支語句* → [*switch語句*](..\chapter3\10_Statements.html#switch_statement)  

<!-- -->

> If語句語法  
> *if語句* → **if** [*if條件*](..\chapter3\10_Statements.html#if_condition) [*程式碼區塊*](..\chapter3\05_Declarations.html#code_block) [*else子句(Clause)*](..\chapter3\10_Statements.html#else_clause) _可選_  
> *if條件* → [*表達式*](..\chapter3\04_Expressions.html#expression) | [*宣告*](..\chapter3\05_Declarations.html#declaration)  
> *else子句(Clause)* → **else** [*程式碼區塊*](..\chapter3\05_Declarations.html#code_block) | **else** [*if語句*](..\chapter3\10_Statements.html#if_statement)  

<!-- -->

> Switch語句語法  
> *switch語句* → **switch** [*表達式*](..\chapter3\04_Expressions.html#expression) **{** [*SwitchCase列表*](..\chapter3\10_Statements.html#switch_cases) _可選_ **}**  
> *SwitchCase列表* → [*SwitchCase*](..\chapter3\10_Statements.html#switch_case) [*SwitchCase列表*](..\chapter3\10_Statements.html#switch_cases) _可選_  
> *SwitchCase* → [*case標簽*](..\chapter3\10_Statements.html#case_label) [*多條語句(Statements)*](..\chapter3\10_Statements.html#statements) | [*default標簽*](..\chapter3\10_Statements.html#default_label) [*多條語句(Statements)*](..\chapter3\10_Statements.html#statements)  
> *SwitchCase* → [*case標簽*](..\chapter3\10_Statements.html#case_label) **;** | [*default標簽*](..\chapter3\10_Statements.html#default_label) **;**  
> *case標簽* → **case** [*case項列表*](..\chapter3\10_Statements.html#case_item_list) **:**  
> *case項列表* → [*模式*](..\chapter3\07_Patterns.html#pattern) [*guard-clause*](..\chapter3\10_Statements.html#guard_clause) _可選_ | [*模式*](..\chapter3\07_Patterns.html#pattern) [*guard-clause*](..\chapter3\10_Statements.html#guard_clause) _可選_ **,** [*case項列表*](..\chapter3\10_Statements.html#case_item_list)  
> *default標簽* → **default** **:**  
> *guard-clause* → **where** [*guard-expression*](..\chapter3\10_Statements.html#guard_expression)  
> *guard-expression* → [*表達式*](..\chapter3\04_Expressions.html#expression)  

<!-- -->

> 標記語句語法  
> *標記語句(Labeled Statement)* → [*語句標簽*](..\chapter3\10_Statements.html#statement_label) [*迴圈語句*](..\chapter3\10_Statements.html#loop_statement) | [*語句標簽*](..\chapter3\10_Statements.html#statement_label) [*switch語句*](..\chapter3\10_Statements.html#switch_statement)  
> *語句標簽* → [*標簽名稱*](..\chapter3\10_Statements.html#label_name) **:**  
> *標簽名稱* → [*識別符號*](..\chapter3\02_Lexical_Structure.html#identifier)  

<!-- -->

> 控制傳遞語句(Control Transfer Statement) 語法  
> *控制傳遞語句* → [*break語句*](..\chapter3\10_Statements.html#break_statement)  
> *控制傳遞語句* → [*continue語句*](..\chapter3\10_Statements.html#continue_statement)  
> *控制傳遞語句* → [*fallthrough語句*](..\chapter3\10_Statements.html#fallthrough_statement)  
> *控制傳遞語句* → [*return語句*](..\chapter3\10_Statements.html#return_statement)  

<!-- -->

> Break 語句語法  
> *break語句* → **break** [*標簽名稱*](..\chapter3\10_Statements.html#label_name) _可選_  

<!-- -->

> Continue 語句語法  
> *continue語句* → **continue** [*標簽名稱*](..\chapter3\10_Statements.html#label_name) _可選_  

<!-- -->

> Fallthrough 語句語法  
> *fallthrough語句* → **fallthrough**  

<!-- -->

> Return 語句語法  
> *return語句* → **return** [*表達式*](..\chapter3\04_Expressions.html#expression) _可選_  

<a name="generic_parameters_and_arguments"></a>
## 泛型參數

> 泛型形參子句(Generic Parameter Clause) 語法  
> *泛型參數子句* → **<** [*泛型參數列表*](GenericParametersAndArguments.html#generic_parameter_list) [*約束子句*](GenericParametersAndArguments.html#requirement_clause) _可選_ **>**  
> *泛型參數列表* → [*泛形參數*](GenericParametersAndArguments.html#generic_parameter) | [*泛形參數*](GenericParametersAndArguments.html#generic_parameter) **,** [*泛型參數列表*](GenericParametersAndArguments.html#generic_parameter_list)  
> *泛形參數* → [*型別名稱*](..\chapter3\03_Types.html#type_name)  
> *泛形參數* → [*型別名稱*](..\chapter3\03_Types.html#type_name) **:** [*型別標識*](..\chapter3\03_Types.html#type_identifier)  
> *泛形參數* → [*型別名稱*](..\chapter3\03_Types.html#type_name) **:** [*協定合成型別*](..\chapter3\03_Types.html#protocol_composition_type)  
> *約束子句* → **where** [*約束列表*](GenericParametersAndArguments.html#requirement_list)  
> *約束列表* → [*約束*](GenericParametersAndArguments.html#requirement) | [*約束*](GenericParametersAndArguments.html#requirement) **,** [*約束列表*](GenericParametersAndArguments.html#requirement_list)  
> *約束* → [*一致性約束*](GenericParametersAndArguments.html#conformance_requirement) | [*同型別約束*](GenericParametersAndArguments.html#same_type_requirement)  
> *一致性約束* → [*型別標識*](..\chapter3\03_Types.html#type_identifier) **:** [*型別標識*](..\chapter3\03_Types.html#type_identifier)  
> *一致性約束* → [*型別標識*](..\chapter3\03_Types.html#type_identifier) **:** [*協定合成型別*](..\chapter3\03_Types.html#protocol_composition_type)  
> *同型別約束* → [*型別標識*](..\chapter3\03_Types.html#type_identifier) **==** [*型別標識*](..\chapter3\03_Types.html#type_identifier)  

<!-- -->

> 泛型實參子句語法  
> *(泛型參數子句Generic Argument Clause)* → **<** [*泛型參數列表*](GenericParametersAndArguments.html#generic_argument_list) **>**  
> *泛型參數列表* → [*泛型參數*](GenericParametersAndArguments.html#generic_argument) | [*泛型參數*](GenericParametersAndArguments.html#generic_argument) **,** [*泛型參數列表*](GenericParametersAndArguments.html#generic_argument_list)  
> *泛型參數* → [*型別*](..\chapter3\03_Types.html#type)  

<a name="declarations"></a>
## 宣告 (Declarations) 

> 宣告語法  
> *宣告* → [*導入宣告*](..\chapter3\05_Declarations.html#import_declaration)  
> *宣告* → [*常數宣告*](..\chapter3\05_Declarations.html#constant_declaration)  
> *宣告* → [*變數宣告*](..\chapter3\05_Declarations.html#variable_declaration)  
> *宣告* → [*型別別名宣告*](..\chapter3\05_Declarations.html#typealias_declaration)  
> *宣告* → [*函式宣告*](..\chapter3\05_Declarations.html#function_declaration)  
> *宣告* → [*列舉宣告*](..\chapter3\05_Declarations.html#enum_declaration)  
> *宣告* → [*結構宣告*](..\chapter3\05_Declarations.html#struct_declaration)  
> *宣告* → [*類別宣告*](..\chapter3\05_Declarations.html#class_declaration)  
> *宣告* → [*協定宣告*](..\chapter3\05_Declarations.html#protocol_declaration)  
> *宣告* → [*建構器宣告*](..\chapter3\05_Declarations.html#initializer_declaration)  
> *宣告* → [*析構器宣告*](..\chapter3\05_Declarations.html#deinitializer_declaration)  
> *宣告* → [*擴展宣告*](..\chapter3\05_Declarations.html#extension_declaration)  
> *宣告* → [*下標腳本宣告*](..\chapter3\05_Declarations.html#subscript_declaration)  
> *宣告* → [*運算子宣告*](..\chapter3\05_Declarations.html#operator_declaration)  
> *宣告(Declarations)列表* → [*宣告*](..\chapter3\05_Declarations.html#declaration) [*宣告(Declarations)列表*](..\chapter3\05_Declarations.html#declarations) _可選_  
> *宣告描述符(Specifiers)列表* → [*宣告描述符(Specifier)*](..\chapter3\05_Declarations.html#declaration_specifier) [*宣告描述符(Specifiers)列表*](..\chapter3\05_Declarations.html#declaration_specifiers) _可選_  
> *宣告描述符(Specifier)* → **class** | **mutating** | **nonmutating** | **override** | **static** | **unowned** | **unowned(safe)** | **unowned(unsafe)** | **weak**  

<!-- -->

> 頂級(Top Level) 宣告語法  
> *頂級宣告* → [*多條語句(Statements)*](..\chapter3\10_Statements.html#statements) _可選_  

<!-- -->

> 程式碼區塊語法  
> *程式碼區塊* → **{** [*多條語句(Statements)*](..\chapter3\10_Statements.html#statements) _可選_ **}**  

<!-- -->

> 導入(Import)宣告語法  
> *導入宣告* → [*特性(Attributes)列表*](..\chapter3\06_Attributes.html#attributes) _可選_ **import** [*導入型別*](..\chapter3\05_Declarations.html#import_kind) _可選_ [*導入路徑*](..\chapter3\05_Declarations.html#import_path)  
> *導入型別* → **typealias** | **struct** | **class** | **enum** | **protocol** | **var** | **func**  
> *導入路徑* → [*導入路徑識別符號*](..\chapter3\05_Declarations.html#import_path_identifier) | [*導入路徑識別符號*](..\chapter3\05_Declarations.html#import_path_identifier) **.** [*導入路徑*](..\chapter3\05_Declarations.html#import_path)  
> *導入路徑識別符號* → [*識別符號*](..\chapter3\02_Lexical_Structure.html#identifier) | [*運算子*](..\chapter3\02_Lexical_Structure.html#operator)  

<!-- -->

> 常數宣告語法  
> *常數宣告* → [*特性(Attributes)列表*](..\chapter3\06_Attributes.html#attributes) _可選_ [*宣告描述符(Specifiers)列表*](..\chapter3\05_Declarations.html#declaration_specifiers) _可選_ **let** [*模式建構器列表*](..\chapter3\05_Declarations.html#pattern_initializer_list)  
> *模式建構器列表* → [*模式建構器*](..\chapter3\05_Declarations.html#pattern_initializer) | [*模式建構器*](..\chapter3\05_Declarations.html#pattern_initializer) **,** [*模式建構器列表*](..\chapter3\05_Declarations.html#pattern_initializer_list)  
> *模式建構器* → [*模式*](..\chapter3\07_Patterns.html#pattern) [*建構器*](..\chapter3\05_Declarations.html#initializer) _可選_  
> *建構器* → **=** [*表達式*](..\chapter3\04_Expressions.html#expression)  

<!-- -->

> 變數宣告語法  
> *變數宣告* → [*變數宣告頭(Head)*](..\chapter3\05_Declarations.html#variable_declaration_head) [*模式建構器列表*](..\chapter3\05_Declarations.html#pattern_initializer_list)  
> *變數宣告* → [*變數宣告頭(Head)*](..\chapter3\05_Declarations.html#variable_declaration_head) [*變數名*](..\chapter3\05_Declarations.html#variable_name) [*型別注解*](..\chapter3\03_Types.html#type_annotation) [*程式碼區塊*](..\chapter3\05_Declarations.html#code_block)  
> *變數宣告* → [*變數宣告頭(Head)*](..\chapter3\05_Declarations.html#variable_declaration_head) [*變數名*](..\chapter3\05_Declarations.html#variable_name) [*型別注解*](..\chapter3\03_Types.html#type_annotation) [*getter-setter塊*](..\chapter3\05_Declarations.html#getter_setter_block)  
> *變數宣告* → [*變數宣告頭(Head)*](..\chapter3\05_Declarations.html#variable_declaration_head) [*變數名*](..\chapter3\05_Declarations.html#variable_name) [*型別注解*](..\chapter3\03_Types.html#type_annotation) [*getter-setter關鍵字(Keyword)塊*](..\chapter3\05_Declarations.html#getter_setter_keyword_block)  
> *變數宣告* → [*變數宣告頭(Head)*](..\chapter3\05_Declarations.html#variable_declaration_head) [*變數名*](..\chapter3\05_Declarations.html#variable_name) [*型別注解*](..\chapter3\03_Types.html#type_annotation) [*建構器*](..\chapter3\05_Declarations.html#initializer) _可選_ [*willSet-didSet程式碼區塊*](..\chapter3\05_Declarations.html#willSet_didSet_block)  
> *變數宣告頭(Head)* → [*特性(Attributes)列表*](..\chapter3\06_Attributes.html#attributes) _可選_ [*宣告描述符(Specifiers)列表*](..\chapter3\05_Declarations.html#declaration_specifiers) _可選_ **var**  
> *變數名稱* → [*識別符號*](..\chapter3\02_Lexical_Structure.html#identifier)  
> *getter-setter塊* → **{** [*getter子句*](..\chapter3\05_Declarations.html#getter_clause) [*setter子句*](..\chapter3\05_Declarations.html#setter_clause) _可選_ **}**  
> *getter-setter塊* → **{** [*setter子句*](..\chapter3\05_Declarations.html#setter_clause) [*getter子句*](..\chapter3\05_Declarations.html#getter_clause) **}**  
> *getter子句* → [*特性(Attributes)列表*](..\chapter3\06_Attributes.html#attributes) _可選_ **get** [*程式碼區塊*](..\chapter3\05_Declarations.html#code_block)  
> *setter子句* → [*特性(Attributes)列表*](..\chapter3\06_Attributes.html#attributes) _可選_ **set** [*setter名稱*](..\chapter3\05_Declarations.html#setter_name) _可選_ [*程式碼區塊*](..\chapter3\05_Declarations.html#code_block)  
> *setter名稱* → **(** [*識別符號*](..\chapter3\02_Lexical_Structure.html#identifier) **)**  
> *getter-setter關鍵字(Keyword)塊* → **{** [*getter關鍵字(Keyword)子句*](..\chapter3\05_Declarations.html#getter_keyword_clause) [*setter關鍵字(Keyword)子句*](..\chapter3\05_Declarations.html#setter_keyword_clause) _可選_ **}**  
> *getter-setter關鍵字(Keyword)塊* → **{** [*setter關鍵字(Keyword)子句*](..\chapter3\05_Declarations.html#setter_keyword_clause) [*getter關鍵字(Keyword)子句*](..\chapter3\05_Declarations.html#getter_keyword_clause) **}**  
> *getter關鍵字(Keyword)子句* → [*特性(Attributes)列表*](..\chapter3\06_Attributes.html#attributes) _可選_ **get**  
> *setter關鍵字(Keyword)子句* → [*特性(Attributes)列表*](..\chapter3\06_Attributes.html#attributes) _可選_ **set**  
> *willSet-didSet程式碼區塊* → **{** [*willSet子句*](..\chapter3\05_Declarations.html#willSet_clause) [*didSet子句*](..\chapter3\05_Declarations.html#didSet_clause) _可選_ **}**  
> *willSet-didSet程式碼區塊* → **{** [*didSet子句*](..\chapter3\05_Declarations.html#didSet_clause) [*willSet子句*](..\chapter3\05_Declarations.html#willSet_clause) **}**  
> *willSet子句* → [*特性(Attributes)列表*](..\chapter3\06_Attributes.html#attributes) _可選_ **willSet** [*setter名稱*](..\chapter3\05_Declarations.html#setter_name) _可選_ [*程式碼區塊*](..\chapter3\05_Declarations.html#code_block)  
> *didSet子句* → [*特性(Attributes)列表*](..\chapter3\06_Attributes.html#attributes) _可選_ **didSet** [*setter名稱*](..\chapter3\05_Declarations.html#setter_name) _可選_ [*程式碼區塊*](..\chapter3\05_Declarations.html#code_block)  

<!-- -->

> 型別別名宣告語法  
> *型別別名宣告* → [*型別別名頭(Head)*](..\chapter3\05_Declarations.html#typealias_head) [*型別別名賦值*](..\chapter3\05_Declarations.html#typealias_assignment)  
> *型別別名頭(Head)* → **typealias** [*型別別名名稱*](..\chapter3\05_Declarations.html#typealias_name)  
> *型別別名名稱* → [*識別符號*](..\chapter3\02_Lexical_Structure.html#identifier)  
> *型別別名賦值* → **=** [*型別*](..\chapter3\03_Types.html#type)  

<!-- -->

> 函式宣告語法  
> *函式宣告* → [*函式頭*](..\chapter3\05_Declarations.html#function_head) [*函式名*](..\chapter3\05_Declarations.html#function_name) [*泛型參數子句*](GenericParametersAndArguments.html#generic_parameter_clause) _可選_ [*函式簽名(Signature)*](..\chapter3\05_Declarations.html#function_signature) [*函式體*](..\chapter3\05_Declarations.html#function_body)  
> *函式頭* → [*特性(Attributes)列表*](..\chapter3\06_Attributes.html#attributes) _可選_ [*宣告描述符(Specifiers)列表*](..\chapter3\05_Declarations.html#declaration_specifiers) _可選_ **func**  
> *函式名* → [*識別符號*](..\chapter3\02_Lexical_Structure.html#identifier) | [*運算子*](..\chapter3\02_Lexical_Structure.html#operator)  
> *函式簽名(Signature)* → [*parameter-clauses*](..\chapter3\05_Declarations.html#parameter_clauses) [*函式結果*](..\chapter3\05_Declarations.html#function_result) _可選_  
> *函式結果* → **->** [*特性(Attributes)列表*](..\chapter3\06_Attributes.html#attributes) _可選_ [*型別*](..\chapter3\03_Types.html#type)  
> *函式體* → [*程式碼區塊*](..\chapter3\05_Declarations.html#code_block)  
> *parameter-clauses* → [*參數子句*](..\chapter3\05_Declarations.html#parameter_clause) [*parameter-clauses*](..\chapter3\05_Declarations.html#parameter_clauses) _可選_  
> *參數子句* → **(** **)** | **(** [*參數列表*](..\chapter3\05_Declarations.html#parameter_list) **...** _可選_ **)**  
> *參數列表* → [*參數*](..\chapter3\05_Declarations.html#parameter) | [*參數*](..\chapter3\05_Declarations.html#parameter) **,** [*參數列表*](..\chapter3\05_Declarations.html#parameter_list)  
> *參數* → **inout** _可選_ **let** _可選_ **#** _可選_ [*參數名*](..\chapter3\05_Declarations.html#parameter_name) [*本地參數名*](..\chapter3\05_Declarations.html#local_parameter_name) _可選_ [*型別注解*](..\chapter3\03_Types.html#type_annotation) [*預設參數子句*](..\chapter3\05_Declarations.html#default_argument_clause) _可選_  
> *參數* → **inout** _可選_ **var** **#** _可選_ [*參數名*](..\chapter3\05_Declarations.html#parameter_name) [*本地參數名*](..\chapter3\05_Declarations.html#local_parameter_name) _可選_ [*型別注解*](..\chapter3\03_Types.html#type_annotation) [*預設參數子句*](..\chapter3\05_Declarations.html#default_argument_clause) _可選_  
> *參數* → [*特性(Attributes)列表*](..\chapter3\06_Attributes.html#attributes) _可選_ [*型別*](..\chapter3\03_Types.html#type)  
> *參數名* → [*識別符號*](..\chapter3\02_Lexical_Structure.html#identifier) | **_**  
> *本地參數名* → [*識別符號*](..\chapter3\02_Lexical_Structure.html#identifier) | **_**  
> *預設參數子句* → **=** [*表達式*](..\chapter3\04_Expressions.html#expression)  

<!-- -->

> 列舉宣告語法  
> *列舉宣告* → [*特性(Attributes)列表*](..\chapter3\06_Attributes.html#attributes) _可選_ [*聯合式列舉*](..\chapter3\05_Declarations.html#union_style_enum) | [*特性(Attributes)列表*](..\chapter3\06_Attributes.html#attributes) _可選_ [*原始值式列舉*](..\chapter3\05_Declarations.html#raw_value_style_enum)  
> *聯合式列舉* → [*列舉名*](..\chapter3\05_Declarations.html#enum_name) [*泛型參數子句*](GenericParametersAndArguments.html#generic_parameter_clause) _可選_ **{** [*union-style-enum-members*](..\chapter3\05_Declarations.html#union_style_enum_members) _可選_ **}**  
> *union-style-enum-members* → [*union-style-enum-member*](..\chapter3\05_Declarations.html#union_style_enum_member) [*union-style-enum-members*](..\chapter3\05_Declarations.html#union_style_enum_members) _可選_  
> *union-style-enum-member* → [*宣告*](..\chapter3\05_Declarations.html#declaration) | [*聯合式(Union Style)的列舉case子句*](..\chapter3\05_Declarations.html#union_style_enum_case_clause)  
> *聯合式(Union Style)的列舉case子句* → [*特性(Attributes)列表*](..\chapter3\06_Attributes.html#attributes) _可選_ **case** [*聯合式(Union Style)的列舉case列表*](..\chapter3\05_Declarations.html#union_style_enum_case_list)  
> *聯合式(Union Style)的列舉case列表* → [*聯合式(Union Style)的case*](..\chapter3\05_Declarations.html#union_style_enum_case) | [*聯合式(Union Style)的case*](..\chapter3\05_Declarations.html#union_style_enum_case) **,** [*聯合式(Union Style)的列舉case列表*](..\chapter3\05_Declarations.html#union_style_enum_case_list)  
> *聯合式(Union Style)的case* → [*列舉的case名*](..\chapter3\05_Declarations.html#enum_case_name) [*元組型別*](..\chapter3\03_Types.html#tuple_type) _可選_  
> *列舉名* → [*識別符號*](..\chapter3\02_Lexical_Structure.html#identifier)  
> *列舉的case名* → [*識別符號*](..\chapter3\02_Lexical_Structure.html#identifier)  
> *原始值式列舉* → [*列舉名*](..\chapter3\05_Declarations.html#enum_name) [*泛型參數子句*](GenericParametersAndArguments.html#generic_parameter_clause) _可選_ **:** [*型別標識*](..\chapter3\03_Types.html#type_identifier) **{** [*原始值式列舉成員列表*](..\chapter3\05_Declarations.html#raw_value_style_enum_members) _可選_ **}**  
> *原始值式列舉成員列表* → [*原始值式列舉成員*](..\chapter3\05_Declarations.html#raw_value_style_enum_member) [*原始值式列舉成員列表*](..\chapter3\05_Declarations.html#raw_value_style_enum_members) _可選_  
> *原始值式列舉成員* → [*宣告*](..\chapter3\05_Declarations.html#declaration) | [*原始值式列舉case子句*](..\chapter3\05_Declarations.html#raw_value_style_enum_case_clause)  
> *原始值式列舉case子句* → [*特性(Attributes)列表*](..\chapter3\06_Attributes.html#attributes) _可選_ **case** [*原始值式列舉case列表*](..\chapter3\05_Declarations.html#raw_value_style_enum_case_list)  
> *原始值式列舉case列表* → [*原始值式列舉case*](..\chapter3\05_Declarations.html#raw_value_style_enum_case) | [*原始值式列舉case*](..\chapter3\05_Declarations.html#raw_value_style_enum_case) **,** [*原始值式列舉case列表*](..\chapter3\05_Declarations.html#raw_value_style_enum_case_list)  
> *原始值式列舉case* → [*列舉的case名*](..\chapter3\05_Declarations.html#enum_case_name) [*原始值賦值*](..\chapter3\05_Declarations.html#raw_value_assignment) _可選_  
> *原始值賦值* → **=** [*字面量*](..\chapter3\02_Lexical_Structure.html#literal)  

<!-- -->

> 結構宣告語法  
> *結構宣告* → [*特性(Attributes)列表*](..\chapter3\06_Attributes.html#attributes) _可選_ **struct** [*結構名稱*](..\chapter3\05_Declarations.html#struct_name) [*泛型參數子句*](GenericParametersAndArguments.html#generic_parameter_clause) _可選_ [*型別繼承子句*](..\chapter3\03_Types.html#type_inheritance_clause) _可選_ [*結構主體*](..\chapter3\05_Declarations.html#struct_body)  
> *結構名稱* → [*識別符號*](..\chapter3\02_Lexical_Structure.html#identifier)  
> *結構主體* → **{** [*宣告(Declarations)列表*](..\chapter3\05_Declarations.html#declarations) _可選_ **}**  

<!-- -->

> 類別宣告語法  
> *類別宣告* → [*特性(Attributes)列表*](..\chapter3\06_Attributes.html#attributes) _可選_ **class** [*類別名*](..\chapter3\05_Declarations.html#class_name) [*泛型參數子句*](GenericParametersAndArguments.html#generic_parameter_clause) _可選_ [*型別繼承子句*](..\chapter3\03_Types.html#type_inheritance_clause) _可選_ [*類別主體*](..\chapter3\05_Declarations.html#class_body)  
> *類別名* → [*識別符號*](..\chapter3\02_Lexical_Structure.html#identifier)  
> *類別主體* → **{** [*宣告(Declarations)列表*](..\chapter3\05_Declarations.html#declarations) _可選_ **}**  

<!-- -->

> 協定(Protocol)宣告語法  
> *協定宣告* → [*特性(Attributes)列表*](..\chapter3\06_Attributes.html#attributes) _可選_ **protocol** [*協定名*](..\chapter3\05_Declarations.html#protocol_name) [*型別繼承子句*](..\chapter3\03_Types.html#type_inheritance_clause) _可選_ [*協定主體*](..\chapter3\05_Declarations.html#protocol_body)  
> *協定名* → [*識別符號*](..\chapter3\02_Lexical_Structure.html#identifier)  
> *協定主體* → **{** [*協定成員宣告(Declarations)列表*](..\chapter3\05_Declarations.html#protocol_member_declarations) _可選_ **}**  
> *協定成員宣告* → [*協定屬性宣告*](..\chapter3\05_Declarations.html#protocol_property_declaration)  
> *協定成員宣告* → [*協定方法宣告*](..\chapter3\05_Declarations.html#protocol_method_declaration)  
> *協定成員宣告* → [*協定建構器宣告*](..\chapter3\05_Declarations.html#protocol_initializer_declaration)  
> *協定成員宣告* → [*協定下標腳本宣告*](..\chapter3\05_Declarations.html#protocol_subscript_declaration)  
> *協定成員宣告* → [*協定關聯型別宣告*](..\chapter3\05_Declarations.html#protocol_associated_type_declaration)  
> *協定成員宣告(Declarations)列表* → [*協定成員宣告*](..\chapter3\05_Declarations.html#protocol_member_declaration) [*協定成員宣告(Declarations)列表*](..\chapter3\05_Declarations.html#protocol_member_declarations) _可選_  

<!-- -->

> 協定屬性宣告語法  
> *協定屬性宣告* → [*變數宣告頭(Head)*](..\chapter3\05_Declarations.html#variable_declaration_head) [*變數名*](..\chapter3\05_Declarations.html#variable_name) [*型別注解*](..\chapter3\03_Types.html#type_annotation) [*getter-setter關鍵字(Keyword)塊*](..\chapter3\05_Declarations.html#getter_setter_keyword_block)  

<!-- -->

> 協定方法宣告語法  
> *協定方法宣告* → [*函式頭*](..\chapter3\05_Declarations.html#function_head) [*函式名*](..\chapter3\05_Declarations.html#function_name) [*泛型參數子句*](GenericParametersAndArguments.html#generic_parameter_clause) _可選_ [*函式簽名(Signature)*](..\chapter3\05_Declarations.html#function_signature)  

<!-- -->

> 協定建構器宣告語法  
> *協定建構器宣告* → [*建構器頭(Head)*](..\chapter3\05_Declarations.html#initializer_head) [*泛型參數子句*](GenericParametersAndArguments.html#generic_parameter_clause) _可選_ [*參數子句*](..\chapter3\05_Declarations.html#parameter_clause)  

<!-- -->

> 協定下標腳本宣告語法  
> *協定下標腳本宣告* → [*下標腳本頭(Head)*](..\chapter3\05_Declarations.html#subscript_head) [*下標腳本結果(Result)*](..\chapter3\05_Declarations.html#subscript_result) [*getter-setter關鍵字(Keyword)塊*](..\chapter3\05_Declarations.html#getter_setter_keyword_block)  

<!-- -->

> 協定關聯型別宣告語法  
> *協定關聯型別宣告* → [*型別別名頭(Head)*](..\chapter3\05_Declarations.html#typealias_head) [*型別繼承子句*](..\chapter3\03_Types.html#type_inheritance_clause) _可選_ [*型別別名賦值*](..\chapter3\05_Declarations.html#typealias_assignment) _可選_  

<!-- -->

> 建構器宣告語法  
> *建構器宣告* → [*建構器頭(Head)*](..\chapter3\05_Declarations.html#initializer_head) [*泛型參數子句*](GenericParametersAndArguments.html#generic_parameter_clause) _可選_ [*參數子句*](..\chapter3\05_Declarations.html#parameter_clause) [*建構器主體*](..\chapter3\05_Declarations.html#initializer_body)  
> *建構器頭(Head)* → [*特性(Attributes)列表*](..\chapter3\06_Attributes.html#attributes) _可選_ **convenience** _可選_ **init**  
> *建構器主體* → [*程式碼區塊*](..\chapter3\05_Declarations.html#code_block)  

<!-- -->

> 析構器宣告語法  
> *析構器宣告* → [*特性(Attributes)列表*](..\chapter3\06_Attributes.html#attributes) _可選_ **deinit** [*程式碼區塊*](..\chapter3\05_Declarations.html#code_block)  

<!-- -->

> 擴展(Extension)宣告語法  
> *擴展宣告* → **extension** [*型別標識*](..\chapter3\03_Types.html#type_identifier) [*型別繼承子句*](..\chapter3\03_Types.html#type_inheritance_clause) _可選_ [*extension-body*](..\chapter3\05_Declarations.html#extension_body)  
> *extension-body* → **{** [*宣告(Declarations)列表*](..\chapter3\05_Declarations.html#declarations) _可選_ **}**  

<!-- -->

> 下標腳本宣告語法  
> *下標腳本宣告* → [*下標腳本頭(Head)*](..\chapter3\05_Declarations.html#subscript_head) [*下標腳本結果(Result)*](..\chapter3\05_Declarations.html#subscript_result) [*程式碼區塊*](..\chapter3\05_Declarations.html#code_block)  
> *下標腳本宣告* → [*下標腳本頭(Head)*](..\chapter3\05_Declarations.html#subscript_head) [*下標腳本結果(Result)*](..\chapter3\05_Declarations.html#subscript_result) [*getter-setter塊*](..\chapter3\05_Declarations.html#getter_setter_block)  
> *下標腳本宣告* → [*下標腳本頭(Head)*](..\chapter3\05_Declarations.html#subscript_head) [*下標腳本結果(Result)*](..\chapter3\05_Declarations.html#subscript_result) [*getter-setter關鍵字(Keyword)塊*](..\chapter3\05_Declarations.html#getter_setter_keyword_block)  
> *下標腳本頭(Head)* → [*特性(Attributes)列表*](..\chapter3\06_Attributes.html#attributes) _可選_ **subscript** [*參數子句*](..\chapter3\05_Declarations.html#parameter_clause)  
> *下標腳本結果(Result)* → **->** [*特性(Attributes)列表*](..\chapter3\06_Attributes.html#attributes) _可選_ [*型別*](..\chapter3\03_Types.html#type)  

<!-- -->

> 運算子宣告語法  
> *運算子宣告* → [*前綴運算子宣告*](..\chapter3\05_Declarations.html#prefix_operator_declaration) | [*後綴運算子宣告*](..\chapter3\05_Declarations.html#postfix_operator_declaration) | [*中綴運算子宣告*](..\chapter3\05_Declarations.html#infix_operator_declaration)  
> *前綴運算子宣告* → **運算子** **prefix** [*運算子*](..\chapter3\02_Lexical_Structure.html#operator) **{** **}**  
> *後綴運算子宣告* → **運算子** **postfix** [*運算子*](..\chapter3\02_Lexical_Structure.html#operator) **{** **}**  
> *中綴運算子宣告* → **運算子** **infix** [*運算子*](..\chapter3\02_Lexical_Structure.html#operator) **{** [*中綴運算子屬性*](..\chapter3\05_Declarations.html#infix_operator_attributes) _可選_ **}**  
> *中綴運算子屬性* → [*優先級子句*](..\chapter3\05_Declarations.html#precedence_clause) _可選_ [*結和性子句*](..\chapter3\05_Declarations.html#associativity_clause) _可選_  
> *優先級子句* → **precedence** [*優先級水平*](..\chapter3\05_Declarations.html#precedence_level)  
> *優先級水平* → 數值 0 到 255  
> *結和性子句* → **associativity** [*結和性*](..\chapter3\05_Declarations.html#associativity)  
> *結和性* → **left** | **right** | **none**  

<a name="patterns"></a>
## 模式

> 模式(Patterns) 語法  
> *模式* → [*通配符模式*](..\chapter3\07_Patterns.html#wildcard_pattern) [*型別注解*](..\chapter3\03_Types.html#type_annotation) _可選_  
> *模式* → [*識別符號模式*](..\chapter3\07_Patterns.html#identifier_pattern) [*型別注解*](..\chapter3\03_Types.html#type_annotati(Value Binding)on) _可選_  
> *模式* → [*值綁定模式*](..\chapter3\07_Patterns.html#value_binding_pattern)  
> *模式* → [*元組模式*](..\chapter3\07_Patterns.html#tuple_pattern) [*型別注解*](..\chapter3\03_Types.html#type_annotation) _可選_  
> *模式* → [*enum-case-pattern*](..\chapter3\07_Patterns.html#enum_case_pattern)  
> *模式* → [*type-casting-pattern*](..\chapter3\07_Patterns.html#type_casting_pattern)  
> *模式* → [*表達式模式*](..\chapter3\07_Patterns.html#expression_pattern)  

<!-- -->

> 通配符模式語法  
> *通配符模式* → **_**  

<!-- -->

> 識別符號模式語法  
> *識別符號模式* → [*識別符號*](..\chapter3\02_Lexical_Structure.html#identifier)  

<!-- -->

> 值綁定(Value Binding)模式語法  
> *值綁定模式* → **var** [*模式*](..\chapter3\07_Patterns.html#pattern) | **let** [*模式*](..\chapter3\07_Patterns.html#pattern)  

<!-- -->

> 元組模式語法  
> *元組模式* → **(** [*元組模式元素列表*](..\chapter3\07_Patterns.html#tuple_pattern_element_list) _可選_ **)**  
> *元組模式元素列表* → [*元組模式元素*](..\chapter3\07_Patterns.html#tuple_pattern_element) | [*元組模式元素*](..\chapter3\07_Patterns.html#tuple_pattern_element) **,** [*元組模式元素列表*](..\chapter3\07_Patterns.html#tuple_pattern_element_list)  
> *元組模式元素* → [*模式*](..\chapter3\07_Patterns.html#pattern)  

<!-- -->

> 列舉用例模式語法  
> *enum-case-pattern* → [*型別標識*](..\chapter3\03_Types.html#type_identifier) _可選_ **.** [*列舉的case名*](..\chapter3\05_Declarations.html#enum_case_name) [*元組模式*](..\chapter3\07_Patterns.html#tuple_pattern) _可選_  

<!-- -->

> 型別轉換模式語法  
> *type-casting-pattern* → [*is模式*](..\chapter3\07_Patterns.html#is_pattern) | [*as模式*](..\chapter3\07_Patterns.html#as_pattern)  
> *is模式* → **is** [*型別*](..\chapter3\03_Types.html#type)  
> *as模式* → [*模式*](..\chapter3\07_Patterns.html#pattern) **as** [*型別*](..\chapter3\03_Types.html#type)  

<!-- -->

> 表達式模式語法  
> *表達式模式* → [*表達式*](..\chapter3\04_Expressions.html#expression)  

<a name="attributes"></a>
## 特性

> 特性語法  
> *特色* → **@** [*特性名*](..\chapter3\06_Attributes.html#attribute_name) [*特性參數子句*](..\chapter3\06_Attributes.html#attribute_argument_clause) _可選_  
> *特性名* → [*識別符號*](..\chapter3\02_Lexical_Structure.html#identifier)  
> *特性參數子句* → **(** [*平衡語彙單元列表*](..\chapter3\06_Attributes.html#balanced_tokens) _可選_ **)**  
> *特性(Attributes)列表* → [*特色*](..\chapter3\06_Attributes.html#attribute) [*特性(Attributes)列表*](..\chapter3\06_Attributes.html#attributes) _可選_  
> *平衡語彙單元列表* → [*平衡語彙單元*](..\chapter3\06_Attributes.html#balanced_token) [*平衡語彙單元列表*](..\chapter3\06_Attributes.html#balanced_tokens) _可選_  
> *平衡語彙單元* → **(** [*平衡語彙單元列表*](..\chapter3\06_Attributes.html#balanced_tokens) _可選_ **)**  
> *平衡語彙單元* → **[** [*平衡語彙單元列表*](..\chapter3\06_Attributes.html#balanced_tokens) _可選_ **]**  
> *平衡語彙單元* → **{** [*平衡語彙單元列表*](..\chapter3\06_Attributes.html#balanced_tokens) _可選_ **}**  
> *平衡語彙單元* → **任意識別符號, 關鍵字, 字面量或運算子**  
> *平衡語彙單元* → **任意標點除了(, ), [, ], {, 或 }**

<a name="expressions"></a>
## 表達式

> 表達式語法  
> *表達式* → [*前綴表達式*](..\chapter3\04_Expressions.html#prefix_expression) [*二元表達式列表*](..\chapter3\04_Expressions.html#binary_expressions) _可選_  
> *表達式列表* → [*表達式*](..\chapter3\04_Expressions.html#expression) | [*表達式*](..\chapter3\04_Expressions.html#expression) **,** [*表達式列表*](..\chapter3\04_Expressions.html#expression_list)  

<!-- -->

> 前綴表達式語法  
> *前綴表達式* → [*前綴運算子*](..\chapter3\02_Lexical_Structure.html#prefix_operator) _可選_ [*後綴表達式*](..\chapter3\04_Expressions.html#postfix_expression)  
> *前綴表達式* → [*寫入寫出(in-out)表達式*](..\chapter3\04_Expressions.html#in_out_expression)  
> *寫入寫出(in-out)表達式* → **&** [*識別符號*](..\chapter3\02_Lexical_Structure.html#identifier)  

<!-- -->

> 二元表達式語法  
> *二元表達式* → [*二元運算子*](..\chapter3\02_Lexical_Structure.html#binary_operator) [*前綴表達式*](..\chapter3\04_Expressions.html#prefix_expression)  
> *二元表達式* → [*賦值運算子*](..\chapter3\04_Expressions.html#assignment_operator) [*前綴表達式*](..\chapter3\04_Expressions.html#prefix_expression)  
> *二元表達式* → [*條件運算子*](..\chapter3\04_Expressions.html#conditional_operator) [*前綴表達式*](..\chapter3\04_Expressions.html#prefix_expression)  
> *二元表達式* → [*型別轉換運算子*](..\chapter3\04_Expressions.html#type_casting_operator)  
> *二元表達式列表* → [*二元表達式*](..\chapter3\04_Expressions.html#binary_expression) [*二元表達式列表*](..\chapter3\04_Expressions.html#binary_expressions) _可選_  

<!-- -->

> 賦值運算子語法  
> *賦值運算子* → **=**  

<!-- -->

> 三元條件運算子語法  
> *三元條件運算子* → **?** [*表達式*](..\chapter3\04_Expressions.html#expression) **:**  

<!-- -->

> 型別轉換運算子語法  
> *型別轉換運算子* → **is** [*型別*](..\chapter3\03_Types.html#type) | **as** **?** _可選_ [*型別*](..\chapter3\03_Types.html#type)  

<!-- -->

> 主表達式語法  
> *主表達式* → [*識別符號*](..\chapter3\02_Lexical_Structure.html#identifier) [*泛型參數子句*](GenericParametersAndArguments.html#generic_argument_clause) _可選_  
> *主表達式* → [*字面量表達式*](..\chapter3\04_Expressions.html#literal_expression)  
> *主表達式* → [*self表達式*](..\chapter3\04_Expressions.html#self_expression)  
> *主表達式* → [*超類別表達式*](..\chapter3\04_Expressions.html#superclass_expression)  
> *主表達式* → [*閉包表達式*](..\chapter3\04_Expressions.html#closure_expression)  
> *主表達式* → [*圓括號表達式*](..\chapter3\04_Expressions.html#parenthesized_expression)  
> *主表達式* → [*隱式成員表達式*](..\chapter3\04_Expressions.html#implicit_member_expression)  
> *主表達式* → [*通配符表達式*](..\chapter3\04_Expressions.html#wildcard_expression)  

<!-- -->

> 字面量表達式語法  
> *字面量表達式* → [*字面量*](..\chapter3\02_Lexical_Structure.html#literal)  
> *字面量表達式* → [*陣列字面量*](..\chapter3\04_Expressions.html#array_literal) | [*字典字面量*](..\chapter3\04_Expressions.html#dictionary_literal)  
> *字面量表達式* → **&#95;&#95;FILE&#95;&#95;** | **&#95;&#95;LINE&#95;&#95;** | **&#95;&#95;COLUMN&#95;&#95;** | **&#95;&#95;FUNCTION&#95;&#95;**  
> *陣列字面量* → **[** [*陣列字面量項列表*](..\chapter3\04_Expressions.html#array_literal_items) _可選_ **]**  
> *陣列字面量項列表* → [*陣列字面量項*](..\chapter3\04_Expressions.html#array_literal_item) **,** _可選_ | [*陣列字面量項*](..\chapter3\04_Expressions.html#array_literal_item) **,** [*陣列字面量項列表*](..\chapter3\04_Expressions.html#array_literal_items)  
> *陣列字面量項* → [*表達式*](..\chapter3\04_Expressions.html#expression)  
> *字典字面量* → **[** [*字典字面量項列表*](..\chapter3\04_Expressions.html#dictionary_literal_items) **]** | **[** **:** **]**  
> *字典字面量項列表* → [*字典字面量項*](..\chapter3\04_Expressions.html#dictionary_literal_item) **,** _可選_ | [*字典字面量項*](..\chapter3\04_Expressions.html#dictionary_literal_item) **,** [*字典字面量項列表*](..\chapter3\04_Expressions.html#dictionary_literal_items)  
> *字典字面量項* → [*表達式*](..\chapter3\04_Expressions.html#expression) **:** [*表達式*](..\chapter3\04_Expressions.html#expression)  

<!-- -->

> Self 表達式語法  
> *self表達式* → **self**  
> *self表達式* → **self** **.** [*識別符號*](..\chapter3\02_Lexical_Structure.html#identifier)  
> *self表達式* → **self** **[** [*表達式*](..\chapter3\04_Expressions.html#expression) **]**  
> *self表達式* → **self** **.** **init**  

<!-- -->

> 超類別表達式語法  
> *超類別表達式* → [*超類別方法表達式*](..\chapter3\04_Expressions.html#superclass_method_expression) | [*超類別下標表達式*](..\chapter3\04_Expressions.html#超類別下標表達式) | [*超類別建構器表達式*](..\chapter3\04_Expressions.html#superclass_initializer_expression)  
> *超類別方法表達式* → **super** **.** [*識別符號*](..\chapter3\02_Lexical_Structure.html#identifier)  
> *超類別下標表達式* → **super** **[** [*表達式*](..\chapter3\04_Expressions.html#expression) **]**  
> *超類別建構器表達式* → **super** **.** **init**  

<!-- -->

> 閉包表達式語法  
> *閉包表達式* → **{** [*閉包簽名(Signational)*](..\chapter3\04_Expressions.html#closure_signature) _可選_ [*多條語句(Statements)*](..\chapter3\10_Statements.html#statements) **}**  
> *閉包簽名(Signational)* → [*參數子句*](..\chapter3\05_Declarations.html#parameter_clause) [*函式結果*](..\chapter3\05_Declarations.html#function_result) _可選_ **in**  
> *閉包簽名(Signational)* → [*識別符號列表*](..\chapter3\02_Lexical_Structure.html#identifier_list) [*函式結果*](..\chapter3\05_Declarations.html#function_result) _可選_ **in**  
> *閉包簽名(Signational)* → [*捕獲(Capature)列表*](..\chapter3\04_Expressions.html#capture_list) [*參數子句*](..\chapter3\05_Declarations.html#parameter_clause) [*函式結果*](..\chapter3\05_Declarations.html#function_result) _可選_ **in**  
> *閉包簽名(Signational)* → [*捕獲(Capature)列表*](..\chapter3\04_Expressions.html#capture_list) [*識別符號列表*](..\chapter3\02_Lexical_Structure.html#identifier_list) [*函式結果*](..\chapter3\05_Declarations.html#function_result) _可選_ **in**  
> *閉包簽名(Signational)* → [*捕獲(Capature)列表*](..\chapter3\04_Expressions.html#capture_list) **in**  
> *捕獲(Capature)列表* → **[** [*捕獲(Capature)說明符*](..\chapter3\04_Expressions.html#capture_specifier) [*表達式*](..\chapter3\04_Expressions.html#expression) **]**  
> *捕獲(Capature)說明符* → **weak** | **unowned** | **unowned(safe)** | **unowned(unsafe)**  

<!-- -->

> 隱式成員表達式語法  
> *隱式成員表達式* → **.** [*識別符號*](..\chapter3\02_Lexical_Structure.html#identifier)  

<!-- -->

> 圓括號表達式(Parenthesized Expression)語法  
> *圓括號表達式* → **(** [*表達式元素列表*](..\chapter3\04_Expressions.html#expression_element_list) _可選_ **)**  
> *表達式元素列表* → [*表達式元素*](..\chapter3\04_Expressions.html#expression_element) | [*表達式元素*](..\chapter3\04_Expressions.html#expression_element) **,** [*表達式元素列表*](..\chapter3\04_Expressions.html#expression_element_list)  
> *表達式元素* → [*表達式*](..\chapter3\04_Expressions.html#expression) | [*識別符號*](..\chapter3\02_Lexical_Structure.html#identifier) **:** [*表達式*](..\chapter3\04_Expressions.html#expression)  

<!-- -->

> 通配符表達式語法  
> *通配符表達式* → **_**  

<!-- -->

> 後綴表達式語法  
> *後綴表達式* → [*主表達式*](..\chapter3\04_Expressions.html#primary_expression)  
> *後綴表達式* → [*後綴表達式*](..\chapter3\04_Expressions.html#postfix_expression) [*後綴運算子*](..\chapter3\02_Lexical_Structure.html#postfix_operator)  
> *後綴表達式* → [*函式呼叫表達式*](..\chapter3\04_Expressions.html#function_call_expression)  
> *後綴表達式* → [*建構器表達式*](..\chapter3\04_Expressions.html#initializer_expression)  
> *後綴表達式* → [*顯示成員表達式*](..\chapter3\04_Expressions.html#explicit_member_expression)  
> *後綴表達式* → [*後綴self表達式*](..\chapter3\04_Expressions.html#postfix_self_expression)  
> *後綴表達式* → [*動態型別表達式*](..\chapter3\04_Expressions.html#dynamic_type_expression)  
> *後綴表達式* → [*下標表達式*](..\chapter3\04_Expressions.html#subscript_expression)  
> *後綴表達式* → [*強制取值(Forced Value)表達式*](..\chapter3\04_Expressions.html#forced_value_expression)  
> *後綴表達式* → [*可選鏈(Optional Chaining)表達式*](..\chapter3\04_Expressions.html#optional_chaining_expression)  

<!-- -->

> 函式呼叫表達式語法  
> *函式呼叫表達式* → [*後綴表達式*](..\chapter3\04_Expressions.html#postfix_expression) [*圓括號表達式*](..\chapter3\04_Expressions.html#parenthesized_expression)  
> *函式呼叫表達式* → [*後綴表達式*](..\chapter3\04_Expressions.html#postfix_expression) [*圓括號表達式*](..\chapter3\04_Expressions.html#parenthesized_expression) _可選_ [*後綴閉包(Trailing Closure)*](..\chapter3\04_Expressions.html#trailing_closure)  
> *後綴閉包(Trailing Closure)* → [*閉包表達式*](..\chapter3\04_Expressions.html#closure_expression)  

<!-- -->

> 建構器表達式語法  
> *建構器表達式* → [*後綴表達式*](..\chapter3\04_Expressions.html#postfix_expression) **.** **init**  

<!-- -->

> 顯式成員表達式語法  
> *顯示成員表達式* → [*後綴表達式*](..\chapter3\04_Expressions.html#postfix_expression) **.** [*十進制數字*](..\chapter3\02_Lexical_Structure.html#decimal_digit)  
> *顯示成員表達式* → [*後綴表達式*](..\chapter3\04_Expressions.html#postfix_expression) **.** [*識別符號*](..\chapter3\02_Lexical_Structure.html#identifier) [*泛型參數子句*](GenericParametersAndArguments.html#generic_argument_clause) _可選_  

<!-- -->

> 後綴Self 表達式語法  
> *後綴self表達式* → [*後綴表達式*](..\chapter3\04_Expressions.html#postfix_expression) **.** **self**  

<!-- -->

> 動態型別表達式語法  
> *動態型別表達式* → [*後綴表達式*](..\chapter3\04_Expressions.html#postfix_expression) **.** **dynamicType**  

<!-- -->

> 附屬腳本表達式語法  
> *附屬腳本表達式* → [*後綴表達式*](..\chapter3\04_Expressions.html#postfix_expression) **[** [*表達式列表*](..\chapter3\04_Expressions.html#expression_list) **]**  

<!-- -->

> 強制取值(Forced Value)語法  
> *強制取值(Forced Value)表達式* → [*後綴表達式*](..\chapter3\04_Expressions.html#postfix_expression) **!**  

<!-- -->

> 可選鏈表達式語法  
> *可選鏈表達式* → [*後綴表達式*](..\chapter3\04_Expressions.html#postfix_expression) **?**  

<a name="lexical_structure"></a>
## 詞法結構

> 識別符號語法  
> *識別符號* → [*識別符號頭(Head)*](..\chapter3\02_Lexical_Structure.html#identifier_head) [*識別符號字元列表*](..\chapter3\02_Lexical_Structure.html#identifier_characters) _可選_  
> *識別符號* → **`** [*識別符號頭(Head)*](..\chapter3\02_Lexical_Structure.html#identifier_head) [*識別符號字元列表*](..\chapter3\02_Lexical_Structure.html#identifier_characters) _可選_ **`**  
> *識別符號* → [*隱式參數名*](..\chapter3\02_Lexical_Structure.html#implicit_parameter_name)  
> *識別符號列表* → [*識別符號*](..\chapter3\02_Lexical_Structure.html#identifier) | [*識別符號*](..\chapter3\02_Lexical_Structure.html#identifier) **,** [*識別符號列表*](..\chapter3\02_Lexical_Structure.html#identifier_list)  
> *識別符號頭(Head)* → Upper- or lowercase letter A through Z  
> *識別符號頭(Head)* → U+00A8, U+00AA, U+00AD, U+00AF, U+00B2–U+00B5, or U+00B7–U+00BA  
> *識別符號頭(Head)* → U+00BC–U+00BE, U+00C0–U+00D6, U+00D8–U+00F6, or U+00F8–U+00FF  
> *識別符號頭(Head)* → U+0100–U+02FF, U+0370–U+167F, U+1681–U+180D, or U+180F–U+1DBF  
> *識別符號頭(Head)* → U+1E00–U+1FFF  
> *識別符號頭(Head)* → U+200B–U+200D, U+202A–U+202E, U+203F–U+2040, U+2054, or U+2060–U+206F  
> *識別符號頭(Head)* → U+2070–U+20CF, U+2100–U+218F, U+2460–U+24FF, or U+2776–U+2793  
> *識別符號頭(Head)* → U+2C00–U+2DFF or U+2E80–U+2FFF  
> *識別符號頭(Head)* → U+3004–U+3007, U+3021–U+302F, U+3031–U+303F, or U+3040–U+D7FF  
> *識別符號頭(Head)* → U+F900–U+FD3D, U+FD40–U+FDCF, U+FDF0–U+FE1F, or U+FE30–U+FE44  
> *識別符號頭(Head)* → U+FE47–U+FFFD  
> *識別符號頭(Head)* → U+10000–U+1FFFD, U+20000–U+2FFFD, U+30000–U+3FFFD, or U+40000–U+4FFFD  
> *識別符號頭(Head)* → U+50000–U+5FFFD, U+60000–U+6FFFD, U+70000–U+7FFFD, or U+80000–U+8FFFD  
> *識別符號頭(Head)* → U+90000–U+9FFFD, U+A0000–U+AFFFD, U+B0000–U+BFFFD, or U+C0000–U+CFFFD  
> *識別符號頭(Head)* → U+D0000–U+DFFFD or U+E0000–U+EFFFD  
> *識別符號字元* → 數值 0 到 9  
> *識別符號字元* → U+0300–U+036F, U+1DC0–U+1DFF, U+20D0–U+20FF, or U+FE20–U+FE2F  
> *識別符號字元* → [*識別符號頭(Head)*](..\chapter3\02_Lexical_Structure.html#identifier_head)  
> *識別符號字元列表* → [*識別符號字元*](..\chapter3\02_Lexical_Structure.html#identifier_character) [*識別符號字元列表*](..\chapter3\02_Lexical_Structure.html#identifier_characters) _可選_  
> *隱式參數名* → **$** [*十進制數字列表*](..\chapter3\02_Lexical_Structure.html#decimal_digits)  

<!-- -->

> 字面量語法  
> *字面量* → [*整型字面量*](..\chapter3\02_Lexical_Structure.html#integer_literal) | [*浮點數字面量*](..\chapter3\02_Lexical_Structure.html#floating_point_literal) | [*字串字面量*](..\chapter3\02_Lexical_Structure.html#string_literal)  

<!-- -->

> 整型字面量語法  
> *整型字面量* → [*二進制字面量*](..\chapter3\02_Lexical_Structure.html#binary_literal)  
> *整型字面量* → [*八進制字面量*](..\chapter3\02_Lexical_Structure.html#octal_literal)  
> *整型字面量* → [*十進制字面量*](..\chapter3\02_Lexical_Structure.html#decimal_literal)  
> *整型字面量* → [*十六進制字面量*](..\chapter3\02_Lexical_Structure.html#hexadecimal_literal)  
> *二進制字面量* → **0b** [*二進制數字*](..\chapter3\02_Lexical_Structure.html#binary_digit) [*二進制字面量字元列表*](..\chapter3\02_Lexical_Structure.html#binary_literal_characters) _可選_  
> *二進制數字* → 數值 0 到 1  
> *二進制字面量字元* → [*二進制數字*](..\chapter3\02_Lexical_Structure.html#binary_digit) | **_**  
> *二進制字面量字元列表* → [*二進制字面量字元*](..\chapter3\02_Lexical_Structure.html#binary_literal_character) [*二進制字面量字元列表*](..\chapter3\02_Lexical_Structure.html#binary_literal_characters) _可選_  
> *八進制字面量* → **0o** [*八進字數字*](..\chapter3\02_Lexical_Structure.html#octal_digit) [*八進制字元列表*](..\chapter3\02_Lexical_Structure.html#octal_literal_characters) _可選_  
> *八進字數字* → 數值 0 到 7  
> *八進制字元* → [*八進字數字*](..\chapter3\02_Lexical_Structure.html#octal_digit) | **_**  
> *八進制字元列表* → [*八進制字元*](..\chapter3\02_Lexical_Structure.html#octal_literal_character) [*八進制字元列表*](..\chapter3\02_Lexical_Structure.html#octal_literal_characters) _可選_  
> *十進制字面量* → [*十進制數字*](..\chapter3\02_Lexical_Structure.html#decimal_digit) [*十進制字元列表*](..\chapter3\02_Lexical_Structure.html#decimal_literal_characters) _可選_  
> *十進制數字* → 數值 0 到 9  
> *十進制數字列表* → [*十進制數字*](..\chapter3\02_Lexical_Structure.html#decimal_digit) [*十進制數字列表*](..\chapter3\02_Lexical_Structure.html#decimal_digits) _可選_  
> *十進制字元* → [*十進制數字*](..\chapter3\02_Lexical_Structure.html#decimal_digit) | **_**  
> *十進制字元列表* → [*十進制字元*](..\chapter3\02_Lexical_Structure.html#decimal_literal_character) [*十進制字元列表*](..\chapter3\02_Lexical_Structure.html#decimal_literal_characters) _可選_  
> *十六進制字面量* → **0x** [*十六進制數字*](..\chapter3\02_Lexical_Structure.html#hexadecimal_digit) [*十六進制字面量字元列表*](..\chapter3\02_Lexical_Structure.html#hexadecimal_literal_characters) _可選_  
> *十六進制數字* → 數值 0 到 9, a through f, or A through F  
> *十六進制字元* → [*十六進制數字*](..\chapter3\02_Lexical_Structure.html#hexadecimal_digit) | **_**  
> *十六進制字面量字元列表* → [*十六進制字元*](..\chapter3\02_Lexical_Structure.html#hexadecimal_literal_character) [*十六進制字面量字元列表*](..\chapter3\02_Lexical_Structure.html#hexadecimal_literal_characters) _可選_  

<!-- -->

> 浮點型字面量語法  
> *浮點數字面量* → [*十進制字面量*](..\chapter3\02_Lexical_Structure.html#decimal_literal) [*十進制分數*](..\chapter3\02_Lexical_Structure.html#decimal_fraction) _可選_ [*十進制指數*](..\chapter3\02_Lexical_Structure.html#decimal_exponent) _可選_  
> *浮點數字面量* → [*十六進制字面量*](..\chapter3\02_Lexical_Structure.html#hexadecimal_literal) [*十六進制分數*](..\chapter3\02_Lexical_Structure.html#hexadecimal_fraction) _可選_ [*十六進制指數*](..\chapter3\02_Lexical_Structure.html#hexadecimal_exponent)  
> *十進制分數* → **.** [*十進制字面量*](..\chapter3\02_Lexical_Structure.html#decimal_literal)  
> *十進制指數* → [*浮點數e*](..\chapter3\02_Lexical_Structure.html#floating_point_e) [*正負號*](..\chapter3\02_Lexical_Structure.html#sign) _可選_ [*十進制字面量*](..\chapter3\02_Lexical_Structure.html#decimal_literal)  
> *十六進制分數* → **.** [*十六進制字面量*](..\chapter3\02_Lexical_Structure.html#hexadecimal_literal) _可選_  
> *十六進制指數* → [*浮點數p*](..\chapter3\02_Lexical_Structure.html#floating_point_p) [*正負號*](..\chapter3\02_Lexical_Structure.html#sign) _可選_ [*十六進制字面量*](..\chapter3\02_Lexical_Structure.html#hexadecimal_literal)  
> *浮點數e* → **e** | **E**  
> *浮點數p* → **p** | **P**  
> *正負號* → **+** | **-**  

<!-- -->

> 字元型字面量語法  
> *字串字面量* → **"** [*參考文字*](..\chapter3\02_Lexical_Structure.html#quoted_text) **"**  
> *參考文字* → [*參考文字條目*](..\chapter3\02_Lexical_Structure.html#quoted_text_item) [*參考文字*](..\chapter3\02_Lexical_Structure.html#quoted_text) _可選_  
> *參考文字條目* → [*跳脫字元*](..\chapter3\02_Lexical_Structure.html#escaped_character)  
> *參考文字條目* → **\(** [*表達式*](..\chapter3\04_Expressions.html#expression) **)**  
> *參考文字條目* → 除了"­, \­, U+000A, or U+000D的所有Unicode的字元  
> *跳脫字元* → **\0** | **\\** | **\t** | **\n** | **\r** | **\"** | **\'**  
> *跳脫字元* → **\x** [*十六進制數字*](..\chapter3\02_Lexical_Structure.html#hexadecimal_digit) [*十六進制數字*](..\chapter3\02_Lexical_Structure.html#hexadecimal_digit)  
> *跳脫字元* → **\u** [*十六進制數字*](..\chapter3\02_Lexical_Structure.html#hexadecimal_digit) [*十六進制數字*](..\chapter3\02_Lexical_Structure.html#hexadecimal_digit) [*十六進制數字*](..\chapter3\02_Lexical_Structure.html#hexadecimal_digit) [*十六進制數字*](..\chapter3\02_Lexical_Structure.html#hexadecimal_digit)  
> *跳脫字元* → **\U** [*十六進制數字*](..\chapter3\02_Lexical_Structure.html#hexadecimal_digit) [*十六進制數字*](..\chapter3\02_Lexical_Structure.html#hexadecimal_digit) [*十六進制數字*](..\chapter3\02_Lexical_Structure.html#hexadecimal_digit) [*十六進制數字*](..\chapter3\02_Lexical_Structure.html#hexadecimal_digit) [*十六進制數字*](..\chapter3\02_Lexical_Structure.html#hexadecimal_digit) [*十六進制數字*](..\chapter3\02_Lexical_Structure.html#hexadecimal_digit) [*十六進制數字*](..\chapter3\02_Lexical_Structure.html#hexadecimal_digit) [*十六進制數字*](..\chapter3\02_Lexical_Structure.html#hexadecimal_digit)  

<!-- -->

> 運算子語法語法  
> *運算子* → [*運算子字元*](..\chapter3\02_Lexical_Structure.html#operator_character) [*運算子*](..\chapter3\02_Lexical_Structure.html#operator) _可選_  
> *運算子字元* → **/** | **=** | **-** | **+** | **!** | **&#42;** | **%** | **<** | **>** | **&** | **|** | **^** | **~** | **.**  
> *二元運算子* → [*運算子*](..\chapter3\02_Lexical_Structure.html#operator)  
> *前綴運算子* → [*運算子*](..\chapter3\02_Lexical_Structure.html#operator)  
> *後綴運算子* → [*運算子*](..\chapter3\02_Lexical_Structure.html#operator)  

<a name="types"></a>
## 型別

> 型別語法  
> *型別* → [*陣列型別*](..\chapter3\03_Types.html#array_type) | [*函式型別*](..\chapter3\03_Types.html#function_type) | [*型別標識*](..\chapter3\03_Types.html#type_identifier) | [*元組型別*](..\chapter3\03_Types.html#tuple_type) | [*可選型別*](..\chapter3\03_Types.html#optional_type) | [*隱式解析可選型別*](..\chapter3\03_Types.html#implicitly_unwrapped_optional_type) | [*協定合成型別*](..\chapter3\03_Types.html#protocol_composition_type) | [*元型型別*](..\chapter3\03_Types.html#metatype_type)  

<!-- -->

> 型別注解語法  
> *型別注解* → **:** [*特性(Attributes)列表*](..\chapter3\06_Attributes.html#attributes) _可選_ [*型別*](..\chapter3\03_Types.html#type)  

<!-- -->

> 型別標識語法  
> *型別標識* → [*型別名稱*](..\chapter3\03_Types.html#type_name) [*泛型參數子句*](GenericParametersAndArguments.html#generic_argument_clause) _可選_ | [*型別名稱*](..\chapter3\03_Types.html#type_name) [*泛型參數子句*](GenericParametersAndArguments.html#generic_argument_clause) _可選_ **.** [*型別標識*](..\chapter3\03_Types.html#type_identifier)  
> *類別名* → [*識別符號*](..\chapter3\02_Lexical_Structure.html#identifier)  

<!-- -->

> 元組型別語法  
> *元組型別* → **(** [*元組型別主體*](..\chapter3\03_Types.html#tuple_type_body) _可選_ **)**  
> *元組型別主體* → [*元組型別的元素列表*](..\chapter3\03_Types.html#tuple_type_element_list) **...** _可選_  
> *元組型別的元素列表* → [*元組型別的元素*](..\chapter3\03_Types.html#tuple_type_element) | [*元組型別的元素*](..\chapter3\03_Types.html#tuple_type_element) **,** [*元組型別的元素列表*](..\chapter3\03_Types.html#tuple_type_element_list)  
> *元組型別的元素* → [*特性(Attributes)列表*](..\chapter3\06_Attributes.html#attributes) _可選_ **inout** _可選_ [*型別*](..\chapter3\03_Types.html#type) | **inout** _可選_ [*元素名*](..\chapter3\03_Types.html#element_name) [*型別注解*](..\chapter3\03_Types.html#type_annotation)  
> *元素名* → [*識別符號*](..\chapter3\02_Lexical_Structure.html#identifier)  

<!-- -->

> 函式型別語法  
> *函式型別* → [*型別*](..\chapter3\03_Types.html#type) **->** [*型別*](..\chapter3\03_Types.html#type)  

<!-- -->

> 陣列型別語法  
> *陣列型別* → [*型別*](..\chapter3\03_Types.html#type) **[** **]** | [*陣列型別*](..\chapter3\03_Types.html#array_type) **[** **]**  

<!-- -->

> 可選型別語法  
> *可選型別* → [*型別*](..\chapter3\03_Types.html#type) **?**  

<!-- -->

> 隱式解析可選型別(Implicitly Unwrapped Optional Type)語法  
> *隱式解析可選型別* → [*型別*](..\chapter3\03_Types.html#type) **!**  

<!-- -->

> 協定合成型別語法  
> *協定合成型別* → **protocol** **<** [*協定識別符號列表*](..\chapter3\03_Types.html#protocol_identifier_list) _可選_ **>**  
> *協定識別符號列表* → [*協定識別符號*](..\chapter3\03_Types.html#protocol_identifier) | [*協定識別符號*](..\chapter3\03_Types.html#protocol_identifier) **,** [*協定識別符號列表*](..\chapter3\03_Types.html#protocol_identifier_list)  
> *協定識別符號* → [*型別標識*](..\chapter3\03_Types.html#type_identifier)  

<!-- -->

> 元(Metatype)型別語法  
> *元型別* → [*型別*](..\chapter3\03_Types.html#type) **.** **Type** | [*型別*](..\chapter3\03_Types.html#type) **.** **Protocol**  

<!-- -->

> 型別繼承子句語法  
> *型別繼承子句* → **:** [*型別繼承列表*](..\chapter3\03_Types.html#type_inheritance_list)  
> *型別繼承列表* → [*型別標識*](..\chapter3\03_Types.html#type_identifier) | [*型別標識*](..\chapter3\03_Types.html#type_identifier) **,** [*型別繼承列表*](..\chapter3\03_Types.html#type_inheritance_list)
