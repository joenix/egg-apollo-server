# graphql 学习笔记

## `apollo-server`

- typeDefs 定义 graphql 类型
- resolvers 定义解析 graphql 的函数 resolvers 函数接收四个参数

```
fieldName: (parent, args, context, info) => data;
```

    - parent: An object that contains the result returned from the resolver on the
      parent type
    - args: An object that contains the arguments passed to the field
    - context: An object shared by all resolvers in a GraphQL operation. We use the
      context to contain per-request state such as authentication information and
      access our data sources. context 的值为new ApolloServer传入的context,同时 context.dataSources=dataSources

```js
 function initializeDataSources() {
    if (config.dataSources) {
      const context = requestContext.context;

      const dataSources = config.dataSources();

      for (const dataSource of Object.values(dataSources)) {
        if (dataSource.initialize) {
          dataSource.initialize({
            context,
            cache: requestContext.cache,
          });
        }
      }

      if ('dataSources' in context) {
        throw new Error(
          'Please use the dataSources config option instead of putting dataSources on the context yourself.',
        );
      }

      (context as any).dataSources = dataSources;
    }
  }
}
```

    - info: Information about the execution state of the operation which should only be used in advanced cases

- context 上下文 可以设置为函数，函数接受到的参数
  在`apollo-server`,`apollo-server-express`中是{req,res} 在`apollo-server-koa`中
  是{ctx}
- dataSources 可以设置为函数，函数返回值是对象,对象每个 key 对应的 value 是一个
  [DataSource](https://github.com/apollographql/apollo-server/tree/master/packages/apollo-datasource)
  函数，

```js
new ApolloServer({
	typeDefs,
	resolvers,
	context,
	dataSources: () => ({
		launchAPI: new LaunchAPI(),
		userAPI: new UserAPI({ store })
	})
});
```

注释有两种""或者""" """
