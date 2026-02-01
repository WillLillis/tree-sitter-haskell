# Testing locale support in wasm parsers

Build the parser in debug mode for wasm, and run a single test. I've added some really hacky debugging by
printing out the size passed to the wasm stdlib's `malloc` implementation via `tree_sitter_debug_message`.
The test illustrates that the default locale is `"C"`, and `iswupper(L'\u053d)` with this locale returns
`false`. We then set the locale to `"en_US.utf8"`, and confirm this with the return value. After setting
this locale, `iswupper(L'\u053d)` returns `true`.

Because the tree-sitter wasm stdlib doesn't currently export `setlocale`, the first call to `iswupper` should
match the behavior of all parsers in wasm currently, meaning there isn't any unicode support builtin. This
support can be added, but at a fairly steep cost in size for the wasm stdlib.

```
tree-sitter t --wasm --include "char: symbols" --debug-build
```
