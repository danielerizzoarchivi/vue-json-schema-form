---
barra lateraleProfondità: 2
---

# Configurazione (API)

## Proprieta' dei parametri

### schema
* obbligatorio:`vero`
* Digitare: "oggetto".
*Valore predefinito: "non definito".

Schema JSON per descrivere i dati del modulo,
Seguire la specifica [JSON Schema](https://json-schema.org/understanding-json-schema/index.html)

```
titolo: 'Renderizza come titolo',
descrizione: 'rendered as description information' // supporta il codice html
```

Qui, se è "titolo" o "descrizione" in "oggetto" o "array", verrà visualizzato come titolo e descrizione del contenitore di wrapper "FieldGroupWrap".

Il "titolo" interno "descrizione" verrà reso dal componente "widget" come titolo e descrizione di "formItem"

:::suggerimento su come nascondersi
* Se `title` non è configurato, l'attributo `description` non verrà visualizzato.
* Caso speciale: per il tipo `object` `array`, puoi controllare se visualizzarlo tramite il parametro ['ui:showTitle': false](#ui-schema)
:::


**Esempio: Configura modulo informazioni utente**
::: dimostrazione
```html
<modello>
    <visualizza-modulo
        v-model="formData"
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
                titolo: "Modulo informazioni utente",
                descrizione: 'Un semplice esempio di modulo.',
                tipo: 'oggetto',
                necessario: [
                    'nome di battesimo',
                    'cognome'
                ],
                proprietà: {
                    nome di battesimo: {
                        tipo: 'stringa',
                        titolo: "Nome",
                        impostazione predefinita: "giugno"
                    },
                    cognome: {
                        tipo: 'stringa',
                        titolo: 'Cognome'
                    },
                    età: {
                        tipo: 'intero',
                        titolo: 'Età',
                        massimo: 80,
                        minimo: 16
                    },
                    biografia: {
                        tipo: 'stringa',
                        titolo: 'Bio',
                        Lunghezza min: 10
                    },
                    parola d'ordine: {
                        tipo: 'stringa',
                        titolo: "Password",
                        lunghezza minima: 3
                    },
                    telefono: {
                        tipo: 'stringa',
                        titolo: 'Telefono',
                        Lunghezza min: 10
                    }
                }
            }
        };
    }
};
</script>
```
:::

### schema dell'interfaccia utente
* Digitare: "oggetto".
*Valore predefinito: `{}`
* `Non richiesto`: l'interfaccia utente può anche essere configurata direttamente in `schema`

>* `0.0.16` e versioni successive supportano la configurazione di `ui-schema` nel parametro `schema` [fai clic per visualizzare] (#ui-schema è configurato nello schema)
>* Le versioni successive a `0.1.0` supportano la configurazione di [schema-errore](#schema-errore) in [schema-ui](#schema-ui). (`ui-schema` e `error-schema` hanno lo stesso formato ed entrambi appartengono alla visualizzazione dell'interfaccia utente. Una copia può essere facilmente configurata)

Utilizzato per configurare lo stile di visualizzazione del modulo, i dati JSON ordinari, la specifica non "JSON Schema".

#### configurazione della funzione fui:xxx
* Dopo la versione "1.9.0", tutte le configurazioni dell'interfaccia utente supportano la configurazione funzionale tramite "fui:xxx", che accetta tre parametri: "parentFormData", "rootFormData" e "prop".
```js
// Ad esempio, configura un attributo segnaposto dinamico
'fui:placeholder': (genitore, root, prop) => {
    console.log(genitore, root, prop);
    restituisce parent.txtColore;
}
```


#### espressione dello schema dell'interfaccia utente
* Dopo la versione "0.2", tutte le configurazioni sotto forma di "ui:xxx" supportano le espressioni (le espressioni non sono supportate in ui:opzioni per la differenziazione)
Le espressioni Moustache possono utilizzare due variabili integrate "parentFormData" e "rootFormData".
* `parentFormData` Il valore FormData del genitore del nodo corrente
* Valore FormData `rootFormData` del nodo radice

> L'espressione di configurazione restituirà il risultato tramite "nuova funzione", in modo da poter effettivamente accedere alle variabili globali nell'espressione.

Ad esempio: (Fare riferimento qui: [uiSchema utilizzando le espressioni](https://form.lljj.me/#/demo?type=uiSchema%28Expression%29))
```
'ui:title': `{{ parentFormData.age > 18 ? 'Hehehe' : 'Heyhehe' }}`
```

:::mancia
* La struttura dei dati di configurazione è coerente con "schema" e tutti gli attributi di configurazione dell'interfaccia utente iniziano con "ui:".
* Puoi anche configurare tutte le proprietà in `ui:options` senza iniziare con `ui:`
* Se `ui:xx` e `ui:options` sono configurati con l'attributo `xx`, la priorità in `ui:options` è più alta. Infatti, puoi configurare tutti i parametri in `ui:options` ` Within; qui puoi seguire le abitudini personali, si consiglia di utilizzare il seguente formato dei parametri
> Nota: ui-schema è costituito da normali dati json, non dalla sintassi standard dello schema JSON
:::

:::nota di avvertenza
* `ui:hidden` `ui:widget` `ui:field` `ui:fieldProps` non supporta la configurazione in `ui:options`
:::


Il formato generale dei parametri è il seguente:
```js
schema ui = {
     // Sostituisce il titolo dello schema
    'ui:title': 'Sostituisci titolo schema',

    // Sostituisce la descrizione dello schema
    'ui:description': 'Sostituisci le informazioni sulla descrizione della descrizione dello schema',

    // Configura la funzione tramite fui:xxx per calcolare le opzioni correnti dell'interfaccia utente
    'fui:placeholder': (genitore, root, prop) => {
        console.log(genitore, root, prop);
        restituisce parent.txtColore;
    },

    // Se deve essere richiesta la configurazione di un singolo campo, la priorità è superiore alla configurazione dello schema'
    // tipo bool, 'ui:required': vero,
    //il valore predefinito non è definito
    'ui: richiesto': vero,

    // Callback dell'operazione per gli elementi dell'array
    //il valore predefinito non è definito
    // comando 片举值 moveUp | moveDown | rimuovi | aggiungi | batchPush | setNewTarget
    'ui:afterArrayOperate': (formData, comando, payload) => {
        debugger;
    },

    //Il valore quando l'input dell'elemento del modulo è vuoto, il valore predefinito non è definito
    'ui:emptyValue': non definito,

     // Se nascondere il nodo corrente, l'espressione di configurazione è supportata (la configurazione nelle opzioni non è supportata)
    // https://vue-json-schema-form.lljj.me/zh/guide/data-linkage.html#ui-schema%E9%85%8D%E7%BD%AE%E8%A1%A8% E8%BE%BE%E5%BC%8F
    'ui: nascosto': falso,

     // Campi personalizzati (la configurazione nelle opzioni non è supportata)
    // https://vue-json-schema-form.lljj.me/zh/guide/adv-config.html#%E8%87%AA%E5%AE%9A%E4%B9%89campo
    'ui:field': 'nomecomponente',

    // Prop aggiuntivi passati al campo durante la personalizzazione del campo, ricevono parametri tramite prop: { fieldProps } (la configurazione nelle opzioni non è supportata)
    'ui:fieldProps': non definito,

    // Componente widget personalizzato, (la configurazione nelle opzioni non è supportata)
    // https://vue-json-schema-form.lljj.me/zh/guide/adv-config.html#%E8%87%AA%E5%AE%9A%E4%B9%89widget
    'ui:widget': 'el-slider',

    // Passa al componente formItem labelWidth, con priorità più alta (antdv formItem non ha questo parametro, puoi usare fieldAttrs per configurare labelCol per controllare la larghezza dell'etichetta)
    // Puoi anche configurare labelWidth': '50px' in fieldAttrs
    'ui:labelWidth': '50px',

    'ui:opzioni': {
            //gli slot con ambito vengono implementati utilizzando la funzione render
            // Configura renderScopedSlots per restituire la chiave dell'oggetto come slotName e il corpo della funzione restituisce vnode
            // riferimento alla funzione di rendering: https://cn.vuejs.org/v2/guide/render-function.html#%E6%B7%B1%E5%85%A5%E6%95%B0%E6%8D%AE %E5%AF%B9%E8%B1%A1
            renderScopedSlot(h) {
                ritorno {
                    aggiungere: (props) => h('span', '.com')
                };
            },

            // slot, è necessario utilizzare la funzione render per l'implementazione
            //Configura renderChildren e restituisce Vnode[] dove slot è slotName
            // riferimento alla funzione di rendering: https://cn.vuejs.org/v2/guide/render-function.html#%E6%B7%B1%E5%85%A5%E6%95%B0%E6%8D%AE %E5%AF%B9%E8%B1%A1
            renderChildren(h) {
                ritorno [
                    h('intervallo', {
                        slot: 'suffisso',
                    }, 'suffisso')
                ];
            },

            // Ottieni l'istanza del componente widget. Non è consigliabile utilizzarlo a meno che non sia necessario.
            // Dopo aver montato il componente widget, questo metodo viene richiamato per trasferire l'istanza VM.
            // Versione supportata: "0.4.1"
            getWidget: (widgetVm) => {
                console.log(widgetVm);
            },

            // onChange
            // Supporta la versione 1.3
            /**
             *
             * @param curVal valore corrente
             * @param preVal L'ultimo valore
             * @param parentFormData Il valore del nodo principale corrente. Per i valori reattivi, puoi impostare altri valori che devono essere collegati qui.
             * @param rootFormData Il valore del nodo genitore corrente. Per i valori reattivi, altri valori che devono essere collegati possono essere impostati qui.
             */
            onChange({ curVal, preVal, parentFormData, rootFormData }) {
                console.log('modifica:', curVal, preVal, parentFormData, rootFormData);
            },

            // Mostra titolo? Valido solo per i tipi con tipo "oggetto" e "array".
            mostraTitolo: vero,

             // Mostra descrizione? Valido solo per i tipi con tipo "oggetto" e "array".
            mostraDescrizione: falso,

            // Non configurato per impostazione predefinita, aggiunto nella versione 0.2, utilizzato per configurare rapidamente la larghezza delle colonne nei layout a più colonne. Naturalmente, puoi anche utilizzare fieldStyle per configurare lo stile.
            larghezza: '100px',

            attrae: {
                // Passa la funzione vue render attrs al componente Widget, che può essere configurato solo sui nodi foglia.
                // Lo configuri anche nel livello esterno. Il programma combinerà attrs e altri attributi esterni e li passerà al sottocomponente tramite attrs.
                // I parametri configurati qui verranno passati al componente widget. Quando i prop del componente widget entrano in conflitto con i parametri generali di uiSchema, è possibile utilizzare la configurazione attr.
                messa a fuoco automatica: vero,
                width: '99px', // Questo viene passato direttamente al componente widget anziché alla configurazione della larghezza esterna
            },
            stile: {
                // Passa lo stile della funzione di rendering vue al componente Widget, che può essere configurato solo sui nodi foglia.
                boxShadow: '0 0 6px 2px #2b9939'
            },
            classe: {
                // Aggiunto nella versione 0.1.0
                // Lo passa al componente Widget tramite la classe della funzione vue render, che può essere configurata solo sui nodi foglia.
                className_hei: vero
            },
            stile campo: {
                // Aggiunto nella versione 0.1.0
                // Passa lo stile della funzione di rendering vue al componente Field, supportando tutti i nodi del campo
                sfondo: 'rosso'
            },
            campoClasse: {
                // Aggiunto nella versione 0.1.0
                // Lo passa al componente Field tramite la classe della funzione di rendering vue, supportando tutti i nodi del campo
                fieldClass: vero
            },
            fieldAttr: {
                // Passa la funzione vue render attrs al componente Field, supportando tutti i nodi
                'attr-x': 'xxx'
            },

            // Tutti gli altri parametri verranno uniti in attrs e passati al componente Widget
            digitare: 'area di testo',
            segnaposto: "Inserisci il tuo contenuto"
    }
}
```

>1. `ui:field` Per i componenti dei campi personalizzati, vedere qui [Campo personalizzato](/zh/guide/adv-config.html#Campo personalizzato)
>1. `ui:widget` Per i componenti del widget personalizzato, vedere qui [Widget personalizzato](/zh/guide/adv-config.html#Widget personalizzato)
>1. `ui:widget` configura `HiddenWidget` o `hidden` per nascondere l'elemento corrente
>1. `ui:hidden` supporta le espressioni di configurazione. Per i dettagli, vedi qui [ui-schema ui:hidden configurazione espressione](/zh/guide/data-linkage.html#ui-schema configurazione espressione)

### schema dell'interfaccia utente - eventi
Gli eventi di emissione dei componenti possono essere configurati tramite uiSchema widgetListeners

:::avvertimento
* Tieni presente che questa configurazione è adatta solo per "vue2".
* La versione di `vue3` può passare direttamente `ui:onXxx`, vedere: [ascoltatori vue3](https://v3.cn.vuejs.org/guide/migration/listeners-removed.html#%E6%A6 % 82%E8%A7%88)
:::

> Come segue: configurare gli eventi nel componente widget configurando ui widgetListener

```js
{
    'ui:opzioni': {
        widgetAscoltatori: {
            ingresso(evento) {
                console.log('ui input', evento);
            }
        }
    }
}
```



### schema dell'interfaccia utente - slots
Puoi configurare la funzione di rendering tramite uiSchema per passare lo slot al componente Widget. L'utilizzo è il seguente:

> Nota che la versione vue2 qui deve distinguere tra slot e scopeSlots. La configurazione è la seguente
>
> [Documentazione ufficiale di riferimento della funzione di rendering](https://cn.vuejs.org/v2/guide/render-function.html#%E6%B7%B1%E5%85%A5%E6%95%B0%E6% 8D%AE%E5%AF%B9%E8%B1%A1)

* slot - `renderChildren` (仅vue2)

> Nota: tutti gli slot nella versione vue3 vengono passati uniformemente sotto forma di "renderScopedSlots".

```js
{
    'ui:opzioni': {
        // slot, è necessario utilizzare la funzione render per l'implementazione
        //Configura renderChildren e restituisce Vnode[] dove slot è slotName
        // riferimento alla funzione di rendering: https://cn.vuejs.org/v2/guide/render-function.html#%E6%B7%B1%E5%85%A5%E6%95%B0%E6%8D%AE %E5%AF%B9%E8%B1%A1
        renderChildren(h) {
            ritorno [
                h('intervallo', {
                    slot: 'suffisso',
                }, 'suffisso')
            ];
        }
    }
}
```

* scopedSlots - `renderScopedSlots` （vue3、vue2）
> vue3 versione h è l'API globale, `import { h } da 'vue'`
>
> Allo stesso tempo, la configurazione della versione vue3 "renderScopedSlots" può essere un oggetto puro, vue3 non distingue gli slot con ambito

```js
{
    'ui:opzioni': {
        // vista2
        //gli slot con ambito vengono implementati utilizzando la funzione render
        // Configura renderScopedSlots. La chiave dell'oggetto restituito è slotName e il corpo della funzione restituisce vnode.
        // riferimento alla funzione di rendering: https://cn.vuejs.org/v2/guide/render-function.html#%E6%B7%B1%E5%85%A5%E6%95%B0%E6%8D%AE %E5%AF%B9%E8%B1%A1
        renderScopedSlot(h){
            ritorno {
                aggiungere: (props) => h('span', '.com')
            };
        }
    },

    'ui:opzioni': {
        // vista3
        // gli slot vengono implementati utilizzando la funzione render
        // vue3 renderScopedSlots può essere una funzione o un oggetto puro come segue
        // riferimento alla funzione di rendering vue3: https://v3.cn.vuejs.org/guide/render-function.html#%E6%8F%92%E6%A7%BD
        renderScopedSlot: {
            predefinito: (props) =>h('span', props.text)
        }
    }
}
```

#### Lo schema dell'interfaccia utente è configurato nello schema

Nelle versioni successive a "0.0.16", tutte le configurazioni di "ui-schema" supportano la configurazione diretta nel parametro "schema".

* La configurazione separata di `ui-schema` ha una priorità più alta rispetto alla configurazione di `schema`
* Il vantaggio è che può essere configurato in un'unica configurazione, ma lo `schema` non sarà più un puro file `JSON Schema` e la soluzione dovrà essere selezionata in base allo scenario reale.

Il formato è il seguente:
```json
{
    "titolo": "Dimostrazione",
    "tipo": "oggetto",
    "ui:ordine": [
        "biologico",
        "nome di battesimo"
    ],
    "proprietà": {
        "nome di battesimo": {
            "tipo": "stringa",
            "titolo": "Nome",
            "ui:placeholder": "Inserisci FirstName (configurato nello schema)"
        },
        "biografia": {
            "tipo": "stringa",
            "titolo": "Biografia",
            "Lunghezzamin": 10,
            "ui:opzioni": {
                "tipo": "area testo",
                "placeholder": "Inserisci il nome (configurato nello schema)",
                "righe": 4
            }
        }
    }
}
```

#### Dimostrazione della configurazione dello schema dell'interfaccia utente: reimposta lo stile del widget del modulo
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
            moduloDati: {
                numero 1,
                numeroEnumRadio: 2,
                interoIntervallo: 50,
            },
            schema: {
                tipo: 'oggetto',
                proprietà: {
                    nome di battesimo: {
                        tipo: 'stringa',
                        titolo: "Nome",
                        'ui:placeholder': 'Inserisci FirstName (configurato nello schema)'
                    },
                    biografia: {
                        tipo: 'stringa',
                        titolo: 'Bio',
                        minLunghezza: 10,
                        'ui:opzioni': {
                            digitare: 'area di testo',
                            segnaposto: 'segnaposto (configurato nello schema)',
                            righe: 4
                        }
                    },
                    testo di input: {
                        titolo: "Inserisci testo",
                        tipo: 'stringa'
                    },
                    numero: {
                        titolo: 'Numero (componente di rendering predefinito)',
                        tipo: 'numero'
                    },
                    interoIntervallo: {
                        titolo: 'Intervallo di numeri interi (usando ElSlider)',
                        tipo: 'intero',
                        minimo: 42,
                        massimo: 100
                    }
                }
            },
            schema dell'interfaccia utente: {
                'ui:ordine': ['numero', '*'],
                testo di input: {
                    'ui:description': 'Le informazioni sulla descrizione vengono reimpostate qui',
                    'ui:emptyValue': '',
                    'ui:opzioni': {
                        stile: {
                            boxShadow: '0 0 6px 2px #2b9939'
                        },
                        classe: {
                            className_hei: vero
                        },
                        digitare: 'area di testo',
                        messa a fuoco automatica: vero,
                        righe: 6,
                        segnaposto: "Inserisci il tuo contenuto"
                    }
                },
                numero: {
                    'ui:title': 'Il titolo viene reimpostato qui'
                },
                interoIntervallo: {
                    'ui:widget': 'el-slider',
                }
            }
        }
    }
};
</script>
```
:::

::: nota di avvertenza
La struttura dei dati di configurazione è coerente con "schema", non con "formData".

Ad esempio, configura gli elementi dell'array:
```js

// schema
schema = {
    tipo: 'oggetto',
    proprietà: {
        Elenco articoli fissi: {
            tipo: 'array',
            titolo: "Un elenco di elementi fissi",
            elementi: [
                {
                    titolo: "Un valore stringa",
                    tipo: 'stringa',
                    lunghezza massima: 2
                }
            ]
        }
    }
}
```

```js
// Configurazione corretta
schema ui = {
    Elenco articoli fissi: {
         // Mantiene la stessa struttura dello schema qui
         elementi: [
             {
                 'ui:opzioni': {
                    ...
                }
             }
         ]
    }
}
```

```js
//configurazione errata
schema ui = {
    ElencoArticolifissi: [
         {
             'ui:opzioni': {
                ...
            }
        }
    ]
}
```

:::

### schema degli errori
* Digitare: "oggetto".
*Valore predefinito: `{}`
* `Non richiesto`: l'interfaccia utente può anche essere configurata direttamente in `schema`

>* `0.0.16` e versioni successive supportano la configurazione di `error-schema` nel parametro `schema` [fai clic per visualizzare] (#error-schema è configurato nello schema)
>* Le versioni successive a `0.1.0` supportano la configurazione di [schema-errore](#schema-errore) in [schema-ui](#schema-ui). (`ui-schema` e `error-schema` hanno lo stesso formato ed entrambi appartengono alla visualizzazione dell'interfaccia utente. Una copia può essere facilmente configurata)

Utilizzato per configurare le informazioni sulla copia degli errori di verifica del modulo, i dati JSON ordinari, la specifica dello schema non JSON

La configurazione dei dati viene salvata come `ui-schema`, la differenza è:
1. Utilizza "err:" come prefisso
1. Utilizza come chiave `err:${name}` del tipo di errore dello schema configurato, ad esempio `err:format`, `err:required`, `err:type`

::: mancia
 * La struttura dei dati di configurazione è coerente con lo schema e tutti gli attributi di configurazione "error" iniziano con "err:".
 * Puoi anche configurare tutte le proprietà in `err:options` senza iniziare con `err:`
 * Se l'attributo `xx` è configurato in `err:xx` e `err:options`, la priorità in `err:options` è più alta. Infatti, puoi configurare tutti i parametri in `err:options` all'interno; tu puoi seguire le tue abitudini personali qui
 > Nota: lo schema degli errori è dati json standard, non la sintassi standard dello schema JSON
 :::

#### Lo schema degli errori è configurato nello schema

Nelle versioni successive a "0.0.16", tutte le configurazioni di "error-schema" supportano la configurazione diretta nel parametro "schema".

> Utilizza un formato simile a [ui-schema configurato nello schema] (#ui-schema configurato nello schema)

Dimostrazione della configurazione dello schema di errore: reimposta le informazioni sull'errore del modulo

::: dimostrazione
```html
<modello>
    <visualizza-modulo
        v-model="formData"
        :schema="schema"
        :ui-schema="uiSchema"
        :errore-schema="erroreSchema"
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
                    'pagina iniziale',
                    'biologico'
                ],
                proprietà: {
                    nomeutente: {
                        tipo: 'stringa',
                        titolo: 'nome utente',
                        predefinito: 'Liu.Jun'
                    },
                    pagina iniziale: {
                        tipo: 'stringa',
                        formato: "uri",
                        titolo: "Home page personale",
                        'err:required': 'Inserisci l'indirizzo della tua home page personale (configurato nello schema)',
                        'err:format': 'Inserisci l'indirizzo URL corretto (configurato nello schema)'
                    },
                    biografia: {
                        tipo: 'stringa',
                        titolo: 'firma',
                        Lunghezza min: 10
                    },
                    elencoDiStringhe: {
                        tipo: 'array',
                        titolo: 'Un elenco di stringhe',
                        descrizione: 'Contiene almeno due articoli',
                        Oggetti unici: vero,
                        minimoArticoli: 2,
                        elementi: {
                            tipo: 'stringa',
                            predefinito: 'rotondo'
                        }
                    },
                    Elenco articoli fissi: {
                        tipo: 'array',
                        titolo: "Un elenco di elementi fissi",
                        elementi: [
                            {
                                titolo: "Un valore stringa",
                                tipo: 'stringa',
                                lunghezza massima: 2
                            }
                        ]
                    }
                }
            },
            schema dell'interfaccia utente: {
                biografia: {
                    'ui: tipo': 'area testo',
                    'ui:placeholder': 'Inserisci...',
                    'err:required': 'Inserisci (configurato nello schema ui)',
                },
            },
            erroreSchema: {
                nomeutente: {
                    'err:opzioni': {
                        obbligatorio: 'Inserisci il nome utente'
                    }
                },
                biografia: {
                    'err:minLength': 'La lunghezza minima della firma è 10 stringhe'
                },
                elencoDiStringhe: {
                    'err:uniqueItems': 'Non può contenere valori duplicati',
                    elementi: {
                        'err:opzioni': {
                            obbligatorio: 'non può essere vuoto~'
                        }
                    }
                },
                Elenco articoli fissi: {
                    elementi: [
                        {
                            'err:maxLength': 'Vecchio, puoi inserire al massimo due caratteri'
                        }
                    ]
                }
            }
        }
    }
}
</script>
```
:::

### formati personalizzati
* Digitare: "oggetto".
*Valore predefinito: `{}`

Personalizza le regole di verifica, chiama il metodo `avj.addFormat` per aggiungere un nuovo formato, [Visualizza](https://github.com/ajv-validator/ajv#addformatstring-name-stringregexpfunctionobject-format---ajv)

Di seguito, la dimostrazione aggiunge un tipo di verifica del prezzo:

::: l'importo della demo è maggiore di 0 < 999999,99, mantieni due cifre decimali
```html
<modello>
    <visualizza-modulo
        v-model="formData"
        :schema="schema"
        :custom-formats="customFormats"
        :errore-schema="erroreSchema"
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
                    'prezzo'
                ],
                proprietà: {
                    prezzo: {
                        tipo: 'stringa',
                        titolo: "Prezzo",
                        descrizione: 'Inserisci un numero, con un massimo di due punti decimali, il valore massimo è 999999,99',
                        formato: 'prezzo'
                    }
                }
            },
            erroreSchema: {
                prezzo: {
                    'err:opzioni': {
                        obbligatorio: 'Inserisci il prezzo',
                        formato: 'Conserva fino a due cifre decimali, valore massimo 999999,99'
                    }
                }
            },
            formati personalizzati: {
                prezzo(valore) {
                    valore restituito !== '' && /^[0-9]\d*$|^\d+(\.\d{1,2})$/.test(valore) && valore >= 0 && valore <= 999999,99
                }
           }
        }
    }
}
</script>
```
:::

### regola personalizzata
* Digitare: "funzione".
*Valore predefinito: `-`

* Personalizza le regole di verifica per implementare la verifica dei dati del modulo in modo simile al validatore delle regole el-form
* [Visualizza i dettagli qui](/zh/guide/validate.html#custom-rule-custom verify)

### valore / modello v
* Digitare: "oggetto".
*Valore predefinito: `{}`

Forma valori di associazione, "Per i valori che non richiedono l'associazione bidirezionale, è possibile utilizzare le proprietà del valore".


### piè di pagina
* Digitare: "oggetto".

```js
// valore di default
formFooter = {
    show: true, // Indica se visualizzare il fondo predefinito
    okBtn: 'Salva', // Conferma il testo del pulsante
    okBtnProps: { type: 'primary' }, // Passa gli oggetti di scena del pulsante di conferma, come la configurazione dello stato di caricamento del pulsante okBtnProps: { loading: true }
    cancelBtn: 'Annulla', //Testo del pulsante Annulla

    // Passa in modo trasparente i parametri al componente formItem in formFooter
    // Ad esempio, vue3-ant configura wrapperCol formItemAttrs = { wrapperCol: { span: 10, offset: 5 }}
    formItemAttrs: {}
}
```

#### formFooter show: false Come verificare manualmente
Dopo aver configurato formFooter `show: false`, devi attivare manualmente la verifica del modulo, l'invio e altre operazioni. Puoi fare riferimento ai seguenti metodi:
1. Puoi utilizzare direttamente l'attributo [$$uiFormRef](/zh/guide/basic-config.html#uiformref) dell'istanza del modulo per ottenere il riferimento al modulo del componente ui, come il componente elForm di elementUi, e quindi direttamente chiamare il corrispondente metodo "validate". Can

```html
<modello>
  <vjsf ref="mioForm" />
