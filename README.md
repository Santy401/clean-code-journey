# clean-code-journey

## ExpenseCore

**Sistema de gestión y control financiero personal**

> Prueba técnica de arquitectura hexagonal enfocada en la independencia de las reglas de negocio, la consistencia de las operaciones financieras y la claridad de las responsabilidades del sistema.

## 1. Descripción general

ExpenseCore es un sistema de administración financiera personal diseñado para registrar, organizar y analizar movimientos económicos.

Su propósito es proporcionar una visión clara del comportamiento financiero de una persona, permitiéndole conocer cuánto dinero recibe, cuánto gasta, cómo distribuye sus recursos y cuál es su balance disponible.

El proyecto se desarrolla como una prueba técnica cuyo objetivo es demostrar que las reglas fundamentales de un negocio pueden mantenerse independientes de los mecanismos utilizados para interactuar con el sistema.

## 2. Problema

El control de las finanzas personales suele realizarse mediante anotaciones dispersas, cálculos manuales o herramientas que no mantienen una relación clara entre ingresos, gastos y presupuestos.

Esto genera dificultades como:

- Desconocimiento del dinero disponible.
- Falta de organización de los movimientos financieros.
- Dificultad para identificar las principales fuentes de gasto.
- Ausencia de seguimiento de presupuestos.
- Inconsistencias en los balances calculados.
- Poca claridad sobre el comportamiento financiero durante un período.

ExpenseCore busca resolver estos problemas mediante un modelo de gestión financiera sencillo, consistente y basado en reglas explícitas.

## 3. Objetivo general

**Diseñar y desarrollar un sistema de gestión financiera personal que permita administrar ingresos, gastos, categorías y presupuestos, garantizando la consistencia de las operaciones mediante reglas de negocio independientes de los mecanismos externos del sistema.**

La prueba técnica deberá demostrar que el comportamiento financiero conserva sus reglas y resultados independientemente de cómo se soliciten, registren o consulten las operaciones.

## 4. Objetivos específicos

1. Permitir el registro y seguimiento de ingresos y gastos.
2. Organizar los movimientos financieros mediante categorías.
3. Calcular el balance disponible a partir de los movimientos registrados.
4. Establecer presupuestos para controlar los gastos.
5. Validar las operaciones según reglas financieras definidas.
6. Consultar resúmenes financieros por períodos.
7. Mantener un comportamiento coherente ante operaciones válidas e inválidas.
8. Demostrar la independencia conceptual de las reglas de negocio frente a los mecanismos externos.

## 5. Alcance funcional

### 5.1 Gestión de movimientos

El sistema permitirá:

- Registrar ingresos.
- Registrar gastos.
- Consultar movimientos existentes.
- Modificar movimientos registrados.
- Eliminar movimientos cuando corresponda.
- Asociar movimientos con categorías.
- Consultar movimientos dentro de un período determinado.

Cada movimiento tendrá una identidad única, un tipo, un importe, una fecha, una categoría y una descripción opcional.

### 5.2 Gestión de categorías

El usuario podrá organizar sus movimientos mediante categorías.

El sistema permitirá crear, consultar, modificar y desactivar categorías.

Las categorías estarán asociadas a ingresos o gastos, evitando clasificaciones incompatibles.

### 5.3 Balance financiero

ExpenseCore calculará el balance financiero considerando los ingresos y gastos registrados.

**Balance = Total de ingresos − Total de gastos**

El balance representa el resultado neto de los movimientos registrados, no necesariamente el saldo real de una cuenta bancaria.

El usuario podrá consultar el balance general y el correspondiente a un período determinado.

### 5.4 Gestión de presupuestos

El sistema permitirá definir límites de gasto para categorías durante períodos específicos.

Cada presupuesto contará con:

- Categoría asociada.
- Período de aplicación.
- Importe máximo permitido.
- Cantidad consumida.
- Cantidad restante.
- Estado del presupuesto.

Los presupuestos tendrán una función informativa y de seguimiento. Superar un límite generará una condición de exceso, pero no impedirá registrar un gasto válido.

### 5.5 Resumen financiero

El sistema ofrecerá información consolidada sobre:

- Total de ingresos.
- Total de gastos.
- Balance neto.
- Gastos por categoría.
- Presupuestos utilizados.
- Presupuestos excedidos.
- Movimientos registrados durante un período.

## 6. Reglas de negocio

### RN-01. Validez de los importes

Todo movimiento financiero debe tener un importe mayor que cero.

No se permiten importes negativos ni iguales a cero.

### RN-02. Clasificación de movimientos

Cada movimiento debe identificarse exclusivamente como ingreso o gasto.

Un movimiento no puede pertenecer simultáneamente a ambos tipos.

### RN-03. Categorías compatibles

