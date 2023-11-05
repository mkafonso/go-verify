  <h2 align="center">
   ##  GO SCHEMA ## 
  </h2>


This open-source project is a schema validation toolkit inspired by [zod.dev](https://zod.dev/). Feel free to use and modify it for any purpose. If you have any suggestions for improvement, please don't hesitate to reach out. Thank you!


## Contents

- [Getting started](#how-to-use)
- Basic Usage
  - [String Validation](#string-validation)
  - [Number Validation](#number-validation)
  - [Struct Validation](#struct-validation)
 

## <div id="how-to-use" />How to use

Add the package to your project using the following command:

```go
go get github.com/mkafonso/go-schema
```

Import the package in your project:

```go
import (
  "github.com/mkafonso/go-schema/z"
)
```

Once you've added the package and imported it into your project, you can start using the schema validation toolkit for your specific needs.


## <div id="string-validation" />String Validation

In this section, we'll explore the fundamental usage of the schema validation toolkit for strings using the go-schema package. You'll learn how to create a schema and perform various string validations.

In the first example, we create a string schema using z.NewStringSchema() and attempt to parse the string "Luna." Since "Luna" is a valid string, no error is expected.
```go
// Create a schema for string
schema := z.NewStringSchema()
result, err := schema.Parse("Luna")
```

In the second example, we again create a string schema. This time, we attempt to parse the value 42 as a string. Since 42 is not a valid string, we expect an error with the custom error message provided.
```go
// Expect an error when the string is invalid and return a custom error message
schema := z.NewStringSchema()
result, err := schema.Parse(42, "Custom error message")
```

In the third example, we use the same schema to parse a non-string value (42) and expect an error. The provided custom error messages are checked in order, and the first one is returned in the error message.
```go
// Expect an error when the string is invalid and return the first custom error message
schema := z.NewStringSchema()
result, err := schema.Parse(42, "First custom error message", "Second custom error message")
```

In the next example, we create a string schema and use the Min method to check the length of the string. We expect the string to be at least 2 characters long, and if it's not, a custom error message will be returned.
```go
// Check the string length with the Min Method
schema := z.NewStringSchema()
result, err := schema.Min(2, "length must be at least 2 characters").Parse("Luna")
```

Similarly, in the following example, we use the Max method to ensure that the string does not exceed a length of 10 characters. If it does, a custom error message is returned.
```go
// Check the string length with the Max Method
schema := z.NewStringSchema()
result, err := schema.Max(10, "length must not exceed 10 characters").Parse("Luna")
```

In the penultimate example, we check whether the string is a valid email address using the Email method. If the string is not a valid email, a custom error message is provided.
```go
// Check if the string is a valid email address
schema := z.NewStringSchema()
result, err := schema.Email("custom error message").Parse("email@email.com")
```

Finally, you can combine multiple validation methods to perform various checks on the string. In this example, we first check the string's length and then verify if it's a valid email address. If any validation fails, the respective custom error message is returned.
```go
// Combine methods
schema := z.NewStringSchema()
result, err := schema.Min(100, "length custom error").Email("email error message").Parse("me@there.com")
```


## <div id="number" />Number Validation

In this section, we'll explore how to create a schema for numbers using the go-schema package and perform various numeric validations.

To start, we create a numeric schema using z.NewNumberSchema() and attempt to parse the number 42.5. Since 42.5 is a valid number, no error is expected.
```go
// Create a schema for numbers
schema := z.NewNumberSchema()
result, err := schema.Parse(42.5)
```

In this example, we once again create a number schema, but this time, we attempt to parse the value "Hi" as a number. Since "Hi" is not a valid number, we expect an error with the custom error message provided.
```go
// Expect an error when the number is invalid and return a custom error message
schema := z.NewNumberSchema()
result, err := schema.Parse("Hi", "Custom error message")
```

## <div id="struct-validation" />Struct Validation

In this section, we'll explore how to perform validation on Go structs using the go-schema package. This allows you to validate and parse data into structured Go types, such as the Person struct defined in the example.


In the first example, we define a Person struct with Name and Age fields. We then create a map containing valid data for a person. We use z.ParseStruct to parse the data into a Person struct. Since the data is valid and matches the struct's structure, no error is expected.
```go
// Define a Person struct
type Person struct {
  Name string `json:"name"`
  Age  int    `json:"age"`
}

// Valid Data Example
data := map[string]interface{}{
  "name": "Alice",
  "age":  30,
}

person := Person{}
err := z.ParseStruct(data, &person)
```

In the second example, we provide data with a missing age field. We attempt to parse this data into a Person struct. Because the data is missing a required field, we expect an error.
```go
// Missing Field Example
data := map[string]interface{}{
  "name": "Bob",
}

person := Person{}
err := z.ParseStruct(data, &person)
```

The third example demonstrates an attempt to assign an invalid data type to the age field. We provide a string as the age value when it should be an integer. An error is expected in this case.
```go
// Invalid Field Type (age)
data := map[string]interface{}{
  "name": "Charlie",
  "age":  "invalid-type",
}

person := Person{}
err := z.ParseStruct(data, &person)
```

In the final example, we intentionally use the wrong reference for z.ParseStruct. Instead of passing &person to capture the result in the person variable, we mistakenly use person without the reference. This is an incorrect usage of the function and will result in an error.
```go
// Example of an Invalid Target (Wrong Reference)
data := map[string]interface{}{
  "name": "David",
  "age":  25,
}

person := Person{}
err := z.ParseStruct(data, person)
```
