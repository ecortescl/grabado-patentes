<script setup>
// Importamos ref para manejar el estado reactivo
import { ref, onMounted } from 'vue';

// Variables reactivas
const patente = ref('');
const fontesCargadas = ref(false);

// Método para generar la imagen
const generarImagen = () => {
  const canvas = document.getElementById('patenteCanvas');
  const ctx = canvas.getContext('2d');
  
  // Limpiar canvas
  ctx.clearRect(0, 0, canvas.width, canvas.height);
  
  // Dibujar fondo
  ctx.fillStyle = '#FFFFFF';
  ctx.fillRect(0, 0, canvas.width, canvas.height);
  
  // Borde
  ctx.strokeStyle = '#000000';
  ctx.lineWidth = 4;
  ctx.strokeRect(10, 10, canvas.width - 20, canvas.height - 20);
  
  // Configurar texto
  ctx.fillStyle = '#000000';
  ctx.font = '60px "BusStop"';
  ctx.textAlign = 'center';
  ctx.textBaseline = 'middle';
  
  // Dibujar patente
  ctx.fillText(patente.value.toUpperCase(), canvas.width / 2, canvas.height / 2);
};

// Método para descargar la imagen
const descargarImagen = () => {
  const canvas = document.getElementById('patenteCanvas');
  const enlace = document.createElement('a');
  enlace.download = 'patente-' + patente.value + '.png';
  enlace.href = canvas.toDataURL('image/png');
  enlace.click();
};

// Cargar fuente personalizada
onMounted(() => {
  const fontFace = new FontFace('BusStop', 'url(/fonts/bus-stop.ttf)');
  
  fontFace.load().then(function(loadedFace) {
    document.fonts.add(loadedFace);
    fontesCargadas.value = true;
    // Si hay una patente predefinida, generar imagen
    if (patente.value) {
      generarImagen();
    }
  }).catch(function(error) {
    console.error('Error al cargar la fuente:', error);
  });
});
</script>

