/* ==========================================================
   SORTEIO SIPAT AZUL 2026
   ==========================================================
   1. Elementos DOM
   2. Estado da aplicação
   3. Utilitários
   4. Tela cheia
   5. Modal de confirmação
   6. Configuração do sorteio
   7. Atualização da interface
   8. Áudio
   9. Confete
   10. Histórico de sorteados
   11. Roleta
   12. Eventos
   13. Inicialização
   ========================================================== */

// ==========================================================
// ELEMENTOS DOM
// ==========================================================

const displayNumero = document.getElementById('displayNumero');
const botaoSortear = document.getElementById('botaoSortear');
const botaoReiniciar = document.getElementById('botaoReiniciar');
const botaoAplicarIntervalo = document.getElementById('botaoAplicarIntervalo');
const botaoTelaCheia = document.getElementById('botaoTelaCheia');

const campoNumeroInicial = document.getElementById('numeroInicial');
const campoNumeroFinal = document.getElementById('numeroFinal');

const grade = document.getElementById('grade');
const notaVazia = document.getElementById('notaVazia');

const preenchimentoProgresso = document.getElementById('preenchimentoProgresso');
const rotuloProgresso = document.getElementById('rotuloProgresso');
const rotuloRestante = document.getElementById('rotuloRestante');
const rotuloContagem = document.getElementById('rotuloContagem');

const mensagemConcluido = document.getElementById('mensagemConcluido');
const regiaoViva = document.getElementById('regiaoViva');

const modalConfirmacao = document.getElementById('modalConfirmacao');
const textoModal = document.getElementById('textoModal');
const confirmarModal = document.getElementById('confirmarModal');
const cancelarModal = document.getElementById('cancelarModal');

// ==========================================================
// ESTADO
// ==========================================================

let numeroInicial = 1;
let numeroFinal = 100;
let totalNumeros = 100;

let restantes = [];
let ordemSorteada = [];
let sorteando = false;

let contextoAudio;

// ==========================================================
// UTILITÁRIOS
// ==========================================================

function formatarNumero(numero) {
    return String(numero);
}

function embaralhar(array) {
    for (let i = array.length - 1; i > 0; i--) {
        const j = Math.floor(Math.random() * (i + 1));

        [array[i], array[j]] =
            [array[j], array[i]];
    }

    return array;
}

function gerarIntervalo(inicio, fim) {
    const lista = [];

    for (let numero = inicio; numero <= fim; numero++) {
        lista.push(numero);
    }

    return embaralhar(lista);
}

// ==========================================================
// TELA CHEIA
// ==========================================================

function alternarTelaCheia() {

    if (!document.fullscreenElement) {
        document.documentElement.requestFullscreen();
        return;
    }

    document.exitFullscreen();
}

function atualizarBotaoTelaCheia() {
    botaoTelaCheia.textContent =
        document.fullscreenElement
            ? '🗗'
            : '⛶';
}

// ==========================================================
// MODAL
// ==========================================================

function abrirModalConfirmacao(
    mensagem,
    aoConfirmar
) {

    textoModal.textContent = mensagem;

    modalConfirmacao.classList.add('ativo');

    confirmarModal.onclick = () => {

        modalConfirmacao.classList.remove('ativo');

        aoConfirmar();
    };

    cancelarModal.onclick = () => {
        modalConfirmacao.classList.remove('ativo');
    };
}

// ==========================================================
// CONFIGURAÇÃO DO SORTEIO
// ==========================================================

function aplicarIntervalo() {
    if (sorteando) return;

    const inicio = parseInt(campoNumeroInicial.value, 10);
    const fim = parseInt(campoNumeroFinal.value, 10);

    if (Number.isNaN(inicio) || Number.isNaN(fim) || fim <= inicio) {
        alert('Informe um intervalo válido, com o número final maior que o inicial.');
        return;
    }

    if (ordemSorteada.length > 0) {
        const confirmar = confirm('Alterar o intervalo vai reiniciar o sorteio atual e apagar os números já sorteados. Deseja continuar?');
        if (!confirmar) return;
    }

    numeroInicial = inicio;
    numeroFinal = fim;
    totalNumeros = fim - inicio + 1;

    inicializarEstado();

    // Oculta a configuração
    document.querySelector('.config-intervalo').classList.add('oculto');
}

