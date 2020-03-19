### typescript笔记
> ###### 参考网址：
>> ###### https://ts.xcatliu.com/
>> ###### https://zhongsp.gitbooks.io/typescript-handbook/

---
##### *typescript只会进行静态检查，如果发现有错误，编译的时候就会报错，而不是在编译之后插入其他代码
---

***TypeScript 编译的时候即使报错了，还是会生成编译结果**，我们仍然可以使用这个编译之后的文件。

如果要在报错的时候终止 js 文件的生成，可以在 `tsconfig.json` 中配置 `noEmitOnError`即可。关于 `tsconfig.json`，请参阅[官方手册](http://www.typescriptlang.org/docs/handbook/tsconfig-json.html)[（中文版）](https://zhongsp.gitbooks.io/typescript-handbook/content/doc/handbook/tsconfig.json.html)。

---
