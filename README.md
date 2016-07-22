# TypeScriptBuilder

This small library translates TypeScript type definition based on C# types.

Install by nuget:
```
Install-Package TypeScriptBuilder
```

## Supported features
- Resolving type dependency
- Generics
- Type inheritance
- Enums
- Nullable types
- Dictionary converison (to strong type TS indexed objects)
- Excluding types
- `any` for types that can't be converted

## Usage / Samples
```cs
var
    ts = new TypeScriptGenerator();
    
// add types you need (dependant types will be added automatically)
ts.AddCSType(typeof(User));

// write to file
File.WriteAllText("Test.ts", ts.ToString());
```
Sample C# class:
```cs
class User 
{
  public int Id;
  public string Login;
  
  [TSExclude] // exclude field, can be applied on class too
  public bool Active;
}
```
Output TypeScript:
```ts
export interface User
{
  Id: number;
  Login: string;
}
```

## Dictionary

Dictionaries with keys of type `int` or `string` will be translated to strong typed TS indexed objects:
```cs
class Entity<T>
{
    public T Value;
}
class Test 
{
    public Dictionary<int, Entity<DateTime>> Repo;
}
```
```ts
export interface Entity<T> {
    Value: T;
}
export interface Test {
    Repo: { [index: number]: Entity<Date> };
}

```

## Control attributes
- `TSExclude` - skips generation of type or field
- `TSAny` - skips type analizes, emits `any` instead

To learn more run `Test` project in the solution.