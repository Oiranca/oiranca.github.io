title: Un novato junto a "React Testing" última parte (Firing Events)
author: Samuel Romero Arbelo
tags: []
categories: []
date: 2021-05-09 10:48:00
---
![](https://i.imgur.com/StMOQvK.png)


Me gustaría empezar este último post sobre **"React Testing"** comentando que nada más entrar en el apartado de **Firing Events** la documentación nos indica que en muchos proyectos se usa **fireEvent, cuando la mayor parte deberias usar** **[User-Event](https://testing-library.com/docs/ecosystem-user-event/)**


 # FireEvent

Son eventos de los propios elementos del DOM, dichos eventos los podemos ver en la documentación oficial **[Developer Mozilla](https://developer.mozilla.org/es/docs/Web/API)**.

Como por el ejmplo el evento **Click**, el cual se ejecuta al pulsar nuestro ratón. 


![](https://i.imgur.com/822ydEe.gif)

Aquí podemos ver un ejemplo sencillo de como funcionaría un componente **Button** y su test:

**Componente**
   
```
        import * as React from "react";
        import "./Button.scss";

        const Button = ({ onClick, text }) => {
          text = "Click fireEvent";
          return (
            <div className={"Button"}>
              <h3>DOMFireEvent</h3>
              <button type="submit" onClick={onClick}>
                {text}
              </button>
            </div>
          );
        };

        export default Button;


```

<br/>

 **Test**

  ```
        import * as React from "react";
        import * as Testing from "@testing-library/react";
        import Button from "./Button";  

        test("mouseEvent click", () => {
          const changeHandle = jest.fn();
          const component = Testing.render(
            <Button onClick={changeHandle}>Click button to fireEvent</Button>
          );

          const button = component.getByText("Click fireEvent");

          Testing.fireEvent.click(button);

          expect(changeHandle).toHaveBeenCalledTimes(1);
        });
  ```



* ### FireEvent[eventName]

En este punto la documentación aconseja  que veamos los [tipos de eventos](https://github.com/testing-library/dom-testing-library/blob/master/src/event-map.js) que podemos encontrar.

Esta manera de llamar al evento es más directa, por ejemplo tenémos el evento **change** el cual nos simularía un cambio en el elemento.

![](https://i.imgur.com/Dg3jK8L.gif)

Aquí podemos ver un ejemplo sencillo de como funcionaría un componente **Input** y su test:


**Componente**
       
   
```
      import * as React from "react";
      import "./InputFireEvent.scss";

      const InputFireEvent = () => {
        const [value, setValue] = React.useState("");
        const handleChange = (ev) => {
          ev.preventDefault();
          const inputtedValue = ev.currentTarget.value;
          setValue(inputtedValue);
        };

        return (
          <div className={"input-with-title"}>
            <h3>Change FireEvent</h3>
            <input value={value} aria-label="date-input" onChange={handleChange} />
          </div>
        );
      };

      export default InputFireEvent;



```

<br/>

 **Test**

  ```
      import * as React from "react";
      import * as Testing from "@testing-library/react";
      import InputFireEvent from "./InputFireEvent";

      test("change fireEvent", () => {
        const utils = Testing.render(<InputFireEvent />);
        const input = utils.getByLabelText("date-input");
        const date = "2021-02-03";

        Testing.fireEvent.change(input, { target: { value: date } });
        expect(input.value).toBe("2021-02-03");
      });


  ```

<br/>

* ### CreateEvent[eventName]

Esta opción nos permite crear eventos que luego podemos lazar con **fireEvent**. La ventaja que nos proporciona además de poder usarlo en diferentes puntos de nuestro test, es que si necesita acceder a propiedades de eventos que no se pueden iniciar mediante código, lo puede hacer. 

Puedes encontrar más información en [#createeventeventname](https://testing-library.com/docs/dom-testing-library/api-events/#createeventeventname)