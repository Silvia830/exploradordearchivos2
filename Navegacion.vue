<template>
    <div class="navegacion col-4">
        <div class="navegar">
            <h2 class="textos">Navegación</h2>
            <div 
                v-for="item in navegaciones" 
                :key="item.nombre"
                class="nav-item"
                :class="{ active: item.activo }"
                @click="marcarActivo(item)"
            >
                <font-awesome-icon :icon="item.icono" class="nav-icon" />
                <span>{{ item.nombre }}</span>
            </div>
        </div>
        <div class="carpetas">
            <h2 class="textos">Bibliotecas</h2>

            <svg width="0" height="0" style="position:absolute">
                <linearGradient id="grad-biblioteca" x1="0%" y1="0%" x2="100%" y2="100%">
                    <stop offset="0%" stop-color="#8364ff" />
                    <stop offset="100%" stop-color="#b388ff" />
                </linearGradient>
            </svg>

            <div class="lista-bibliotecas">
                <div
                    v-for="biblioteca in bibliotecas"
                    :key="biblioteca.nombre"
                    class="item-biblioteca"
                    :class="{ activa: carpetaAbiertaNombre === biblioteca.nombre }"
                    @click="abrirBiblioteca(biblioteca)"
                >
                    <svg class="icono-biblioteca" viewBox="0 0 24 24" fill="none">
                        <path
                            :d="biblioteca.icono"
                            stroke="currentColor"
                            stroke-width="1.5"
                        />
                    </svg>
                    <span class="nombre-biblioteca">{{ biblioteca.nombre }}</span>
                </div>
            </div>
        </div>
        <div class="dispositivos">
            <h2 class="textos">Dispositivos</h2>
        </div>
    </div>    
</template>

<script setup>
import { ref } from "vue"
import { rutaActual, carpetaAbiertaNombre } from '../store/ruta.js'

const navegaciones = ref([
    { nombre: "Inicio", icono: "home", ruta: "/home/usuario", activo: true },
    { nombre: "Recientes", icono: "clock", ruta: "/home/usuario/Recientes", activo: false },
    { nombre: "Favoritos", icono: "star", ruta: "/home/usuario/Favoritos", activo: false },
    { nombre: "Papelera", icono: "trash", ruta: "/home/usuario/Papelera", activo: false }
])

const marcarActivo = (item) => {
    navegaciones.value.forEach(nav => nav.activo = false)
    item.activo = true
    rutaActual.value = item.ruta
    carpetaAbiertaNombre.value = null // Inicio/Recientes/etc no abren carpeta en Vista Previa
}

const bibliotecas = ref([
    { nombre: 'Documentos', icono: 'M3 6a2 2 0 0 1 2-2h4l2 2h8a2 2 0 0 1 2 2v9a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2V6z' },
    { nombre: 'Imagenes', icono: 'M4 5h16a1 1 0 0 1 1 1v12a1 1 0 0 1-1 1H4a1 1 0 0 1-1-1V6a1 1 0 0 1 1-1zM3 16l5-5 4 4 5-6 4 5' },
    { nombre: 'Musica', icono: 'M9 18V5l12-2v13M9 18a3 3 0 1 1-6 0 3 3 0 0 1 6 0zM21 16a3 3 0 1 1-6 0 3 3 0 0 1 6 0z' },
    { nombre: 'Videos', icono: 'M4 5h12a1 1 0 0 1 1 1v3l4-2v10l-4-2v3a1 1 0 0 1-1 1H4a1 1 0 0 1-1-1V6a1 1 0 0 1 1-1z' },
])

const abrirBiblioteca = (biblioteca) => {
    navegaciones.value.forEach(nav => nav.activo = false) // se desmarca Inicio/Recientes/etc
    rutaActual.value = '/home/usuario/' + biblioteca.nombre
    carpetaAbiertaNombre.value = biblioteca.nombre // abre el contenido en Vista Previa
}
</script>

<style scoped>
* {
    color: white;
    font-family: 'Courier New', monospace;
}

