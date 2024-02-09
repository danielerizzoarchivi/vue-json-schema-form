##Introduzione
Prima di iniziare, assicurati di comprendere [Vue](https://vuejs.org/) e [JsonSchema](https://json-schema.org/understanding-json-schema/index.html)

## Esperienza
* [Demo di Playground](https://form.lljj.me/ "Vue JSON Schema Form Playground")
* [Vue Visual Activity Editor](https://form.lljj.me/vue-editor.html)
* [Generatore di schemi del modulo visivo](https://form.lljj.me/schema-generator.html "Generatore di schemi del modulo visivo Vue JSON Schema Form")

## Selezione di più versioni
Sono supportate le seguenti versioni Vue e framework Ui. Seleziona la versione in base al framework del tuo progetto.

**L'API e i modelli di utilizzo di ciascuna versione sono coerenti al 99%, con solo le seguenti differenze: **
::: avviso Differenze tra le versioni
* Il prefisso on verrà rimosso dagli eventi di emissione di vue3. Per i dettagli, vedere [Evento Emit Evento](/zh/guide/basic-config.html#Event-emit-event)
* vue3 e Vue `v-model` non utilizzano le prop `modelValue`, è richiesta una conversione qui, [vedi dettagli](/zh/guide/#vue3-ant-v-model-%E7%89%B9%E6 % AE%8A%E5%A4%84%E7%90%86)
:::

### @lljj/vue-json-schema-form
* Libreria dell'interfaccia utente di adattamento: `Vue2` `ElementUi`
  * Tieni presente che i componenti relativi all'elemento devono essere registrati a livello globale e possono anche essere utilizzati secondo necessità in base alle istruzioni della console.
* nome del pacchetto: `@lljj/vue-json-schema-form`
* indirizzo umd cdn:[@lljj/vue-json-schema-form cdn](https://npm.elemecdn.com/@lljj/vue-json-schema-form/dist/vueJsonSchemaForm.umd.min.js)
* Il modulo del tag script umd introduce la variabile globale esposta `window.vueJsonSchemaForm`, il componente esposto `window.vueJsonSchemaForm.default` e registra anche il componente globale `VueForm`
* [parco giochi](https://form.lljj.me/#/demo?type=Simple)

### @lljj/vue2-form-iview3
* Libreria dell'interfaccia utente di adattamento: `Vue2` `iview3`
  * Tieni presente che i componenti correlati a iview3 devono essere registrati a livello globale e possono anche essere utilizzati secondo necessità in base alle istruzioni della console.
* nome del pacchetto: `@lljj/vue2-form-iview3`
* umd cdn地址：[@lljj/vue2-form-iview3 cdn](https://npm.elemecdn.com/@lljj/vue2-form-iview3/dist/vue2-form-iview3.umd.min.js)
* Il modulo tag script umd introduce la variabile globale esposta `window.vue2FormIview3`, il componente esposto `window.vue2FormIview3.default` e registra il componente globale `vue2FormIview3`.
* [parco giochi](https://form.lljj.me/#/demo?type=Simple&ui=VueIview3Form)

### @lljj/vue3-form-element
* Libreria dell'interfaccia utente di adattamento: `Vue3` `ElementPlus`
    * Tieni presente che devi registrare i componenti correlati a ElementPlus a livello globale e puoi anche utilizzarli secondo necessità in base alle istruzioni della console.
* nome del pacchetto: `@lljj/vue3-form-element`
* umd cdn地址：[@lljj/vue3-form-element cdn](https://npm.elemecdn.com/@lljj/vue3-form-element/dist/vue3-form-element.umd.min.js)
* Il modulo del tag script umd introduce le variabili globali esposte `window.vue3FormElement` e i componenti esposti `window.vue3FormElement.default`
* [parco giochi](https://form.lljj.me/v3/#/demo?type=Simple)

### @lljj/vue3-form-naive
* Libreria dell'interfaccia utente di adattamento: `Vue3` `naive`
  * Tieni presente che è necessario registrare i componenti correlati a vue3 naive a livello globale e puoi anche utilizzarli secondo necessità in base alle istruzioni della console.
* nome del pacchetto: `@lljj/vue3-form-naive`
* umd cdn in:[@lljj/vue3-form-naive cdn](https://npm.elemecdn.com/@lljj/vue3-form-naive/dist/vue3-form-naive.umd.min.js) ;
* Il modulo del tag script umd introduce le variabili globali esposte `window.vue3FormNaive` e i componenti esposti `window.vue3FormNaive.default`
* [parco giochi](https://form.lljj.me/v3/#/demo?type=Simple&ui=VueNaiveForm)

### @lljj/vue3-form-ant
* Libreria dell'interfaccia utente di adattamento: `Vue3` `antdv`
    * Tieni presente che i componenti relativi a vue3 antdv devono essere registrati a livello globale e possono anche essere utilizzati secondo necessità in base alle istruzioni della console.
* nome del pacchetto: `@lljj/vue3-form-ant`
* Indirizzo umd cdn:[@lljj/vue3-form-ant cdn](https://npm.elemecdn.com/@lljj/vue3-form-ant/dist/vue3-form-ant.umd.min.js)
* Il modulo del tag script umd introduce le variabili globali esposte `window.vue3FormAnt` e i componenti esposti `window.vue3FormAnt.default`
* [parco giochi](https://form.lljj.me/v3/#/demo?type=Simple&ui=VueAntForm)

::: avviso e nota sulla versione 4x:
* Utilizzare la versione v4, importare { JsonSchemaFormAntdV4 } da "@lljj/vue3-form-ant";
* Si consiglia di utilizzare l'esportazione predefinita per la versione v3
:::

#### vue3 ant, elaborazione speciale naiveUi v-model
Ad esempio, per il componente "a-input", ant vue3 deve utilizzare "v-model:value", ma "v-model" utilizza "modelValue" all'interno dell'intero framework, quindi le proposte incoerenti devono essere elaborate attraverso il componente intermedio gruppo di componenti Converti.

Puoi convertirlo tu stesso oppure puoi utilizzare il metodo integrato "modelValueComponent" per convertirlo, come segue:
```js
// Restituisce un componente che accetta modelValue e update:modelValue v-model
import { modelValueComponent } da '@lljj/vue3-form-ant';
const MyFixInputComponent = modelValueComponent('a-input', {
    model: 'value' // Questo dovrebbe essere passato in base ai parametri del modello del componente ant
});

// anche naive è un'operazione simile
import { modelValueComponent } da '@lljj/vue3-form-naive';
const MyFixInputComponent = modelValueComponent('n-input', {
    model: 'value' // Questo dovrebbe essere passato in base ai parametri del modello del componente naive
});
```

:::mancia
Questo è ancora un po' problematico da usare. Attualmente, i componenti Widget comunemente utilizzati sono stati integrati.
Vedi [ant, componente widget globale aggiuntivo naiveUi vue] (/zh/guide/components.html#vue3-ant, componente globale naiveui-unique)
:::

## Avvio rapido
> **I documenti seguenti utilizzano `@lljj/vue-json-schema-form` come esempio**

### npm

```bash
# Installa
npm install --save @lljj/vue-json-schema-form

# filato
filato aggiungi @lljj/vue-json-schema-form
```

* utilizzo
```js
importa VueForm da '@lljj/vue-json-schema-form';
importa Vue da 'vue';

// Registrarsi a livello globale o registrarsi all'interno del componente
Vue.component('VueForm', VueForm);
```

### introduzione allo script
```html
#introduzione alla sceneggiatura
<script src="//npm.elemecdn.com/@lljj/vue-json-schema-form/dist/vueJsonSchemaForm.umd.min.js"></script>
```

##DEMO
Dimostra un modulo che esegue il rendering delle informazioni dell'utente. Fare clic su Mostra codice per visualizzare il codice sorgente o eseguirlo in codepen.

::: dimostrazione
```html
<modello>
    <visualizza-modulo
        v-model="formData"
        :ui-schema="uiSchema"
        :schema="schema"
    >
    </vue-form>
</modello>

<copione>
esportazione predefinita {
    nome: 'Dimostrazione',
    dati() {
        ritorno {
            formData: {},
            schema: {
                tipo: 'oggetto',
                necessario: [
                    'nome utente',
                    'età',
                ],
                proprietà: {
                    nomeutente: {
                        tipo: 'stringa',
                        titolo: 'nome utente',
                        predefinito: 'Liu.Jun',
                    },
                    età: {
                        tipo: 'numero',
                        titolo: 'Età'
                    },
                    biografia: {
                        tipo: 'stringa',
                        titolo: 'firma',
                        minLunghezza: 10,
                        default: 'Più sai, meno sai',
                    }
                }
            },
            schema dell'interfaccia utente: {
                biografia: {
                    'ui:opzioni': {
                        placeholder: 'Inserisci la tua firma',
                        digitare: 'area di testo',
                        righe: 1
                    }
                }
            }
        };
    }
};
</script>
```
:::

## concetto base
Genera il modulo corrispondente tramite "JSON Schema".
* L'attributo schema `title` viene utilizzato come titolo del modulo
* L'attributo schema `description` serve come descrizione del modulo

In base alla ricorsione dei componenti, i dati vengono visualizzati passo dopo passo, come mostrato di seguito: (Fare clic per ingrandire)
![Vjsf](/vjsf.jpg)

Implica due concetti, "Campo" e "Widget".
* `Field` viene utilizzato per visualizzare il componente corrispondente a ciascun nodo, che può essere qualsiasi nodo. Generalmente, il componente conterrà il componente `FormItem`.
* `Widget` è un componente utilizzato per visualizzare le informazioni di input dell'utente, come `input`, `select`, ed è racchiuso dal componente `FormItem`
> `Field` `Widget` può essere personalizzato tramite `ui-schema`,
> Per i metodi dettagliati, vedere [Campo personalizzato](/zh/guide/adv-config.html#Campo personalizzato), [Widget personalizzato](/zh/guide/adv-config.html#Widget personalizzato)

## Metodo di esposizione
```js
importa VueForm, {
    getDefaultFormState,
    campoProps,
    vueUtils,
    formUtils,
    schemaConvalida,
    i18n
} da '@lljj/vue-json-schema-form';
```

#### VueForm
I componenti VueForm vengono esportati per impostazione predefinita

#### getDefaultFormState
Calcola il valore dell'attuale "FormState" tramite "JSON Schema".
* Schema:(schema, formData, rootSchema, includeUnDefinitedValues)

>* schema schema dell'oggetto da calcolare
>* formData `object` Il valore corrente del formData, se non disponibile, può essere passato `unDefinito`
>* rootSchema `object` Lo schema del nodo root dello schema che deve essere calcolato
>* includeUnDefinitedValues ​​`boolean` Indica se includere valori non definiti, predefinito `true`

> Non utilizzare `ui-schema` `ui:field` generalmente non viene utilizzato

#### campoProps
Configurazione delle prop del campo, se devi utilizzare `ui:field` per personalizzare il componente del campo, devi usarlo per definire le prop del componente
> Non utilizzare `ui-schema` `ui:field` generalmente non viene utilizzato

#### vueUtils
Fornisce alcuni metodi di utilità interni relativi a Vue. Per i dettagli, [vedi il codice sorgente](https://github.com/lljj-x/vue-json-schema-form/blob/master/packages/lib/utils/vueUtils .js)
> Non utilizzare `ui-schema` `ui:field` generalmente non viene utilizzato

#### formUtils
Fornire alcuni metodi di utilità interni relativi al modulo. Per i dettagli, [vedere il codice sorgente](https://github.com/lljj-x/vue-json-schema-form/blob/master/packages/lib/utils/formUtils .js)
> Non utilizzare `ui-schema` `ui:field` generalmente non viene utilizzato

#### schemaValidate
Fornisce alcuni metodi relativi alla verifica interna dello schema. Per i dettagli, [vedi il codice sorgente](https://github.com/lljj-x/vue-json-schema-form/blob/master/packages/lib/utils/ schema/validate.js)
> Non utilizzare `ui-schema` `ui:field` generalmente non viene utilizzato

## illustrare
* Seguendo le specifiche dello schema JSON, devi solo fornire `JSON Schema` per generare il modulo corrispondente.
* Configura rapidamente visualizzazioni dell'interfaccia utente personalizzate e verifica i messaggi di errore, che possono essere adattati alle librerie dell'interfaccia utente di uso comune. Puoi adattarti a ElementUi, iView o alle librerie di componenti sviluppate da te tramite la configurazione.
* Utilizzare [ajv](https://github.com/epoberezkin/ajv) per la verifica dello schema del modulo
* Idee di progettazione e riferimento all'indice di analisi dello schema [react-jsonschema-form](https://github.com/rjsf-team/react-jsonschema-form)
