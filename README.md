Claro, aquí te dejo una versión mejorada de la documentación, más clara y estructurada, y con un enfoque más detallado sobre el funcionamiento del contrato:

---

# SubastaSmartContract

**SubastaSmartContract** es un contrato inteligente desarrollado en Solidity que permite realizar subastas descentralizadas en la blockchain de Ethereum. Los participantes pueden realizar ofertas en ETH, y el contrato garantiza transparencia, seguridad y un manejo eficiente de las transacciones. El dueño del contrato tiene control sobre el proceso y puede retirar los fondos recaudados, mientras que los participantes no ganadores pueden retirar sus fondos con un pequeño descuento.

---

## Variables

### Variables Públicas

* **`owner`**
  Dirección del creador del contrato. Solo el dueño tiene permisos para finalizar la subasta y retirar los fondos recaudados.

* **`mejorOfertante`**
  Dirección del participante que ha realizado la mejor oferta hasta el momento.

* **`mejorOferta`**
  Monto en **wei** de la mejor oferta realizada hasta ahora.

* **`inicio`**
  Marca el tiempo en el que se inició la subasta, expresado en segundos desde el inicio de la época Unix.

* **`duracion`**
  Duración total de la subasta en segundos. Se define al momento de crear el contrato.

* **`finalizada`**
  Booleano que indica si la subasta ha terminado o está activa.

* **`historialOfertas`**
  Mapeo que almacena un registro de las ofertas realizadas por cada dirección. Permite realizar reembolsos parciales a los usuarios.

* **`depositos`**
  Mapeo que lleva un registro del total acumulado ofertado por cada dirección. Se actualiza cada vez que un participante realiza una oferta.

* **`ofertantes`**
  Arreglo que contiene las direcciones de todos los participantes que han realizado ofertas durante la subasta.

---

## Funciones

### `constructor(uint _duracionInicial)`

Inicializa el contrato, establece la dirección del **`owner`** y define la duración de la subasta en segundos.

### `ofertar() external payable`

Permite que cualquier usuario realice una oferta durante la subasta, siempre que la subasta esté activa.
**Requisitos**:

* La oferta debe ser al menos un 5% mayor que la mejor oferta actual.
* La oferta se acumula a los depósitos previos de la misma dirección, si es el caso.

### `finalizar() external`

Permite al **`owner`** finalizar la subasta una vez que el tiempo haya expirado.
**Requisitos**:

* Solo el **`owner`** puede ejecutarla.
* Una vez ejecutada, emite el evento `SubastaFinalizada`.

### `retirar() external`

Permite a los participantes no ganadores retirar sus depósitos después de que la subasta haya finalizado.
**Requisitos**:

* No está disponible para el ganador.
* Se descuenta una comisión del 2% sobre el monto total del depósito.

### `retirarFondos() external`

Permite al **`owner`** retirar el monto total acumulado en el contrato después de que la subasta haya finalizado.

### `tiempoRestante() external view returns (uint)`

Devuelve el tiempo restante en segundos antes de que la subasta finalice.
Si la subasta ya ha finalizado, retorna **0**.

### `mostrarOfertas() external view returns (address[], uint[])`

Devuelve dos arreglos:

1. Las direcciones de los participantes (oferentes).
2. Los montos totales ofertados por cada uno.

### `ganador() external view returns (address, uint)`

Devuelve la dirección del mejor ofertante y el monto de su oferta.

### `reembolsoParcial() external`

Permite a un usuario recuperar todas sus ofertas anteriores, excepto la última, si ha ofertado más de una vez.
Solo disponible mientras la subasta esté activa.

### `verBalance() external view returns (uint)`

Devuelve el balance total de ETH acumulado en el contrato (en **wei**).

---

## Eventos

### `NuevaOferta(address indexed oferente, uint monto)`

Emitido cada vez que un usuario realiza una nueva oferta válida.

### `SubastaFinalizada(address ganador, uint monto)`

Emitido cuando el **`owner`** finaliza la subasta, registrando al ganador y su monto.

### `FondosRetirados(address indexed to, uint amount)`

Emitido cuando el **`owner`** retira los fondos acumulados en el contrato.

### `ReembolsoParcial(address indexed to, uint amount)`

Emitido cuando un usuario realiza un reembolso parcial de sus ofertas anteriores, excluyendo la última.

---

## ¿Cómo funciona?

1. **Iniciar la subasta**
   El **`owner`** despliega el contrato y establece una duración fija para la subasta. El tiempo de inicio se toma automáticamente en el momento de despliegue.

2. **Realizar una oferta**
   Los participantes pueden realizar ofertas durante el tiempo de la subasta. Cada nueva oferta debe superar a la mejor oferta anterior por al menos un **5%**. Las ofertas se acumulan a lo largo del tiempo.

3. **Reembolsos parciales**
   Si un participante realiza múltiples ofertas, puede retirar el monto acumulado de las ofertas previas (menos la última) usando la función **`reembolsoParcial`**.

4. **Finalizar la subasta**
   Una vez que el tiempo de la subasta ha expirado, el **`owner`** puede finalizarla. Se emite el evento **`SubastaFinalizada`**, y el ganador es determinado.

5. **Retirar fondos**
   El **`owner`** puede retirar todos los fondos acumulados tras finalizar la subasta utilizando la función **`retirarFondos`**.
   Los participantes no ganadores pueden retirar sus depósitos, descontando una comisión del 2%, usando la función **`retirar`**.

---

Este contrato proporciona una manera sencilla, pero segura, de realizar subastas en la blockchain de Ethereum, asegurando transparencia y control para todas las partes involucradas.

---

Con esta documentación, ahora puedes tener una visión clara del propósito y funcionamiento del contrato, además de estar listo para cualquier duda o modificación futura.
