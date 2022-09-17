---
layout: page
title: Strings
breadcrumb: strings
---

### Why a string class is needed

The main reason why TGUI needs its own string class is to support unicode in a cross-platform way.

Windows and Unix platforms handle unicode very differently, making std::string and std::wstring unusable. On Linux (and probably all other non-Windows platforms as well), strings are usually UTF-8 encoded by default. This means that you can just write unicode characters in an std::string, print this string to the terminal or use the string as a filename. Unicode characters work out of the box. On Windows on the other hand, you need to use whar_t and std::wstring if you want to do things with unicode. Since c++11 we have new string classes such as std::u32string, but those can only be used for storing the strings. When you need to interact with the OS you would still need to convert the `char32_t*` to `char*` or `whar_t*` depending on the platform. A custom string class is needed to provide easy conversions between different encodings.

TGUI used to use `sf::String` everywhere in the past instead of having its own string class. Although the fact that TGUI now supports alternative backends (and can thus be build without SFML) has something to do with it, it was actually not the only reason for the change. One annoyance with `sf::String` was that it didn't work together with std::cout. Another problem was how it handled UTF-8 on Linux. The great thing on UNIX platforms was that unicode characters in strings just worked, but as soon as a string was copied into an `sf::String`, it would mess up the encoding. You had to manually call the fromUtf8 function each time, which annoyed me a bit.


### UTF-8 / UTF-32

TGUI stores all strings in UTF-32 format (the `std::u32string` class is used internally). Any operation that uses a different encoding will thus cause an implicit conversion. This was done to make accessing the N-th character using the `[]` operator simple and fast.

Unlike some other string classes, TGUI assumes UTF-8 encoding on all byte-strings. If you pass a `char*` or `std::string` to TGUI, then it will expect UTF-8 encoding. Similarly, any function in `tgui::String` that returns a `char*` or `std::string` will return a string in UTF-8 encoding. This way unicode can work out of the box on UNIX platforms.  
The downside of this is that you can't use the locale-depend strings on Windows. You have to use either ASCII characters or use a widestring. I don't consider this an issue since you should use widestrings (or a class like `tgui::String`) on Windows anyway if you want to properly support unicode.

Here are 4 examples of how you could use string literals. The limitations mentioned here are related to the string literals themselves and are not caused by TGUI.
```c++
tgui::String s1 = "Hello ☺"; // Works on Linux but not on Windows
tgui::String s2 = L"Hello \u263A"; // Cross-platform, but string gets converted from widestring to UTF-32
tgui::String s3 = U"Hello \u263A"; // Cross-platform, string is UTF-32 so no conversion needed
tgui::String s4 = U"Hello ☺"; // Cross-platform only if source file is saved with correct encoding
```


### Functionality

You might not expect it, but the `tgui::String` is the largest class in TGUI. The API mirrors that of `std::u32string`, but with versions for different character/string types and with additional functions.

For example, for the push_back function the following variants are all available:
```c++
void push_back(char ch);
void push_back(wchar_t ch);
void push_back(char8_t ch); // Only in c++20
void push_back(char16_t ch);
void push_back(char32_t ch);
```

Next to all the traditional string functions, several additional functions are also provided that may be useful:
```c++
tgui::String s1 = str.trim();      // Removes whitespace at the beginning and end of the string

tgui::String s2 = str.toLower();   // Converts string to lowercase (only ASCII letters are affected)
tgui::String s3 = str.toUpper();   // Converts string to uppercase (only ASCII letters are affected)

bool b1 = str.startsWith("Test");  // Checks whether the string begins with "Test"
bool b2 = str.endsWith("Test");    // Checks whether the string ends with "Test"

bool b3 = str1.equalIgnoreCase(str2); // Checks whether both strings are identical if the case of ASCII letters is ignored

str.replace("AB", "CDE");   // Replaces each occurance of "AB" with "CDE" in the string

std::vector<tgui::String> parts = {"One", "Two", "Three"};
tgui::String s4 = tgui::String::join(parts, "; "); // Joins the parts into one string (s4 == "One; Two; Three")

parts = tgui::String("A, B, C").split(',');        // Splits the string on a character (parts == {"A", " B", " C"})
parts = tgui::String("A, B, C").split(',', true);  // Splits the string on a character and trims each part (parts == {"A", "B", "C"})
```


### Number conversions

The string class also provides functions to construct the string from a number or to parse the string and returning the numeric value.

There is a constructor that accepts numbers (of any type):
```c++
tgui::String s1(6);
tgui::String s2(0.4);
```

For floating point values, you may want to specify the decimals behind the comma:
```c++
tgui::String s = tgui::String::fromNumberRounded(1.987, 2);  // s == "1.99"
```

To convert the string to a number, you can use `toInt`, `toUInt` or `toFloat`. These functions have an optional parameter with a value that should be returned if the string can't be parsed.
```c++
int val1 = tgui::String("5").toInt();     // val1 == 5
int val2 = tgui::String("NaN").toInt();   // val2 == 0
int val3 = tgui::String("NaN").toInt(-1); // val3 == -1
```

If you want to convert the string to a number but want a boolean value on error, then you can use `attemptToInt`, `attemptToUInt` or `attemptToFloat`.
```c++
float val;
bool success = str.attemptToFloat(val);
```

### String conversions

TGUI was required to have its own string class, but I wanted to hide it as much as possible. So whether you are using `char*`, `whar_t*`, `char8_t*` `char16_t*`, `char32_t*`, `std::string`, `std::wstring`, `std::u8string`, `std::u16string`, `std::u32string` or `sf::String`, there is an implicit constructor for it.

To convert from a `tgui::String` to an std string, you can use either the explicit conversion operators or the `toXXX` functions:
```c++
std::string strUtf8(str);
std::wstring strWide(str);
std::u16string strUtf16(str);
std::u32string strUtf32(str);

std::string strUft8 = str.toStrString();
std::wstring strWide = str.toWideString();
std::u16string strUtf16 = str.toUtf16();
std::u32string strUtf32 = str.toUtf32();
```

If TGUI has been build with the SFML backend then a conversion operators also exists for `sf::String`:
```c++
sf::String strSFML(str)
```

If c++20 is enabled in your project then there is also a `std::u8string` conversion operator and a `toUtf8()` function.
