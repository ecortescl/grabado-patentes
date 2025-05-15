<script setup>
// Importamos ref para manejar el estado reactivo
import { ref, onMounted } from 'vue';

// Variables reactivas
const patente = ref('');
const fontesCargadas = ref(false);
const tamanioAltura = ref(8); // Valor predeterminado en mm (entre 7 y 10)
const menuAbierto = ref(false); // Estado del menú móvil

// Meta SEO datos
const seoData = {
  title: 'Grabado de Patentes Chile - Normativa, Requisitos y Generador',
  description: 'Todo sobre el grabado de patentes en Chile: nueva Ley 21.601, requisitos, dónde realizar el grabado y generador de patentes para impresión. Obligatorio desde mayo 2025.',
  keywords: 'grabado de patentes, ley 21601, patentes en vidrios, patentes Chile, grabado obligatorio, normativa patentes, generador patentes',
  siteName: 'Grabado de Patentes Chile',
  url: 'https://grabadopatentes.resolutionsweb.cl/',
  imageUrl: '/public/social.png'
};

// Método para alternar el menú móvil
const toggleMenu = () => {
  menuAbierto.value = !menuAbierto.value;
};

// Método para cerrar el menú al hacer clic en un enlace
const cerrarMenu = () => {
  menuAbierto.value = false;
};

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
  // La altura del texto está determinada por el tamaño de fuente
  // 1mm = ~3.78px (aproximadamente)
  const tamanioPx = tamanioAltura.value * 3.78;
  ctx.fillStyle = '#000000';
  ctx.font = `${tamanioPx}px "BusStop"`;
  ctx.textAlign = 'center';
  ctx.textBaseline = 'middle';
  
  // Dibujar patente
  ctx.fillText(patente.value.toUpperCase(), canvas.width / 2, canvas.height / 2);
  
  // Dibujar regla de referencia para tamaño (10mm)
  ctx.fillRect(10, canvas.height - 20, 37.8, 2); // 10mm = 37.8px
  ctx.font = '10px Arial';
  ctx.fillText('10mm', 30, canvas.height - 25);
};

// Método para descargar la imagen
const descargarImagen = () => {
  const canvas = document.getElementById('patenteCanvas');
  const enlace = document.createElement('a');
  enlace.download = 'patente-' + patente.value + '.png';
  enlace.href = canvas.toDataURL('image/png');
  enlace.click();
};

// Controlar el scroll para efectos visuales del navbar
const scrolled = ref(false);

const handleScroll = () => {
  scrolled.value = window.scrollY > 50;
};

// Cargar fuente personalizada y configurar eventos
onMounted(() => {
  const fontFace = new FontFace('BusStop', 'url(/fonts/bus-stop.ttf)');
  
  fontFace.load().then(function(loadedFace) {
    document.fonts.add(loadedFace);
    fontesCargadas.value = true;
    // Si hay una patente predefinida, generar imagen
    if (patente.value) {
      generarImagen();
    }

    // Configurar título de la página para SEO
    document.title = seoData.title;
  }).catch(function(error) {
    console.error('Error al cargar la fuente:', error);
  });
  
  // Añadir evento de scroll
  window.addEventListener('scroll', handleScroll);
  // Verificar scroll inicial
  handleScroll();
});
</script>

