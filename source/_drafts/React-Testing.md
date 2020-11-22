title: React Testing
author: Samuel Romero Arbelo
date: 2020-11-21 13:14:36
tags:
---
Esta semana, me he unido a un proyecto open source para la plataforma [***Huella positiva***](https://huellapositiva.com/), en dicho proyecto contrubillo en el equipo de **frontend**, ya que hace poco terminé un bootcamp de React.

La verdad que ha sido una experiencia bastante buena, donde la sensación de aprender cada día, es de lo mejor, y además en un ambiente agradable donde todos te animan a seguir mejorando.

Para poder ayudar en la parte de *testing*, he tenido que ir aprendiendo poco a poco [***React-Testing***](https://testing-library.com/docs/react-testing-library/intro), ya que es una de las librerías que usan para comprobar los componentes de React.


### ***¿De dónde viene?***

Es una librería construida sobre **DOM Testing Library** la cual nos proporciona una manera fácil de probar nodos del DOM. 

El uso de dicha librería se puede llevar a diferentes **Frameworks**:

   * [Angular](https://testing-library.com/docs/angular-testing-library/intro/)
   * [React](https://testing-library.com/docs/react-testing-library/intro/)
   * [Vue](https://testing-library.com/docs/vue-testing-library/intro/)

También existe un **plugin** para usar esta librería para test **end-to-end**, en [**Cypress**](https://testing-library.com/docs/cypress-testing-library/intro/) y una implementeción para [**React Native**](https://testing-library.com/docs/react-native-testing-library/intro/).

Una de sus grandes ventajas es que funciona con cualquier entorno que proporcione **DOM API**, como por ejemplo [**JEST**](https://jestjs.io/), [**Mocha**](https://mochajs.org/), [**JSDOM**](https://github.com/jsdom/jsdom) **o un explorador real**.

### ***React Testing***

Como comenté al principio del artículo, en este proyecto usarémos ***React-Testing***, porque nos proporciona una gran herramienta para comprobar los componentes de [**React**](https://react-bootstrap.github.io/) que creemos. 

Además de ser una solución ligera para probar los componentes, proporciona funciones sencillas además de **react-dom y react-dom/test-utils**, con la que fomentar mejores prácticas de pruebas.

Interactúa directamente con los **nodos del DOM**, de una manera muy similar a un usuario buscando elementos en nuestra web, ya sea por **etiqueta de texto**, botones o enlace por su texto,.., además para esos elementos que no tiene sentido tener una etiqueta de texto, podemos usar la búsqueda por **data-textid**, ya que es una de los atributos que podemos usar en *HTML*.

### ***¿Cómo podemos instalarla?***

Si creamos la aplicación con el comando ***`npx create-react-app 'nombre de la aplicación'`*** ya viene instalado y no sería necesario usar los comandos que describo más abajo.



	 		npm install --save-dev @testing-library/react

**- Si usas 'yarn'**

			yarn add --dev @testing-library/react


### ***Primera prueba***


En este primer ejemplo vamos a mostrar un texto al darle check a un **checkbox**.

##### - Componente de React
```
import React, {useState} from 'react';

const CheckButton = ({children}) => {
    const [showMessage, setShowMessage]= useState(false);


    return (
        <div>
            <label htmlFor="check"> Enseñar mensaje</label>
            <input type="checkbox" name="check-prueba" id="check" onChange={e => setShowMessage(e.target.checked)}
                   checked={showMessage}/>
            {showMessage ? children : null}
        </div>
    );


};


export default CheckButton;
```

##### - Prueba

En el test buscamos el **label** por el contenido de su texto, comentar que en la importación de **@testing-library/react** he usado el seudónimo **Testing** como apunte personal para ver los métodos de dicha librería, los cuales veremos más a fondo más adelante en un futuro **post**.

```
import '@testing-library/jest-dom';
import React from 'react';
import * as Testing from '@testing-library/react';
import CheckButton from "./CheckButton";


test('Muestra los hijos al pulsar el check', () => {


    const testMessage = 'Esto es un test';

    Testing.render(<CheckButton>{testMessage}</CheckButton>)

    expect(Testing.screen.queryByText(testMessage)).toBeNull();


// '/enseñar/i' el texto del label debe contener la palabra que encontramos entre las barras


    Testing.fireEvent.click(Testing.screen.getByLabelText(/enseñar/i));

    expect(Testing.screen.getByText(testMessage)).toBeInTheDocument();

})
```

 -  **Puedes encontrar más ejemplos en el github de [React Testing](https://github.com/testing-library/react-testing-library#basic-example)**.

##### **continuará....**