// Reinicia todo o estado do sorteio para o intervalo configurado atualmente
function inicializarEstado() {
    restantes = gerarIntervalo(numeroInicial, numeroFinal);
    ordemSorteada = [];
    displayNumero.textContent = '--';
    displayNumero.classList.remove('girando', 'parado');
    grade.innerHTML = '';
    grade.appendChild(notaVazia);
    notaVazia.style.display = 'block';
    mensagemConcluido.style.display = 'none';
    botaoSortear.disabled = false;
    botaoSortear.textContent = 'Sortear número';
    atualizarContadores();
}

// ==========================================================
// INTERFACE
// ==========================================================

// Atualiza os textos e a barra de progresso conforme os números já sorteados
function atualizarContadores() {
    const quantidadeSorteada = ordemSorteada.length;
    preenchimentoProgresso.style.width = (quantidadeSorteada / totalNumeros * 100) + '%';
    rotuloProgresso.textContent = quantidadeSorteada + ' de ' + totalNumeros + ' sorteados';
    rotuloRestante.textContent = (totalNumeros - quantidadeSorteada) + ' restantes';
    rotuloContagem.textContent = quantidadeSorteada + (quantidadeSorteada === 1 ? ' ficha' : ' fichas');
}

// ==========================================================
// ÁUDIO
// ==========================================================

function obterContextoAudio() {
    if (!contextoAudio) {
        try { contextoAudio = new (window.AudioContext || window.webkitAudioContext)(); }
        catch (erro) { contextoAudio = null; }
    }
    return contextoAudio;
}

// Toca um "tique" curto durante o giro da roleta
function tocarTique(frequencia) {
    const ctx = obterContextoAudio();
    if (!ctx) return;
    const osc = ctx.createOscillator();
    const ganho = ctx.createGain();
    osc.type = 'square';
    osc.frequency.value = frequencia;
    ganho.gain.value = 0.035;
    osc.connect(ganho).connect(ctx.destination);
    osc.start();
    ganho.gain.exponentialRampToValueAtTime(0.0001, ctx.currentTime + 0.08);
    osc.stop(ctx.currentTime + 0.09);
}

// Toca um acorde de três notas ao concluir o sorteio de um número
function tocarAcorde() {
    const ctx = obterContextoAudio();
    if (!ctx) return;
    [660, 880, 1320].forEach((frequencia, indice) => {
        const osc = ctx.createOscillator();
        const ganho = ctx.createGain();
        osc.type = 'sine';
        osc.frequency.value = frequencia;
        ganho.gain.value = 0.05;
        osc.connect(ganho).connect(ctx.destination);
        const tempo = ctx.currentTime + indice * 0.09;
        osc.start(tempo);
        ganho.gain.exponentialRampToValueAtTime(0.0001, tempo + 0.35);
        osc.stop(tempo + 0.36);
    });
}

// ==========================================================
// CONFETE
// ==========================================================

function lancarConfete() {
    const cores = ['#7ac142', '#a8d977', '#17b4f2', '#0072ce', '#ffffff', '#f2994a'];
    for (let i = 0; i < 36; i++) {
        const pedaco = document.createElement('div');
        pedaco.className = 'pedaco-confete';
        pedaco.style.left = Math.random() * 100 + 'vw';
        pedaco.style.background = cores[Math.floor(Math.random() * cores.length)];
        const duracao = 1.6 + Math.random() * 1.2;
        pedaco.style.animationDuration = duracao + 's';
        pedaco.style.opacity = 0.9;
        document.body.appendChild(pedaco);
        setTimeout(() => pedaco.remove(), duracao * 1000 + 100);
    }
}

// ==========================================================
// HISTÓRICO
// ==========================================================

