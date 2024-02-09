# @lljj/vue3-form-core
The vue3 version core can adapt to different vue3 ui libraries based on this.

The core of adaptation is that the corresponding type is your own component library, and handles the conversion between the default `props` and the props of your own component library

> For the adaptation plan, please see [@lljj/vue3-form-element](https://github.com/lljj-x/vue-json-schema-form/tree/master/packages/lib/vue3/vue3-form -element), [@lljj/vue3-form-ant](https://github.com/lljj-x/vue-json-schema-form/tree/master/packages/lib/vue3/vue3-form-ant )


## compatibility
The npm package is directly es6+ source code and needs to be escaped through babe when building the lib.

For example, configure rollup babel plugin:

```js
babel({
    exclude: /node_modules\/(?!(@lljj)\/).*/, //Ignore skip @lljj
    extensions: ['.js', '.vue'],
})
```

## Install

```ssh
## npm
npm install --save @lljj/vue3-form-core

## yarn
yarn add @lljj/vue3-form-core
```


According to the following format, configure the mapping relationship of the corresponding component in the current component library. You can directly configure the global component name or component constructor. `The default component props are in the elementUi format. If the props format is different, an intermediate component is required for conversion`;
```js
import createVue2Core from '@lljj/vue3-form-core';

const globalOptions = {
        // Mapping relationship between widget component and existing component library    WIDGET_MAP: {
        //Map default widget components by schema type by default
      types: {
             // type boolean
             boolean: 'el-switch',

             // type string
             string: 'el-input',

             // type number
             number: 'el-input-number',

             // type integer
             integer: 'el-input-number',
         },

         //Map default widget components according to schema format, with priority higher than types
         formats: {
             // format: color
             color: 'el-color-picker',

             // format: time
             time: TimePickerWidget, // format 20:20:39+00:00

             // format: date
             date: DatePickerWidget, // format 2018-11-13

             // format: date-time
             'date-time': DateTimePickerWidget, // Format 2018-11-13T20:20:39+00:00
         },

         // Some common common types
         common: {
             //select option
             select: SelectWidget,

             //radio
             radioGroup: RadioWidget,

             // checkout
             checkboxGroup: CheckboxesWidget,
         },

         // Configure some components here that have been adapted for the current UI library. They will be automatically registered as global components at runtime. You do not need to register them as global components or configure them.
         // Vue3 can only obtain the current app within the component, so the registration time is in the form component setup, and it will only be registered once.
         widgetComponents: {
             CheckboxesWidget,
             RadioWidget,
             SelectWidget,
             TimePickerWidget,
             DatePickerWidget,
             DateTimePickerWidget
         }
     },

     // Mapping relationships of other form-related components
     COMPONENT_MAP: {
         // form component
         form: 'el-form',

         // formItem component
         formItem: 'el-form-item',

         // button component
         button: 'el-button',

         // popover, used to display the description when the mouse is moved into the left and right layout of formLable
         popover: 'el-popover'
     },
     HELPERS: {
         // Whether mini displays description
         isMiniDes(formProps) {
             return formProps && ['left', 'right'].includes(formProps.labselPosition);
         }
     }
};

const mySchemaForm = createVue2Core(globalOptions);

```

To adapt to a new UI framework, you only need to adapt the above components.

## License
Apache-2.0
