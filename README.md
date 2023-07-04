Demonstration of a gotcha of `nx watch ...` command. If one of the watched files has a timestamp change, but content remains the same, it can cause an infinite loop.

```shell
nx watch --projects=demo -- nx build demo
```

Then update a file, such as `packages/demo/src/index.ts`, then watch as the build keeps looping forever. This happens because the `build` target depends on `gen-schema`, and `nx gen-schema demo` updates the timestamp of `packages/demo/src/schema.ts`. Thus, the watch re-runs the build, which re-runs `gen-schema`, and so on.

A workaround is to make sure `gen-schema` is cacheable, so Nx does not re-run the target if the source has not changed.

