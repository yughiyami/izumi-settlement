# Izumi · Liquidación Compartida

**Un pedido que una MYPE no puede atender sola se convierte en una oferta coordinada con su red — y el dinero se reparte entre quienes lo produjeron, en la misma transacción en que el cliente paga.**

Construido sobre Stellar. Track: Realtime Finance.

---

## El problema

Una MYPE peruana recibe un pedido de 60 prendas. Puede hacer 20. Llama a dos talleres de su red que completan las otras 40. El pedido se entrega.

Hasta ahí, el problema está resuelto por WhatsApp y confianza. Lo que no está resuelto es el dinero.

El cliente le paga **todo** al coordinador. El coordinador le paga a los talleres cuando puede: una semana, dos, tres. Durante ese tiempo los talleres financiaron con su propio capital a alguien que no es su cliente, sin contrato, sin garantía y sin registro. Si el coordinador se demora, se enferma o decide pagar otra cosa primero, el taller no tiene a quién reclamarle.

Ese retraso no es un problema de software de gestión. Es un problema de **orden de flujo del dinero**: hoy el dinero entra a un solo bolsillo y se reparte después, manualmente, por confianza.

Esa fricción es la razón por la que estas redes no escalan. Un taller acepta colaborar con quien ya conoce, porque el riesgo de cobro es personal, no contractual. La red se detiene en el borde de la confianza directa.

## La premisa

**La oferta aceptada es la instrucción de pago.**

Cuando el cliente acepta la versión de una oferta, ya está definido exactamente quién aportó qué y por cuánto. Esa composición —que Izumi ya calcula, versiona y audita— deja de ser un documento comercial y pasa a ser el reparto ejecutable de la liquidación.

El cliente paga una vez. En el mismo cierre de ledger de Stellar, cada participante recibe su parte: el coordinador su margen, cada proveedor su costo confirmado. No hay un bolsillo intermedio que reparta después. No hay comisión pendiente. No hay que confiar en que el coordinador pague.

El coordinador deja de ser tesorero de plata ajena y vuelve a ser lo que debería: quien consigue y coordina el pedido.

## Por qué Stellar y no otra cosa

No es preferencia de red. Es que el caso de uso exige cuatro cosas a la vez, y pocas redes las dan juntas:

- **Reparto atómico**: varios destinatarios en una sola transacción. O cobran todos, o no cobra nadie. Sin estados intermedios donde el dinero quedó a mitad de camino.
- **Costo por participante irrelevante**: repartir entre cuatro talleres un pedido de S/ 3.000 no puede costar en comisiones lo que gana el más chico. En Stellar el fee es una fracción de centavo por operación.
- **Denominación estable**: el taller cotiza en soles y necesita cobrar valor, no exposición a un activo volátil. USDC sobre Stellar, con off-ramp existente en la región.
- **Confirmación en segundos**: el proveedor ve el pago antes de soltar la mercadería. Un cierre de ledger son ~5 segundos.

Y una quinta que solo se ve después: cada liquidación deja **historial verificable de ventas cumplidas** para negocios que hoy no tienen historial crediticio de ningún tipo. Eso es la base de lo que viene después de este MVP.

## Qué se construye

Sobre el núcleo de Izumi, que ya existe y funciona: solicitud → aportes de proveedores → oferta versionada → aceptación → cumplimiento, con canal de WhatsApp, capa de propuestas asistida por IA bajo confirmación humana, y auditoría por organización.

Lo que este proyecto agrega:

1. **Contrato de reparto en Soroban.** Recibe el identificador del pedido y la composición de la oferta aceptada; retiene el pago del cliente y libera a cada participante según lo acordado. Reglas fijadas al aceptar, no al pagar.
2. **Liquidación desde la aceptación.** La transición que hoy marca una oferta como aceptada pasa a preparar la liquidación con esa composición exacta. Cambiar la oferta exige nueva aceptación y nueva liquidación — la regla ya existente del dominio, ahora con consecuencia económica.
3. **Cobro por QR en soles.** El cliente escanea y paga; la conversión y los fees quedan resueltos por debajo. Ni el cliente ni el taller necesitan saber qué es una trustline, un stroop o un XLM.
4. **Recibo verificable por participante.** Cada proveedor recibe el hash de la transacción donde cobró. Es su comprobante, auditable de forma independiente en Stellar Expert, sin depender de que el coordinador lo confirme.

## Qué NO es

Este proyecto se toma en serio sus límites, y los declara antes de que los pregunten:

- **No custodia fondos de terceros fuera del flujo de un pedido.** El dinero entra y se reparte; no hay saldo en reposo administrado por la plataforma.
- **No certifica inventario, identidad ni calidad.** Que un aporte esté registrado en blockchain no prueba que el taller tenga las prendas. La cadena liquida un acuerdo; no verifica el mundo físico.
- **No es factoring, crédito ni adelanto.** Se liquida lo que el cliente pagó, cuando lo pagó.
- **No hay token propio.** Se usa USDC. Un token propio sería un problema regulatorio sin un beneficio para el taller.
- **No hay agente que mueva dinero.** La capa de IA propone; una persona confirma. Las herramientas del agente son de solo lectura y su única escritura es una propuesta pendiente de aprobación. Esa regla no se relaja para la liquidación.

## Estado

Núcleo de dominio, API, interfaz web y móvil: construidos. Canal de WhatsApp y capa agéntica: construidos, pendientes de prueba con credenciales reales.

Capa de liquidación en Stellar: es lo que se construye en esta edición. Testnet, con contract ID verificable y evidencia on-chain de un ciclo completo.

## Criterio de éxito de la demo

Un pedido de 60 prendas con tres participantes. El cliente paga una vez. Tres wallets cambian de saldo en la misma transacción, en el mismo cierre de ledger, en las proporciones exactas de la oferta que se aceptó. Verificable por cualquiera en Stellar Expert, sin acceso a nuestra base de datos.

Si el reparto no coincide con la oferta aceptada, la demo falló. Ese es el único número que importa.
