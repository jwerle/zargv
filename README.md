zargv
=====

> Parse command line arguments supporting a variety of flag formats.

## Installation

In your project's `zz.toml` file, add the following:

```toml
[repos]
zargv = "git://github.com/jwerle/zargv.git"
list = "git://github.com/jwerle/zz-list.git" ## peer dep

[dependencies]
zargv = "*"
```

## Usage

```c++
using zargv::{ ArgumentOptions }
using zargv

fn main(int argc, char **argv) -> int {
  int mut num = 0;
  bool mut help = false;
  bool mut version = false;
  char * mut name;
  char * mut array[3];

  new args = zargv::parser();
  args.int(&num, ArgumentOptions { name: "num", alias: "n" });
  args.bool(&help, ArgumentOptions { name: "help", alias: "h" });
  args.bool(&version, ArgumentOptions { name: "version", alias: "V" });
  args.string(&name, ArgumentOptions { name: "name", alias: "N" });
  args.array(&array, ArgumentOptions { name: "array", alias: "A" });
  args.parse(argv, argc);
  return 0;
}
```

## API

### `new+tail args = zargv::parser()`

Initializes a new `Parser` with tail.

```c++
new+4 args = zargv::parser();
```

### `args.int(out, opts)`

Creates an `int` argument from pointer and options.

```c++
int mut num = 3000; // 3000 is the default value
args.int(&num, ArgumentOptions {
  name: "port", alias: "p"
})
```

### `args.uint(out, opts)`

Creates an `uint` argument from pointer and options.

```c++
uint mut num = 3000; // 3000 is the default value
args.int(&num, ArgumentOptions {
  name: "port", alias: "p"
})
```

### `args.float(out, opts)`

Creates a `float` argument from pointer and options.

```c++
float mut num = 0.0;
args.float(&num, ArgumentOptions {
  name: "num"
})
```

### `args.double(out, opts)`

Creates a `double` argument from pointer and options.

```c++
double mut num = 0.0;
args.doublr(&num, ArgumentOptions {
  name: "num"
})
```

### `args.bool(out, opts)`

Creates a `bool` argument from pointer and options.

```c++
bool mut boolean = false;
args.float(&boolean ArgumentOptions {
  name: "boolean"
})
```

### `args.string(out, opts)`

Creates a `string` argument from pointer and options.

```c++
char mut *host = 0;
args.string(&host, ArgumentOptions {
  name: "host"
});
```

### `args.array(out, opts)`

Creates an `array` argument from pointer and options. A max number of items
to add to this array must be given with `options.max`. A type must be
specified otherwise a string type is assumed.

```c++
char * mut names[3];
args.array(&array, ArgumentOptions {
  name: "name"
});
```

### `args.rest(out, opts)`

Creates a `rest` argument from pointer and options where all arguments
after the `--` in a argument list are added to this pointer. A max number of
items must be given with `options.max`. The default is `1`. This function
assumes a pointer to a `char *[]` array that can hold at least `options.max`
items.

```c++
char * mut rest[3];
args.array(&rest, ArgumentOptions {
  max: 3
});
```

### `args.unkown(out, opts)`

Creates a container for unknown arguments to be mapped to. A given pointer
must be large enough to hold `options.max` unknown arguments.

```c++
char * mut unknown[3];
args.array(&unkown, ArgumentOptions {
  max: 3
});
```

### `args.parse(argv, argc)`

Parses `argc` arguments in `char **argv` mapping values to argument pointers
specified by the various argument schema functions.

```c++
// `argv` and `argc` from `main()`
args.parse(argv, argc);
```

## License

MIT