<template>
  <!-- Navbar -->
  <nav 
    class="fixed top-0 left-0 right-0 z-50 transition-all duration-300" 
    :class="{'bg-blue-800 shadow-lg': scrolled, 'bg-blue-800/90 backdrop-blur-md': !scrolled}"
  >
    <div class="container mx-auto px-4">
      <div class="flex justify-between items-center py-4">
        <div class="flex items-center">
          <div class="text-white text-xl font-bold flex items-center">
            <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor" class="w-8 h-8 mr-2" aria-hidden="true">
              <path stroke-linecap="round" stroke-linejoin="round" d="M8.25 18.75a1.5 1.5 0 0 1-3 0m3 0a1.5 1.5 0 0 0-3 0m3 0h6m-9 0H3.375a1.125 1.125 0 0 1-1.125-1.125V14.25m17.25 4.5a1.5 1.5 0 0 1-3 0m3 0a1.5 1.5 0 0 0-3 0m3 0h1.125c.621 0 1.129-.504 1.09-1.124a17.902 17.902 0 0 0-3.213-9.193 2.056 2.056 0 0 0-1.58-.86H14.25M16.5 18.75h-2.25m0-11.177v-.958c0-.568-.422-1.048-.987-1.106a48.554 48.554 0 0 0-10.026 0 1.106 1.106 0 0 0-.987 1.106v7.635m12-6.677v6.677m0 4.5v-4.5m0 0h-12" />
            </svg>
            <span>Grabado de Patentes</span>
          </div>
        </div>
        
        <!-- Menú de escritorio -->
        <div class="hidden md:flex space-x-6 items-center">
          <a href="#inicio" @click="cerrarMenu" class="text-white hover:text-blue-200 flex items-center">
            <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor" class="w-5 h-5 mr-1" aria-hidden="true">
              <path stroke-linecap="round" stroke-linejoin="round" d="m2.25 12 8.954-8.955c.44-.439 1.152-.439 1.591 0L21.75 12M4.5 9.75v10.125c0 .621.504 1.125 1.125 1.125H9.75v-4.875c0-.621.504-1.125 1.125-1.125h2.25c.621 0 1.125.504 1.125 1.125V21h4.125c.621 0 1.125-.504 1.125-1.125V9.75M8.25 21h8.25" />
            </svg>
            Inicio
          </a>
          <a href="#normativa" @click="cerrarMenu" class="text-white hover:text-blue-200 flex items-center">
            <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor" class="w-5 h-5 mr-1" aria-hidden="true">
              <path stroke-linecap="round" stroke-linejoin="round" d="M12 6.042A8.967 8.967 0 0 0 6 3.75c-1.052 0-2.062.18-3 .512v14.25A8.987 8.987 0 0 1 6 18c2.305 0 4.408.867 6 2.292m0-14.25a8.966 8.966 0 0 1 6-2.292c1.052 0 2.062.18 3 .512v14.25A8.987 8.987 0 0 0 18 18a8.967 8.967 0 0 0-6 2.292m0-14.25v14.25" />
            </svg>
            Normativa
          </a>
          <a href="#requisitos" @click="cerrarMenu" class="text-white hover:text-blue-200 flex items-center">
            <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor" class="w-5 h-5 mr-1" aria-hidden="true">
              <path stroke-linecap="round" stroke-linejoin="round" d="M11.35 3.836c-.065.21-.1.433-.1.664 0 .414.336.75.75.75h4.5a.75.75 0 0 0 .75-.75 2.25 2.25 0 0 0-.1-.664m-5.8 0A2.251 2.251 0 0 1 13.5 2.25H15c1.012 0 1.867.668 2.15 1.586m-5.8 0c-.376.023-.75.05-1.124.08C9.095 4.01 8.25 4.973 8.25 6.108V8.25m8.9-4.414c.376.023.75.05 1.124.08 1.131.094 1.976 1.057 1.976 2.192V16.5A2.25 2.25 0 0 1 18 18.75h-2.25m-7.5-10.5H4.875c-.621 0-1.125.504-1.125 1.125v11.25c0 .621.504 1.125 1.125 1.125h9.75c.621 0 1.125-.504 1.125-1.125V18.75m-7.5-10.5h6.375c.621 0 1.125.504 1.125 1.125v9.375m-8.25-3 1.5 1.5 3-3.75" />
            </svg>
            Requisitos
          </a>
          <a href="#generador" @click="cerrarMenu" class="text-white hover:text-blue-200 flex items-center">
            <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor" class="w-5 h-5 mr-1" aria-hidden="true">
              <path stroke-linecap="round" stroke-linejoin="round" d="M9.53 16.122a3 3 0 0 0-5.78 1.128 2.25 2.25 0 0 1-2.4 2.245 4.5 4.5 0 0 0 8.4-2.245c0-.399-.078-.78-.22-1.128Zm0 0a15.998 15.998 0 0 0 3.388-1.62m-5.043-.025a15.994 15.994 0 0 1 1.622-3.395m3.42 3.42a15.995 15.995 0 0 0 4.764-4.648l3.876-5.814a1.151 1.151 0 0 0-1.597-1.597L14.146 6.32a15.996 15.996 0 0 0-4.649 4.763m3.42 3.42a6.776 6.776 0 0 0-3.42-3.42" />
            </svg>
            Generador
          </a>
        </div>
        
        <!-- Botón de menú móvil -->
        <div class="md:hidden">
          <button @click="toggleMenu" class="focus:outline-none text-white">
            <svg v-if="!menuAbierto" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor" class="w-6 h-6">
              <path stroke-linecap="round" stroke-linejoin="round" d="M3.75 6.75h16.5M3.75 12h16.5m-16.5 5.25h16.5" />
            </svg>
            <svg v-else xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor" class="w-6 h-6">
              <path stroke-linecap="round" stroke-linejoin="round" d="M6 18L18 6M6 6l12 12" />
            </svg>
          </button>
        </div>
      </div>
      
      <!-- Menú móvil desplegable -->
      <div 
        v-show="menuAbierto"
        class="md:hidden py-4 transition-all duration-300"
      >
        <div class="flex flex-col space-y-4">
          <a href="#inicio" @click="cerrarMenu" class="text-white hover:text-blue-200 flex items-center">
            <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor" class="w-5 h-5 mr-2">
              <path stroke-linecap="round" stroke-linejoin="round" d="m2.25 12 8.954-8.955c.44-.439 1.152-.439 1.591 0L21.75 12M4.5 9.75v10.125c0 .621.504 1.125 1.125 1.125H9.75v-4.875c0-.621.504-1.125 1.125-1.125h2.25c.621 0 1.125.504 1.125 1.125V21h4.125c.621 0 1.125-.504 1.125-1.125V9.75M8.25 21h8.25" />
            </svg>
            Inicio
          </a>
          <a href="#normativa" @click="cerrarMenu" class="text-white hover:text-blue-200 flex items-center">
            <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor" class="w-5 h-5 mr-2">
              <path stroke-linecap="round" stroke-linejoin="round" d="M12 6.042A8.967 8.967 0 0 0 6 3.75c-1.052 0-2.062.18-3 .512v14.25A8.987 8.987 0 0 1 6 18c2.305 0 4.408.867 6 2.292m0-14.25a8.966 8.966 0 0 1 6-2.292c1.052 0 2.062.18 3 .512v14.25A8.987 8.987 0 0 0 18 18a8.967 8.967 0 0 0-6 2.292m0-14.25v14.25" />
            </svg>
            Normativa
          </a>
          <a href="#requisitos" @click="cerrarMenu" class="text-white hover:text-blue-200 flex items-center">
            <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor" class="w-5 h-5 mr-2">
              <path stroke-linecap="round" stroke-linejoin="round" d="M11.35 3.836c-.065.21-.1.433-.1.664 0 .414.336.75.75.75h4.5a.75.75 0 0 0 .75-.75 2.25 2.25 0 0 0-.1-.664m-5.8 0A2.251 2.251 0 0 1 13.5 2.25H15c1.012 0 1.867.668 2.15 1.586m-5.8 0c-.376.023-.75.05-1.124.08C9.095 4.01 8.25 4.973 8.25 6.108V8.25m8.9-4.414c.376.023.75.05 1.124.08 1.131.094 1.976 1.057 1.976 2.192V16.5A2.25 2.25 0 0 1 18 18.75h-2.25m-7.5-10.5H4.875c-.621 0-1.125.504-1.125 1.125v11.25c0 .621.504 1.125 1.125 1.125h9.75c.621 0 1.125-.504 1.125-1.125V18.75m-7.5-10.5h6.375c.621 0 1.125.504 1.125 1.125v9.375m-8.25-3 1.5 1.5 3-3.75" />
            </svg>
            Requisitos
          </a>
          <a href="#generador" @click="cerrarMenu" class="text-white hover:text-blue-200 flex items-center">
            <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor" class="w-5 h-5 mr-2">
              <path stroke-linecap="round" stroke-linejoin="round" d="M9.53 16.122a3 3 0 0 0-5.78 1.128 2.25 2.25 0 0 1-2.4 2.245 4.5 4.5 0 0 0 8.4-2.245c0-.399-.078-.78-.22-1.128Zm0 0a15.998 15.998 0 0 0 3.388-1.62m-5.043-.025a15.994 15.994 0 0 1 1.622-3.395m3.42 3.42a15.995 15.995 0 0 0 4.764-4.648l3.876-5.814a1.151 1.151 0 0 0-1.597-1.597L14.146 6.32a15.996 15.996 0 0 0-4.649 4.763m3.42 3.42a6.776 6.776 0 0 0-3.42-3.42" />
            </svg>
            Generador
          </a>
        </div>
      </div>
    </div>
  </nav>

  <!-- Espaciador para compensar el navbar fijo -->
  <div class="h-16"></div>



  <!-- Hero Section -->
  <section id="inicio" class="bg-gradient-to-r from-blue-700 to-blue-900 text-white py-20">
    <div class="container mx-auto px-4 text-center">
      <h1 class="text-4xl md:text-5xl font-bold mb-4">Grabado de Patentes en Chile</h1>
      <p class="text-xl md:text-2xl mb-8">Nueva normativa obligatoria desde el 15 de mayo de 2025</p>
      <p class="max-w-3xl mx-auto text-lg">
        A partir de esta fecha, todos los vehículos motorizados que circulan en Chile deberán tener las patentes grabadas en vidrios y espejos, tal como estipula la Ley 21.601.
      </p>
      <a href="#normativa" class="mt-8 bg-white text-blue-800 px-6 py-3 rounded-lg font-medium hover:bg-blue-100 transition inline-block">
        <div class="flex items-center">
          <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor" class="w-5 h-5 mr-2">
            <path stroke-linecap="round" stroke-linejoin="round" d="m11.25 11.25.041-.02a.75.75 0 0 1 1.063.852l-.708 2.836a.75.75 0 0 0 1.063.853l.041-.021M21 12a9 9 0 1 1-18 0 9 9 0 0 1 18 0Zm-9-3.75h.008v.008H12V8.25Z" />
          </svg>
          Conocer más
        </div>
      </a>
    </div>
  </section>

  <!-- Generador de Patentes -->
  <section id="generador" class="py-16 bg-blue-50">
    <div class="container mx-auto px-4">
      <h2 class="text-3xl font-bold text-center mb-8 text-gray-800">Generador de Grabado de Patentes</h2>
      <p class="text-center text-gray-600 mb-8">Ingresa tu patente y genera una vista previa para impresión según la normativa del grabado obligatorio</p>
      
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
          <label for="tamanioAltura" class="block text-sm font-medium text-gray-700 mb-2">
            Altura del texto (mm)
          </label>
          <div class="flex items-center">
            <input 
              type="range" 
              id="tamanioAltura" 
              v-model="tamanioAltura" 
              min="7" 
              max="10" 
              step="0.5"
              @input="generarImagen"
              class="w-full h-2 bg-gray-200 rounded-lg appearance-none cursor-pointer"
            />
            <span class="ml-2 text-sm font-medium text-gray-700">{{ tamanioAltura }}mm</span>
          </div>
          <p class="text-xs text-gray-500 mt-1">La normativa exige una altura entre 7 y 10 mm</p>
        </div>
        
        <div class="mb-6">
          <p class="block text-sm font-medium text-gray-700 mb-2">Vista previa</p>
          <div class="bg-gray-100 p-4 rounded border border-gray-200">
            <canvas id="patenteCanvas" width="300" height="150" class="mx-auto"></canvas>
            <p v-if="!fontesCargadas" class="text-center text-gray-500 text-sm mt-2">Cargando fuente...</p>
            <p class="text-center text-xs text-gray-500 mt-2">
              La imagen incluye una línea de referencia de 10mm para verificar el tamaño al imprimir
            </p>
          </div>
        </div>

        <div class="mb-6 p-3 bg-blue-50 rounded-md">
          <h4 class="text-sm font-medium text-blue-800 mb-2">Instrucciones para impresión correcta:</h4>
          <ol class="text-xs text-gray-700 list-decimal pl-4 space-y-1">
            <li>Descarga la imagen generada</li>
            <li>Al imprimir, asegúrate de desactivar la opción "Ajustar a página"</li>
            <li>Verifica que la línea de referencia mida exactamente 10mm con una regla</li>
            <li>El texto debe tener una altura entre 7mm y 10mm para cumplir la normativa</li>
            <li>Recorta la imagen siguiendo el borde negro</li>
          </ol>
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
      <h2 class="text-3xl font-bold text-center mb-12 text-gray-800">Todo sobre la normativa del grabado de patentes</h2>
      
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
      
      <div class="grid md:grid-cols-2 gap-8">
        <div class="bg-white p-6 rounded-lg shadow-md">
          <h3 class="text-xl font-semibold mb-4 text-center text-blue-800">Parabrisas</h3>
          <img 
            src="https://mtt.gob.cl/wp-content/uploads/2024/05/Grabado_2_pages-to-jpg-0003-900x506.jpg" 
            alt="Ubicación del grabado de patente en el parabrisas según normativa chilena" 
            class="w-full h-auto rounded-lg mb-4"
            loading="lazy"
            width="900"
            height="506"
          />
          <p class="text-gray-700 text-center">Esquina inferior derecha del parabrisas, visible desde el exterior.</p>
        </div>
        
        <div class="bg-white p-6 rounded-lg shadow-md">
          <h3 class="text-xl font-semibold mb-4 text-center text-blue-800">Vidrios laterales</h3>
          <img 
            src="https://mtt.gob.cl/wp-content/uploads/2024/05/Grabado_2_pages-to-jpg-0002-900x506.jpg" 
            alt="Ubicación del grabado de patentes en los vidrios laterales del vehículo según ley 21.601" 
            class="w-full h-auto rounded-lg mb-4"
            loading="lazy"
            width="900"
            height="506"
          />
          <p class="text-gray-700 text-center">En la esquina inferior trasera de cada vidrio lateral, visible desde el exterior.</p>
        </div>
        
        <div class="bg-white p-6 rounded-lg shadow-md">
          <h3 class="text-xl font-semibold mb-4 text-center text-blue-800">Luneta trasera</h3>
          <img 
            src="https://mtt.gob.cl/wp-content/uploads/2024/05/Grabado_2_pages-to-jpg-0004-900x506.jpg" 
            alt="Posición correcta del grabado de patentes en la luneta trasera según normativa obligatoria" 
            class="w-full h-auto rounded-lg mb-4"
            loading="lazy"
            width="900"
            height="506"
          />
          <p class="text-gray-700 text-center">Esquina inferior derecha de la luneta, visible desde el exterior.</p>
        </div>
        
        <div class="bg-white p-6 rounded-lg shadow-md">
          <h3 class="text-xl font-semibold mb-4 text-center text-blue-800">Espejos laterales</h3>
          <img 
            src="https://mtt.gob.cl/wp-content/uploads/2024/05/Grabado_2_pages-to-jpg-0005-900x506.jpg" 
            alt="Grabado de patentes en espejos laterales con las medidas correctas según la ley" 
            class="w-full h-auto rounded-lg mb-4"
            loading="lazy"
            width="900"
            height="506"
          />
          <p class="text-gray-700 text-center">En la parte posterior del espejo, visible desde el exterior.</p>
        </div>
        
        <div class="bg-white p-6 rounded-lg shadow-md col-span-1 md:col-span-2">
          <h3 class="text-xl font-semibold mb-4 text-center text-blue-800">Excepciones</h3>
          <div class="max-w-2xl mx-auto">
            <p class="text-gray-700 text-center mb-2">Vehículos policiales, FF.AA., o extranjeros en tránsito temporal quedan exentos.</p>
            <p class="text-gray-700 text-center">Taxis o camiones con patente pintada en costados deberán realizar el grabado solo en espejos laterales y parabrisas.</p>
          </div>
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
      <h2 class="text-3xl font-bold mb-8">Recursos adicionales</h2>
      <p class="text-xl mb-8">Esta página es informativa para ayudarte a entender la nueva normativa</p>
      <div class="flex flex-col md:flex-row justify-center gap-4">
        <a 
          href="https://www.bcn.cl/leychile/navegar?idNorma=1193830" 
          target="_blank"
          class="bg-white text-blue-800 px-6 py-3 rounded-lg font-medium hover:bg-blue-100 transition"
        >
          Leer la Ley 21.601
        </a>
        <a 
          href="https://mtt.gob.cl/grabado-de-patentes"
          target="_blank" 
          class="bg-transparent border-2 border-white text-white px-6 py-3 rounded-lg font-medium hover:bg-white hover:text-blue-800 transition"
        >
          Sitio oficial MTT
        </a>
      </div>
    </div>
  </section>

  <!-- Footer -->
  <footer class="bg-gray-900 text-white py-8">
    <div class="container mx-auto px-4 text-center">
      <p>&copy; 2024 Grabado de Patentes Chile. Todos los derechos reservados.</p>
      <p class="mt-2 text-gray-400">Información basada en la Ley 21.601</p>
      <p class="mt-2 text-gray-400">
        Fuente oficial: <a href="https://mtt.gob.cl/grabado-de-patentes" target="_blank" class="text-blue-400 hover:text-blue-300 underline">Ministerio de Transportes y Telecomunicaciones</a>
      </p>
      
      <!-- Enlaces internos para SEO -->
      <div class="mt-4 grid grid-cols-1 md:grid-cols-3 gap-4 text-sm max-w-4xl mx-auto">
        <div>
          <h3 class="text-white font-semibold mb-2">Información de grabado</h3>
          <ul class="space-y-1">
            <li><a href="#normativa" class="text-gray-400 hover:text-white">Normativa del grabado de patentes</a></li>
            <li><a href="#requisitos" class="text-gray-400 hover:text-white">Posición del grabado en vidrios</a></li>
            <li><a href="#generador" class="text-gray-400 hover:text-white">Generador de patentes para grabado</a></li>
          </ul>
        </div>
        <div>
          <h3 class="text-white font-semibold mb-2">Recursos útiles</h3>
          <ul class="space-y-1">
            <li><a href="https://www.bcn.cl/leychile/navegar?idNorma=1193830" target="_blank" class="text-gray-400 hover:text-white">Ley 21.601 oficial</a></li>
            <li><a href="https://mtt.gob.cl/grabado-de-patentes" target="_blank" class="text-gray-400 hover:text-white">MTT Grabado de Patentes</a></li>
          </ul>
        </div>
        <div>
          <h3 class="text-white font-semibold mb-2">Ciudades con servicio</h3>
          <p class="text-gray-400 text-xs">
            Santiago, Concepción, Valparaíso, Viña del Mar, Rancagua, Temuco, Antofagasta, La Serena, Puerto Montt
          </p>
        </div>
      </div>
      
      <!-- Texto SEO para palabras clave al final de página -->
      <div class="mt-6 text-xs text-gray-600 max-w-2xl mx-auto">
        <p>El grabado de patentes en Chile es obligatorio según la Ley 21.601. Todos los vehículos motorizados deben realizar el grabado de su patente en vidrios y espejos antes del 15 de mayo de 2025. Esta medida busca reducir el robo de vehículos y mejorar la seguridad vial en todo el país.</p>
      </div>
      
      <div class="mt-8 pt-6 border-t border-gray-800">
        <div class="flex flex-col items-center justify-center">
          <a href="https://resolutionsweb.cl" target="_blank" class="mb-2">
            <img 
              src="https://resolutionsweb.cl/wp-content/uploads/2024/05/Sin-titulo-3.png" 
              alt="Logo de Resolutions Web, desarrolladores del sitio Grabado de Patentes Chile" 
              class="h-10 w-auto"
              loading="lazy"
              width="200"
              height="50"
            />
          </a>
          <p class="text-sm text-gray-500">Desarrollado por <a href="https://resolutionsweb.cl" target="_blank" class="text-gray-400 hover:text-white transition">www.resolutionsweb.cl</a></p>
        </div>
      </div>
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