</modello>

<copione>
esportazione predefinita {
  invio asincrono() {
    // pseudocodice
    attendi questo.$refs.myForm.$$uiFormRef.validate()
    this.postData()
  }
}
</script>
```

2. Se il pulsante di invio è ancora all'interno del modulo, puoi utilizzare direttamente il parametro dello slot di ambito per ottenere il riferimento del modulo.

Riferimento: [slot-scope](/zh/guide/basic-config.html# slot-scope-slot)


### etichetta di riserva
* Digitare: "booleano".
* predefinito:`falso`

Quando "schema" non è configurato con "titolo", se utilizzare il nome dell'attributo corrente come modulo "etichetta".

Come segue, configura `fallback-label` come `true`, `label` verrà visualizzato come `street_address`
```js
schema = {
    proprietà: {
        indirizzo: {
            tipo: 'stringa'
        }
    }
}
```

### oggetti di scena
* Digitare: "oggetto".
"form-props" supporta le seguenti due parti di parametri:

1. Parte del parametro fissa (questa parte del parametro non ha nulla a che fare con la libreria dell'interfaccia utente attualmente utilizzata)
```js
// valore di default
formProps = {
    layoutColumn: 1, // 1 2 3, supporta il layout a 1 2 3 colonne, se utilizzi il modulo in linea, questa configurazione non è valida
    inline: false, // Modalità modulo inline, si consiglia di non configurare labelPosition con top quando è acceso e di non configurare antd con labelCol wrapperCol.
    inlineFooter: false, // Se vuoi salvare i pulsanti e gli elementi del modulo da visualizzare su una riga, devi configurare true
    labelSuffix: '：', // suffisso etichetta
    labelPosition: 'top', // La posizione dell'etichetta del campo del modulo
    isMiniDes: false, // Se dare la priorità alla visualizzazione delle informazioni sulla descrizione in formato mini (il testo dell'etichetta e le informazioni sulla descrizione vengono visualizzati insieme)
    defaultSelectFirstOption: true, // Il pulsante di opzione è obbligatorio, indipendentemente dal fatto che il primo sia selezionato per impostazione predefinita
    popover: {}, // Passato in modo trasparente al componente popver della libreria dei componenti dell'interfaccia utente, come l'elemento ui Popover, antd a-popover
}
```

2. Parametri del componente del modulo della libreria dell'interfaccia utente corrente
I parametri diversi da quelli fissi sopra verranno passati in modo trasparente al componente del modulo della libreria dell'interfaccia utente corrente, come elementUi el-form, IView i-form...
```js
formProps = {
    layoutColumn: 2, // 1 2 3, supporta il layout a 1 2 3 colonne, se si utilizza il modulo in linea questa configurazione non è valida

    //Di seguito sono riportati i parametri del componente del modulo
    // 如elementUi el-form labelWidth
    labelWidth: 'auto', // La larghezza dell'etichetta del campo modulo, ad esempio "50px"
}
```

### modalità rigorosa
La modalità rigorosa, quando attivata, corrisponderà rigorosamente a `anyOf`/`onyOf` quando si calcola il valore predefinito del modulo. Vedi il problema: [#157](https://github.com/lljj-x/vue-json-schema-form/issues/157)

* Digitare: "booleano".
* predefinito:`falso`


## Evento Emetti evento
Tutti gli eventi di emissione sono i seguenti:

::: dimostrazione
```html
<modello>
    <visualizza-modulo
        v-model="formData"
        :schema="schema"
        @on-submit="handlerSubmit"
        @on-cancel="handlerCancel"
        @on-change="handlerChange"
    >
    </vue-form>
