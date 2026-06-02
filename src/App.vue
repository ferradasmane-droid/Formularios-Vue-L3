<script setup>
import { ref, computed } from "vue";

const nombre = ref("");
const edad = ref(null);
const biografia = ref("");

const nivel = ref("");
const interesesSeleccionados = ref([]);
const paisSeleccionado = ref(null);
const tecnologiasSeleccionadas = ref([]);

const enviado = ref(false);

const niveles = [
  { texto: "Junior", valor: "junior" },
  { texto: "Semi Senior", valor: "semi senior" },
  { texto: "Senior", valor: "senior" },
];

const intereses = [
  { texto: "Frontend", valor: "frontend" },
  { texto: "Backend", valor: "backend" },
  { texto: "Diseño UX/UI", valor: "diseño ux/ui" },
  { texto: "Bases de datos", valor: "bases de datos" },
];

const paises = [
  { code: "CL", name: "Chile" },
  { code: "AR", name: "Argentina" },
  { code: "PE", name: "Perú" },
  { code: "CO", name: "Colombia" },
  { code: "Mx", name: "México" },
];

const tecnologias = [
  { nombre: "Vue", valor: "vue" },
  { nombre: "React", valor: "react" },
  { nombre: "Angular", valor: "angular" },
  { nombre: "Svelte", valor: "svelte" },
];

const nombreValido = computed(() => nombre.value.trim().length > 0);

const edadValida = computed(() => {
  return edad.value !== null && edad.value >= 0 && edad.value <= 120;
});

const interesValido = computed(() => interesesSeleccionados.value.length >= 1);

const formularioValido = computed(() => {
  return (
    nombreValido.value &&
    edadValida.value &&
    interesValido.value &&
    paisSeleccionado.value
  );
});

const contadorBiografia = computed(() => biografia.value.length);

const payload = computed(() => {
  return {
    nombre: nombre.value,
    edad: edad.value,
    biografia: biografia.value,
    nivel: nivel.value,
    intereses: interesesSeleccionados.value,
    pais: paisSeleccionado.value,
    tecnologias: tecnologiasSeleccionadas.value,
  };
});

const enviarFormulario = () => {
  enviado.value = true;
  console.log(
    "Payload JSON del formulario:",
    JSON.stringify(payload.value, null, 2),
  );
  alert(
    "Formulario enviado correctamente. Revisa la consola para ver el JSON.",
  );
};

const limpiarFormulario = () => {
  nombre.value = "";
  edad.value = null;
  biografia.value = "";
  nivel.value = "junior";
  interesesSeleccionados.value = [];
  paisSeleccionado.value = null;
  tecnologiasSeleccionadas.value = [];
  enviado.value = false;
};
</script>

