title: Un novato junto a "React Testing" última parte
author: Samuel Romero Arbelo
date: 2021-05-09 10:48:43
tags:
---
# Firing Events

Primeramente me gustaría cometar una conclusión que nos indica la documentación. Dicha conclusión, nos indica que en muchos proyectos se usa **fireEvent, cuando la mayor parte deberias usar** **[User-Event](https://testing-library.com/docs/ecosystem-user-event/)**


* ### FireEvent

Son evento de los propios elementos del DOM, los cuales podemos encontrar en las **[Referencias de la APIWeb de Mozilla](https://developer.mozilla.org/es/docs/Web/API)**

> fireEvent(node: HTMLElement, event: Event)

En estes ejemplo vamos a usar **[MouseEvent](https://developer.mozilla.org/es/docs/Web/API/MouseEvent)**:


**Componente**
       
   
```
import * as React from "react";
import "./Button.scss";

const Button = () => {
  return (
    <div className={"Button"}>
      <h3>DOMFireEvent</h3>
      <button type="submit">Click button to fireEvent</button>
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

test("MouseEvent Click", () => {
  Testing.render(<Button type="submit">Click button to fireEvent</Button>);
  Testing.fireEvent(
    Testing.screen.getByText("Click button to fireEvent"),
    new MouseEvent("click", {
      bubbles: true,
      cancelable: true,
    })
  );
});
  ```

<br/>


* ### FireEvent[eventName]

En este punto la documentación aconseja  que veamos los [tipos de eventos](https://github.com/testing-library/dom-testing-library/blob/master/src/event-map.js) que podemos encontrar.

Esta manera de llamar al evento es más directa, por ejemplo tenémos el evento **change** el cual nos simularía un cambio en el elemento.



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

test("Change FireEvent", () => {
  const utils = Testing.render(<InputFireEvent />);
  const input = utils.getByLabelText("date-input");
  const date = "2021-02-03";

  Testing.fireEvent.change(input, { target: { value: date } });
  expect(input.value).toBe("2021-02-03");
});


  ```

<br/>

* ### CreateEvent[eventName]

Esta opción nos permite crear eventos que luego podemos lazar con **fireEvent**. La ventaja que nos proporciona además de poder usarlo en diferentes puntos de nuestro test, es que si necesita acceder a propiedades de eventos que no se pueden iniciar mediante programación, lo puede hacer.