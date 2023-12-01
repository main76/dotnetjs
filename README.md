# DotnetJs

.Net Framework support in javascript

## Get DotnetJs

1. Nodejs

    Firstly, run

    ```npm
    npm install dotnetjs --save
    ```

    If you are using TypeScript:

    ```typescript
    import * as DotnetJs from 'dotnetjs';
    ```

    else just like the others:

    ```javascript
    var DotnetJs = require('dotnetjs');
    ```

2. Browser

    Run ```npm install dotnetjs --save```

    or download the files in the dist directory.

    or run ```bower install dotnetjs```

    ```html
    <!--if debug-->
    <script src="dotnet.js"></script>
    <!--if release-->
    <script src="dotnet.min.js"></script>
    <script src="your.js"></script>
    ```

    in your .ts file:

    ```typescript
    /// <reference path="dotnet.d.ts" />
    ```

## Work with DotnetJs

DotnetJs uses similiar interface as it is in .Net Framework.

### Linq

If you are about to use complicated Linq Expressions, first make an instance of LinqIntermediate by using LinqStart:

```typescript
var expression = DotnetJs.Linq.LinqStart(enumerable);
```

The enumerable can be any type that implements IEnumerable, in addition, I implemented it for the Array. Then you can do like the following:

```typescript
expression.Where(...).Select(...).ToArray();
```

Or use:

```typescript
DotnetJs.Linq.Where(enumerable, ...).Select(...).ToArray();
```

Remember to use ```ToArray``` or ```ToList``` or ```ToDictionary``` to end the expression(if the result is still IEnumerable). No matter how long your LINQ is, the time complexity is always O(n).

### String Format

The match case: ```{index[,alignment][:format]}```, with 2 optional parameters (alignment and format).

1. index

    Index indicate the index of the object in the following parameter args[].

2. alignment

    Alignment will do PadLeft or PadRight with spaces. If it is positive then do PadLeft, else do PadRight.

3. format

    With the magic char ':', you can control the format of your toString method. For numbers, the following specifiers ['D', 'E', 'F', 'G', 'N', 'P', 'X'] are implemented, [usage portal](https://msdn.microsoft.com/en-us/library/dwhawy9k(v=vs.110).aspx).
    Now, let's see the following example:

    ```typescript
    var number = 1.0437E21;
    var specifier = 'G';
    console.writeLine("   {0,-22} {1:G}", specifier + ":", number);
    // output:   G:                     1.0437e+21
    
    class Foo {
        public bar: number;
        public toString(format: string) {
            if (format == null)
                return this.bar.toString();
            if (format == '!')
                return '!' + this.bar;
            return '?' + this.bar;
        }
    }

    var foo = new Foo();
    foo.bar = 65521;

    console.log(String.Format('test Foo toString: {0:!}, {0:?}, {0,10}', foo));
    // output: test Foo toString: !65521, ?65521,      65521
    ```

### Indexer for the collections

As there isn't a way to implement indexer in typescript you have to call the element of List or IDictionay, by using ```GetValue(index || key)``` and ```SetValue(index || key, value)```, but not ```collection[index || key]```.

### GetHashCode

Both object, string, boolean or number are supported for the GetHashCode Method, actually for the object, it is more likely to be called as a 'unique id'.

To get a new hashcode for an object, call the method with parameter 'ture'. But please be careful, this may cause unexpected errors (e.g.: when you are using ```Dictionary```).

### Equals

If you inherit from TypeScript abstract class ValueType, remember to override the Equals method, else it will compare the result of GetHashCode() to decide whether it equals to the other.

### ContainsKey

```typescript
var obj = { name: 'readme' };

console.log(obj.ContainsKey('name'));
// output: true
```

### TODO

More extensions. If you have any ideas, please feel free to contact me.

## Test

First change directory to tests, then run ```tsc``` to compile the typescript files, then run ```npm link ../``` secondly, to set up the local dependencies, finally run ```node index.js``` to start the test.

## Contributors

[Master76](https://github.com/Master76) Author

[AsherWang](https://github.com/AsherWang) RegExp suport