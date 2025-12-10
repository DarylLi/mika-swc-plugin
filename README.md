# mika-swc-plugin

compile ts or jsx code on brower side

## Get started

```bash
npm install mika-swc-plugin
```

### 1. 针对 ts 内容简易设置

```javascript
//transform typescript code:
import { useEffect, useState } from "react";
import initSwc, { transformSync } from "@swc/wasm-web";

const tsCode = `
interface User {
  name: string;
  age: number;
}

const greet = (user: User): string => {
  return \`Hello, \${user.name}!\`;
};
`;
async function importAndRunSwcOnMount() {
  await initSwc();
  const result = await transformSync(tsCode, {
    jsc: {
      parser: {
        syntax: "typescript",
        tsx: false,
      },
      target: "es2020",
    },
  });
  return result;
}
importAndRunSwcOnMount().then((result) => {
  console.log(result.code);
});
```

### 2. transform jsx file

```javascript
//transform jsx format file content:
const jsxCode = `

const Component = ({ title, count }) => {
  return <div><h1>{title}</h1><p>{count}</p></div>;
};
`;

const output = transformSync(jsxCode, {
  module: { type: "es6" },
  minify: false,
  sourceMaps: false,
  jsc: {
    parser: {
      syntax: "ecmascript",
      jsx: true,
      // tsx: true, // transform tsx
      minify: { compress: { unused: true } },
    },
    transform: {
      react: {
        runtime: "classic", //"automatic" | "classic" | "preserve"
        pragma: "React.createElement",
        pragmaFrag: "React.Fragment",
      },
    },
    target: "es2020",
  },
  module: {
    type: "commonjs",
  },
}).code;
```

### 3. 基础压缩

```javascript
const result = await transform(code, {
  minify: true, // 简单启用压缩
  jsc: {
    parser: { syntax: "ecmascript", jsx: true },
    target: "es5",
  },
});
```

### 4. 详细压缩配置

```javascript
const result = await transform(code, {
  jsc: {
    parser: { syntax: "ecmascript", jsx: true },
    minify: {
      compress: {
        unused: true, // 删除未使用的变量
        dead_code: true, // 删除死代码
        drop_console: true, // 删除 console.*
        drop_debugger: true, // 删除 debugger
        collapse_vars: true, // 内联变量
        evaluate: true, // 求值常量表达式
        if_return: true, // 优化 if/return
        join_vars: true, // 合并 var 声明
        loops: true, // 优化循环
        reduce_vars: true, // 优化重复变量
        side_effects: true, // 删除无副作用代码
        switches: true, // 优化 switch
        typeofs: true, // 优化 typeof
        booleans: true, // 优化布尔值
        conditionals: true, // 优化条件语句
        properties: true, // 优化属性访问
        keep_fargs: true, // 保持函数参数
        keep_fnames: false, // 不保持函数名
        keep_classnames: false, // 不保持类名
      },
      mangle: {
        toplevel: true, // 混淆顶层作用域
        keep_classnames: false, // 不保持类名
        keep_fnames: false, // 不保持函数名
        keep_private_props: false, // 不保持私有属性
        safari10: false, // Safari 10 兼容
      },
      format: {
        comments: false, // 删除注释
        beautify: false, // 不美化
        braces: false, // 不总是使用大括号
      },
    },
    target: "es5",
  },
  minify: true,
});
```
## why use swc ?
<img width="2422" height="1148" alt="01640f542ee02ab92254a3f504e687e9" src="https://github.com/user-attachments/assets/d4646a60-78cd-4286-b9b0-085ba15d1c8b" />
<img width="2354" height="1032" alt="502396b9846a6958e7299f16df74070f" src="https://github.com/user-attachments/assets/bd12f4aa-d0f5-4201-8a27-524597d72ffa" />


## reference

- [@swc/wasm-web](https://swc.rs/docs/usage/wasm) - This modules allows you to synchronously transform code inside the browser using WebAssembly.
- [SWC 官方文档](https://swc.rs/)
- [SWC Playground](https://play.swc.rs/)
- [GitHub 仓库](https://github.com/swc-project/swc)
- [@swc/wasm-web NPM](https://www.npmjs.com/package/@swc/wasm-web)
