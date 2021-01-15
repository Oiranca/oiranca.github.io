title: Un novato junto a "React Testing" primera parte...
author: Samuel Romero Arbelo
tags: []
categories: []
date: 2020-11-28 13:14:00
---
![](https://i.imgur.com/StMOQvK.png)

Esta semana, me he unido al equipo de **frontend** en un proyecto **open source** para la plataforma [***Huella positiva***](https://huellapositiva.com/), en dicho proyecto contribuyo con mis conocimientos en React, ya que hace poco terminé un bootcamp orientado a desarrollo web.

La verdad, que el entrar a formar parte de este equipo de desarrollo, ha sido una experiencia bastante buena, donde la sensación de aprender cada día, es de lo mejor, y además en un ambiente agradable donde todos te animan a seguir mejorando.

Para poder ayudar en la parte de *testing*, he tenido que ir aprendiendo poco a poco [***React-Testing***](https://testing-library.com/docs/react-testing-library/intro), ya que es una de las librerías que usan para comprobar los componentes de React.


#### ¿De dónde viene React Testing?

Es una librería construida sobre **DOM Testing Library** la cual nos proporciona una manera fácil de probar nodos del DOM. 

El uso de dicha librería se puede llevar a diferentes **Frameworks**:

* [**Angular**](https://testing-library.com/docs/angular-testing-library/intro/)
* [**React**](https://testing-library.com/docs/react-testing-library/intro/)
* [**Vue**](https://testing-library.com/docs/vue-testing-library/intro/)

También existe un **plugin** para usar esta librería para test **end-to-end**, en [**Cypress**](https://testing-library.com/docs/cypress-testing-library/intro/) y una implementeción para [**React Native**](https://testing-library.com/docs/react-native-testing-library/intro/).

Una de sus grandes ventajas es que funciona con cualquier entorno que proporcione **DOM API**, como por ejemplo [**JEST**](https://jestjs.io/), [**Mocha**](https://mochajs.org/), [**JSDOM**](https://github.com/jsdom/jsdom) **o un explorador real**.

#### React Testing

Como comenté al principio del artículo, en este proyecto usarémos ***React-Testing***, ya que nos proporciona una gran herramienta para comprobar los componentes del proyectos que nos pida usar [**React**](https://react-bootstrap.github.io/), como es el caso del que tenemos entre manos. Además de ser una solución ligera para probar los componentes, proporciona funciones sencillas, además de **react-dom y react-dom/test-utils**, con la que fomentar mejores prácticas de pruebas.

Interactúa directamente con los **nodos del DOM**, de una manera muy similar a un usuario buscando elementos en nuestra web, con diferentes consultas :

* **ByLabelText**
   
* **ByPlaceholderText**

* **ByText**
     
* **ByAltText**
 
* **ByTitle**
 
* **ByDisplayValue**

* **ByRole**
 
* **ByTestId**

#### ¿Cómo podemos instalarla?

Si creamos la aplicación con el comando ***`npx create-react-app 'nombre de la aplicación'`*** ya viene instalado y no sería necesario usar los comandos que describo más abajo.

 `npm install --save-dev @testing-library/react`

npm install --save-dev @testing-library/react

**- Si usas 'yarn'**

`yarn add --dev @testing-library/react`


#### Primera prueba


En este primer ejemplo vamos a comprobar que se mostraría un texto al darle check a un **checkbox**.

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

En el test buscamos el elmento que está relacionado con el texto de la *etiqueta label*, en este caso sería el *input*, comentar que en la importación de **@testing-library/react** he usado el seudónimo **Testing** como apunte personal para ver los métodos de dicha librería, los cuales veremos más a fondo más adelante en un futuro **post**.

```
import '@testing-library/jest-dom';
import React from 'react';
import * as Testing from '@testing-library/react';
import CheckButton from "./CheckButton";


test('Muestra los hijos al pulsar el check', () => {


    const testMessage = 'Esto es un test';

    Testing.render(<CheckButton>{testMessage}</CheckButton>)

    expect(Testing.screen.queryByText(testMessage)).toBeNull();

    Testing.fireEvent.click(Testing.screen.getByLabelText(/enseñar/i));

    expect(Testing.screen.getByText(testMessage)).toBeInTheDocument();

})
```

 -  **Puedes encontrar más ejemplos en el github de** [**React Testing**](https://github.com/testing-library/react-testing-library#basic-example).

Para terminar la primera parte de esta pequeña investigación de la librería ***React-Testing***, me gustaría comentar que me ha sorprendido bastante lo que nos facilita el poder probar los componentes y la manera sencilla de usar sus métodos, de los cuales hablaré en un segundo post.