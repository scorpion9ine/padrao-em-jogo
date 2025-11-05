<script setup>
import { useAppStore } from '@/stores/app';
import router from '@/router';

const appStore = useAppStore();

let segundos = 0;
appStore.sortearPadroes();
appStore.resetarPontos();

function segundo() {
  segundos++;
  if (segundos == 60) { // Voltei para 60 segundos
    clearInterval(intervalo)
    router.push('/end')
  }
  document.getElementById('segundo').innerHTML = segundos.toString().padStart(2, '0')
}

function verificarAcerto(resposta) {
  if (appStore.verificarAcerto(resposta)) {
    appStore.adicionarPonto(); // Agora chama adicionarAcerto
  } else {
    appStore.adicionarErro();
  }
  appStore.sortearPadroes();
}
function sair() {
  clearInterval(intervalo);
  router.push('/')
}

var intervalo = setInterval(function () { segundo() }, 1000)
</script>

<template>
  <main>
    <div class="cron">
      <div>Cronômetro</div>
      <div class="tempo">
        <span id="minuto">00</span><span>:</span><span id="segundo">00</span>
      </div>
    </div>

    <div class="game-container">
      <h2>Complete o Padrão:</h2>

      <div class="padrao-container">
        <div class="sequencia">
          <div v-for="(item, index) in appStore.padraoEscolhido.pattern"
               :key="index"
               class="item-padrao"
               :class="appStore.padraoEscolhido.type">
            {{ item }}
          </div>
          <div class="item-padrao vazio">?</div>
        </div>

        <p class="descricao">{{ appStore.padraoEscolhido.name }}</p>
      </div>

      <div class="alternativas">
        <div v-for="(alternativa, index) in appStore.alternativasSorteadas"
             :key="index"
             class="alternativa"
             @click="verificarAcerto(alternativa)"
             :class="appStore.padraoEscolhido.type">
          {{ alternativa }}
        </div>
      </div>

      <div class="info-container">
  <div class="estatisticas">
    <div class="pontuacao">
      <p class="ponto">Pontuação: {{ appStore.pontos }}</p>
    </div>
    <div class="erros">
      <p class="erro">Erros: {{ appStore.erros }}</p>
    </div>
    <div class="desempenho">
      <p class="percentual">Acertos: {{ appStore.calcularDesempenho() }}%</p>
    </div>
  </div>

  <div class="sair-container">
    <p @click="sair" class="router-link">Sair</p>
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
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  display: flex;
  flex-direction: column;
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

.cron {
  display: flex;
  justify-content: space-between;
  padding: 20px 30px;
  background: rgba(255, 255, 255, 0.95);
  backdrop-filter: blur(10px);
  margin-bottom: 0;
  font-weight: 700;
  color: #3E423F;
  flex-shrink: 0;
  border-bottom: 1px solid rgba(255, 255, 255, 0.3);
  box-shadow: 0 4px 20px rgba(0, 0, 0, 0.1);
  position: relative;
  z-index: 1;
}