// Monta o elemento visual (ficha) de um número já sorteado
function criarFicha(numero, sequencia, ehMaisRecente) {
    const ficha = document.createElement('div');
    ficha.className = 'ficha' + (ehMaisRecente ? ' mais-recente' : '');
    ficha.innerHTML = '<div class="ficha-sequencia">Nº ' + sequencia + '</div><div class="ficha-numero">' + formatarNumero(numero) + '</div>';
    return ficha;
}

// Adiciona o número sorteado no topo da lista de fichas
function adicionarNumeroSorteado(numero) {
    const fichaAnterior = grade.querySelector('.ficha.mais-recente');
    if (fichaAnterior) fichaAnterior.classList.remove('mais-recente');

    if (notaVazia.parentNode) notaVazia.style.display = 'none';

    const ficha = criarFicha(numero, ordemSorteada.length, true);
    grade.insertBefore(ficha, grade.firstChild);
}

// ==========================================================
// ROLETA
// ==========================================================

// Anima a "roleta" girando até parar no número final sorteado.
// duracaoTotal controla o tempo total do giro (em milissegundos).
function girarRoleta(numeroFinalSorteado, aoConcluir) {

    displayNumero.classList.add('girando');
    displayNumero.classList.remove('parado');

    let tempoDecorrido = 0;
    const duracaoTotal = 4000;

    let atraso = 20;

    function tique() {

        if (tempoDecorrido >= duracaoTotal) {

            displayNumero.textContent =
                formatarNumero(numeroFinalSorteado);

            displayNumero.classList.remove('girando');
            displayNumero.classList.add('parado');

            aoConcluir();
            return;
        }

        const numeroAleatorio =
            Math.floor(Math.random() * totalNumeros) +
            numeroInicial;

        displayNumero.textContent =
            formatarNumero(numeroAleatorio);

        tocarTique(
            180 + Math.random() * 120
        );

        atraso *= 1.07;
        tempoDecorrido += atraso;

        setTimeout(tique, atraso);
    }

    tique();
}

// ==========================================================
// EVENTOS
// ==========================================================

// Trata o clique no botão "Sortear número"
function tratarSorteio() {
    if (sorteando || restantes.length === 0) return;
    sorteando = true;
    botaoSortear.disabled = true;

    const indice = Math.floor(Math.random() * restantes.length);
    const numeroFinalSorteado = restantes[indice];
    restantes.splice(indice, 1);

    girarRoleta(numeroFinalSorteado, () => {
        ordemSorteada.push(numeroFinalSorteado);
        adicionarNumeroSorteado(numeroFinalSorteado);
        atualizarContadores();
        lancarConfete();
        tocarAcorde();
        regiaoViva.textContent = 'Número sorteado: ' + numeroFinalSorteado;

        sorteando = false;
        if (restantes.length === 0) {
            botaoSortear.disabled = true;
            botaoSortear.textContent = 'Sorteio concluído';
            mensagemConcluido.style.display = 'block';
        } else {
            botaoSortear.disabled = false;
        }
    });
}

// Trata o clique no botão "Reiniciar sorteio"
function tratarReinicio() {

    if (sorteando) return;

    const reiniciar = () => {

        document
            .querySelector('.config-intervalo')
            .classList.remove('oculto');

        inicializarEstado();
    };

    if (ordemSorteada.length > 0) {

        abrirModalConfirmacao(
            `Isso vai apagar todos os ${ordemSorteada.length} números já sorteados. Deseja reiniciar o sorteio?`,
            reiniciar
        );

        return;
    }

    reiniciar();
}

// ==========================================================
// INICIALIZAÇÃO
// ==========================================================

// ===== Ligação dos eventos e inicialização =====
botaoSortear.addEventListener('click', tratarSorteio);
botaoReiniciar.addEventListener('click', tratarReinicio);
botaoAplicarIntervalo.addEventListener('click', aplicarIntervalo);

inicializarEstado();

function abrirModalConfirmacao(mensagem, aoConfirmar) {

    textoModal.textContent = mensagem;

    modalConfirmacao.classList.add('ativo');

    confirmarModal.onclick = () => {
        modalConfirmacao.classList.remove('ativo');
        aoConfirmar();
    };

    cancelarModal.onclick = () => {
        modalConfirmacao.classList.remove('ativo');
    };
}

