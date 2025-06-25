SubastaSmartContract
SubastaSmartContract es un contrato inteligente en Solidity para gestionar una subasta descentralizada. Permite que los participantes hagan ofertas en ETH, y el dueño tiene control para finalizar la subasta y retirar fondos.

Variables
owner: Dirección del dueño del contrato (solo él puede finalizar la subasta y retirar fondos).

mejorOfertante: Dirección del participante con la mejor oferta.

mejorOferta: Monto de la mejor oferta realizada.

inicio: Hora de inicio de la subasta.

duracion: Duración total de la subasta en segundos.

finalizada: Si la subasta ha terminado o no.

historialOfertas: Registra las ofertas hechas por cada participante.

depositos: Monto acumulado por cada participante.

ofertantes: Lista de participantes.

Funciones
constructor(uint _duracionInicial): Inicia el contrato, define la duración de la subasta.

ofertar(): Permite hacer una oferta durante la subasta (debe ser un 5% más alta que la mejor oferta).

finalizar(): Solo el dueño puede finalizar la subasta.

retirar(): Permite a los participantes no ganadores retirar su dinero (con una comisión del 2%).

retirarFondos(): Permite al dueño retirar todo el dinero recaudado después de la subasta.

tiempoRestante(): Muestra el tiempo restante de la subasta.

mostrarOfertas(): Muestra los participantes y los montos ofertados.

ganador(): Muestra la dirección del ganador y el monto de su oferta.

reembolsoParcial(): Permite retirar ofertas anteriores (menos la última) mientras la subasta esté activa.

verBalance(): Muestra el balance total del contrato.

Eventos
NuevaOferta(address, uint): Emitido cuando se realiza una nueva oferta.

SubastaFinalizada(address, uint): Emitido cuando el dueño finaliza la subasta.

FondosRetirados(address, uint): Emitido cuando el dueño retira fondos.

ReembolsoParcial(address, uint): Emitido cuando un participante recibe un reembolso parcial.

