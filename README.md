# Formularios Binding en Vue

Este proyecto corresponde al ejercicio **Binding de Formularios en Vue**.

El objetivo es construir una página de registro/perfil utilizando Vue 3, demostrando el uso `v-model`, two-way binding, listas dinámicas con `v-for`, vinculación de valores con `value`, validaciones básicas y resumen en tiempo real.

## Tecnologías utilizadas

- Vue 3
- Vite
- JavaScript
- HTML
- CSS

## Funcionalidades del proyecto

El formulario permite ingresar y seleccionar los siguientes datos:

- Nombre con `v-model.trim`
- Edad con `v-model.number`
- Biografía con `v-model.lazy`
- Nivel mediante radio buttons
- Intereses mediante checkboxes
- País mediante select simple
- Tecnologías mediante select múltiple
- Resumen en tiempo real de los datos ingresados
- Contador de caracteres de la biografía
- Validación básica del formulario
- Botón de envío deshabilitado mientras el formulario sea inválido
- Visualización del payload JSON en consola

## Conceptos aplicados

### Two-way binding

Se utilizó `v-model`para conectar los campos del formulario con las variables del componente.
Esto permite que los datos ingresados por el usuario se actualicen automáticamente en el estado de Vue y en el resumen mostrado en pantalla.

### Modificadores de v-model

Se aplicaron los siguientes modificadores:

- `v-model.trim`: elimina espacios al inicio y al final del nombre.
- `v-model.number`: convierte la edad ingresada en número.
- `v-model.lazy`: actualiza la biografía cuando el usuario termina de editar el campo.

### Listas dinámicas

Las opciones de nivel, intereses, países y tecnologías se generan con `v-for`desde arreglos definidos en el componente.

### Vinculación de valores

Se utilizó `:value`en radio buttons, checkboxes y selects.
En el caso del país, se guarda un objeto completo, por ejemplo:

```js
{
    code: 'CL',
    name: 'Chile'
}
```
## Validaciones
El formulario valida que:

- El nombre sea obligatorio.
- La edad esté entre 0 y 120.
- Se selecione al menos un interés.
- Se seleccione un país.

Mientras el formulario no sea válido, el botón de enviar permanece deshabilitado.


## Instalación y ejecución
Para ejecutar el proyecto, primero se deben instalar las dependencias:

```sh
npm install
```

Luego se inicia el servidor de desarrollo:

```sh
npm run dev
```
Finalmente, se abre en el navegador la URL entregada por Vite, por ejemplo:

`http://localhost:5173/`

## Estructura principal

```Bash
src/
├─ assets/
│    └─ main.css
├─ App.vue
└─ main.js
```
## Autora

Desarrollado por Nelly Ferrada.