</modello>

<copione>
esportazione predefinita {
    nome: 'Dimostrazione',
    metodi: {
        gestoreInvia() {
            const vNode = this.$createElement('pre', JSON.stringify(this.formData, null, 4));
            this.$messaggio({
                tipo: 'successo',
                messaggio: vNode
            });
        },
        gestoreCancel() {
            this.$message.warning('Ho fatto clic su Annulla');
        },
        handlerChange({ vecchioValore, nuovoValore }) {
            const vNode = this.$createElement('pre', JSON.stringify(newValue, null, 4));
            this.$notifica({
                titolo: "Dati di input",
                messaggio: vNode
            });
        },
    },
    dati() {
        ritorno {
            moduloDati: {
               nome: 'Liu.Jun'
            },
            schema: {
                tipo: 'oggetto',
                proprietà: {
                    nome: {
                        titolo: "Inserisci nome",
                        tipo: 'stringa'
                    }
                }
            }
        }
    }
};
</script>
```
:::

### all'invio
* Parametri (formData)

::: avvertimento
Nella versione di vue3 è "submit" e il prefisso "on" viene rimosso.
:::

Fare clic sul pulsante di invio e il modulo supera la verifica

> L'evento verrà attivato solo quando è configurato il display predefinito in basso, [props form-footer](#form-footer)

### convalida-non riuscita
* Parametro (errorObj)

::: avvertimento
La versione di vue3 è "validation-failed", rimuovere il prefisso "on".
:::

Se fai clic sul pulsante di invio e il modulo non viene superato, puoi visualizzare il messaggio di errore qui

> L'evento verrà attivato solo quando è configurato il display predefinito in basso, [props form-footer](#form-footer)


### in fase di annullamento
* Parametri (nessuno)

::: avvertimento
La versione di vue3 è "cancel", rimuovi il prefisso "on".
:::

Fare clic sul pulsante Annulla
> L'evento verrà attivato solo quando è configurato il display predefinito in basso, [props form-footer](#form-footer)

### in cambiamento
* Parametri (newVal, oldVal)

::: avvertimento
La versione di vue3 è "change", rimuovi il prefisso "on".
:::

Il valore del modulo cambia
> Tipologia riferimento, viene riassegnato solo l'oggetto, altrimenti newVal è uguale a oldVal. Vedi [vue watch](https://cn.vuejs.org/v2/api/#vm-watch)

### montato sul modulo
* Parametri (formRef, {formData})

Attraverso questo metodo, è possibile ottenere l'istanza del componente del modulo dell'attuale framework dell'interfaccia utente, che può essere utilizzata per eseguire alcuni metodi del componente del modulo, come (`validate`)

::: avvertimento
La versione di vue3 è "montata su modulo", rimuovere il prefisso "on".
:::

##Metodi
- nessuno

##AttributeAttr
### $$uiFormRef
La versione "1.10" è nuova. Nella versione precedente, è necessario ottenere l'istanza del modulo del framework dell'interfaccia utente in [on-form-montato](#on-form-montato)
* Ottieni comodamente direttamente le istanze dei componenti del modulo del framework dell'interfaccia utente

::: avvertimento
* Nota: questa proprietà verrà impostata dopo "montata".
:::

## Slot per mirino
* nome `default`, il modulo personalizzato contiene contenuto e sovrascriverà il `form-footer` predefinito dopo la configurazione.

I parametri sono: { formData, formRefFn }

::: descrizione del parametro del suggerimento
* `formData` Il valore dell'elemento del modulo corrente, reattivo
* `formRefFn` è `funzione` e la chiamata restituisce l'istanza di riferimento del componente `el-form`
:::
Piace:
::: dimostrazione
```html
<modello>
    <visualizza-modulo
        v-model="formData"
        :schema="schema"
    >
        <div slot-scope="{ formData, formRefFn }">
            <pre style=" background-color: #eee;">{{ JSON.stringify(formData, null, 4) }}</pre>
            <p><el-button @click="consoleLog(formRefFn)" type="primary">点击</el-button></p>
        </div>
    </vue-form>
</modello>

<copione>
esportazione predefinita {
    nome: 'Dimostrazione',
    metodi: {
        consoleLog(getForm) {
            console.log(getForm());
        },
    },
    dati() {
        ritorno {
            moduloDati: {
               nome: 'Liu.Jun'
            },
            schema: {
                tipo: 'oggetto',
                proprietà: {
                    nome: {
                        titolo: "Inserisci nome",
                        tipo: 'stringa'
                    }
                }
            }
        }
    }
};
</script>
```