<template>
  <!-- Navbar -->
  <nav class="bg-blue-800 text-white p-4">
    <div class="container mx-auto flex justify-between items-center">
      <div class="text-xl font-bold">Grabado de Patentes Chile</div>
      <div class="hidden md:flex space-x-6">
        <a href="#inicio" class="hover:text-blue-200">Inicio</a>
        <a href="#normativa" class="hover:text-blue-200">Normativa</a>
        <a href="#requisitos" class="hover:text-blue-200">Requisitos</a>
        <a href="#generador" class="hover:text-blue-200">Generador</a>
        <a href="#contacto" class="hover:text-blue-200">Contacto</a>
      </div>
      <div class="md:hidden">
        <button class="focus:outline-none">
          <svg class="w-6 h-6" fill="none" stroke="currentColor" viewBox="0 0 24 24">
            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4 6h16M4 12h16M4 18h16"></path>
          </svg>
        </button>
      </div>
    </div>
  </nav>

  <!-- Hero Section -->
  <section id="inicio" class="bg-gradient-to-r from-blue-700 to-blue-900 text-white py-20">
    <div class="container mx-auto px-4 text-center">
      <h1 class="text-4xl md:text-5xl font-bold mb-4">Grabado de Patentes</h1>
      <p class="text-xl md:text-2xl mb-8">Nueva normativa obligatoria desde el 15 de mayo de 2025</p>
      <p class="max-w-3xl mx-auto text-lg">
        A partir de esta fecha, todos los vehículos motorizados que circulan en Chile deberán tener las patentes grabadas en vidrios y espejos, tal como estipula la Ley 21.601.
      </p>
      <button class="mt-8 bg-white text-blue-800 px-6 py-3 rounded-lg font-medium hover:bg-blue-100 transition">
        Solicitar grabado
      </button>
    </div>
  </section>

  <!-- Generador de Patentes -->
  <section id="generador" class="py-16 bg-blue-50">
    <div class="container mx-auto px-4">
      <h2 class="text-3xl font-bold text-center mb-8 text-gray-800">Generador de Patentes</h2>
      <p class="text-center text-gray-600 mb-8">Ingresa tu patente y genera una vista previa</p>
      
      <div class="max-w-lg mx-auto bg-white p-6 rounded-lg shadow-md">
        <div class="mb-6">
          <label for="patente" class="block text-sm font-medium text-gray-700 mb-2">Patente</label>
          <input 
            type="text" 
            id="patente" 
            v-model="patente" 
            @input="generarImagen" 
            placeholder="Ej: ABCD12" 
            maxlength="6"
            class="w-full px-3 py-2 border border-gray-300 rounded-md focus:outline-none focus:ring-2 focus:ring-blue-500"
          />
          <p class="text-xs text-gray-500 mt-1">Ingresa hasta 6 caracteres (letras y números)</p>
        </div>
        
        <div class="mb-6">
          <p class="block text-sm font-medium text-gray-700 mb-2">Vista previa</p>
          <div class="bg-gray-100 p-4 rounded border border-gray-200">
            <canvas id="patenteCanvas" width="300" height="150" class="mx-auto"></canvas>
            <p v-if="!fontesCargadas" class="text-center text-gray-500 text-sm mt-2">Cargando fuente...</p>
          </div>
        </div>
        
        <button 
          @click="descargarImagen" 
          :disabled="!patente || !fontesCargadas" 
          class="w-full bg-blue-600 text-white py-2 px-4 rounded-md hover:bg-blue-700 focus:outline-none focus:ring-2 focus:ring-blue-500 disabled:bg-gray-400 disabled:cursor-not-allowed"
        >
          Descargar imagen
        </button>
      </div>
    </div>
  </section>

  <!-- Sección Informativa -->
  <section id="normativa" class="py-16 bg-white">
    <div class="container mx-auto px-4">
      <h2 class="text-3xl font-bold text-center mb-12 text-gray-800">Todo sobre la normativa</h2>
      
      <div class="grid md:grid-cols-2 gap-8 mb-12">
        <div class="bg-gray-50 p-6 rounded-lg shadow-md">
          <h3 class="text-xl font-semibold mb-4 text-blue-800">¿Cuántos vidrios se deben grabar?</h3>
          <p class="text-gray-700">
            El grabado debe estar presente en al menos, 6 vidrios (laterales, luneta y parabrisas), además de los 2 espejos laterales. 
            En caso que el vehículo tuviese más, sólo se exigirá el mínimo (ejemplo, furgones o buses). 
            Si se trata de aquellos que cuenten con menos vidrios o espejos (por ejemplo, motos), la patente deberá estar grabada en todos sus espejos.
          </p>
        </div>
        <div class="bg-gray-50 p-6 rounded-lg shadow-md">
          <h3 class="text-xl font-semibold mb-4 text-blue-800">¿Qué características debe tener el grabado?</h3>
          <ul class="text-gray-700 list-disc pl-5 space-y-2">
            <li>Que sea permanente. Vale decir, que haya implicado un desgaste al vidrio y espejo, más allá de la técnica utilizada (arenado, tallado, ácido, etc.)</li>
            <li>Que la letra sea legible, y en formato normal. Es decir, que no use elementos como cursiva o negrita. Y todo, siempre en mayúscula.</li>
            <li>Para las letras y dígitos de los vidrios, entre 7 y 10 milímetros de altura.</li>
            <li>Por su parte, en los espejos laterales, la altura deberá ser entre 5 y 10 milímetros.</li>
          </ul>
        </div>
      </div>
    </div>
  </section>

  <!-- Sección Ubicación del Grabado -->
  <section id="requisitos" class="py-16 bg-gray-100">
    <div class="container mx-auto px-4">
      <h2 class="text-3xl font-bold text-center mb-12 text-gray-800">¿Dónde va el grabado?</h2>
      
      <div class="grid md:grid-cols-2 lg:grid-cols-3 gap-8">
        <div class="bg-white p-6 rounded-lg shadow-md">
          <div class="w-16 h-16 bg-blue-700 rounded-full flex items-center justify-center mb-4 mx-auto">
            <span class="text-white text-2xl font-bold">1</span>
          </div>
          <h3 class="text-xl font-semibold mb-2 text-center">Parabrisas</h3>
          <p class="text-gray-700 text-center">Esquina inferior derecha del parabrisas, visible desde el exterior.</p>
        </div>
        
        <div class="bg-white p-6 rounded-lg shadow-md">
          <div class="w-16 h-16 bg-blue-700 rounded-full flex items-center justify-center mb-4 mx-auto">
            <span class="text-white text-2xl font-bold">2</span>
          </div>
          <h3 class="text-xl font-semibold mb-2 text-center">Vidrios laterales</h3>
          <p class="text-gray-700 text-center">En la esquina inferior trasera de cada vidrio lateral, visible desde el exterior.</p>
        </div>
        
        <div class="bg-white p-6 rounded-lg shadow-md">
          <div class="w-16 h-16 bg-blue-700 rounded-full flex items-center justify-center mb-4 mx-auto">
            <span class="text-white text-2xl font-bold">3</span>
          </div>
          <h3 class="text-xl font-semibold mb-2 text-center">Luneta trasera</h3>
          <p class="text-gray-700 text-center">Esquina inferior derecha de la luneta, visible desde el exterior.</p>
        </div>
        
        <div class="bg-white p-6 rounded-lg shadow-md">
          <div class="w-16 h-16 bg-blue-700 rounded-full flex items-center justify-center mb-4 mx-auto">
            <span class="text-white text-2xl font-bold">4</span>
          </div>
          <h3 class="text-xl font-semibold mb-2 text-center">Espejos laterales</h3>
          <p class="text-gray-700 text-center">En la parte posterior del espejo, visible desde el exterior.</p>
        </div>
        
        <div class="bg-white p-6 rounded-lg shadow-md col-span-1 md:col-span-2 lg:col-span-1">
          <div class="w-16 h-16 bg-blue-700 rounded-full flex items-center justify-center mb-4 mx-auto">
            <span class="text-white text-2xl font-bold">5</span>
          </div>
          <h3 class="text-xl font-semibold mb-2 text-center">Excepciones</h3>
          <p class="text-gray-700 text-center">Vehículos policiales, FF.AA., o extranjeros en tránsito temporal quedan exentos.</p>
          <p class="text-gray-700 text-center mt-2">Taxis o camiones con patente pintada en costados deberán realizar el grabado solo en espejos laterales y parabrisas.</p>
        </div>
      </div>
    </div>
  </section>

  <!-- Sección Vehículos Nuevos y Sanciones -->
  <section class="py-16 bg-white">
    <div class="container mx-auto px-4">
      <div class="grid md:grid-cols-2 gap-8">
        <div class="bg-gray-50 p-6 rounded-lg shadow-md">
          <h3 class="text-xl font-semibold mb-4 text-blue-800">¿Y los vehículos nuevos?</h3>
          <p class="text-gray-700">
            Desde septiembre de 2024, según consigna la norma, las concesionarias deben entregar todos los vehículos nuevos con las patentes debidamente grabadas en vidrios y espejos.
          </p>
        </div>
        
        <div class="bg-gray-50 p-6 rounded-lg shadow-md">
          <h3 class="text-xl font-semibold mb-4 text-blue-800">¿A qué se arriesgan quienes no cumplan?</h3>
          <p class="text-gray-700">
            A partir del 15 de mayo de 2025, tener los vidrios y espejos grabados de forma correcta es necesario para poder sacar la revisión técnica de tu vehículo. Quienes sean controlados por fiscalizadores, inspectores o Carabineros y no cuenten con el grabado, arriesgan una multa que va desde 1 a 1,5 UTM.
          </p>
        </div>
      </div>
    </div>
  </section>

  <!-- Sección Contacto -->
  <section id="contacto" class="py-16 bg-blue-800 text-white">
    <div class="container mx-auto px-4 text-center">
      <h2 class="text-3xl font-bold mb-8">¿Necesitas más información?</h2>
      <p class="text-xl mb-8">Contáctanos para agendar el grabado de patentes para tu vehículo</p>
      <button class="bg-white text-blue-800 px-6 py-3 rounded-lg font-medium hover:bg-blue-100 transition">
        Contactar ahora
      </button>
    </div>
  </section>

  <!-- Footer -->
  <footer class="bg-gray-900 text-white py-8">
    <div class="container mx-auto px-4 text-center">
      <p>&copy; 2024 Grabado de Patentes Chile. Todos los derechos reservados.</p>
      <p class="mt-2 text-gray-400">Información basada en la Ley 21.601</p>
    </div>
  </footer>
</template>

<style>
@import './style.css';

@font-face {
  font-family: 'BusStop';
  src: url('/fonts/bus-stop.ttf') format('truetype');
  font-weight: normal;
  font-style: normal;
}
</style>