<template>
  <main class="contenedor">
    <section class="formulario">
      <h1>Registro de Perfil</h1>
      <p class="descripcion">
        Ejercicio de binding de formularios en Vue con v-model, :value, v-for y
        validación básica.
      </p>

      <form @submit.prevent="enviarFormulario">
        <h2>Datos básicos</h2>

        <div class="grupo">
          <label for="nombre">Nombre</label>
          <input
            id="nombre"
            type="text"
            v-model.trim="nombre"
            placeholder="Ingresa tu nombre"
            :class="{ error: !nombreValido && nombre.length === 0 }"
          />
          <small v-if="!nombreValido">El nombre es obligatorio</small>
        </div>

        <div class="grupo">
          <label for="edad">Edad</label>
          <input
          id="edad"
          type="number"
          v-model.number="edad"
          placeholder="Ingresa tu edad"
          :class="{ error: edad !== null && !edadValida}"
          />
          <small v-if="edad !== null && !edadValida">
            La edad debe estar entre 0 y 120.
          </small>
        </div>

        <div class="grupo">
          <label for="biografia">Biografía</label>
          <textarea 
            id="biografia"
            v-model.lazy="biografia"
             placeholder="Escribe una biografía"
          ></textarea>
          <small>Caracteres: {{ contadorBiografia }}</small>
        </div>

        <h2>Preferencias</h2>

        <div class="grupo">
          <label>Nivel</label>

          <div 
            v-for="opcion in niveles" 
            :key="opcion.valor" 
            class="opcion"
          >
            <input
              type="radio"
              name="nivel"
              v-model="nivel"
              :value="opcion.valor"
            />
            <span>{{ opcion.texto }}</span>
          </div>
        </div>

        <div class="grupo">
          <label>Intereses</label>

          <div 
            v-for="interes in intereses" 
            :key="interes.valor" 
            class="opcion"
          >
            <input
              type="checkbox"
              v-model="interesesSeleccionados"
              :value="interes.valor"
            />
            <span>{{ interes.texto }}</span>
          </div>

          <small v-if="!interesValido">
            Debes selecionar al menos un interés.
          </small>
        </div>

        <div class="grupo">
          <label for="pais">País</label>
          <select id="pais" v-model="paisSeleccionado">
            <option disabled :value="null">Selecciona un país</option>

            <option 
             v-for="pais in paises" 
             :key="pais.code" 
             :value="pais"
             >
              {{ pais.name }}
            </option>
          </select>

          <small v-if="!paisSeleccionado"> 
            Debes seleccionar un país. 
          </small>
        </div>

        <div class="grupo">
          <label for="tecnologias">Tecnologías</label>
          <select 
            id="tecnologias" 
            multiple 
            v-model="tecnologiasSeleccionadas"
            >
            <option
              v-for="tecnologia in tecnologias"
              :key="tecnologia.valor"
              :value="tecnologia.valor"
            >
              {{ tecnologia.nombre }}
            </option>
          </select>

          <small>
            Puedes seleccionar varias tecnologías usando Ctrl + clic.
          </small>
        </div>

        <div class="botones">
          <button type="submit" :disabled="!formularioValido">
            Enviar
          </button>

          <button type="button" class="secundario" @click="limpiarFormulario">
            Limpiar
          </button>
        </div>
      </form>
    </section>

    <section class="resumen">
      <h2>Resumen en tiempo real</h2>

      <p><strong>Nombre:</strong> {{ nombre || 'Sin ingresar' }}</p>
      <p><strong>Edad:</strong> {{ edad ?? 'Sin ingresar' }}</p>
      <p>
        <strong>País</strong>
        {{ paisSeleccionado ? paisSeleccionado.name : 'Sin seleccionar' }}
      </p>

      <p><strong>Nivel:</strong> {{ nivel }}</p>

      <p>
        <strong>Intereses</strong>
        {{ interesesSeleccionados.length ? interesesSeleccionados.join(",") : 'Sin seleccionar'}}
      </p>

      <p>
        <strong>Tecnologías:</strong>
        {{ tecnologiasSeleccionadas.length ? tecnologiasSeleccionadas.join(",") : 'Sin selecccionar'}}
      </p>

      <p><strong>Biografía:</strong></p>
      <p class="bio">
        {{ biografia || 'Sin biografía ingresada' }}
      </p>

      <p><strong>Caracteres biografía:</strong> {{ contadorBiografia }}</p>

      <div class="estado">
        <p v-if="formularioValido" class="valido">
          Formulario válido
        </p>

        <p v-else class="invalido">
          Formulario incompleto o inválido
        </p>
      </div>

      <div v-if="enviado" class="mensaje">
        Formulario enviado correctamente.
      </div>

      <h3>Payload JSON</h3>
      <pre>{{ payload }}</pre>
    </section>
  </main>
</template>

<style scoped>
* {
  box-sizing: border-box;
}

.contenedor {
  min-height: 100vh;
  padding: 30px;
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 25px;
  background-color: #f4f6f8;
  font-family: Arial, Helvetica, sans-serif;
}

.formulario,
.resumen {
  background-color: white;
  padding: 25px;
  border-radius: 12px;
  box-shadow: 0 4px 12px rgba(0, 0, 0e, 0.08);
}

h1,
h2,
h3 {
  color: #253041;
}

.descripcion {
  color: #555;
  margin-bottom: 20px;
}

.grupo {
  margin-bottom: 18px;
  display: flex;
  flex-direction: column;
  gap: 8px;
}

input,
textarea,
select {
  padding: 10px;
  border: 1px solid #bbb;
  border-radius: 8px;
  font-size: 15px;
}

textarea {
  min-height: 90px;
  resize: vertical;
}

select[multiple] {
  min-height: 110px;
}

.opcion {
  display: flex;
  align-items: center;
  gap: 8px;
}

.opcion input {
  width: auto;
}

small {
  color: #c0392b;
  font-size: 13px;
}

.error {
  border: 2px solid #c0392b;
  background-color: #fff5f5;
}

.botones {
  display: flex;
  gap: 12px;
  margin-top: 20px;
}

button {
  padding: 10px 18px;
  border: none;
  border-radius: 8px;
  background-color: #2d6cdf;
  color: white;
  font-weight: bold;
  cursor: pointer;
}

button:hover {
  background-color: #1d4fa3;
}

button:disabled {
  background-color: #aaa;
  cursor: not-allowed;
}

.secundario {
  background-color: #6c757d;
}

.secundario:hover {
  background-color: #f1f1f1;
  padding: 12px;
  border-radius: 8px;
  min-height: 50px;
}

.valido {
  color: #1e8449;
  font-weight: bold;
}

.invalido {
  color: #c0392b;
  font-weight: bold;
}

.mensaje {
  background-color: #d4edda;
  color: #155724;
  padding: 12px;
  border-radius: 8px;
  margin: 15px 0;
}

pre {
  background-color: #222;
  color: #eee;
  padding: 15px;
  border-radius: 8px;
  overflow-x: auto;
}

@media (max-width: 768px) {
  .contenedor {
    grid-template-columns: 1fr;
    padding: 15px;
  }
}
</style>
