layout: un novato junto a react
title: Un novato junto a "React Testing" segunda parte...
tags: []
categories: []
date: 2021-01-15 07:49:00
---
![](https://i.imgur.com/StMOQvK.png)



En esta segunda parte vamos a ver las **queries** o **consultas** las cuales nos van ayudar para la realización de los test.

# Queries o Consultas

* #### Variantes

	* **Variantes que devuelven el primer elemento**
	
		- **getBy :** Retorna el primer nodo que coincide para una consulta, la cual arroja un error si no encuentra un elemento coincidente o si se encuentra más de uno.
						
		- **querryBy :** Retorna el primer nodo que coincide con la consulta y nos devuelve un **null** si no encuentra coincidencias. Como nos comenta la documentacion, este tipo de consultas es muy útil si queremos verificar que un elemento está presente o no.	
		
		- **findBy :** Retorna una promesa, la cual se resuelve cuando se encuentra un elemento coincidente con la consulta realizada, si no encuentro ninguna coincidencia o nos encuentra más de una después de que pasen los 1000 ms que tiene predeterminados, nos rechaza dicha promesa.
		
	* **Variantes que devuelven un array de elementos**
	
		- **getAllBy :** Retorna un array con todos los nodos coincidentes, nos devolvería un error si no encuentra elementos para dicha consulta.
						
		- **querryAllBy :** Retorna un array con todos los nodos coincidentes para la consulta y una array vacío si no encuntra coincidencias.	
		
		- **findAllBy :** Retorna una promesa, la cual se resuelve en un array cuando se encuentra los nodos coincidente con la consulta realizada, si no encuentro ninguna coincidencia después de que pasen los 1000 ms que tiene predeterminados, nos rechaza dicha promesa.


* #### Opciones 

Los argumentos de una consulta pueden ser *string, expresion regular o funciones*, además podemos encontrar opciones para ajustar como el nodo es analizado, la cuales son :

*  *Precision:*

	Algunas Apis acepta un objeto como final del argumento que se le pasa en la consulta y que puede afectar la precisión de la coincidencias de las *string* pasada:
	
	* **exact :** Por defecto viene en *true*, coincide con cadenas completas y distingue entre mayúsculas y minúsculas. Cuando es inicializado a *false* coincide con subcadenas y no distingue entre mayúsculas y minúsculas. 
		En la documentación aconsejan que usar *expresiones regulares* porque te dan más control sobre la coincidencia aproximada.
	
	* **normalizer :** Con esta opción lo que hacemos es anular el comportamiento de *Normalization*, la cual se ejecuta siempre antes de la lógica para la búsqueda. 

* *Normalización:*

	Como comentamos en una de la opciones de la precisión, esta se ejecuta antes de la lógica y por defecto quita los espacios en blanco antes y después del texto y regulariza los espacios entre palabras a uno sólo.
	
	Si queremo evitar esta normalización o poner opciones alternativas, les podemos pasar una **normalizer function**, la cual recibirá una *string* y espera una versión normalizada de dicha *string*.
	
	Tenemos el método **getDefaultNormalizer**, el cual nos permite introducirlo en nuestra opciones y pasarle como parámetro:
	
	* **trim:** El cual nos quitará los espacios en blanco, tanto al principio como al final de la cadena, viene como predeterminado en *true*.
		* **collapseWhitespace:** Nos quitaría los espacios extras entre palabras dejándolo solamentes en uno, viene por defecto en *true*.


* **Screen** 

	Todas las consultas que exportamos mediante el *DOM Testing Library* aceptan un contenedor como primer argumento y es común consultar todo el *document.body*, por eso la librería *DOM Testing Library* también nos exporta un objeto **screen** el cual contiene todas las consultas que están vinculadas previamente usando la funcionalidad *within*, que podemos encontrar en el apartado [*Custom Queries*](https://testing-library.com/docs/dom-testing-library/api-helpers/#custom-queries).


* **Screen.debug**

	Método que nos ayuda hacer *debug* del documento en un sólo elemento o en un array de elementos.


* **Queries**

	Estas consultas, son consultas base y requieren de le pasemos un contenedor como primer argumento.


 **ByLabelText** 
	
Nos buscará la etiqueta que coincida con el *TextMatch* que le pasemos y luego nos encontrará el elemento asociado con dicha etiqueta.
	
	
   ```
        getByLabelText(
              container: HTMLElement, //if you're using `screen`, then skip this argument
              text: TextMatch,
              options?: {
                selector?: string = '*',
                exact?: boolean = true,
                normalizer?: NormalizerFn,
              }): HTMLElement
   ```


<br/>

* **Ejemplo en código**
       
<br/>

**Componente**
       
   
```
     import * as React from 'react';
        const Label = () => {

            return (<><label htmlFor="username-input">Label</label>
                <input id="username-input"/>
            </>);
        };

        export default Label;
```



 **Test**
    

  ```
      import * as React from 'react';
          import * as Testing from '@testing-library/react';
          import Label from "./Label";

          test('test for label queries', () => {

              Testing.render(<Label/>);
              Testing.screen.getByLabelText('Label');

          });
  ```

<br/>

**ByPlaceholderText**
	
Nos buscará en todos los elementos que contentagan *placeholder* hasta econtrar uno que coincida con el *TextMatch* que le pasemos.
	
	 

  ```
      getByPlaceholderText(
            container: HTMLElement, //if you're using `screen`, then skip this argument
            text: TextMatch,
            options?: {
              exact?: boolean = true,
              normalizer?: NormalizerFn,
            }): HTMLElement
  ```
<br/>

* **Ejemplo en código**
       
<br/>

**Componente**

```
    import * as React from 'react';


    const InputWithPlaceHolder=()=>{

        return(
            <input type="text" placeholder={'I am a placeholder'}/>
        );

    }

    export default InputWithPlaceHolder;
```

   **Test**
    
```
    import * as React from 'react';
    import * as Testing from '@testing-library/react';
    import InputWithPlaceHolder from "./InputWithPlaceHolder";

    test('test with getBy Placeholder', () => {

            Testing.render(<InputWithPlaceHolder/>);
            Testing.screen.getAllByPlaceholderText('I am a placeholder')

    })
```
	
~ **Buscar por placeholder no es la mejor opción, por lo general se usa getByLabelText** ~

**ByText**
	
Nos buscará  en todos lo elementos que contengan texto en el nodo con *textContent* que coincida con el *TextMatch* que le pasemos. Además funciona con *inputs* cuyo atributos sean del tipo *submit o button*. 

Podemos pasarle como opción *ignore* en estado *false* si queremos evitar que nos devuelva como *true* una etiqueta script.
	
```
		getByText(
		  container: HTMLElement, //if you're using `screen`, then skip this argument
		  text: TextMatch,
		  options?: {
		    selector?: string = '*',
		    exact?: boolean = true,
		    ignore?: string|boolean = 'script, style',
		    normalizer?: NormalizerFn,
		  }): HTMLElement
```

<br/>

* **Ejemplo en código**

<br/>

**Componente**
       

```
        import * as React from 'react';


        const Label = () => {

            return (<><label htmlFor="username-input">Label</label>
                <input id="username-input"/>
            </>);
        };

        export default Label;
```

   **Test**
    
```
    import * as React from 'react';
    import * as Testing from '@testing-library/react';
    import Label from "./Label";


    test('queries getByText', () => {

        Testing.render(<Label/>);
        Testing.screen.getByText(/Label/i);

    });
```

<br/>

**ByAltText**
	
Esta búsqueda es solamente soportada por elementos que contengan el atributo **alt**,como por ejemplo`<img>, <input> y <area>` ya que nos devolvería dichos elementos si su atributo *alt* coincide con el *TextMatch* indicado.
	

```
		getByAltText(
		  container: HTMLElement, //if you're using `screen`, then skip this argument
		  text: TextMatch,
		  options?: {
		    exact?: boolean = true,
		    normalizer?: NormalizerFn,
		  }): HTMLElement
```
<br/>

* **Ejemplo en código**
       
<br/>

**Componente**
       
```
    import * as React from 'react';
    import IconReactTesting from '../../img/octopus-128x128.png'


    const Img = () => {

        return (<img src={IconReactTesting} alt={'This is a img'}/>)
    };

    export default Img;
```

**Test**

```
         import * as React from 'react';
            import IconReactTesting from '../../img/octopus-128x128.png'


            const Img = () => {

                return (<img src={IconReactTesting} alt={'This is a img'}/>)
            };

            export default Img;
```
<br/>

**ByTitle**
	
Esta búsqueda nos devolvería el elemento que contenga en su atributo *title* el *TextMatch* que le pasemos, funcionaría con elementos *svg* ya que contengan dentro la etiqueta *title*. 
	
```
		getByTitle(
		  container: HTMLElement, //if you're using `screen`, then skip this argument
		  title: TextMatch,
		  options?: {
		    exact?: boolean = true,
		    normalizer?: NormalizerFn,
		  }): HTMLElement
```

<br/>

* **Ejemplo en código**
       
<br/>

**Componente**
       
```
        import * as React from 'react';

        const InputWithTitle = () => {
            return (
                <input type={'text'} title={'This a input with title'}/>
            )
        }

        export default InputWithTitle;

```
   
**Test**
     
```
        import * as React from 'react';
        import * as Testing from '@testing-library/react';
        import InputWithTitle from "./InputWithTitle";


        test('queries getByTitle', () => {
            Testing.render(<InputWithTitle/>);
            Testing.screen.getByTitle('This a input with title');
        })
```
<br/>

**ByDisplayValue**
	
Esta búsqueda nos devolvería el **input, textarea o select** que contengan el valor indicado en el *TextMatch*.
	

```
		getByDisplayValue(
		  container: HTMLElement, //if you're using `screen`, then skip this argument
		  value: TextMatch,
		  options?: {
		    exact?: boolean = true,
		    normalizer?: NormalizerFn,
		  }): HTMLElement
```
<br/>

* **Ejemplo en código**

<br/>

**Componente**

```
    import * as React from 'react';

    const InputWithDisplayValue = () => {
        return (
            <input type={'text'} title={'This a input with title'} value={'This a input with display value'}
                   readOnly={true}/>
        )
    }

    export default InputWithDisplayValue;

```
   
**Test**
     
```
    import * as React from 'react';
    import * as Testing from '@testing-library/react';
    import InputWithDisplayValue from "./InputWithDisplayValue";


    test('queries getByTitle', () => {
        Testing.render(<InputWithDisplayValue/>);
        Testing.screen.getByDisplayValue('This a input with display value');
    });
```
<br/>	

**ByRole**
 	
 Búsqueda por el atributo *role* incluyendo también en los elementos que ya tienen dicha etiqueta implícita [(tabla de elementos HTML con roles predeterminados)](https://www.w3.org/TR/html-aria/#docconformance), hay que tener en cuenta que introducir un *atributo role* en un elemento que ya lo tiene por defecto, nos puede causar problemas, ya que podríamos introducir un *role* que no le corresponde.
 	
 Si tenemos varios elementos con el mismo *role* podríamos buscarlo por su [nombre accesible](https://developer.paciellogroup.com/blog/2017/04/what-is-an-accessible-name/) nombre accesible, dicha busqueda no sustituye a *ByAlt o ByTitle*.
 	
 Tenemos varias opciones que podemos configurar para la búsqueda, como *hidden, selected, checked, pressed, queryFallbacks y levels* (podrás ver algo más sobre la configuración en la parte **ByRole** que encuentras **[en la documentación](https://testing-library.com/docs/dom-testing-library/api-queries)**).
 	
		
```
getByRole(
		  container: HTMLElement, //if you're using `screen`, then skip this argument
		  role: TextMatch,
		  options?: {
		    exact?: boolean = true,
		    hidden?: boolean = false,
		    name?: TextMatch,
		    normalizer?: NormalizerFn,
		    selected?: boolean,
		    checked?: boolean,
		    pressed?: boolean,
		    queryFallbacks?: boolean,
		    level?: number,
		  }): HTMLElement
```
          
<br/>

* **Ejemplo en código**
       
<br/>

**Componente**

```
    import * as React from 'react';

    const Button = () => {

        return (
            <button type="submit">Esto es un boton</button>
        )
    };

    export default Button;
```
   
**Test**

```
    import * as React from 'react';
    import * as Testing from '@testing-library/react';
    import Button from "./Button";

    test('Querie getByRole', () => {
        Testing.render(<Button/>);
        Testing.screen.getByRole('button');
    })
```
<br/>

**ByTestId**

Solamente es recomendable cuando ninguna de las *queries* anteriores nos permite hacer la búsqueda.
    
        
```
getByTestId(
      container: HTMLElement, // if you're using `screen`, then skip this argument
      text: TextMatch,
      options?: {
        exact?: boolean = true,
        normalizer?: NormalizerFn,
      }): HTMLElement
```
<br/>

* **Ejemplo en código**

<br/>

**Componente**

```
    import * as React from 'react';


    const ExampleByTestId=()=>{
        return(
            <div data-testid={'custom-element'}>
                <p>Para el Ejeplo de la querie getByTestId</p>
            </div>
        )
    };

    export default ExampleByTestId;
```

**Test**

```
    import * as React from 'react';
    import * as Testing from '@testing-library/react';
    import ExampleByTestId from "./ExampleByTestId";


    test('Querie getByTestId',()=>{
        Testing.render(<ExampleByTestId/>);
        Testing.screen.getByTestId('custom-element');
    });

```
<br/>

En este post he querido ver las *queries* que nos brinda esta librería, para terminar mi aprendizaje de **React Testing** veremos en el siguiente los **Firing Events** y algunos de los **métodos que hereda de DOM Testing Library** .

***Nos vemos en el próximo post,......***