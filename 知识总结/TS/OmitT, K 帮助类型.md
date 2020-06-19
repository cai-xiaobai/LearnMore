Omit<T, K> 帮助类型

作者： Marius Schulz

原文链接： https://mariusschulz.com/blog/the-omit-helper-type-in-typescript



https://segmentfault.com/a/1190000022429482

3.5 版本之后，TypeScript 在 *lib.es5.d.ts* 里添加了一个 `Omit<T, K>` 帮助类型。Omit<T, K> 类型让我们可以从另一个对象类型中剔除某些属性，并创建一个新的对象类型