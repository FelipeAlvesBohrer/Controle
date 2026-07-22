<!DOCTYPE html>
<html lang="pt-BR">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Expediente de Atividades - S. M. A. Negri</title>
  <!-- Tailwind CSS CDN -->
  <script src="https://cdn.tailwindcss.com"></script>
  <!-- Lucide Icons -->
  <script src="https://unpkg.com/lucide@latest"></script>
  <script>
    tailwind.config = {
      theme: {
        extend: {
          fontFamily: {
            serif: ['"Times New Roman"', 'Times', 'Baskerville', 'Georgia', 'serif'],
          },
          colors: {
            vinho: {
              50: '#fdf2f4',
              100: '#fbe6e9',
              200: '#f7d0d6',
              600: '#800020', // Cor Principal Vinho (Burgundy)
              700: '#66001a',
              800: '#4d0013',
              900: '#33000d',
            }
          }
        }
      }
    }
  </script>
  <style>
    body, input, select, textarea, button {
      font-family: 'Times New Roman', Times, Baskerville, Georgia, serif !important;
    }

    /* Estilização da barra de rolagem horizontal das abas */
    .no-scrollbar::-webkit-scrollbar {
      display: none;
    }
    .no-scrollbar {
      -ms-overflow-style: none;
      scrollbar-width: none;
    }

    @media print {
      .no-print {
        display: none !important;
      }
      body {
        background-color: white !important;
        color: black !important;
        font-size: 11pt !important;
      }
      .print-shadow-none {
        box-shadow: none !important;
        border: 1px solid #e5e7eb !important;
      }
      input, select, textarea {
        border: none !important;
        background: transparent !important;
        padding: 0 !important;
        appearance: none !important;
        -webkit-appearance: none !important;
        resize: none !important;
      }
      .page-break-inside-avoid {
        page-break-inside: avoid;
      }
    }
  </style>