.navegacion {
    height: 100vh;
    background: linear-gradient(180deg, #0a0015 0%, #1a0030 50%, #0a0015 100%);
    border-right: 2px solid rgba(131, 100, 255, 0.3);
    box-shadow: 4px 0 30px rgba(131, 100, 255, 0.1);
}

.navegar {
    width: 100%;
    height: 30%;
}

.carpetas {
    width: 100%;
    height: 40%;
    overflow-y: auto;
    
}

.dispositivos {
    width: 100%;
    height: 30%;
}

.textos {
    font-size: 0.8rem;
    padding-left: 20px;
    padding-top: 20px;
    padding-bottom: 10px;

    color: #8a6eff;
    text-transform: uppercase;
    letter-spacing: 3px;
    text-shadow: 0 0 10px rgba(131, 100, 255, 0.3);
}

.nav-item {
    display: flex;
    align-items: center;
    gap: 12px;
    padding: 10px 20px;
    cursor: pointer;
    transition: all 0.3s ease;
    margin: 4px 10px;
    border-radius: 8px;
    position: relative;
    border: 1px solid transparent;
}

.nav-item:hover {
    transform: translateX(6px);
    background: rgba(131, 100, 255, 0.08);

    box-shadow: 0 0 20px rgba(131, 100, 255, 0.05);
}

.nav-item:hover .nav-icon {
    color: #a88bff;
    text-shadow: 0 0 20px rgba(131, 100, 255, 0.5);
}

.nav-item.active {
    background: rgba(131, 100, 255, 0.15);
    border: 1px solid rgba(131, 100, 255, 0.4);
    box-shadow: 
        0 0 20px rgba(131, 100, 255, 0.2),
        inset 0 0 30px rgba(131, 100, 255, 0.05);
    transform: translateX(6px);
}

.nav-item.active::after {
    content: '';
    position: absolute;
    left: 0;
    top: 20%;
    height: 60%;
    width: 3px;
    background: linear-gradient(180deg, 
        transparent,
        #8364ff,
        #b388ff,
        #8364ff,
        transparent
    );
    border-radius: 2px;
    box-shadow: 
        0 0 15px #8364ff,
        0 0 30px rgba(131, 100, 255, 0.3);
    animation: pulseNeon 2s ease-in-out infinite;
}

.nav-item.active .nav-icon {
    color: #b388ff;
    text-shadow: 
        0 0 20px rgba(131, 100, 255, 0.6),
        0 0 40px rgba(131, 100, 255, 0.3),
        0 0 60px rgba(131, 100, 255, 0.1);
}

.nav-item.active span {
    color: #d4c4ff;
    text-shadow: 0 0 20px rgba(131, 100, 255, 0.3);
}

.nav-icon {
    width: 18px;
    color: #6a4e9a;
    font-size: 16px;
    transition: all 0.3s ease;
}

.nav-item span {
    font-size: 0.9rem;
    color: #8a7aaa;
    transition: all 0.3s ease;
    letter-spacing: 0.5px;
}

.nav-item:not(.active):hover .nav-icon {
    color: #a88bff;
    text-shadow: 0 0 15px rgba(131, 100, 255, 0.3);
}

.nav-item:not(.active):hover span {
    color: #b8a8e0;
}

@keyframes pulseNeon {
    0%, 100% {
        opacity: 1;
        box-shadow: 0 0 15px #8364ff, 0 0 30px rgba(131, 100, 255, 0.3);
    }
    50% {
        opacity: 0.7;
        box-shadow: 0 0 25px #8364ff, 0 0 50px rgba(131, 100, 255, 0.5);
    }
}

.navegacion::-webkit-scrollbar {
    width: 4px;
}

.navegacion::-webkit-scrollbar-track {
    background: rgba(131, 100, 255, 0.05);
}

.navegacion::-webkit-scrollbar-thumb {
    background: linear-gradient(180deg, #8364ff, #b388ff);
    border-radius: 2px;
    box-shadow: 0 0 15px rgba(131, 100, 255, 0.3);
}

.lista-bibliotecas {
    display: flex;
    flex-direction: column;
    gap: 4px;
    padding: 12px;
}

.item-biblioteca {
    --glow: rgba(131, 100, 255, 0.7);
    display: flex;
    align-items: center;
    gap: 12px;
    padding: 10px 20px;
    margin: 4px 10px;
    border-radius: 8px;
    border: 1px solid transparent;
    cursor: pointer;
    transition: all 0.3s ease;
}

.item-biblioteca:hover {
    transform: translateX(6px);
    background: rgba(131, 100, 255, 0.08);
    box-shadow: 0 0 20px rgba(131, 100, 255, 0.05);
}

.item-biblioteca.activa {
    background: rgba(131, 100, 255, 0.15);
    border: 1px solid rgba(131, 100, 255, 0.4);
    box-shadow: 0 0 20px rgba(131, 100, 255, 0.2), inset 0 0 30px rgba(131, 100, 255, 0.05);
}

.item-biblioteca.activa .icono-biblioteca { color: #b388ff; }
.item-biblioteca.activa .nombre-biblioteca { color: #d4c4ff; }

.icono-biblioteca {
    width: 18px;
    height: 18px;
    flex-shrink: 0;
    color: #6a4e9a;
    filter: drop-shadow(0 0 4px var(--glow));
    animation: pulso-biblioteca 3s ease-in-out infinite;
    transition: color 0.3s ease, filter 0.3s ease, transform 0.3s ease;
}

.item-biblioteca:hover .icono-biblioteca {
    color: #a88bff;
    --glow: rgba(179, 136, 255, 0.9);
    transform: scale(1.1);
    animation-play-state: paused;
}

@keyframes pulso-biblioteca {
    50% { filter: drop-shadow(0 0 8px var(--glow)); }
}

.nombre-biblioteca {
    font-size: 12.5px;
    letter-spacing: 0.6px;
    color: #e8e8ff;
}
</style>