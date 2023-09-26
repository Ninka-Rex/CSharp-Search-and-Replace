# CSharp-Search-and-Replace
This is a C# class that provides a method to search and replace binary patterns in a given file. It can be used to patch executable files or other binary files with custom modifications.

## Usage
To use this class, you need to create an instance of the `PatchUtility` class and call its `SearchAndReplace` method with the following parameters:

- `filePath`: the path of the file to be patched
- `searchPatterns`: an array of hexadecimal strings representing the patterns to be searched for
- `replacePatterns`: an array of hexadecimal strings representing the patterns to be replaced with

The method returns `true` if the patching was successful, or `false` if there was an error or a pattern was not found.

The hexadecimal strings can contain spaces or question marks for readability. A question mark represents a wildcard, meaning that any value can match at that position. For example, `"0F 85 ?? ?? ?? ?? 48"` will match any six bytes after `"0F 85"` and before `"48"`.

The search and replace patterns must have the same length, otherwise the method will throw an exception. The replace pattern can also contain wildcards, meaning that the original value will be preserved at that position. For example, `"E9 05 01 00 ?? 90"` will replace the first four bytes with `"E9 05 01 00"`, keep the fifth byte as it is, and replace the sixth byte with `"90"`.

The method will also create a backup of the original file by copying it to a new file with a `.bak` extension. If the backup file already exists, it will not overwrite it.

The method will log its progress and results to the console.

## Example
Here is an example of how to use this class to patch a file called `"testapp.exe"` with some custom modifications:

```csharp
string filePath = "testapp.exe";   
string[] searchPatterns = {
        "0F 85 ?? ?? ?? ?? 48 8B 8D ?? ?? ?? ?? E8 ?? ?? ?? ?? E8 ?? ?? ?? ?? B9 ?? ?? ?? ?? E8 ?? ?? ?? ?? 81 E0 ?? ?? ?? ?? 79 09 83 E8 01 83 C8 FE 83 C0 01", // Search Pattern 1
        "75 14 48 8B 05 ?? ?? ?? ?? 48 8B 08 E8 ?? ?? ?? ?? E9 ?? ?? ?? ?? C6 85 ?? ?? ?? ?? 01 48 8B 05 ?? ?? ?? ?? 48 8B 00 80 38 00 75 16 48 8B 05", // Search Pattern 2
        "81 05 ?? ?? ?? ?? ?? ?? ?? ?? 81 F8" // etc..
    };
string[] replacePatterns = {
        "E9 05 01 00 ?? 90 ?? ?? ?? ?? ?? ?? ?? ?? ?? ?? ?? ?? ?? ?? ?? ?? ?? ?? ?? ?? ?? ?? ?? ?? ?? ?? ?? ?? ?? ?? ?? ?? ?? ?? ?? ?? ?? ?? ?? ?? ?? ?? ?? ??", // Replacement Pattern 1
        "EB ?? ?? ?? ?? ?? ?? ?? ?? ?? ?? ?? ?? ?? ?? ?? ?? ?? ?? ?? ?? ?? ?? ?? ?? ?? ?? ?? ?? ?? ?? ?? ?? ?? ?? ?? ?? ?? ?? ?? ?? ?? ?? ?? ?? ?? ??", // Replacement Pattern 2
        "?? ?? ?? ?? E9 A5 05 00 ?? 90 ?? ??" // etc..
    };

if (PatchUtility.SearchAndReplace(filePath, searchPatterns, replacePatterns))
{
    Console.WriteLine("Success!");
}
else
{
    Console.WriteLine("Failure!");
}
```
