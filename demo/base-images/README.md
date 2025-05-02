# Sample Stacks

Sample of stacks.

## Development

To build the stack use the `./build` script:

```text
Usage:
  ./build.sh [-f <prefix>] [-p <platform>] <dir>
    -f    prefix to use for images      (default: cnbs/sample-base)
    -p    prefix to use for images      (default: amd64)
   <dir>  directory of stack to build
```

Example:

```bash
./build.sh -f patbaumgartner/buildpack-base alpine
./build.sh -f patbaumgartner/buildpack-base noble
```

To use this stack see the [Sample Builders](../builders)