</head>
<body class="bg-slate-100 text-slate-900 font-serif min-h-screen p-4 md:p-8">

  <div class="max-w-5xl mx-auto space-y-6">

    <!-- BARRA DE FERRAMENTAS (NO PRINT) -->
    <div class="no-print bg-black rounded-xl shadow-lg p-6 border border-slate-800 flex flex-wrap items-center justify-between gap-4 text-white">
      <div>
        <h1 class="text-xl font-bold flex items-center gap-2">
          <i data-lucide="clipboard-list" class="text-vinho-600"></i> Expediente de Atividades
        </h1>
        <p class="text-xs text-slate-400 mt-1">S. M. A. Negri • Lançamento e Planejamento de Atividades</p>
      </div>

      <div class="flex flex-wrap items-center gap-3">
        <button onclick="adicionarLinha()" class="inline-flex items-center gap-2 px-3 py-2 text-xs font-medium text-white bg-vinho-600 rounded-lg hover:bg-vinho-700 transition-colors">
          <i data-lucide="plus" class="w-4 h-4"></i> Nova Tarefa
        </button>
        <button onclick="limparTabela()" class="inline-flex items-center gap-2 px-3 py-2 text-xs font-medium text-slate-300 bg-slate-800 border border-slate-700 rounded-lg hover:bg-slate-700 transition-colors">
          <i data-lucide="rotate-ccw" class="w-4 h-4"></i> Limpar
        </button>
        <button onclick="exportarCSV()" class="inline-flex items-center gap-2 px-3 py-2 text-xs font-medium text-slate-300 bg-slate-800 border border-slate-700 rounded-lg hover:bg-slate-700 transition-colors">
          <i data-lucide="download" class="w-4 h-4"></i> CSV
        </button>
        <button onclick="window.print()" class="inline-flex items-center gap-2 px-3 py-2 text-xs font-medium text-white bg-vinho-600 rounded-lg hover:bg-vinho-700 shadow-sm transition-colors">
          <i data-lucide="printer" class="w-4 h-4"></i> Imprimir / PDF
        </button>
      </div>
    </div>

    <!-- SELETOR EM ABAS (ANO -> MÊS -> DIA) - NO PRINT -->
    <div class="no-print bg-white rounded-xl shadow-md border border-slate-200 p-4 space-y-4">
      
      <!-- 1. ABAS DE SELEÇÃO DE ANO -->
      <div>
        <div class="flex items-center justify-between mb-2">
          <span class="text-xs font-bold uppercase tracking-wider text-slate-500 flex items-center gap-1">
            <i data-lucide="calendar" class="w-3.5 h-3.5 text-vinho-600"></i> 1. Selecione o Ano
          </span>
          <button onclick="adicionarNovoAno()" class="text-xs font-bold text-vinho-600 hover:text-vinho-800 flex items-center gap-1 bg-vinho-50 px-2 py-1 rounded border border-vinho-200 transition-colors">
            <i data-lucide="plus" class="w-3 h-3"></i> Adicionar Ano
          </button>
        </div>
        <div id="abasAnos" class="flex gap-2 overflow-x-auto no-scrollbar pb-1 items-center">
          <!-- Gerado via JS -->
        </div>
      </div>

      <!-- 2. ABAS DE SELEÇÃO DE MÊS -->
      <div>
        <div class="flex items-center justify-between mb-2">
          <span class="text-xs font-bold uppercase tracking-wider text-slate-500 flex items-center gap-1">
            <i data-lucide="calendar-days" class="w-3.5 h-3.5 text-vinho-600"></i> 2. Selecione o Mês
          </span>
        </div>
        <div id="abasMeses" class="flex gap-2 overflow-x-auto no-scrollbar pb-1">
          <!-- Gerado via JS -->
        </div>
      </div>

      <!-- 3. ABAS DE SELEÇÃO DE DIA -->
      <div>
        <div class="flex items-center justify-between mb-2">
          <span class="text-xs font-bold uppercase tracking-wider text-slate-500 flex items-center gap-1">
            <i data-lucide="clock" class="w-3.5 h-3.5 text-vinho-600"></i> 3. Selecione o Dia
          </span>
          <span id="badgeDataSelecionada" class="text-xs font-bold text-vinho-600 bg-vinho-50 px-2.5 py-0.5 rounded-full border border-vinho-200">
            --/--/----
          </span>
        </div>
        <div id="abasDias" class="flex gap-1.5 overflow-x-auto no-scrollbar pb-1">
          <!-- Gerado via JS -->
        </div>
      </div>

    </div>

    <!-- FOLHA DO EXPEDIENTE -->
    <div id="folhaExpediente" class="bg-white rounded-xl shadow-xl border border-slate-200 p-6 md:p-10 print-shadow-none">

      <!-- CABEÇALHO DO ESCRITÓRIO -->
      <div class="border-b-2 border-vinho-600 pb-6 mb-6">
        <div class="flex flex-col md:flex-row justify-between items-start md:items-center gap-4">
          <div>
            <h1 class="text-3xl font-extrabold text-black tracking-tight">S. M. A. NEGRI</h1>
            <p class="text-sm font-bold text-vinho-600 tracking-widest uppercase mt-0.5">Escritório Previdenciário</p>
          </div>
          <div class="text-right">
            <span class="inline-block px-3 py-1 bg-black text-white font-semibold text-xs rounded uppercase tracking-wider mb-1">
              REGISTRO DE EXPEDIENTE
            </span>
            <p class="text-xs text-slate-500">Relatório Diário de Demandas</p>
          </div>
        </div>

        <!-- DADOS DA SESSÃO / INFORMAÇÕES DA DATA SELECIONADA -->
        <div class="grid grid-cols-1 sm:grid-cols-3 gap-4 text-sm mt-6 bg-slate-50 p-4 rounded-lg border border-slate-200">
          <div>
            <label class="block text-xs font-bold text-slate-700 uppercase mb-1">Responsável / Profissional</label>
            <input type="text" id="nomeProfissional" value="Caique Silva Holander" oninput="atualizarAssinatura(this.value)" class="w-full bg-white border border-slate-300 rounded px-2 py-1 text-slate-900 font-bold focus:ring-2 focus:ring-vinho-600 focus:outline-none">
          </div>

          <div>
            <label class="block text-xs font-bold text-slate-700 uppercase mb-1">Data em Exibição</label>
            <div id="displayDataExtenso" class="w-full bg-white border border-slate-300 rounded px-2 py-1 text-slate-900 font-bold">
              -- DE -- DE ----
            </div>
          </div>

          <div>
            <label class="block text-xs font-bold text-slate-700 uppercase mb-1">Turno / Período</label>
            <select id="turnoExpediente" class="w-full bg-white border border-slate-300 rounded px-2 py-1 text-slate-900 focus:ring-2 focus:ring-vinho-600 focus:outline-none">
              <option value="Integral">Integral (Manhã e Tarde)</option>
              <option value="Manhã">Manhã</option>
              <option value="Tarde">Tarde</option>
              <option value="Noite">Noite</option>
            </select>
          </div>
        </div>
      </div>

      <!-- PAINEL DE MÉTRICAS E RESUMO -->
      <div class="grid grid-cols-1 sm:grid-cols-3 gap-4 mb-6">
        <div class="p-3 bg-slate-50 border border-slate-200 rounded-lg text-center">
          <p class="text-xs font-bold text-slate-600 uppercase">Total de Demandas</p>
          <p id="totalDemandas" class="text-2xl font-bold text-black mt-0.5">0</p>
        </div>
        <div class="p-3 bg-emerald-50 border border-emerald-100 rounded-lg text-center">
          <p class="text-xs font-bold text-emerald-700 uppercase">Concluídas</p>
          <p id="totalConcluidas" class="text-2xl font-bold text-emerald-800 mt-0.5">0</p>
        </div>
        <div class="p-3 bg-amber-50 border border-amber-100 rounded-lg text-center">
          <p class="text-xs font-bold text-amber-700 uppercase">Em Andamento</p>
          <p id="totalAndamento" class="text-2xl font-bold text-amber-800 mt-0.5">0</p>
        </div>
      </div>

      <!-- TABELA DE TAREFAS / LANÇAMENTOS -->
      <div class="overflow-x-auto">
        <table class="w-full text-left text-sm border-collapse">
          <thead>
            <tr class="bg-black text-white uppercase tracking-wider font-bold">
              <th class="p-2.5 text-center w-12 border-r border-slate-800">Nº</th>
              <th class="p-2.5 border-r border-slate-800 w-48">Cliente / Processo / Ref.</th>
              <th class="p-2.5 border-r border-slate-800">Descrição da Atividade (O que foi / será feito)</th>
              <th class="p-2.5 border-r border-slate-800 w-40 text-center">Status</th>
              <th class="p-2.5 w-10 text-center no-print">Ação</th>
            </tr>
          </thead>
          <tbody id="tabelaAtividades" class="divide-y divide-slate-200 border-b border-slate-200">
            <!-- Linhas geradas via JavaScript -->
          </tbody>
        </table>
      </div>

      <!-- BOTÃO ADICIONAR RÁPIDO (NO PRINT) -->
      <div class="mt-4 no-print">
        <button onclick="adicionarLinha()" class="w-full py-2 border-2 border-dashed border-slate-300 rounded-lg text-slate-600 font-bold hover:border-vinho-600 hover:text-vinho-600 transition-colors flex items-center justify-center gap-2 text-xs">
          <i data-lucide="plus-circle" class="w-4 h-4"></i> Adicionar Nova Linha de Atividade
        </button>
      </div>

      <!-- OBSERVAÇÕES E NOTAS DO DIA -->
      <div class="mt-6 pt-4 border-t border-slate-200">
        <label class="block text-xs font-bold text-slate-800 uppercase mb-1">Observações Gerais / Anotações do Expediente</label>
        <textarea id="observacoesGerais" rows="3" placeholder="Insira aqui notas adicionais, pendências para o próximo expediente ou prazos urgentes..." class="w-full bg-slate-50 border border-slate-200 rounded p-2 text-sm text-slate-900 focus:ring-2 focus:ring-vinho-600 focus:outline-none"></textarea>
      </div>

      <!-- ASSINATURA -->
      <div class="mt-12 pt-8 border-t border-slate-200 flex justify-end page-break-inside-avoid">
        <div class="text-center w-72">
          <div class="border-b border-black mb-2"></div>
          <p id="nomeAssinatura" class="font-bold text-slate-900 text-sm uppercase">Caique Silva Holander</p>
          <p class="text-xs text-slate-600">Assinatura / Responsável</p>
        </div>
      </div>

      <div class="mt-8 text-center text-xs text-slate-400">
        S. M. A. Negri • Expediente gerado em <span id="dataEmissao"></span>
      </div>

    </div>
  </div>

  <script>
    // Inicializar Ícones
    lucide.createIcons();

    const nomesMeses = ['Janeiro', 'Fevereiro', 'Março', 'Abril', 'Maio', 'Junho', 'Julho', 'Agosto', 'Setembro', 'Outubro', 'Novembro', 'Dezembro'];
    const diasSemanaMin = ['Dom', 'Seg', 'Ter', 'Qua', 'Qui', 'Sex', 'Sáb'];

    // Lista de anos (Iniciando em 2026)
    let anosDisponiveis = [2026, 2027, 2028];

    // Estado da seleção atual
    const dataAtual = new Date();
    let anoSelecionado = Math.max(2026, dataAtual.getFullYear());
    let mesSelecionado = dataAtual.getMonth(); // 0 a 11
    let diaSelecionado = dataAtual.getDate();

    document.getElementById('dataEmissao').innerText = dataAtual.toLocaleDateString('pt-BR');

    // 1. Renderizar Abas de Anos
    function renderizarAbasAnos() {
      const container = document.getElementById('abasAnos');
      container.innerHTML = '';

      anosDisponiveis.forEach(ano => {
        const btn = document.createElement('button');
        const isSelected = ano === anoSelecionado;
        btn.className = `px-4 py-1.5 rounded-lg text-xs font-bold transition-all whitespace-nowrap ${
          isSelected 
            ? 'bg-black text-white shadow' 
            : 'bg-slate-100 text-slate-700 hover:bg-slate-200'
        }`;
        btn.innerText = ano;
        btn.onclick = () => {
          anoSelecionado = ano;
          renderizarAbasAnos();
          renderizarAbasMeses();
          renderizarAbasDias();
          atualizarHeaderData();
        };
        container.appendChild(btn);
      });
    }

    // Adicionar um novo ano
    function adicionarNovoAno() {
      const ultimoAno = anosDisponiveis[anosDisponiveis.length - 1];
      const novoAno = ultimoAno + 1;
      anosDisponiveis.push(novoAno);
      anoSelecionado = novoAno;
      renderizarAbasAnos();
      renderizarAbasMeses();
      renderizarAbasDias();
      atualizarHeaderData();
    }

    // 2. Renderizar Abas de Meses
    function renderizarAbasMeses() {
      const container = document.getElementById('abasMeses');
      container.innerHTML = '';

      nomesMeses.forEach((nome, idx) => {
        const btn = document.createElement('button');
        const isSelected = idx === mesSelecionado;
        btn.className = `px-3 py-1.5 rounded-lg text-xs font-bold transition-all whitespace-nowrap ${
          isSelected 
            ? 'bg-vinho-600 text-white shadow' 
            : 'bg-slate-100 text-slate-700 hover:bg-slate-200'
        }`;
        btn.innerText = nome;
        btn.onclick = () => {
          mesSelecionado = idx;
          const totalDiasNoMes = new Date(anoSelecionado, mesSelecionado + 1, 0).getDate();
          if (diaSelecionado > totalDiasNoMes) diaSelecionado = totalDiasNoMes;

          renderizarAbasMeses();
          renderizarAbasDias();
          atualizarHeaderData();
        };
        container.appendChild(btn);
      });
    }

    // 3. Renderizar Abas de Dias
    function renderizarAbasDias() {
      const container = document.getElementById('abasDias');
      container.innerHTML = '';

      const totalDias = new Date(anoSelecionado, mesSelecionado + 1, 0).getDate();

      for (let dia = 1; dia <= totalDias; dia++) {
        const dObj = new Date(anoSelecionado, mesSelecionado, dia);
        const diaSemanaIndex = dObj.getDay();
        const nomeSemana = diasSemanaMin[diaSemanaIndex];
        const isFimDeSemana = (diaSemanaIndex === 0 || diaSemanaIndex === 6);
        const isSelected = dia === diaSelecionado;

        const btn = document.createElement('button');
        btn.className = `flex flex-col items-center justify-center min-w-[42px] px-2 py-1.5 rounded-lg text-xs font-bold transition-all ${
          isSelected 
            ? 'bg-vinho-600 text-white ring-2 ring-vinho-600 ring-offset-1 shadow' 
            : isFimDeSemana 
              ? 'bg-amber-50 text-amber-800 border border-amber-200 hover:bg-amber-100' 
              : 'bg-slate-100 text-slate-800 hover:bg-slate-200'
        }`;

        btn.innerHTML = `
          <span>${dia.toString().padStart(2, '0')}</span>
          <span class="text-[9px] opacity-80 uppercase font-normal">${nomeSemana}</span>
        `;

        btn.onclick = () => {
          diaSelecionado = dia;
          renderizarAbasDias();
          atualizarHeaderData();
        };

        container.appendChild(btn);
      }
    }

    // Atualiza os cabeçalhos de data no documento
    function atualizarHeaderData() {
      const diaFormatted = diaSelecionado.toString().padStart(2, '0');
      const mesFormatted = (mesSelecionado + 1).toString().padStart(2, '0');
      const nomeMes = nomesMeses[mesSelecionado];

      document.getElementById('badgeDataSelecionada').innerText = ${diaFormatted}/${mesFormatted}/${anoSelecionado};
      document.getElementById('displayDataExtenso').innerText = ${diaFormatted} DE ${nomeMes.toUpperCase()} DE ${anoSelecionado};
    }

    // Atualiza nome na assinatura em tempo real
    function atualizarAssinatura(valor) {
      document.getElementById('nomeAssinatura').innerText = valor || 'Caique Silva Holander';
    }

    let contadorLinhas = 0;

    // Função para adicionar nova linha de atividade
    function adicionarLinha(cliente = '', descricao = '', status = 'Pendente') {
      contadorLinhas++;
      const tbody = document.getElementById('tabelaAtividades');
      const tr = document.createElement('tr');
      tr.className = 'hover:bg-slate-50 transition-colors';
      tr.dataset.id = contadorLinhas;

      tr.innerHTML = `
        <td class="p-2 text-center font-bold text-slate-400 border-r border-slate-200 item-numero">
          ${tbody.children.length + 1}
        </td>
        <td class="p-1 border-r border-slate-200">
          <input type="text" value="${cliente}" placeholder="Ex: Cliente / NB / Benefício" class="w-full bg-transparent focus:bg-white focus:ring-1 focus:ring-vinho-600 rounded px-2 py-1 text-sm">
        </td>
        <td class="p-1 border-r border-slate-200">
          <input type="text" value="${descricao}" placeholder="Descreva a tarefa feita ou a fazer..." class="w-full bg-transparent focus:bg-white focus:ring-1 focus:ring-vinho-600 rounded px-2 py-1 text-sm">
        </td>
        <td class="p-1 border-r border-slate-200 text-center">
          <select onchange="atualizarMetricas()" class="status-select w-full bg-transparent text-sm text-center focus:bg-white focus:ring-1 focus:ring-vinho-600 rounded px-1 py-1 font-semibold">
            <option value="Pendente" ${status === 'Pendente' ? 'selected' : ''}>⏳ A Fazer / Pendente</option>
            <option value="Em Andamento" ${status === 'Em Andamento' ? 'selected' : ''}>🔄 Em Andamento</option>
            <option value="Concluído" ${status === 'Concluído' ? 'selected' : ''}>✅ Concluído</option>
            <option value="Cancelado" ${status === 'Cancelado' ? 'selected' : ''}>🚫 Cancelado</option>
          </select>
        </td>
        <td class="p-1 text-center no-print">
          <button onclick="removerLinha(this)" class="text-slate-400 hover:text-rose-600 p-1 transition-colors" title="Remover atividade">
            <i data-lucide="trash-2" class="w-4 h-4"></i>
          </button>
        </td>
      `;

      tbody.appendChild(tr);
      lucide.createIcons();
      atualizarNumeracao();
      atualizarMetricas();
    }

    // Remover linha
    function removerLinha(btn) {
      const tr = btn.closest('tr');
      tr.remove();
      atualizarNumeracao();
      atualizarMetricas();
    }

    // Reordenar numeração das linhas
    function atualizarNumeracao() {
      const linhas = document.querySelectorAll('#tabelaAtividades tr');
      linhas.forEach((tr, index) => {
        tr.querySelector('.item-numero').innerText = index + 1;
      });
    }

    // Atualiza contadores e totais
    function atualizarMetricas() {
      const linhas = document.querySelectorAll('#tabelaAtividades tr');
      let total = linhas.length;
      let concluidas = 0;
      let andamento = 0;

      linhas.forEach(tr => {
        const status = tr.querySelector('.status-select').value;

        if (status === 'Concluído') concluidas++;
        if (status === 'Em Andamento') andamento++;
      });

      document.getElementById('totalDemandas').innerText = total;
      document.getElementById('totalConcluidas').innerText = concluidas;
      document.getElementById('totalAndamento').innerText = andamento;
    }

    // Limpar tabela
    function limparTabela() {
      if (confirm('Deseja realmente limpar todas as atividades do expediente?')) {
        document.getElementById('tabelaAtividades').innerHTML = '';
        document.getElementById('observacoesGerais').value = '';
        atualizarMetricas();
        carregarLinhasIniciais();
      }
    }

    // Carregar linhas padrão ao iniciar
    function carregarLinhasIniciais() {
      adicionarLinha('', '', 'Pendente');
      adicionarLinha('', '', 'Pendente');
      adicionarLinha('', '', 'Pendente');
    }

    // Exportar em CSV
    function exportarCSV() {
      const diaFormatted = diaSelecionado.toString().padStart(2, '0');
      const mesFormatted = (mesSelecionado + 1).toString().padStart(2, '0');
      const dataRef = ${diaFormatted}-${mesFormatted}-${anoSelecionado};
      const profissional = document.getElementById('nomeProfissional').value || 'Caique Silva Holander';

      let csv = S. M. A. Negri - Escritorio Previdenciario - Expediente de Atividades\n;
      csv += Data: ${dataRef}; Responsavel: ${profissional}\n\n;
      csv += Item;Referencia / Cliente;Descricao;Status\n;

      const linhas = document.querySelectorAll('#tabelaAtividades tr');
      linhas.forEach((tr, index) => {
        const inputs = tr.querySelectorAll('input');
        const select = tr.querySelector('select');
        const ref = inputs[0].value.replace(/;/g, ',');
        const desc = inputs[1].value.replace(/;/g, ',');
        const status = select.value;

        csv += ${index + 1};${ref};${desc};${status}\n;
      });

      const blob = new Blob(["\ufeff" + csv], { type: 'text/csv;charset=utf-8;' });
      const link = document.createElement('a');
      const url = URL.createObjectURL(blob);
      link.setAttribute('href', url);
      link.setAttribute('download', Expediente_SMANegri_${dataRef}.csv);
      document.body.appendChild(link);
      link.click();
      document.body.removeChild(link);
    }

    // Inicialização da interface e abas
    renderizarAbasAnos();
    renderizarAbasMeses();
    renderizarAbasDias();
    atualizarHeaderData();
    carregarLinhasIniciais();
  </script>
</body>
</html>
