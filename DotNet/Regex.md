

## The Core Building Blocks

Most characters in Regex just match themselves (e.g., `cat` matches the word "cat"). But certain special characters—called **metacharacters**—have secret powers.

### The Cheat Sheet

|**Character**|**What it means**|**Example**|**Matches...**|
|---|---|---|---|
|**`\d`**|Any single **digit** (0-9)|`\d\d\d`|`123`, `902`|
|**`\w`**|Any **word** character (Letters, digits, underscores)|`\w\w`|`a1`, `hi`, `_x`|
|**`\s`**|Any **whitespace** (Spaces, tabs, newlines)|`\s`||
|**`.`** (Dot)|**Any character** at all (except a newline)|`c.t`|`cat`, `cot`, `c9t`|
|**`^`**|The **start** of a line|`^Hello`|"Hello world" (but not "Say Hello")|
|**`$`**|The **end** of a line|`end$`|"The end" (but not "ending")|


## Quantifiers (How Many?)

By default, a character like `\d` only matches **one** single digit. To match multiple characters in a row, we use **quantifiers** right after the character.

- **`+`** = **1 or more** times.
    
- **`*`** = **0 or more** times.
    
- **`?`** = **0 or 1** time (makes it optional).
    
- **`{n}`** = Exactly **n** times.


Example:
`\d+` = "111", "123", "421", ...
`\d*` = "", "1", "23", "456" ...
`\d?` = "", "1", "2", "3" ...
`\d{3}-\d{4}` = "123-4567"

"\w\d?" = "A1", "B", "C3", ...
"\w{2}-\d{3}" = "AB-123", "cd-456"




### Character Classes `[...]` (The "Pick One" box)

Square brackets mean: _"Match **any single character** that is inside these brackets."_

- `[aeiou]` matches any vowel.
    
- `[a-z]` matches any lowercase letter from a to z.
    
- `[a-zA-Z0-9]` matches any letter or number.



Example:
`[abc]\d?` = a9, b, c0
`[a-z]` = a, b, c, ...
`[A-Z]` = A, B, C, ...
`[0-9]` = 0, 1, 2, 3, ...
`[A-Z]{3}` = ABC, DEF, ...
`\d[B]+` = 0B, 1BBB, 123BBB




### Capture Groups `(...)` (The "Save This" box)

Parentheses group things together, and they tell Regex: _"Hey, remember the specific text that matched inside here so I can extract it later."_

- This is what we used in your video code script! `stream_id-([a-zA-Z0-9]+)` says: look for "stream_id-", and then **capture** the letters and numbers that follow it.



`stream_id-([A-Za-Z0-9]+)`:
stream_id-abc09: abc09 
stream_id-a: a
stream-id-09990asvz: 09990asvz


```c#

var match = Regex.Match(text, "\w{3}-(d{3})-w{2}")

if (match.Success)
{
	return match.Groups[1].Value;
}
else 
{
	return null;
}

```
`abc-123-db`: 123
`abc-444-22`: 444


