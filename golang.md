# Golang Tips

## Developing and testing against unpublished module code

This is explained in detail [here](https://golang.org/doc/modules/managing-dependencies#unpublished).

**Example:**

I am developing on two different Golang projects using modules:
* github.com/manoj-gupta/go-utils module ( it has helper functions)
* github.com/manoj-gupta/go-toolkit module(that depends on the first one)

After making changes to `go-utils`, I need this in `go-toolkit` to test the code.

In `go-toolkit`, do the following:

```
▶ go mod edit -replace github.com/manoj-gupta/go-utils=../go-utils                                     
```

`go.mod` will get updated to take the local copy of code

```
diff --git a/go.mod b/go.mod
index 823145a9b..5693fc1b4 100644
--- a/go.mod
+++ b/go.mod
@@ -44,3 +44,5 @@ require (
        golang.org/x/time v0.0.0-20210220033141-f8bda1e9f3ba
        google.golang.org/protobuf v1.25.0
 )
+
+replace github.com/manoj-gupta/go-utils => ../go-utils
```

You are all set to test your code.

**Note:** In `go mod edit` you can give the absolute path as well. I have used relative path here.
