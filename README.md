## vue-json-schema-form

Based on Vue2 / Vue3, [JSON Schema](https://json-schema.org/understanding-json-schema/index.html) generate a Form with complete verification, your :star2: :star2: :star2: It’s the greatest support

[View Document](https://vue-json-schema-form.lljj.me) - [Playground](https://form.lljj.me) - [Visual Form Schema Generator](https://form .lljj.me/schema-generator.html)

## ui framework support
* [vue2 ElementUi](https://github.com/lljj-x/vue-json-schema-form/tree/master/packages/lib/vue2/vue2-form-element)
* [vue2 Iview3](https://github.com/lljj-x/vue-json-schema-form/tree/master/packages/lib/vue2/vue2-form-iview3)
* [vue3 Element Plus](https://github.com/lljj-x/vue-json-schema-form/tree/master/packages/lib/vue3/vue3-form-element)
* [vue3 Antdv](https://github.com/lljj-x/vue-json-schema-form/tree/master/packages/lib/vue3/vue3-form-ant)
* [vue3 NaiveUi](https://github.com/lljj-x/vue-json-schema-form/tree/master/packages/lib/vue3/vue3-form-naive)


1. Install dependencies

```ssh
yarn install
```


2. Run `Playground/Form Schema Generator/Activity Editor` at the same time
```ssh
# Visual form Schema editor http://127.0.0.1:8800/schema-generator.html
# (H5) Activity Editor http://127.0.0.1:8800/vue-editor.html

yarn run demo:dev
```

3. Single operation (specifying entry compiles faster)
```ssh
# Just run the playground
yarn run demo:dev --dir=index

# Only run the form Schema generator
yarn run demo:dev --dir=schema-generator

# Only run the (H5) active editor
yarn run demo:dev --dir=vue-editor
```
### illustrate
* Following the `JSON Schema` specification, you only need to give `JSON Schema` to generate the corresponding form.
* Quickly configure personalized UI views and verify error messages, and can be adapted to commonly used UI libraries
* Use [ajv](https://github.com/epoberezkin/ajv) for form schema verification
* Design ideas and schema parsing index reference [react-jsonschema-form](https://github.com/rjsf-team/react-jsonschema-form)

## Relevant information
[JSON Schema](https://json-schema.org/understanding-json-schema/index.html) |
[Vue](https://cn.vuejs.org/)

### Why develop
When doing front-end visual editing, in order to solve the versatility of the data configuration form, `JSON Schema` is used to describe the data structure and dynamically generate the form.

The advantage of this is that in addition to solving the duplication of work in each configuration form, the server can also maintain the same verification rules as the front-end based on the same schema. However, for the use of vue elementUi, no suitable library has been found that can be used directly, so in the following paragraph It's time to decide to implement one yourself.
