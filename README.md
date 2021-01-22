# Dev Lens

It's aspecification and implementation of dev-lens protocol and toolchain

# Lens

Every dev lens is cli tool, which can get some flags. After run it read current state from stdin, make something with state and prints new state to stdout with zero return code.

Lens must be pure function in terms of files, it cannot load anything from filesystem nor assumes anything about it.

In such case lens should fails with clear stdout, non-zero return code and message error in stderr.

# State

State is bunch of files, which is delimited by some random line at file beginning. Delimiter always should be wrapped with newline characters `0xA`, even on first line. There is should not be delimiter at last line of state.

So first line of state must be a newline character, followed by delimiter, followed by newline.

After delimiter there is always file URN, followed by newline, followed by file content. Files can be binary!

First file have special meaning - it's a state manifest, in JSON. Manifest should not appears in itself.
Manifest always has `urn:x-dev-lens:manifest` URN.

Format:
```

xxx-some-random-line-maybe-UUID-xxx
urn:x-dev-lens:manifest
{
  "types": {
    "urn:file:some-file-id": "app/js"
  }
}
xxx-some-random-line-maybe-UUID-xxx
urn:file:some-file-id
console.log("Hello, it's JS example!")
```

# Resolver

Resolvers are resolving (huh?..) URNs. It still follows lens API (so it's cli tool, with flags, stdin/out/er etc), but it completely domain-specific and can access filesystem, net, etc...

# Core

Core is responsible for piping lens and resolvers in specific order, which is defined by some sort of user config. It can apply some optimizations, based on lens support.

# Optimizations

Filters for decreasing input

Diffs for decreasing output

Shared memory communication for less copies

Daemon mode