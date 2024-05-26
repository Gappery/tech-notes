# Chapter2: Meaningful Names
* Intention-Revealing Names
    * why it exists
    * what it does
    * how it's used
* Avoid Disinformation
    * avoid words whose entrenched meanings vary from our intended meaning.
        * `hp // names of Unix platforms/ you want to use it as abbreviation of hypotenuse`
    * avoid using some word which has some special meaning to programmers
        * do not refer to a grouping of accounts as an `accountList` unless it’s actually a `List` (`accountGroup` is better)
        * TODO: furthermore, even better not to encode container type
    * avoid using names which vary in small ways
        * `XYZControllerForEfficientHandlingOfStrings` VS `XYZControllerForEfficientStorageOfStrings`
    * avoid some special characters as start of variable names
        * uppercase `O` VS number `0`
        * lowercase `l` VS number `1`
* Make Meaningful Distinctions
    * avoid changing name in an arbitrary way because of duplication
        * `klass` or `class2` just because name `class` is already used
    * avoid meaningless noise words
        * `productInfo` VS `productData`
        * TODO: How is NameString better than Name? Would a Name ever be a floating point number? If so, it breaks an earlier rule about disinformation.
* Use Pronounceable Names
    * programming is a social activity
    * `genymdhms` vs `generationTimestamp`
* Use Searchable Names
    * Single-letter names and numeric constants are not easy to locate across a body of text.
        * `MAX_CLASSES_PER_STUDENT` vs `7`
        * `e` vs `event`
        * single-letter names can ONLY be used as local variables inside short methods
        * The length of a name should correspond to the size of its scope
* Avoid Type Encodings (encoding type or scope information into names)
    * we already have much richer type systems in modern languages
    * type encoding makes it harder to change the name or type of a var/function/class
    * Hungarian Notation (prefix as type)
        * `lAccountNum`: variable is a long integer
* Avoid Member Prefixes
    * ?? you should be using an editing environment that highlights or colorizes members to make them distinct.
    * ~~`m_name`~~ VS `name`
* Interface && Implementation
    * no prefix for interface (~~`IShapeFactory`~~ VS `ShapeFactory`)
    * if must encode, then encode implementation name (`ShapeFactoryImp`/`CShapeFactory`)
* Avoid Mental Mapping (meaningful names)
    * clarity is king!
    * `i` `j` `k` are fine for small scope variables without conflict (they're traditional)
* Class Name
    * not verb
    * not ambiguous name (`Manager` `Data` `Processor` `Info`)
* Method Name
    * should be verb
    * accessors, mutators, and predicates should be named for their value and prefixed with `get`, `set`, and `is`
    * if constructor is overloaded, use static factory methods with names that describe the arguments:
        * ~~`Complex fulcrumPoint = new Complex(23.0);`~~
        * `Complex fulcrumPoint = Complex.FromRealNumber(23.0);`
* One Word per Concept
    * a consistent lexicon is a great boon to the programmers who must use your code.
    * find one for the equivalent
        * `fetch`, `retrieve` and `get`
        * `controller`, `manager` and `driver`
* Don't Pun
    * using the same term for two different ideas is essentially a pun.
    * if `add` currently means creating a new value, then we shouldn't use it as the verb for concatenating two values
        * change to use `insert` `append`
* Solution/Problem Domain Names
    * use computer science terms instead of draw name from problem domain if there's already a different name for the same concept
        * algorithm names/pattern names (e.g. `AccountVisitor` `JobQueue`)
    * otherwise use the name from the problem domain
        * at least the programmer who maintains your code can ask a domain expert what it means
    * ?? separating solution and problem domain concepts is part of the job of a good programmer and designer
    * ?? the code that has more to do with problem domain concepts should have names drawn from the problem domain
* Meaningful Context VS Redundant Context
    * adding prefix for names is fine if you can't enclosing them into well-named classes/functions/namespaces
        * `state` -> ambiguous if it's used alone (address/state management)
        * can changed to `addrState`
    * but short names are generally better than longer ones
        * `accountAddress` and `customerAddress`: fine names for instances of the class `Address` but could be poor names for classes
        * use `PostalAddress`, `MAC`, and `URI` to differentiate between port/MAC/web addresses
# Chapter3: Functions
* Small