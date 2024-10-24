# Reason
1. could be done in minifier
# Diff
## /out/keep.js
### esbuild
```js
foo("_keepThisProperty");
foo((x, "_keepThisProperty"));
foo(x ? "_keepThisProperty" : "_keepThisPropertyToo");
x[foo("_keepThisProperty")];
x?.[foo("_keepThisProperty")];
({ [foo("_keepThisProperty")]: x });
(class {
  [foo("_keepThisProperty")] = x;
});
var { [foo("_keepThisProperty")]: x } = y;
foo("_keepThisProperty") in x;
```
### rolldown
```js

//#region keep.js
foo("_keepThisProperty");
foo((x, "_keepThisProperty"));
foo(x ? "_keepThisProperty" : "_keepThisPropertyToo");
x[foo("_keepThisProperty")];
x?.[foo("_keepThisProperty")];
foo("_keepThisProperty");
(class {
	[foo("_keepThisProperty")] = x;
});
var { [foo("_keepThisProperty")]: x } = y;
foo("_keepThisProperty") in x;

//#endregion
```
### diff
```diff
===================================================================
--- esbuild	/out/keep.js
+++ rolldown	keep.js
@@ -2,11 +2,9 @@
 foo((x, "_keepThisProperty"));
 foo(x ? "_keepThisProperty" : "_keepThisPropertyToo");
 x[foo("_keepThisProperty")];
 x?.[foo("_keepThisProperty")];
-({
-    [foo("_keepThisProperty")]: x
-});
+foo("_keepThisProperty");
 (class {
     [foo("_keepThisProperty")] = x;
 });
 var {[foo("_keepThisProperty")]: x} = y;

```
## /out/mangle.js
### esbuild
```js
x.a;
x?.a;
x[y ? "a" : z];
x?.[y ? "a" : z];
x[y ? z : "a"];
x?.[y ? z : "a"];
x[y, "a"];
x?.[y, "a"];
({ a: x });
({ ["a"]: x });
({ [(y, "a")]: x });
(class {
  a = x;
});
(class {
  ["a"] = x;
});
(class {
  [(y, "a")] = x;
});
var { a: x } = y;
var { ["a"]: x } = y;
var { [(z, "a")]: x } = y;
"a" in x;
(y ? "a" : z) in x;
(y ? z : "a") in x;
(y, "a") in x;
```
### rolldown
```js

//#region mangle.js
x["_mangleThis"];
x?.["_mangleThis"];
x[y ? "_mangleThis" : z];
x?.[y ? "_mangleThis" : z];
x[y ? z : "_mangleThis"];
x?.[y ? z : "_mangleThis"];
x[y, "_mangleThis"];
x?.[y, "_mangleThis"];
(class {
	[(y, "_mangleThis")] = x;
});
var { "_mangleThis": x } = y;
var { ["_mangleThis"]: x } = y;
var { [(z, "_mangleThis")]: x } = y;
"_mangleThis" in x;
(y ? "_mangleThis" : z) in x;
(y ? z : "_mangleThis") in x;
(y, "_mangleThis") in x;

//#endregion
```
### diff
```diff
===================================================================
--- esbuild	/out/mangle.js
+++ rolldown	mangle.js
@@ -1,33 +1,18 @@
-x.a;
-x?.a;
-x[y ? "a" : z];
-x?.[y ? "a" : z];
-x[y ? z : "a"];
-x?.[y ? z : "a"];
-x[(y, "a")];
-x?.[(y, "a")];
-({
-    a: x
-});
-({
-    ["a"]: x
-});
-({
-    [(y, "a")]: x
-});
+x["_mangleThis"];
+x?.["_mangleThis"];
+x[y ? "_mangleThis" : z];
+x?.[y ? "_mangleThis" : z];
+x[y ? z : "_mangleThis"];
+x?.[y ? z : "_mangleThis"];
+x[(y, "_mangleThis")];
+x?.[(y, "_mangleThis")];
 (class {
-    a = x;
+    [(y, "_mangleThis")] = x;
 });
-(class {
-    ["a"] = x;
-});
-(class {
-    [(y, "a")] = x;
-});
-var {a: x} = y;
-var {["a"]: x} = y;
-var {[(z, "a")]: x} = y;
-("a" in x);
-((y ? "a" : z) in x);
-((y ? z : "a") in x);
-((y, "a") in x);
+var {"_mangleThis": x} = y;
+var {["_mangleThis"]: x} = y;
+var {[(z, "_mangleThis")]: x} = y;
+("_mangleThis" in x);
+((y ? "_mangleThis" : z) in x);
+((y ? z : "_mangleThis") in x);
+((y, "_mangleThis") in x);

```