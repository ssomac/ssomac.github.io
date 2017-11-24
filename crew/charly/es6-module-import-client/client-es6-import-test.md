#ES6 module import (Client)

* Test client
```
Google Chrome (Desktop)
Version 62.0.3202.94 (Official Build) (64-bit)
```

* /module/module_1.js
```
var module_1 = (function () {
	return {
		f: function () { console.log("module-1.f()") },
		v: "charly-module-1"
	}
})();
export { module_1 };
```

* /module/module_2.js
```
export function module_2() {
    return {
        f: function () {
            console.log("module_2.f()");
        },
        v: "charly module_2"
    }
}
```

* /html/import-test.html
```
<!DOCTYPE html>
<html>
<head>
	<meta charset="UTF-8" />
</head>
<body>
	<script type="module">
		console.log("1 context: " + this);
		import { module_1 } from "/module/module_1.js";
		module_1.f();
		console.log(module_1.v);

		import { module_2 } from "/module/module_2.js";
		const module2 = module_2();
		module2.f();
		console.log(module2.v);
	</script>
	<script type="module">
		console.log("2 context: " + this);
		// module_1.f(); // Uncaught ReferenceError
	</script>
	<script>
		console.log("3 context: " + this);
		// console.log(module_3); // Uncaught ReferenceError
	</script>
</body>
</html>
```

* output: http://localhost:3000/html/import-test.html
```
import-test.html:26 3 context: [object Window]
import-test.html:11 1 context: undefined
module_1.js:3 module-1.f()
import-test.html:14 charly-module-1
module_2.js:4 module_2.f()
import-test.html:19 charly module_2
import-test.html:22 2 context: undefined
```