<script setup>
import { useAppStore } from '@/stores/app';
import { useRouter } from 'vue-router';

const appStore = useAppStore();
const router = useRouter();

function reiniciarJogo() {
  appStore.resetarPontos();
  router.push('/padrao-app');
}

function voltarInicio() {
  router.push('/');
}
</script>

<template>
  <main>
    <div class="end-container">
      <h1>Tempo Esgotado!</h1>

      <div class="resultado">
  <div class="pontuacao-final">
    <h2>Sua Pontuação</h2>
    <div class="pontos">{{ appStore.pontos }}</div>
    <p>padrões completados</p>
  </div>

  <div class="estatisticas-finais">
    <div class="estatistica">
      <span class="valor">{{ appStore.totalRespondido }}</span>
      <span class="label">Total respondido</span>
    </div>
    <div class="estatistica">
      <span class="valor acertos">{{ appStore.pontos }}</span>
      <span class="label">Acertos</span>
    </div>
    <div class="estatistica">
      <span class="valor erros">{{ appStore.erros }}</span>
      <span class="label">Erros</span>
    </div>
    <div class="estatistica">
      <span class="valor desempenho">{{ appStore.calcularDesempenho() }}%</span>
      <span class="label">Desempenho</span>
    </div>
  </div>

  <div class="botoes">
    <button @click="reiniciarJogo" class="btn-reiniciar">Jogar Novamente</button>
    <button @click="voltarInicio" class="btn-inicio">Voltar ao Início</button>
  </div>
</div>
    </div>
  </main>
</template>

<style scoped>
main {
  padding: 0;
  margin: 0;
  min-height: 100vh;
  text-align: center;
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
  background:
    radial-gradient(circle at 20% 80%, rgba(120, 119, 198, 0.3) 0%, transparent 50%),
    radial-gradient(circle at 80% 20%, rgba(255, 255, 255, 0.2) 0%, transparent 50%),
    radial-gradient(circle at 40% 40%, rgba(118, 75, 162, 0.4) 0%, transparent 50%);
  animation: float 6s ease-in-out infinite;
}

@keyframes float {
  0%, 100% { transform: translateY(0px) rotate(0deg); }
  50% { transform: translateY(-20px) rotate(180deg); }
}

.end-container {
  background: rgba(255, 255, 255, 0.1);
  backdrop-filter: blur(15px);
  border-radius: 24px;
  padding: 40px 30px;
  margin: 20px;
  width: calc(100% - 40px);
  max-width: 450px;
  border: 1px solid rgba(255, 255, 255, 0.2);
  box-shadow:
    0 20px 40px rgba(0, 0, 0, 0.1),
    inset 0 1px 0 rgba(255, 255, 255, 0.2);
  position: relative;
  z-index: 1;
}

h1 {
  font-size: 2.2rem;
  color: white;
  margin-bottom: 30px;
  font-weight: 700;
  text-shadow:
    0 4px 8px rgba(0, 0, 0, 0.3),
    0 0 20px rgba(255, 255, 255, 0.2);
  background: linear-gradient(135deg, #ffffff 0%, #e0e7ff 100%);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
}

.pontuacao-final {
  margin: 30px 0;
}

.pontuacao-final h2 {
  font-size: 1.3rem;
  color: rgba(255, 255, 255, 0.95);
  margin-bottom: 15px;
  font-weight: 500;
  text-shadow: 0 2px 4px rgba(0, 0, 0, 0.2);
}

.pontos {
  font-size: 4rem;
  font-weight: 700;
  color: #53B175;
  margin: 20px 0;
  text-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
}

.pontuacao-final p {
  font-size: 1.1rem;
  color: rgba(255, 255, 255, 0.9);
  text-shadow: 0 2px 4px rgba(0, 0, 0, 0.2);
}

.botoes {
  display: flex;
  flex-direction: column;
  gap: 15px;
  margin-top: 30px;
}

.btn-reiniciar, .btn-inicio {
  padding: 18px 25px;
  font-size: 1.2rem;
  border: none;
  border-radius: 16px;
  cursor: pointer;
  transition: all 0.3s cubic-bezier(0.25, 0.46, 0.45, 0.94);
  font-weight: 600;
  box-shadow:
    0 8px 25px rgba(0, 0, 0, 0.2),
    0 4px 0 rgba(0, 0, 0, 0.1);
  position: relative;
  overflow: hidden;
}

.btn-reiniciar {
  background: linear-gradient(135deg, #53B175 0%, #429a63 100%);
  color: white;
  border: 1px solid rgba(255, 255, 255, 0.2);
}

.btn-reiniciar:hover {
  background: linear-gradient(135deg, #5bc183 0%, #4aaf72 100%);
  transform: translateY(-3px);
  box-shadow:
    0 12px 35px rgba(83, 177, 117, 0.4),
    0 6px 0 rgba(66, 154, 99, 0.9);
}

.btn-inicio {
  background: linear-gradient(135deg, #3E423F 0%, #2d302e 100%);
  color: white;
  border: 1px solid rgba(255, 255, 255, 0.1);
}

.btn-inicio:hover {
  background: linear-gradient(135deg, #4a4e4b 0%, #3a3d3b 100%);
  transform: translateY(-3px);
  box-shadow:
    0 12px 35px rgba(0, 0, 0, 0.3),
    0 6px 0 rgba(45, 48, 46, 0.9);
}

.btn-reiniciar:active, .btn-inicio:active {
  transform: translateY(-1px);
}

.estatisticas-finais {
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  gap: 15px;
  margin: 30px 0;
}

.estatistica {
  display: flex;
  flex-direction: column;
  align-items: center;
  padding: 20px 15px;
  background: rgba(255, 255, 255, 0.15);
  backdrop-filter: blur(10px);
  border-radius: 16px;
  border: 1px solid rgba(255, 255, 255, 0.2);
  box-shadow: 0 4px 15px rgba(0, 0, 0, 0.1);
}

.estatistica .valor {
  font-size: 2rem;
  font-weight: 700;
  margin-bottom: 8px;
  text-shadow: 0 2px 4px rgba(0, 0, 0, 0.2);
}

.estatistica .valor.acertos {
  color: #53B175;
}

.estatistica .valor.erros {
  color: #ff6b6b;
}

.estatistica .valor.desempenho {
  color: #667eea;
}

.estatistica .label {
  font-size: 0.9rem;
  color: rgba(255, 255, 255, 0.9);
  text-shadow: 0 1px 2px rgba(0, 0, 0, 0.2);
}

/* Efeito de partículas flutuantes */
.end-container::after {
  content: '';
  position: absolute;
  top: -50px;
  left: -50px;
  right: -50px;
  bottom: -50px;
  z-index: -1;
  animation: particles 8s linear infinite;
  pointer-events: none;
}

@keyframes particles {
  0% { transform: translateY(0px) rotate(0deg); }
  100% { transform: translateY(-100px) rotate(360deg); }
}
</style>
