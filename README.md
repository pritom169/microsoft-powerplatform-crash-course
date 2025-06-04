# Microsoft Power Platform Fundamentals
## String Functions in Power Automate
- Go to power automate
- Select `+ Create` from the side menu
- Click on the option `Build an instant cloud flow`
- Then give a name to the flow, in my case `StringFunctions` and select `Manually trigger a flow`. It will lead
us to the canvas.
- Once we are there, we have to choose `+` button which means `Create new action`. From there we will choose `variable`
`initialize new variable`
  - Inside the action, `Name` will be renamed to `OurTextFirstPart`
  - The `Type` will be `String`
  - The value is `Mr. and Mrs. Dursley of number four, Privet Drive, were proud to say that they were perfectly normal`
- We will also do the same for the second variable
  - Inside the action, `Name` will be renamed to `OurTextSecondPart`
  - The `Type` will be `String`
  - The value will be ` , thank you very much.`

| Variable Name | Type | Value |
|---------------|------|-------|
| `OurTextFirstPart` | String | `Mr. and Mrs. Dursley of number four, Privet Drive, were proud to say that they were perfectly normal` |
| `OurTextSecondPart` | String | `, thank you very much.` |


### Table of Contents
- [CONCAT](#1-concat-function)
- [SUBSTRING](#2-substring-function)
- [SLICE](#3-slice-function)
- [REPLACE](#4-replace-function)
- [TOUPPER/TOLOWER](#5-toupper--tolower-functions)
- [INDEXOF](#6-indexof-function)
- [NTHINDEXOF](#7-nthindexof-function)
- [LASTINDEXOF](#8-lastindexof-function)
- [STARTSWITH/ENDSWITH](#9-startswith--endswith-functions)
- [SPLIT](#10-split-function)
- [TRIM](#11-trim-function)

## 1. CONCAT Function
**Purpose**: Combines two or more strings together

**Expression**:
```
concat(variables('OurTextFirstPart'), variables('OurTextSecondPart'))
```

**Result**: `Mr. and Mrs. Dursley of number four, Privet Drive, were proud to say that they were perfectly normal, thank you very much.`

---

## 2. SUBSTRING Function
**Purpose**: Extracts a portion of a string starting from a specified position

**Expression**:
```
substring(variables('OurTextFirstPart'), 0, 15)
```
**Result**: `Mr. and Mrs. Du`

**Another example**:
```
substring(variables('OurTextFirstPart'), 16, 20)
```
**Result**: `rsley of number four`

---

## 3. SLICE Function
**Purpose**: Similar to substring but with different syntax - extracts characters between two positions

**Expression**:
```
slice(variables('OurTextFirstPart'), 0, 15)
```
**Result**: `Mr. and Mrs. Du`

**From position 50 to end**:
```
slice(variables('OurTextFirstPart'), 50, length(variables('OurTextFirstPart')))
```

---

## 4. REPLACE Function
**Purpose**: Replaces all occurrences of a specified string with another string

**Expression**:
```
replace(variables('OurTextFirstPart'), 'Dursley', 'Smith')
```
**Result**: `Mr. and Mrs. Smith of number four, Privet Drive, were proud to say that they were perfectly normal`

**Replace spaces with underscores**:
```
replace(variables('OurTextFirstPart'), ' ', '_')
```

---

## 5. TOUPPER / TOLOWER Functions
**Purpose**: Converts string to uppercase or lowercase

**toUpper Expression**:
```
toUpper(variables('OurTextSecondPart'))
```
**Result**: `, THANK YOU VERY MUCH.`

**toLower Expression**:
```
toLower(variables('OurTextFirstPart'))
```
**Result**: `mr. and mrs. dursley of number four, privet drive, were proud to say that they were perfectly normal`

---

## 6. INDEXOF Function
**Purpose**: Finds the first occurrence of a substring and returns its position

**Expression**:
```
indexOf(variables('OurTextFirstPart'), 'Dursley')
```
**Result**: `16` (zero-based index)

**Find 'four'**:
```
indexOf(variables('OurTextFirstPart'), 'four')
```
**Result**: `37`

---

## 7. NTHINDEXOF Function
**Purpose**: Finds the nth occurrence of a substring

**Expression** (find 2nd occurrence of 'e'):
```
nthIndexOf(variables('OurTextFirstPart'), 'e', 2)
```

**Find 3rd occurrence of 'r'**:
```
nthIndexOf(variables('OurTextFirstPart'), 'r', 3)
```

---

## 8. LASTINDEXOF Function
**Purpose**: Finds the last occurrence of a substring

**Expression**:
```
lastIndexOf(variables('OurTextFirstPart'), 'e')
```
**Result**: Returns the position of the last 'e' in the string

**Find last 'r'**:
```
lastIndexOf(variables('OurTextFirstPart'), 'r')
```

---

## 9. STARTSWITH / ENDSWITH Functions
**Purpose**: Checks if a string starts or ends with a specific substring

**startsWith Expression**:
```
startsWith(variables('OurTextFirstPart'), 'Mr.')
```
**Result**: `true`

**endsWith Expression**:
```
endsWith(variables('OurTextFirstPart'), 'normal')
```
**Result**: `true`

**Check if second part ends with period**:
```
endsWith(variables('OurTextSecondPart'), '.')
```
**Result**: `true`

---

## 10. SPLIT Function
**Purpose**: Splits a string into an array based on a delimiter

### Basic Split Expression:
```
split(variables('OurTextFirstPart'), ' ')
```
**Result**: Creates an array with each word as a separate element

### Apply to Each Implementation:
1. **Add an "Apply to each" action**
2. **Set the input to**: `split(variables('OurTextFirstPart'), ' ')`
3. **Inside the loop, add a "Compose" action with**: `item()`

This will output each word individually:
- `Mr.`
- `and`
- `Mrs.`
- `Dursley`
- etc.

### Access Specific Element:
**Expression to get the 2nd element (index 1)**:
```
split(variables('OurTextFirstPart'), ' ')[1]
```
**Result**: `and`

**Expression to get the 4th element (index 3)**:
```
split(variables('OurTextFirstPart'), ' ')[3]
```
**Result**: `Dursley`

**Split by comma and get first part**:
```
split(concat(variables('OurTextFirstPart'), variables('OurTextSecondPart')), ',')[0]
```

---

## 11. TRIM Function
**Purpose**: Removes leading and trailing whitespace

**Expression**:
```
trim(variables('OurTextSecondPart'))
```
**Result**: `thank you very much.` (removes the leading comma and space)

**Trim concatenated string**:
```
trim(concat(' ', variables('OurTextFirstPart'), ' '))
```

---

## Implementation Tips

### Best Practices
1. **Create variables** for storing results of each function to test them
2. **Use "Compose" actions** to test expressions before using them in conditions
3. **Remember arrays are zero-indexed** (first element is [0], second is [1], etc.)
4. **Combine functions** for more complex operations:
   ```
   toUpper(trim(substring(variables('OurTextFirstPart'), 0, 10)))
   ```

### Common Use Cases
- **Data cleaning**: Use trim, replace, and case conversion
- **Text parsing**: Use split, indexOf, and substring
- **Validation**: Use startsWith, endsWith, and indexOf
- **Dynamic content creation**: Use concat and replace

### Testing Your Implementation
1. Create a new Power Automate flow
2. Add the two initial variables as described in Prerequisites
3. Add "Compose" actions for each function you want to test
4. Run the flow to see the results
5. Use the outputs in subsequent actions as needed

