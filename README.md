# Reto 3: Automatización Web E2E (Selenium + Python) y Matriz de Prueba
**Candidato:** Carlos Dario Lara Garcia (`@Dario215-svg`)   
**Evaluación:** Challenge Técnico Covalto - Prueba Final (Automated Testing & Document)  
**Sistema Bajo Prueba (SUT):** [Swag Labs - SauceDemo](https://www.saucedemo.com/)  

---

##  Objetivo
1. Diseñar una **Matriz de Casos de Prueba ** que documente los escenarios críticos de negocio: autenticación, carrito de compras, **registro completo de datos del usuario en Checkout**, validaciones de campos obligatorios, cálculo financiero e integridad de sesión.
2. Desarrollar un script de automatización web utilizando **Selenium WebDriver** y **Python (Pytest)** que ejecute la matriz de pruebas.

---

##  1. Resumen de Cobertura de la Matriz

| Módulo Evaluado | Casos de Prueba | Positivos | Negativos | Estado en Pytest |
| :--- | :---: | :---: | :---: | :---: |
| **Autenticación y Control de Acceso** | `CP-01`, `CP-02` | 1 | 1 | 2 / 2 Passed |
| **Formulario de Registro (Checkout)** | `CP-03`, `CP-04`, `CP-05`, `CP-06` | 1 | 3 | 4 / 4 Passed |
| **Cálculo Financiero y Sesión (Logout)** | `CP-07` | 1 | 0 |  1 / 1 Passed |
| **TOTAL** | **7 Casos** | **3** | **4** | **7 / 7 PASSED** |

---

##  2. Matriz de Casos de Prueba a Detalle

| ID Caso | Módulo | Nombre del Caso de Prueba | Prioridad | Tipo | Precondiciones | Pasos de Ejecución (Paso a Paso) | Datos de Prueba (Inputs) | Resultado Esperado | Resultado Obtenido | Estado |
| :--- | :--- | :--- | :---: | :---: | :--- | :--- | :--- | :--- | :--- | :---: |
| **CP-01** | Autenticación | Inicio de sesión exitoso con credenciales válidas | Alta | Funcional Positiva | 1. Tener conexión a internet.<br>2. Navegador Chrome abierto sin sesión activa. | **1.** Ingresar a `https://www.saucedemo.com/`.<br>**2.** Hacer clic en el campo `Username` (`id="user-name"`).<br>**3.** Escribir el usuario `standard_user`.<br>**4.** Hacer clic en el campo `Password` (`id="password"`).<br>**5.** Escribir la contraseña `secret_sauce`.<br>**6.** Hacer clic en el botón `Login` (`id="login-button"`). | • **Username:** `standard_user`<br>• **Password:** `secret_sauce` | **1.** Se carga el formulario de login.<br>**2-5.** Los campos aceptan los caracteres.<br>**6.** El sistema autentica al usuario y redirige a `/inventory.html` mostrando el catálogo. | Redirección exitosa a `/inventory.html`. |  Passed |
| **CP-02** | Autenticación | Restricción de acceso para usuario bloqueado |  Alta | Funcional Negativa | 1. Navegador abierto en la pantalla de Login (`https://www.saucedemo.com/`). | **1.** Ubicar el campo `Username` (`id="user-name"`).<br>**2.** Ingresar el usuario bloqueado `locked_out_user`.<br>**3.** Ubicar el campo `Password` (`id="password"`).<br>**4.** Ingresar la contraseña `secret_sauce`.<br>**5.** Hacer clic en el botón `Login` (`id="login-button"`).<br>**6.** Validar el contenedor de error (`data-test="error"`). | • **Username:** `locked_out_user`<br>• **Password:** `secret_sauce` | **1-4.** Se ingresan las credenciales.<br>**5-6.** El sistema impide el acceso, permanece en Login y muestra: *"Epic sadface: Sorry, this user has been locked out."* | Se bloquea el acceso y se muestra la alerta esperada. |  Passed |
| **CP-03** | Registro / Checkout E2E | **Registro completo de datos del comprador y finalización de orden** |  Alta | Funcional Positiva / E2E | 1. Usuario `standard_user` autenticado.<br>2. Encontrarse en el catálogo (`/inventory.html`). | **1.** Ubicar *"Sauce Labs Backpack"* y hacer clic en `Add to cart` (`id="add-to-cart-sauce-labs-backpack"`).<br>**2.** Hacer clic en el ícono del carrito (`class="shopping_cart_link"`).<br>**3.** Hacer clic en `Checkout` (`id="checkout"`).<br>**4.** En el formulario de registro (`/checkout-step-one.html`), ingresar `Carlos Dario` en `First Name` (`id="first-name"`).<br>**5.** Ingresar `Lara Garcia` en `Last Name` (`id="last-name"`).<br>**6.** Ingresar `07000` en `Zip/Postal Code` (`id="postal-code"`).<br>**7.** Hacer clic en `Continue` (`id="continue"`).<br>**8.** En `/checkout-step-two.html`, hacer clic en `Finish` (`id="finish"`).<br>**9.** Validar el encabezado en `/checkout-complete.html`. | • **Producto:** `Sauce Labs Backpack`<br>• **First Name:** `Carlos Dario`<br>• **Last Name:** `Lara Garcia`<br>• **Postal Code:** `07000` | **1-3.** Se accede al formulario de registro.<br>**4-7.** El sistema registra los datos del usuario y avanza al resumen.<br>**8-9.** Se procesa la orden, redirige a `/checkout-complete.html` y muestra: *"Thank you for your order!"*. | Registro completo y compra finalizada con éxito. |  Passed |
| **CP-04** | Registro / Checkout | Validar obligatoriedad del campo "First Name" en registro |  Media | Funcional Negativa | 1. Usuario autenticado.<br>2. Encontrarse en el formulario de registro (`/checkout-step-one.html`). | **1.** Dejar el campo `First Name` (`id="first-name"`) vacío.<br>**2.** Ingresar `Lara Garcia` en `Last Name` (`id="last-name"`).<br>**3.** Ingresar `07000` en `Zip/Postal Code` (`id="postal-code"`).<br>**4.** Hacer clic en `Continue` (`id="continue"`).<br>**5.** Verificar el mensaje en `data-test="error"`. | • **First Name:** *(Vacío)*<br>• **Last Name:** `Lara Garcia`<br>• **Postal Code:** `07000` | **1-3.** Se llenan apellido y código postal.<br>**4-5.** El sistema bloquea el avance y muestra el error: *"Error: First Name is required"*. | Se detiene el avance y muestra *"Error: First Name is required"*. | Passed |
| **CP-05** | Registro / Checkout | Validar obligatoriedad del campo "Last Name" en registro |  Media | Funcional Negativa | 1. Usuario autenticado.<br>2. Encontrarse en el formulario de registro (`/checkout-step-one.html`). | **1.** Ingresar `Carlos Dario` en `First Name` (`id="first-name"`).<br>**2.** Dejar el campo `Last Name` (`id="last-name"`) vacío.<br>**3.** Ingresar `07000` en `Zip/Postal Code` (`id="postal-code"`).<br>**4.** Hacer clic en `Continue` (`id="continue"`).<br>**5.** Verificar el mensaje en `data-test="error"`. | • **First Name:** `Carlos Dario`<br>• **Last Name:** *(Vacío)*<br>• **Postal Code:** `07000` | **1-3.** Se llenan nombre y código postal.<br>**4-5.** El sistema bloquea el avance y muestra el error: *"Error: Last Name is required"*. | Se detiene el avance y muestra *"Error: Last Name is required"*. |  Passed |
| **CP-06** | Registro / Checkout | Validar obligatoriedad del campo "Zip/Postal Code" en registro |  Media | Funcional Negativa | 1. Usuario autenticado.<br>2. Encontrarse en el formulario de registro (`/checkout-step-one.html`). | **1.** Ingresar `Carlos Dario` en `First Name` (`id="first-name"`).<br>**2.** Ingresar `Lara Garcia` en `Last Name` (`id="last-name"`).<br>**3.** Dejar el campo `Zip/Postal Code` (`id="postal-code"`) vacío.<br>**4.** Hacer clic en `Continue` (`id="continue"`).<br>**5.** Verificar el mensaje en `data-test="error"`. | • **First Name:** `Carlos Dario`<br>• **Last Name:** `Lara Garcia`<br>• **Postal Code:** *(Vacío)* | **1-3.** Se llenan nombre y apellido.<br>**4-5.** El sistema bloquea el avance y muestra el error: *"Error: Postal Code is required"*. | Se detiene el avance y muestra *"Error: Postal Code is required"*. |  Passed |
| **CP-07** | Resumen y Sesión | Validación de cálculo de total con impuestos y cierre de sesión (Logout) |  Alta | Regla de Negocio / Funcional | 1. Usuario autenticado con *"Sauce Labs Backpack"* ($29.99) en el carrito. | **1.** Ir al carrito y hacer clic en `Checkout`.<br>**2.** Completar el registro (`Carlos Dario`, `Lara Garcia`, `07000`) y dar clic en `Continue`.<br>**3.** En `/checkout-step-two.html`, localizar el total (`class="summary_total_label"`).<br>**4.** Validar que `Item total: $29.99` + `Tax: $2.40` sea igual a `Total: $32.39`.<br>**5.** Abrir el menú lateral (`id="react-burger-menu-btn"`).<br>**6.** Hacer clic en `Logout` (`id="logout_sidebar_link"`).<br>**7.** Verificar regreso al Login. | • **Subtotal:** `$29.99`<br>• **Tax (8%):** `$2.40`<br>• **Total:** `$32.39` | **1-4.** El sistema calcula correctamente el monto total con impuestos (`$32.39`).<br>**5-7.** Se destruye la sesión activa y regresa a la pantalla de Login. | Total `$32.39` validado y sesión cerrada correctamente. |  Passed |

---

##  3. Evidencia de Ejecución en Consola

Salida obtenida al ejecutar `python -m pytest test_saucedemo.py -v -s`:

```text
test_saucedemo.py::test_cp01_login_exitoso
---------------------------------------------------------------------------
CP-01: Inicio de sesión exitoso con credenciales válidas
---------------------------------------------------------------------------
  [Paso 1] Navegar al sitio web: [https://www.saucedemo.com/](https://www.saucedemo.com/)
  [Paso 2] Ingresar usuario válido: 'standard_user'
  [Paso 3] Ingresar contraseña válida: 'secret_sauce'
  [Paso 4] Hacer clic en el botón 'Login'
  [Validación] Verificar redirección al inventario (/inventory.html)
  [RESULTADO CP-01] PASSED - Acceso concedido correctamente.
PASSED

test_saucedemo.py::test_cp03_registro_completo_y_compra_exitosa
---------------------------------------------------------------------------
CP-03: Flujo E2E - Registro completo de datos del usuario y compra exitosa
---------------------------------------------------------------------------
  [Precondición] Iniciar sesión con 'standard_user'
  [Paso 1] Agregar producto 'Sauce Labs Backpack' al carrito
  [Paso 2] Ir al carrito de compras y hacer clic en 'Checkout'
  [Paso 3] Completar formulario de registro: First Name='Carlos Dario', Last Name='Lara Garcia', Zip='07000'
  [Paso 4] Hacer clic en 'Continue' y posteriormente en 'Finish'
  [Validación] Confirmación en pantalla: 'Thank you for your order!'
  [RESULTADO CP-03] PASSED - Registro completo y orden finalizada.
PASSED
...
================================ 7 passed in 6.84s ================================