Todo movimiento debe pertenecer a una categoría existente, activa y compatible con su tipo.

### RN-04. Integridad del balance

El balance debe reflejar los movimientos vigentes registrados en el sistema.

La modificación o eliminación de un movimiento debe actualizar los resultados financieros correspondientes.

### RN-05. Identidad de movimientos

Cada movimiento debe tener una identidad única que permita distinguirlo de los demás.

### RN-06. Presupuestos válidos

Un presupuesto debe tener un límite mayor que cero, una categoría de gasto válida y un período definido.

### RN-07. Consumo presupuestario

El consumo de un presupuesto corresponde a la suma de los gastos vigentes de su categoría dentro del período establecido.

### RN-08. Exceso presupuestario

Un presupuesto se considera excedido cuando el gasto acumulado supera su límite.

Al alcanzar exactamente el límite, el presupuesto se considera agotado, pero no excedido.

### RN-09. Consistencia de períodos

Las consultas financieras deben considerar únicamente los movimientos cuya fecha pertenezca al período solicitado.

### RN-10. Conservación de la clasificación histórica

Desactivar una categoría no debe eliminar ni invalidar los movimientos registrados anteriormente.

### RN-11. Operaciones inválidas

Cuando una operación incumpla una regla de negocio, deberá rechazarse sin alterar la información financiera existente.

### RN-12. Moneda de operación

Todos los importes de la prueba utilizarán una única moneda definida para el sistema.

No se contemplan conversiones monetarias ni tipos de cambio.

## 7. Escenarios principales

### Escenario A. Registro de un ingreso

Una persona registra un ingreso de $1.500.000 correspondiente a su salario.

**Resultado esperado:** el ingreso queda registrado y el balance aumenta en $1.500.000.

### Escenario B. Registro de un gasto

La persona registra un gasto de $200.000 en alimentación.

**Resultado esperado:** el gasto queda asociado a su categoría y el balance disminuye en $200.000.

### Escenario C. Control presupuestario

La persona establece un presupuesto mensual de $300.000 para alimentación.

Durante ese mes registra gastos acumulados de $350.000.

**Resultado esperado:** el presupuesto presenta un consumo de $350.000 y un exceso de $50.000. Los gastos permanecen registrados.

### Escenario D. Modificación de un movimiento

Un gasto registrado por $80.000 se corrige a $60.000.

**Resultado esperado:** el balance y el consumo presupuestario reflejan la diferencia de $20.000, sin duplicar el movimiento.

### Escenario E. Rechazo de una operación

Se intenta registrar un gasto con importe negativo.

**Resultado esperado:** la operación es rechazada y el estado financiero permanece sin cambios.

## 8. Fuera del alcance

Para mantener el proyecto enfocado, la primera versión no contemplará:

- Conexiones con entidades bancarias.
- Procesamiento de pagos reales.
- Transferencias entre cuentas.
- Gestión de créditos o préstamos.
- Conversión entre monedas.
- Contabilidad empresarial.
- Declaraciones tributarias.
- Conciliación bancaria.
- Gestión financiera compartida entre múltiples usuarios.
- Predicciones financieras.
- Automatizaciones inteligentes.

Estas capacidades podrán evaluarse en futuras ampliaciones, pero no forman parte de los criterios de finalización de la prueba.

## 9. Criterios de éxito

La prueba técnica se considerará satisfactoria cuando:

- **CE-01:** se puedan registrar, consultar, modificar y eliminar movimientos válidos.
- **CE-02:** las operaciones inválidas sean rechazadas sin producir cambios parciales.
- **CE-03:** los balances se calculen correctamente después de registrar, modificar o eliminar movimientos.
- **CE-04:** las categorías respeten las restricciones de clasificación.
- **CE-05:** los presupuestos reflejen correctamente el consumo, el agotamiento y los excesos.
- **CE-06:** las consultas por período produzcan resultados consistentes.
- **CE-07:** las reglas financieras mantengan el mismo comportamiento independientemente del mecanismo utilizado para ejecutar una operación.
- **CE-08:** sea posible cambiar los mecanismos externos sin modificar las reglas fundamentales del negocio.
- **CE-09:** los escenarios principales puedan verificarse mediante resultados observables y reproducibles.

## 10. Resultado esperado

Al finalizar, ExpenseCore deberá constituir un sistema financiero personal funcional, con un dominio claramente definido y reglas de negocio verificables.

El valor de la prueba no estará determinado por la cantidad de funcionalidades implementadas, sino por la consistencia del comportamiento, la claridad de las responsabilidades y la capacidad de mantener las reglas financieras independientes de los elementos externos.

**ExpenseCore debe demostrar que la lógica del negocio puede evolucionar sin depender de la forma en que el sistema se utiliza o se conecta con su entorno.**