html {
  scroll-behavior: smooth;
  scroll-padding-top: 70px; /* Compensar el navbar fijo */
}

body {
  overflow-x: hidden;
}

.container {
  max-width: 1400px;
  margin: 0 auto;
}

@media (min-width: 1600px) {
  .container {
    max-width: 1600px;
  }
  
  section {
    padding-left: 2rem;
    padding-right: 2rem;
  }
}

/* Transición suave para elementos interactivos */
a, button {
  transition: all 0.3s ease;
}

/* Efecto hover para las tarjetas */
.bg-white.p-6.rounded-lg.shadow-md {
  transition: transform 0.3s ease, box-shadow 0.3s ease;
}

.bg-white.p-6.rounded-lg.shadow-md:hover {
  transform: translateY(-5px);
  box-shadow: 0 10px 25px -5px rgba(0, 0, 0, 0.1);
}

/* Animación para la carga de la página */
@keyframes fadeIn {
  from { opacity: 0; transform: translateY(20px); }
  to { opacity: 1; transform: translateY(0); }
}

section {
  animation: fadeIn 0.8s ease-out;
}

section:nth-child(1) { animation-delay: 0s; }
section:nth-child(2) { animation-delay: 0.1s; }
section:nth-child(3) { animation-delay: 0.2s; }
section:nth-child(4) { animation-delay: 0.3s; }
section:nth-child(5) { animation-delay: 0.4s; }
section:nth-child(6) { animation-delay: 0.5s; }
</style>


