<script setup>
import { RouterLink } from 'vue-router'
</script>

<template>
  <main>
    <div class="content">
      <h1>Padrão em Jogo</h1>
      <p>Complete os padrões antes que o tempo acabe!</p>

      <RouterLink class="router-link" to="/padrao-app">Iniciar Jogo</RouterLink>
    </div>
  </main>
</template>

<style scoped>
main {
  padding: 0;
  margin: 0;
  min-height: 100vh;
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  display: flex;
  align-items: center;
  justify-content: center;
  position: relative;
  overflow: hidden;
}

/* Elementos decorativos de fundo */
main::before {
  content: '';
  position: absolute;
  width: 200%;
  height: 200%;
  animation: float 6s ease-in-out infinite;
}

@keyframes float {
  0%, 100% { transform: translateY(0px) rotate(0deg); }
  50% { transform: translateY(-20px) rotate(180deg); }
}

.content {
  text-align: center;
  padding: 40px 30px;
  width: 100%;
  max-width: 450px;
  background: rgba(255, 255, 255, 0.1);
  backdrop-filter: blur(15px);
  border-radius: 24px;
  border: 1px solid rgba(255, 255, 255, 0.2);
  box-shadow:
    0 20px 40px rgba(0, 0, 0, 0.1),
    inset 0 1px 0 rgba(255, 255, 255, 0.2);
  position: relative;
  z-index: 1;
  transform-style: preserve-3d;
  perspective: 1000px;
}

h1 {
  font-family: 'Segoe UI', system-ui, sans-serif;
  color: white;
  font-size: 3rem;
  padding-bottom: 20px;
  font-weight: 800;
  text-shadow:
    0 4px 8px rgba(0, 0, 0, 0.3),
    0 0 20px rgba(255, 255, 255, 0.2);
  margin: 0 0 10px 0;
  background: linear-gradient(135deg, #ffffff 0%, #e0e7ff 100%);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
  letter-spacing: -0.5px;
  transform: translateZ(20px);
}

p {
  margin: 0 auto 40px;
  padding-bottom: 20px;
  color: rgba(255, 255, 255, 0.95);
  font-size: 1.2rem;
  line-height: 1.6;
  font-weight: 400;
  text-shadow: 0 2px 4px rgba(0, 0, 0, 0.2);
  transform: translateZ(15px);
}

.router-link {
  font-size: 1.3rem;
  padding: 18px 35px;
  display: inline-block;
  background: linear-gradient(135deg, #53B175 0%, #429a63 100%);
  color: #fff9ff;
  border-radius: 16px;
  text-decoration: none;
  transition: all 0.4s cubic-bezier(0.25, 0.46, 0.45, 0.94);
  box-shadow:
    0 8px 25px rgba(83, 177, 117, 0.4),
    0 4px 0 rgba(66, 154, 99, 0.8);
  width: 100%;
  max-width: 220px;
  font-weight: 600;
  letter-spacing: 0.5px;
  position: relative;
  overflow: hidden;
  transform: translateZ(25px);
}

.router-link::before {
  content: '';
  position: absolute;
  top: 0;
  left: -100%;
  width: 100%;
  height: 100%;
  background: linear-gradient(90deg, transparent, rgba(255, 255, 255, 0.2), transparent);
  transition: left 0.6s ease;
}

.router-link:hover {
  background: linear-gradient(135deg, #5bc183 0%, #4aaf72 100%);
  transform: translateY(-4px) translateZ(30px);
  box-shadow:
    0 12px 35px rgba(83, 177, 117, 0.6),
    0 6px 0 rgba(66, 154, 99, 0.9);
}

.router-link:hover::before {
  left: 100%;
}

.router-link:active {
  transform: translateY(-1px) translateZ(25px);
  box-shadow:
    0 6px 20px rgba(83, 177, 117, 0.4),
    0 2px 0 rgba(66, 154, 99, 0.8);
}

/* Efeito de partículas flutuantes */
.content::after {
  content: '';
  position: absolute;
  top: -50px;
  left: -50px;
  right: -50px;
  bottom: -50px;
  background:
    radial-gradient(2px 2px at 20% 30%, rgba(255,255,255,0.3) 50%, transparent 100%),
    radial-gradient(2px 2px at 80% 70%, rgba(255,255,255,0.3) 50%, transparent 100%),
    radial-gradient(2px 2px at 40% 90%, rgba(255,255,255,0.3) 50%, transparent 100%),
    radial-gradient(2px 2px at 60% 10%, rgba(255,255,255,0.3) 50%, transparent 100%);
  z-index: -1;
  animation: particles 8s linear infinite;
}

@keyframes particles {
  0% { transform: translateY(0px) rotate(0deg); }
  100% { transform: translateY(-100px) rotate(360deg); }
}
</style>