.tempo {
  font-family: 'Courier New', monospace;
  font-size: 1.3rem;
  font-weight: 700;
  color: #667eea;
  text-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

.game-container {
  background: rgba(255, 255, 255, 0.1);
  backdrop-filter: blur(15px);
  border-radius: 30px 30px 0 0;
  padding: 30px 25px;
  flex: 1;
  display: flex;
  flex-direction: column;
  margin-top: 10px;
  border: 1px solid rgba(255, 255, 255, 0.2);
  border-bottom: none;
  box-shadow:
    0 -20px 40px rgba(0, 0, 0, 0.1),
    inset 0 1px 0 rgba(255, 255, 255, 0.2);
  position: relative;
  z-index: 1;
}

h2 {
  font-size: 1.8rem;
  color: white;
  margin-bottom: 30px;
  font-weight: 700;
  text-align: center;
  text-shadow:
    0 4px 8px rgba(0, 0, 0, 0.3),
    0 0 20px rgba(255, 255, 255, 0.2);
  background: linear-gradient(135deg, #ffffff 0%, #e0e7ff 100%);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
}

.padrao-container {
  margin-bottom: 30px;
  flex-shrink: 0;
}

.sequencia {
  display: flex;
  justify-content: center;
  gap: 15px;
  margin-bottom: 20px;
  flex-wrap: wrap;
}

.item-padrao {
  width: 60px;
  height: 60px;
  display: flex;
  align-items: center;
  justify-content: center;
  border-radius: 16px;
  font-size: 1.4rem;
  font-weight: bold;
  box-shadow:
    0 8px 20px rgba(0, 0, 0, 0.2),
    inset 0 1px 0 rgba(255, 255, 255, 0.3);
  flex-shrink: 0;
  transition: all 0.3s ease;
  border: 2px solid transparent;
}

.item-padrao.vazio {
  background: rgba(248, 249, 250, 0.9);
  border: 2px dashed rgba(222, 226, 230, 0.8);
  color: #6c757d;
  backdrop-filter: blur(10px);
}

.item-padrao:not(.vazio) {
  background: linear-gradient(135deg, #53B175 0%, #429a63 100%);
  color: white;
  border: 2px solid rgba(255, 255, 255, 0.3);
}

.item-padrao.emoji, .alternativa.emoji {
  font-size: 1.8rem;
  background: linear-gradient(135deg, #ff6b6b 0%, #ff5252 100%);
  border-color: rgba(255, 255, 255, 0.3);
}

.item-padrao.color, .alternativa.color {
  font-size: 1.8rem;
  background: linear-gradient(135deg, #4ecdc4 0%, #45b7af 100%);
  border-color: rgba(255, 255, 255, 0.3);
}

.item-padrao.arrow, .alternativa.arrow {
  font-size: 1.8rem;
  background: linear-gradient(135deg, #ffe66d 0%, #ffdd47 100%);
  border-color: rgba(255, 255, 255, 0.3);
  color: #333;
}

.item-padrao.number, .alternativa.number {
  background: linear-gradient(135deg, #53B175 0%, #429a63 100%);
  border-color: rgba(255, 255, 255, 0.3);
  color: white;
}

.item-padrao.shape, .alternativa.shape {
  background: linear-gradient(135deg, #a78bfa 0%, #8b5cf6 100%);
  border-color: rgba(255, 255, 255, 0.3);
  color: white;
}

.item-padrao.letter, .alternativa.letter {
  background: linear-gradient(135deg, #f97316 0%, #ea580c 100%);
  border-color: rgba(255, 255, 255, 0.3);
  color: white;
}

.item-padrao.symbol, .alternativa.symbol {
  background: linear-gradient(135deg, #ec4899 0%, #db2777 100%);
  border-color: rgba(255, 255, 255, 0.3);
  color: white;
}

.descricao {
  font-size: 1.1rem;
  color: rgba(255, 255, 255, 0.95);
  font-weight: 500;
  text-align: center;
  margin: 0;
  text-shadow: 0 2px 4px rgba(0, 0, 0, 0.2);
}

.alternativas {
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  gap: 15px;
  margin: 25px 0;
  flex: 1;
}

.alternativa {
  padding: 20px;
  background: rgba(255, 255, 255, 0.15);
  backdrop-filter: blur(10px);
  border-radius: 16px;
  font-size: 1.8rem;
  cursor: pointer;
  transition: all 0.3s cubic-bezier(0.25, 0.46, 0.45, 0.94);
  border: 2px solid rgba(255, 255, 255, 0.2);
  font-weight: bold;
  display: flex;
  align-items: center;
  justify-content: center;
  min-height: 80px;
  box-shadow:
    0 4px 15px rgba(0, 0, 0, 0.1),
    inset 0 1px 0 rgba(255, 255, 255, 0.2);
  position: relative;
  overflow: hidden;
}

.alternativa::before {
  content: '';
  position: absolute;
  top: 0;
  left: -100%;
  width: 100%;
  height: 100%;
  background: linear-gradient(90deg, transparent, rgba(255, 255, 255, 0.2), transparent);
  transition: left 0.6s ease;
}

.alternativa:hover {
  transform: translateY(-4px);
  box-shadow:
    0 8px 25px rgba(0, 0, 0, 0.2),
    inset 0 1px 0 rgba(255, 255, 255, 0.3);
  background: rgba(255, 255, 255, 0.25);
  border-color: rgba(255, 255, 255, 0.3);
}

.alternativa:hover::before {
  left: 100%;
}

.alternativa:active {
  transform: translateY(-1px);
}

.info-container {
  display: flex;
  justify-content: space-between;
  align-items: flex-end;
  margin-top: auto;
  flex-shrink: 0;
  gap: 20px;
}

.estatisticas {
  display: flex;
  flex-direction: column;
  gap: 8px;
  flex: 1;
}

.pontuacao, .erros, .desempenho {
  margin: 0;
}

.ponto {
  font-size: 1.3rem;
  font-weight: 700;
  color: white;
  margin: 0;
  padding: 12px 18px;
  background: rgba(83, 177, 117, 0.3);
  backdrop-filter: blur(10px);
  border-radius: 12px;
  border: 1px solid rgba(255, 255, 255, 0.2);
  text-shadow: 0 2px 4px rgba(0, 0, 0, 0.2);
  box-shadow: 0 4px 15px rgba(0, 0, 0, 0.1);
}

.erro {
  font-size: 1.1rem;
  font-weight: 700;
  color: white;
  margin: 0;
  padding: 10px 15px;
  background: rgba(255, 107, 107, 0.3);
  backdrop-filter: blur(10px);
  border-radius: 10px;
  border: 1px solid rgba(255, 255, 255, 0.2);
  text-shadow: 0 2px 4px rgba(0, 0, 0, 0.2);
  box-shadow: 0 4px 15px rgba(0, 0, 0, 0.1);
}

.percentual {
  font-size: 1.1rem;
  font-weight: 700;
  color: white;
  margin: 0;
  padding: 10px 15px;
  background: rgba(102, 126, 234, 0.3);
  backdrop-filter: blur(10px);
  border-radius: 10px;
  border: 1px solid rgba(255, 255, 255, 0.2);
  text-shadow: 0 2px 4px rgba(0, 0, 0, 0.2);
  box-shadow: 0 4px 15px rgba(0, 0, 0, 0.1);
}

.sair-container {
  margin: 0;
}

.router-link {
  font-size: 1.2rem;
  padding: 12px 25px;
  background: linear-gradient(135deg, #3E423F 0%, #2d302e 100%);
  color: #fff9ff;
  border-radius: 12px;
  cursor: pointer;
  display: inline-block;
  transition: all 0.3s cubic-bezier(0.25, 0.46, 0.45, 0.94);
  text-decoration: none;
  font-weight: 600;
  box-shadow:
    0 4px 15px rgba(0, 0, 0, 0.2),
    0 2px 0 rgba(45, 48, 46, 0.8);
  border: 1px solid rgba(255, 255, 255, 0.1);
  position: relative;
  overflow: hidden;
}

.router-link::before {
  content: '';
  position: absolute;
  top: 0;
  left: -100%;
  width: 100%;
  height: 100%;
  background: linear-gradient(90deg, transparent, rgba(255, 255, 255, 0.1), transparent);
  transition: left 0.6s ease;
}

.router-link:hover {
  background: linear-gradient(135deg, #4a4e4b 0%, #3a3d3b 100%);
  transform: translateY(-3px);
  box-shadow:
    0 6px 20px rgba(0, 0, 0, 0.3),
    0 3px 0 rgba(45, 48, 46, 0.9);
}

.router-link:hover::before {
  left: 100%;
}

.router-link:active {
  transform: translateY(-1px);
}

/* Efeito de partículas flutuantes */
.game-container::after {
  content: '';
  position: absolute;
  top: -50px;
  left: -50px;
  right: -50px;
  bottom: -50px;
  background:
    radial-gradient(2px 2px at 20% 30%, rgba(255,255,255,0.2) 50%, transparent 100%),
    radial-gradient(2px 2px at 80% 70%, rgba(255,255,255,0.2) 50%, transparent 100%),
    radial-gradient(2px 2px at 40% 90%, rgba(255,255,255,0.2) 50%, transparent 100%),
    radial-gradient(2px 2px at 60% 10%, rgba(255,255,255,0.2) 50%, transparent 100%);
  z-index: -1;
  animation: particles 8s linear infinite;
  pointer-events: none;
}

@keyframes particles {
  0% { transform: translateY(0px) rotate(0deg); }
  100% { transform: translateY(-100px) rotate(360deg); }
}
</style>
