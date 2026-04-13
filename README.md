<!DOCTYPE html>
<html lang="pt-BR">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Controle Financeiro Pessoal</title>
  <style>
    * { box-sizing: border-box; }
    body {
      margin: 0;
      font-family: Arial, sans-serif;
      background: #f3f4f6;
      color: #111827;
    }
    .container {
      max-width: 1500px;
      margin: 20px auto;
      padding: 16px;
    }
    h1, h2, h3 { margin: 0 0 12px; }
    .card {
      background: #fff;
      border-radius: 16px;
      padding: 18px;
      box-shadow: 0 4px 16px rgba(0,0,0,0.08);
      margin-bottom: 18px;
    }
    .grid {
      display: grid;
      grid-template-columns: repeat(auto-fit, minmax(180px, 1fr));
      gap: 12px;
    }
    .grid-2 {
      display: grid;
      grid-template-columns: 1fr 1fr;
      gap: 18px;
    }
    .grid-3 {
      display: grid;
      grid-template-columns: repeat(auto-fit, minmax(260px, 1fr));
      gap: 14px;
    }
    label {
      display: block;
      font-size: 14px;
      font-weight: bold;
      margin-bottom: 6px;
    }
    input, select, button {
      width: 100%;
      padding: 10px 12px;
      border: 1px solid #cbd5e1;
      border-radius: 8px;
      font-size: 14px;
    }
    button {
      border: none;
      font-weight: bold;
      cursor: pointer;
    }
    .btn-primary { background: #2563eb; color: #fff; }
    .btn-success { background: #16a34a; color: #fff; }
    .btn-danger { background: #dc2626; color: #fff; }
    .btn-secondary { background: #475569; color: #fff; }

    .acoes {
      display: grid;
      grid-template-columns: repeat(auto-fit, minmax(180px, 1fr));
      gap: 12px;
      margin-top: 12px;
    }

    .modo-box {
      margin-top: 12px;
      padding: 12px 14px;
      border-radius: 12px;
      font-weight: bold;
      background: #f8fafc;
      border-left: 6px solid #dc2626;
    }
    .modo-entrada {
      border-left-color: #16a34a;
      background: #ecfdf5;
      color: #166534;
    }
    .modo-saida {
      border-left-color: #dc2626;
      background: #fef2f2;
      color: #991b1b;
    }

    .tabela-wrap { overflow-x: auto; }

    table {
      width: 100%;
      border-collapse: collapse;
      margin-top: 12px;
      min-width: 1000px;
    }
    th, td {
      border: 1px solid #cbd5e1;
      padding: 8px 10px;
      text-align: center;
      font-size: 14px;
    }
    th {
      background: #fff200;
      color: #111;
      font-weight: bold;
    }

    .valor-positivo { color: #0f766e; font-weight: bold; }
    .valor-negativo { color: #b91c1c; font-weight: bold; }

    .resumo {
      display: grid;
      grid-template-columns: repeat(auto-fit, minmax(220px, 1fr));
      gap: 12px;
      margin: 12px 0 16px;
    }
    .box {
      border-radius: 12px;
      padding: 14px;
      background: #f8fafc;
      border-left: 6px solid #2563eb;
    }
    .box .valor {
      margin-top: 8px;
      font-size: 24px;
      font-weight: bold;
    }

    .graficos {
      display: grid;
      grid-template-columns: 1fr 1.5fr;
      gap: 18px;
      align-items: start;
      margin-bottom: 18px;
    }

    .grafico-card, .analise-card {
      background: #f8fafc;
      border: 1px solid #e2e8f0;
      border-radius: 14px;
      padding: 14px;
    }

    .grafico-titulo, .analise-titulo {
      font-weight: bold;
      margin-bottom: 10px;
    }

    #pizzaChart, #colunaChart {
      width: 100%;
      height: auto;
      display: block;
      background: #fff;
      border-radius: 10px;
    }

    .legenda {
      display: flex;
      flex-wrap: wrap;
      gap: 14px;
      margin-top: 10px;
      font-size: 13px;
    }

    .legenda-item {
      display: flex;
      align-items: center;
      gap: 6px;
    }

    .cor {
      width: 14px;
      height: 14px;
      border-radius: 3px;
      display: inline-block;
    }

    .report {
      margin-top: 14px;
      background: #f8fafc;
      border: 1px dashed #94a3b8;
      border-radius: 12px;
      padding: 14px;
      white-space: pre-wrap;
      line-height: 1.5;
    }

    .muted {
      color: #64748b;
      font-size: 13px;
    }

    .mini-tabela {
      width: 100%;
      min-width: 100%;
      margin-top: 8px;
    }

    .mini-tabela th {
      background: #e2e8f0;
    }

    .acoes-inline {
      display: flex;
      gap: 6px;
      justify-content: center;
      flex-wrap: wrap;
    }

    @media (max-width: 980px) {
      .graficos, .grid-2 {
        grid-template-columns: 1fr;
      }
    }
  </style>
</head>
<body>
  <div class="container">
    <h1>Controle Financeiro Pessoal</h1>
    <p class="muted">Controle de entradas, saídas, contas fixas e relatório mensal.</p>

    <div class="card">
      <h2>Selecionar Mês de Trabalho</h2>
      <div class="grid">
        <div>
          <label>Mês</label>
          <select id="mesFiltro"></select>
        </div>
        <div>
          <label>Ano</label>
          <select id="anoFiltro"></select>
        </div>
        <div style="display:flex;align-items:end;">
          <button class="btn-primary" onclick="trocarMes()">Atualizar mês</button>
        </div>
      </div>
      <p class="muted">Tudo fica salvo. Troque o mês e o ano para consultar outros períodos.</p>
    </div>

    <div class="card">
      <h2>Adicionar Lançamento</h2>
      <div class="grid">
        <div>
          <label>Data</label>
          <input type="date" id="data" />
          <p class="muted">A data do lançamento acompanha o mês e o ano definidos em Selecionar Mês de Trabalho.</p>
        </div>
        <div>
          <label>Valor</label>
          <input type="number" id="valor" step="0.01" placeholder="Ex.: 1500" />
        </div>
        <div>
          <label>Em que?</label>
          <input type="text" id="emQue" list="listaEmQue" placeholder="Ex.: mercado, combustível, salário" />
          <datalist id="listaEmQue"></datalist>
        </div>
        <div>
          <label>Local / Quem</label>
          <input type="text" id="localQuem" list="listaLocalQuem" placeholder="Ex.: supermercado, João, empresa" />
          <datalist id="listaLocalQuem"></datalist>
        </div>
        <div>
          <label>Tipo de pagamento</label>
          <select id="tipoPagamento">
            <option value="PIX">PIX</option>
            <option value="Débito">Débito</option>
            <option value="Crédito">Crédito</option>
            <option value="Dinheiro">Dinheiro</option>
            <option value="Cartão">Cartão</option>
            <option value="Outro">Outro</option>
          </select>
        </div>
        <div>
          <label>Necessário?</label>
          <select id="necessario" onchange="adicionarAoSelecionarNecessario()">
            <option value="">Selecione</option>
            <option value="SIM">SIM</option>
            <option value="NÃO">NÃO</option>
          </select>
        </div>
      </div>

      <div class="acoes">
        <button id="botaoModo" class="btn-success" onclick="alternarModoLancamento()">ENTRADA</button>
      </div>

      <div id="modoAtual" class="modo-box modo-saida">Modo atual: SAÍDA</div>

      <p class="muted">O botão alterna entre ENTRADA e SAÍDA. Se mudar por engano, basta clicar novamente para voltar à configuração inicial.</p>
    </div>

    <div class="card">
      <h2>Fatura do Cartão</h2>
      <div class="tabela-wrap">
        <table>
          <thead>
            <tr>
              <th>VENCIMENTO</th>
              <th>ITENS DA FATURA</th>
              <th>TOTAL</th>
            </tr>
          </thead>
          <tbody id="tabelaFaturaCartao"></tbody>
        </table>
      </div>
    </div>

    <div class="card">
      <h2>Despesas Fixas</h2>
      <div class="grid">
        <div>
          <label>Nome da despesa</label>
          <input type="text" id="fixaNome" placeholder="Ex.: Água, Luz, Internet" />
        </div>
        <div>
          <label>Vencimento</label>
          <input type="date" id="fixaVencimento" />
        </div>
        <div>
          <label>Valor</label>
          <input type="number" id="fixaValor" step="0.01" placeholder="Ex.: 120,50" />
        </div>
      </div>
      <div class="acoes">
        <button class="btn-success" onclick="adicionarDespesaFixa()">Adicionar despesa fixa</button>
      </div>
      <div class="tabela-wrap">
        <table>
          <thead>
            <tr>
              <th>NOME</th>
              <th>VENCIMENTO</th>
              <th>VALOR</th>
              <th>STATUS</th>
              <th>DATA DO PAGAMENTO</th>
              <th>SITUAÇÃO</th>
              <th>AÇÃO</th>
            </tr>
          </thead>
          <tbody id="tabelaFixas"></tbody>
        </table>
      </div>
    </div>

    <div class="card">
      <h2>Tabela de Lançamentos do Mês Selecionado</h2>
      <div class="tabela-wrap">
        <table>
          <thead>
            <tr>
              <th>DATA</th>
              <th>VALOR</th>
              <th>EM QUE ?</th>
              <th>LOCAL / QUEM</th>
              <th>TIPO DE PAGAMENTO</th>
              <th>PARCELA</th>
              <th>NECESSÁRIO?</th>
              <th>AÇÃO</th>
            </tr>
          </thead>
          <tbody id="tabelaLancamentos"></tbody>
        </table>
      </div>
    </div>

    <div class="card">
      <h2>Relatório Mensal</h2>

      <div class="resumo" id="resumoMes"></div>

      <div class="analise-card" style="margin-bottom:18px;">
        <div class="analise-titulo">Contas Fixas</div>
        <div class="grid-2">
          <div class="box" style="border-left-color:#16a34a">
            <strong>Valor pago</strong>
            <div class="valor" id="fixasPagasBox">R$ 0,00</div>
          </div>
          <div class="box" style="border-left-color:#b91c1c">
            <strong>Valor não pago</strong>
            <div class="valor" id="fixasNaoPagasBox">R$ 0,00</div>
          </div>
        </div>
      </div>

      <div class="graficos">
        <div class="grafico-card">
          <div class="grafico-titulo">Gráfico de pizza — Entradas, Saídas e Saldo</div>
          <svg id="pizzaChart" viewBox="0 0 420 320"></svg>
          <div class="legenda">
            <div class="legenda-item"><span class="cor" style="background:#2563eb"></span>Entrada</div>
            <div class="legenda-item"><span class="cor" style="background:#dc2626"></span>Saída</div>
            <div class="legenda-item"><span class="cor" style="background:#facc15"></span>Saldo positivo</div>
            <div class="legenda-item"><span class="cor" style="background:#7f1d1d"></span>Saldo negativo</div>
          </div>
        </div>

        <div class="grafico-card">
          <div class="grafico-titulo">Gráfico de colunas — Entradas e saídas por dia</div>
          <svg id="colunaChart" viewBox="0 0 760 340"></svg>
          <div class="legenda">
            <div class="legenda-item"><span class="cor" style="background:#2563eb"></span>Entrada</div>
            <div class="legenda-item"><span class="cor" style="background:#dc2626"></span>Saída</div>
          </div>
        </div>
      </div>

      <div class="grid-2">
        <div class="analise-card">
          <div class="analise-titulo">Somatório por Em que? (apenas repetidos no mês)</div>
          <div class="tabela-wrap">
            <table class="mini-tabela">
              <thead>
                <tr><th>EM QUE?</th><th>VALOR GASTO NO MÊS</th></tr>
              </thead>
              <tbody id="analiseEmQue"></tbody>
            </table>
          </div>
        </div>
        <div class="analise-card">
          <div class="analise-titulo">Somatório por Local / Quem (apenas repetidos no mês)</div>
          <div class="tabela-wrap">
            <table class="mini-tabela">
              <thead>
                <tr><th>LOCAL / QUEM</th><th>VALOR GASTO NO MÊS</th></tr>
              </thead>
              <tbody id="analiseLocalQuem"></tbody>
            </table>
          </div>
        </div>
      </div>

      <div class="report" id="relatorioTexto">Nenhum relatório gerado ainda.</div>
    </div>

    <div class="card">
      <h2>Gerenciamento de Dados</h2>
      <div class="acoes">
        <button class="btn-danger" onclick="limparMesSelecionado()">LIMPAR MÊS</button>
        <button class="btn-danger" onclick="limparTodosDados()">LIMPAR TUDO</button>
        <button class="btn-primary" onclick="exportarBackup()">EXPORTAR BACKUP</button>
        <button class="btn-secondary" onclick="document.getElementById('importarBackupInput').click()">IMPORTAR BACKUP</button>
        <input type="file" id="importarBackupInput" style="display:none" accept=".json" onchange="importarBackup(event)" />
      </div>
      <p class="muted">Limpar mês remove apenas os dados inseridos no mês selecionado. Limpar tudo remove todos os dados lançados. Backup salva e restaura as informações.</p>
    </div>
  </div>

  <script>
    let lancamentos = JSON.parse(localStorage.getItem('controleFinanceiroLancamentos') || '[]');
    let despesasFixas = JSON.parse(localStorage.getItem('controleFinanceiroDespesasFixas') || '[]');
    let proximoLancamentoEhEntrada = false;

    function formatarMoeda(valor) {
      return Number(valor || 0).toLocaleString('pt-BR', {
        style: 'currency',
        currency: 'BRL'
      });
    }

    function formatarDataBR(dataISO) {
      const partes = String(dataISO || '').split('-');
      if (partes.length !== 3) return '-';
      return partes[2] + '/' + partes[1] + '/' + partes[0];
    }

    function normalizarTexto(texto) {
      return String(texto || '').trim().toLowerCase();
    }

    function obterMesSelecionado() {
      return parseInt(document.getElementById('mesFiltro').value, 10);
    }

    function obterAnoSelecionado() {
      return parseInt(document.getElementById('anoFiltro').value, 10);
    }

    function obterAnoAtual() {
      return new Date().getFullYear();
    }

    function obterDiaAjustadoParaMesAno(dia, mes, ano) {
      const ultimoDia = new Date(ano, mes, 0).getDate();
      return Math.min(dia, ultimoDia);
    }

    function obterDataHojeISO() {
      const hoje = new Date();
      const ano = hoje.getFullYear();
      const mes = String(hoje.getMonth() + 1).padStart(2, '0');
      const dia = String(hoje.getDate()).padStart(2, '0');
      return ano + '-' + mes + '-' + dia;
    }

    function salvarDados() {
      localStorage.setItem('controleFinanceiroLancamentos', JSON.stringify(lancamentos));
      localStorage.setItem('controleFinanceiroDespesasFixas', JSON.stringify(despesasFixas));
    }

    function atualizarModoVisual() {
      const box = document.getElementById('modoAtual');
      const botao = document.getElementById('botaoModo');

      if (proximoLancamentoEhEntrada) {
        box.className = 'modo-box modo-entrada';
        box.textContent = 'Modo atual: ENTRADA';
        if (botao) {
          botao.textContent = 'SAÍDA';
          botao.className = 'btn-danger';
        }
      } else {
        box.className = 'modo-box modo-saida';
        box.textContent = 'Modo atual: SAÍDA';
        if (botao) {
          botao.textContent = 'ENTRADA';
          botao.className = 'btn-success';
        }
      }
    }

    function alternarModoLancamento() {
      proximoLancamentoEhEntrada = !proximoLancamentoEhEntrada;
      atualizarModoVisual();
    }

    function ajustarDataFormularioParaMesSelecionado() {
      const ano = obterAnoSelecionado();
      const mes = obterMesSelecionado();
      const inputData = document.getElementById('data');

      let dia = 1;
      if (inputData.value) {
        const partesExistentes = inputData.value.split('-');
        if (partesExistentes.length === 3) {
          dia = parseInt(partesExistentes[2], 10) || 1;
        }
      } else {
        dia = new Date().getDate();
      }

      dia = obterDiaAjustadoParaMesAno(dia, mes, ano);

      inputData.value =
        ano + '-' +
        String(mes).padStart(2, '0') + '-' +
        String(dia).padStart(2, '0');
    }

    function sincronizarDataComMesEAnoSelecionados() {
      const inputData = document.getElementById('data');
      inputData.addEventListener('change', function() {
        if (!this.value) return;

        const partes = this.value.split('-');
        if (partes.length !== 3) return;

        const ano = obterAnoSelecionado();
        const mes = obterMesSelecionado();
        let dia = parseInt(partes[2], 10) || 1;
        dia = obterDiaAjustadoParaMesAno(dia, mes, ano);

        this.value =
          ano + '-' +
          String(mes).padStart(2, '0') + '-' +
          String(dia).padStart(2, '0');
      });
    }

    function limparFormulario() {
      document.getElementById('valor').value = '';
      document.getElementById('emQue').value = '';
      document.getElementById('localQuem').value = '';
      document.getElementById('tipoPagamento').value = 'PIX';
      document.getElementById('necessario').value = '';
      proximoLancamentoEhEntrada = false;
      atualizarModoVisual();
      ajustarDataFormularioParaMesSelecionado();
    }

    function limparFormularioFixa() {
      document.getElementById('fixaNome').value = '';
      document.getElementById('fixaVencimento').value = '';
      document.getElementById('fixaValor').value = '';
    }

    function atualizarDatalists() {
      const emQueSet = new Map();
      const localSet = new Map();

      lancamentos.forEach(function(item) {
        if (item.emQue) emQueSet.set(normalizarTexto(item.emQue), item.emQue);
        if (item.localQuem) localSet.set(normalizarTexto(item.localQuem), item.localQuem);
      });

      despesasFixas.forEach(function(item) {
        if (item.nome) emQueSet.set(normalizarTexto(item.nome), item.nome);
      });

      document.getElementById('listaEmQue').innerHTML = Array.from(emQueSet.values())
        .sort()
        .map(function(v) { return '<option value="' + v + '"></option>'; })
        .join('');

      document.getElementById('listaLocalQuem').innerHTML = Array.from(localSet.values())
        .sort()
        .map(function(v) { return '<option value="' + v + '"></option>'; })
        .join('');
    }

    function lancamentosDoMesSelecionado() {
      const mes = obterMesSelecionado();
      const ano = obterAnoSelecionado();

      return lancamentos.filter(function(item) {
        const data = new Date(item.data + 'T00:00:00');
        return data.getMonth() + 1 === mes && data.getFullYear() === ano;
      });
    }

    function despesasFixasDoMesSelecionado() {
      const mes = obterMesSelecionado();
      const ano = obterAnoSelecionado();

      return despesasFixas.filter(function(item) {
        if (!item.vencimento) return false;
        const data = new Date(item.vencimento + 'T00:00:00');
        return data.getMonth() + 1 === mes && data.getFullYear() === ano;
      });
    }

    function diferencaDias(dataInicial, dataFinal) {
      const d1 = new Date(dataInicial + 'T00:00:00');
      const d2 = new Date(dataFinal + 'T00:00:00');
      return Math.round((d2 - d1) / 86400000);
    }

    function calcularSituacaoPagamento(vencimento, pagamento) {
      if (pagamento) {
        const dias = diferencaDias(vencimento, pagamento);
        if (dias === 0) return 'No vencimento (0 dias)';
        if (dias > 0) return 'Atrasado ' + dias + ' dia(s)';
        return 'Adiantado ' + Math.abs(dias) + ' dia(s)';
      }

      const diasParaVencer = diferencaDias(obterDataHojeISO(), vencimento);
      if (diasParaVencer < 0) return 'Atrasado ' + Math.abs(diasParaVencer) + ' dia(s)';
      if (diasParaVencer === 0) return 'Vence hoje (0 dias)';
      return 'Falta(m) ' + diasParaVencer + ' dia(s)';
    }

    function atualizarStatusFixaAutomatico(emQue, valor, dataPagamento) {
      if (!(valor < 0)) return;

      const nomeBusca = normalizarTexto(emQue);
      despesasFixasDoMesSelecionado().forEach(function(item) {
        if (normalizarTexto(item.nome) === nomeBusca && item.status !== 'PAGO') {
          item.status = 'PAGO';
          item.valorPago = Math.abs(valor);
          item.dataPagamento = dataPagamento;
          item.situacaoPagamento = calcularSituacaoPagamento(item.vencimento, dataPagamento);
        }
      });
    }

    function interpretarPrimeiraDataCartao(texto) {
      const valor = String(texto || '').trim();

      if (/^\d{4}-\d{2}-\d{2}$/.test(valor)) return valor;

      if (/^\d{1,2}\/\d{1,2}$/.test(valor)) {
        const partes = valor.split('/');
        const dia = parseInt(partes[0], 10);
        const mes = parseInt(partes[1], 10);
        const ano = obterAnoSelecionado();

        if (Number.isNaN(dia) || Number.isNaN(mes) || dia <= 0 || mes <= 0 || mes > 12) return '';

        const ultimoDia = new Date(ano, mes, 0).getDate();
        if (dia > ultimoDia) return '';

        return ano + '-' + String(mes).padStart(2, '0') + '-' + String(dia).padStart(2, '0');
      }

      return '';
    }

    function gerarParcelasCredito(dataCompra, emQue, valor, localQuem, necessario, totalParcelas) {
      const total = parseInt(totalParcelas, 10);
      if (Number.isNaN(total) || total <= 0) return '1/1';

      const valorParcela = Number((Math.abs(valor) / total).toFixed(2));
      const dataBase = new Date(dataCompra + 'T00:00:00');

      for (let i = 1; i <= total; i += 1) {
        const venc = new Date(dataBase.getFullYear(), dataBase.getMonth() + (i - 1), dataBase.getDate());
        const vencimento =
          venc.getFullYear() + '-' +
          String(venc.getMonth() + 1).padStart(2, '0') + '-' +
          String(venc.getDate()).padStart(2, '0');

        despesasFixas.push({
          nome: emQue,
          vencimento: vencimento,
          valor: valorParcela,
          status: i === 1 ? 'PAGO' : 'NÃO PAGO',
          valorPago: i === 1 ? valorParcela : 0,
          dataPagamento: i === 1 ? dataCompra : '',
          situacaoPagamento: i === 1 ? calcularSituacaoPagamento(vencimento, dataCompra) : '-',
          origem: 'CRÉDITO',
          detalhe: i + '/' + total,
          localQuemOrigem: localQuem,
          necessarioOrigem: necessario
        });
      }

      return '1/' + total;
    }

    function gerarParcelasCartao(dataCompra, emQue, valor, localQuem, necessario, totalParcelas, primeiraDataVencimento) {
      const total = parseInt(totalParcelas, 10);
      if (Number.isNaN(total) || total <= 0) return;

      const primeiraData = interpretarPrimeiraDataCartao(primeiraDataVencimento);
      if (!primeiraData) return;

      const valorParcela = Number((Math.abs(valor) / total).toFixed(2));
      const primeira = new Date(primeiraData + 'T00:00:00');
      const diaBase = primeira.getDate();

      for (let i = 1; i <= total; i += 1) {
        const base = new Date(primeira.getFullYear(), primeira.getMonth() + (i - 1), 1);
        const ultimoDia = new Date(base.getFullYear(), base.getMonth() + 1, 0).getDate();
        const diaFinal = Math.min(diaBase, ultimoDia);
        const venc = new Date(base.getFullYear(), base.getMonth(), diaFinal);
        const vencimento =
          venc.getFullYear() + '-' +
          String(venc.getMonth() + 1).padStart(2, '0') + '-' +
          String(venc.getDate()).padStart(2, '0');

        despesasFixas.push({
          nome: emQue,
          vencimento: vencimento,
          valor: valorParcela,
          status: 'NÃO PAGO',
          valorPago: 0,
          dataPagamento: '',
          situacaoPagamento: '-',
          origem: 'CARTÃO',
          detalhe: i + '/' + total,
          localQuemOrigem: localQuem,
          necessarioOrigem: necessario
        });
      }
    }

    function adicionarLancamento() {
      const data = document.getElementById('data').value;
      let valor = parseFloat(document.getElementById('valor').value);
      const emQue = document.getElementById('emQue').value.trim();
      const localQuem = document.getElementById('localQuem').value.trim();
      const tipoPagamento = document.getElementById('tipoPagamento').value;
      const necessario = document.getElementById('necessario').value;

      if (!data || Number.isNaN(valor) || !emQue || !localQuem || !necessario) {
        alert('Preencha data, valor, em que, local/quem e necessário.');
        document.getElementById('necessario').value = '';
        return;
      }

      const partesData = data.split('-');
      let dataCorrigida = data;
      if (partesData.length === 3) {
        const anoSelecionado = obterAnoSelecionado();
        const mesSelecionado = obterMesSelecionado();
        let diaSelecionado = parseInt(partesData[2], 10) || 1;
        diaSelecionado = obterDiaAjustadoParaMesAno(diaSelecionado, mesSelecionado, anoSelecionado);

        dataCorrigida =
          anoSelecionado + '-' +
          String(mesSelecionado).padStart(2, '0') + '-' +
          String(diaSelecionado).padStart(2, '0');
      }

      valor = Math.abs(valor);

      let tipoMovimento = 'saida';
      if (proximoLancamentoEhEntrada) {
        tipoMovimento = 'entrada';
      } else {
        valor = -valor;
      }

      let parcela = '';

      if (tipoPagamento === 'Crédito') {
        const qtdTexto = prompt('Em quantas vezes? Digite apenas a quantidade. Ex.: 1, 2, 7, 12');
        if (qtdTexto === null) {
          document.getElementById('necessario').value = '';
          return;
        }

        const totalParcelas = parseInt(String(qtdTexto).trim(), 10);
        if (Number.isNaN(totalParcelas) || totalParcelas <= 0) {
          alert('Digite uma quantidade válida de parcelas.');
          document.getElementById('necessario').value = '';
          return;
        }

        parcela = gerarParcelasCredito(dataCorrigida, emQue, valor, localQuem, necessario, totalParcelas);

        lancamentos.push({
          data: dataCorrigida,
          valor: valor,
          emQue: emQue,
          localQuem: localQuem,
          tipoPagamento: tipoPagamento,
          parcela: parcela,
          necessario: necessario,
          tipoMovimento: tipoMovimento
        });

        atualizarStatusFixaAutomatico(emQue, valor, dataCorrigida);
      } else if (tipoPagamento === 'Cartão') {
        const qtdTexto = prompt('Em quantas parcelas? Digite apenas a quantidade. Ex.: 1, 2, 7, 12');
        if (qtdTexto === null) {
          document.getElementById('necessario').value = '';
          return;
        }

        const totalParcelas = parseInt(String(qtdTexto).trim(), 10);
        if (Number.isNaN(totalParcelas) || totalParcelas <= 0) {
          alert('Digite uma quantidade válida de parcelas.');
          document.getElementById('necessario').value = '';
          return;
        }

        const dataTexto = prompt('Informe a data do 1/' + totalParcelas + '.\nUse DD/MM para o ano atual. Ex.: 10/05\nSe for outro formato, use AAAA-MM-DD.');
        if (dataTexto === null) {
          document.getElementById('necessario').value = '';
          return;
        }

        const primeiraData = interpretarPrimeiraDataCartao(dataTexto);
        if (!primeiraData) {
          alert('Digite a data como DD/MM para o ano atual ou AAAA-MM-DD.');
          document.getElementById('necessario').value = '';
          return;
        }

        gerarParcelasCartao(dataCorrigida, emQue, valor, localQuem, necessario, totalParcelas, primeiraData);
      } else {
        lancamentos.push({
          data: dataCorrigida,
          valor: valor,
          emQue: emQue,
          localQuem: localQuem,
          tipoPagamento: tipoPagamento,
          parcela: parcela,
          necessario: necessario,
          tipoMovimento: tipoMovimento
        });

        atualizarStatusFixaAutomatico(emQue, valor, dataCorrigida);
      }

      lancamentos.sort(function(a, b) {
        return new Date(a.data) - new Date(b.data);
      });

      despesasFixas.sort(function(a, b) {
        return new Date(a.vencimento) - new Date(b.vencimento);
      });

      salvarDados();
      atualizarDatalists();
      limparFormulario();
      renderTabela();
      renderTabelaFaturaCartao();
      renderTabelaFixas();
      gerarRelatorio();
    }

    function adicionarAoSelecionarNecessario() {
      if (!document.getElementById('necessario').value) return;
      adicionarLancamento();
    }

    function limparMesSelecionado() {
      if (!confirm('Deseja apagar somente os dados inseridos no mês selecionado?')) return;

      const mes = obterMesSelecionado();
      const ano = obterAnoSelecionado();

      lancamentos = lancamentos.filter(function(item) {
        const data = new Date(item.data + 'T00:00:00');
        return !(data.getMonth() + 1 === mes && data.getFullYear() === ano);
      });

      despesasFixas = despesasFixas.filter(function(item) {
        if (!item.vencimento) return true;
        const data = new Date(item.vencimento + 'T00:00:00');
        return !(data.getMonth() + 1 === mes && data.getFullYear() === ano);
      });

      salvarDados();
      atualizarDatalists();
      renderTabela();
      renderTabelaFaturaCartao();
      renderTabelaFixas();
      gerarRelatorio();
      limparFormulario();
      alert('Dados do mês selecionado foram apagados.');
    }

    function limparTodosDados() {
      if (!confirm('ATENÇÃO: isso apagará todos os dados inseridos de todos os meses. Deseja continuar?')) return;

      lancamentos = [];
      despesasFixas = [];

      salvarDados();
      atualizarDatalists();
      renderTabela();
      renderTabelaFaturaCartao();
      renderTabelaFixas();
      gerarRelatorio();
      limparFormulario();
      alert('Todos os dados inseridos foram apagados.');
    }

    function exportarBackup() {
      const dados = {
        lancamentos: lancamentos,
        despesasFixas: despesasFixas
      };

      const blob = new Blob([JSON.stringify(dados, null, 2)], { type: 'application/json' });
      const url = URL.createObjectURL(blob);
      const a = document.createElement('a');
      a.href = url;
      a.download = 'backup_controle_financeiro.json';
      a.click();
      URL.revokeObjectURL(url);
    }

    function importarBackup(event) {
      const file = event.target.files[0];
      if (!file) return;

      const reader = new FileReader();
      reader.onload = function(e) {
        try {
          const dados = JSON.parse(e.target.result);
          lancamentos = Array.isArray(dados.lancamentos) ? dados.lancamentos : [];
          despesasFixas = Array.isArray(dados.despesasFixas) ? dados.despesasFixas : [];

          salvarDados();
          atualizarDatalists();
          renderTabela();
          renderTabelaFaturaCartao();
          renderTabelaFixas();
          gerarRelatorio();
          alert('Backup restaurado com sucesso.');
        } catch (erro) {
          alert('Erro ao importar backup.');
        }

        event.target.value = '';
      };

      reader.readAsText(file);
    }

    function adicionarDespesaFixa() {
      const nome = document.getElementById('fixaNome').value.trim();
      const vencimento = document.getElementById('fixaVencimento').value;
      const valor = parseFloat(document.getElementById('fixaValor').value);

      if (!nome || !vencimento || Number.isNaN(valor) || valor <= 0) {
        alert('Preencha nome, vencimento e valor corretamente.');
        return;
      }

      despesasFixas.push({
        nome: nome,
        vencimento: vencimento,
        valor: valor,
        status: 'NÃO PAGO',
        valorPago: 0,
        dataPagamento: '',
        situacaoPagamento: '-',
        origem: 'FIXA',
        detalhe: ''
      });

      despesasFixas.sort(function(a, b) {
        return new Date(a.vencimento) - new Date(b.vencimento);
      });

      salvarDados();
      atualizarDatalists();
      limparFormularioFixa();
      renderTabelaFixas();
      renderTabelaFaturaCartao();
      gerarRelatorio();
    }

    function excluirLancamento(indexFiltrado) {
      const item = lancamentosDoMesSelecionado()[indexFiltrado];
      if (!item) return;
      if (!confirm('Deseja excluir este lançamento?')) return;

      const indexReal = lancamentos.findIndex(function(x) { return x === item; });
      if (indexReal >= 0) lancamentos.splice(indexReal, 1);

      salvarDados();
      atualizarDatalists();
      renderTabela();
      renderTabelaFaturaCartao();
      gerarRelatorio();
    }

    function excluirDespesaFixa(indexFiltrado) {
      const item = despesasFixasDoMesSelecionado()[indexFiltrado];
      if (!item) return;
      if (!confirm('Deseja excluir esta despesa fixa?')) return;

      const indexReal = despesasFixas.findIndex(function(x) { return x === item; });
      if (indexReal >= 0) despesasFixas.splice(indexReal, 1);

      salvarDados();
      atualizarDatalists();
      renderTabelaFixas();
      renderTabelaFaturaCartao();
      gerarRelatorio();
    }

    function marcarDespesaFixaPaga(indexFiltrado) {
      const item = despesasFixasDoMesSelecionado()[indexFiltrado];
      if (!item) return;

      item.status = 'PAGO';
      item.valorPago = item.valor;
      item.dataPagamento = obterDataHojeISO();
      item.situacaoPagamento = calcularSituacaoPagamento(item.vencimento, item.dataPagamento);

      salvarDados();
      renderTabelaFixas();
      renderTabelaFaturaCartao();
      gerarRelatorio();
    }

    function desmarcarDespesaFixaPaga(indexFiltrado) {
      const item = despesasFixasDoMesSelecionado()[indexFiltrado];
      if (!item) return;

      item.status = 'NÃO PAGO';
      item.valorPago = 0;
      item.dataPagamento = '';
      item.situacaoPagamento = '-';

      salvarDados();
      renderTabelaFixas();
      renderTabelaFaturaCartao();
      gerarRelatorio();
    }

    function renderTabelaFaturaCartao() {
      const tbody = document.getElementById('tabelaFaturaCartao');
      tbody.innerHTML = '';

      const dados = despesasFixasDoMesSelecionado().filter(function(x) {
        return x.origem === 'CARTÃO';
      });

      if (dados.length === 0) {
        tbody.innerHTML = '<tr><td colspan="3">Nenhuma fatura de cartão neste mês.</td></tr>';
        return;
      }

      const grupos = {};

      dados.forEach(function(item) {
        if (!grupos[item.vencimento]) {
          grupos[item.vencimento] = {
            total: 0,
            itens: []
          };
        }

        grupos[item.vencimento].total += Number(item.valor || 0);

        const descricao = item.nome +
          (item.detalhe ? ' (' + item.detalhe + ')' : '') +
          ' - ' + formatarMoeda(item.valor);

        grupos[item.vencimento].itens.push(descricao);
      });

      Object.keys(grupos).sort().forEach(function(vencimento) {
        const tr = document.createElement('tr');
        tr.innerHTML =
          '<td>' + formatarDataBR(vencimento) + '</td>' +
          '<td style="text-align:left">' + grupos[vencimento].itens.join('<br>') + '</td>' +
          '<td class="valor-negativo">' + formatarMoeda(grupos[vencimento].total) + '</td>';
        tbody.appendChild(tr);
      });
    }

    function renderTabelaFixas() {
      const tbody = document.getElementById('tabelaFixas');
      tbody.innerHTML = '';

      const itens = despesasFixasDoMesSelecionado().slice();

      itens.sort(function(a, b) {
        const hoje = obterDataHojeISO();
        const aPago = a.status === 'PAGO';
        const bPago = b.status === 'PAGO';
        const aDias = diferencaDias(hoje, a.vencimento);
        const bDias = diferencaDias(hoje, b.vencimento);
        const aAtrasado = !aPago && aDias < 0;
        const bAtrasado = !bPago && bDias < 0;

        if (aPago !== bPago) return aPago ? 1 : -1;
        if (aAtrasado !== bAtrasado) return aAtrasado ? 1 : -1;
        if (!aAtrasado && !bAtrasado) return aDias - bDias;
        if (aAtrasado && bAtrasado) return Math.abs(aDias) - Math.abs(bDias);
        return new Date(a.vencimento) - new Date(b.vencimento);
      });

      if (itens.length === 0) {
        tbody.innerHTML = '<tr><td colspan="7">Nenhuma despesa fixa no mês selecionado.</td></tr>';
        return;
      }

      itens.forEach(function(item, index) {
        const pago = item.status === 'PAGO';
        const hoje = new Date();
        const venc = new Date(item.vencimento + 'T00:00:00');
        const tr = document.createElement('tr');

        if (pago) {
          tr.style.backgroundColor = '#3b82f6';
          tr.style.color = '#ffffff';
        } else if (hoje > venc) {
          tr.style.backgroundColor = '#dc2626';
          tr.style.color = '#ffffff';
        }

        const situacaoExibida = pago
          ? (item.situacaoPagamento || '-')
          : calcularSituacaoPagamento(item.vencimento, '');

        const botao = pago
          ? '<button class="btn-secondary" onclick="desmarcarDespesaFixaPaga(' + index + ')">Marcar não pago</button>'
          : '<button class="btn-success" onclick="marcarDespesaFixaPaga(' + index + ')">Marcar pago</button>';

        const nome = item.nome + (item.detalhe ? ' (' + item.detalhe + ')' : '');

        tr.innerHTML =
          '<td>' + nome + '</td>' +
          '<td>' + formatarDataBR(item.vencimento) + '</td>' +
          '<td>' + formatarMoeda(item.valor) + '</td>' +
          '<td>' + item.status + '</td>' +
          '<td>' + (item.dataPagamento ? formatarDataBR(item.dataPagamento) : '-') + '</td>' +
          '<td>' + situacaoExibida + '</td>' +
          '<td><div class="acoes-inline">' + botao + '<button class="btn-danger" onclick="excluirDespesaFixa(' + index + ')">Excluir</button></div></td>';

        tbody.appendChild(tr);
      });
    }

    function renderTabela() {
      const tbody = document.getElementById('tabelaLancamentos');
      tbody.innerHTML = '';

      const itens = lancamentosDoMesSelecionado();

      if (itens.length === 0) {
        tbody.innerHTML = '<tr><td colspan="8">Nenhum lançamento no mês selecionado.</td></tr>';
        return;
      }

      itens.forEach(function(item, index) {
        const tr = document.createElement('tr');
        const classe = item.valor >= 0 ? 'valor-positivo' : 'valor-negativo';

        tr.innerHTML =
          '<td>' + formatarDataBR(item.data) + '</td>' +
          '<td class="' + classe + '">' + formatarMoeda(item.valor) + '</td>' +
          '<td>' + item.emQue + '</td>' +
          '<td>' + item.localQuem + '</td>' +
          '<td>' + item.tipoPagamento + '</td>' +
          '<td>' + (item.parcela || '') + '</td>' +
          '<td>' + item.necessario + '</td>' +
          '<td><button class="btn-danger" onclick="excluirLancamento(' + index + ')">Excluir</button></td>';

        tbody.appendChild(tr);
      });
    }

    function preencherTabelaAnalise(elementId, dados) {
      const tbody = document.getElementById(elementId);
      tbody.innerHTML = '';

      if (dados.length === 0) {
        tbody.innerHTML = '<tr><td colspan="2">Nenhum item repetido no mês.</td></tr>';
        return;
      }

      dados.forEach(function(item) {
        const tr = document.createElement('tr');
        tr.innerHTML =
          '<td>' + item.nome + '</td>' +
          '<td class="valor-negativo">' + formatarMoeda(item.total) + '</td>';
        tbody.appendChild(tr);
      });
    }

    function montarAnaliseAgrupada(itens, campo) {
      const mapa = new Map();

      itens.forEach(function(item) {
        if (item.valor >= 0) return;

        const chave = normalizarTexto(item[campo]);
        if (!chave) return;

        if (!mapa.has(chave)) {
          mapa.set(chave, {
            nome: item[campo].trim(),
            total: 0,
            qtd: 0
          });
        }

        const registro = mapa.get(chave);
        registro.total += Math.abs(item.valor);
        registro.qtd += 1;
      });

      return Array.from(mapa.values())
        .filter(function(item) { return item.qtd >= 2; })
        .sort(function(a, b) { return b.total - a.total; });
    }

    function polarToCartesian(cx, cy, r, angle) {
      const rad = (angle - 90) * Math.PI / 180;
      return {
        x: cx + r * Math.cos(rad),
        y: cy + r * Math.sin(rad)
      };
    }

    function describeArc(cx, cy, r, startAngle, endAngle) {
      const start = polarToCartesian(cx, cy, r, endAngle);
      const end = polarToCartesian(cx, cy, r, startAngle);
      const largeArcFlag = endAngle - startAngle <= 180 ? '0' : '1';
      return ['M', cx, cy, 'L', start.x, start.y, 'A', r, r, 0, largeArcFlag, 0, end.x, end.y, 'Z'].join(' ');
    }

    function desenharPizza(entrada, saida, saldo) {
      const svg = document.getElementById('pizzaChart');
      svg.innerHTML = '';

      const dados = [];
      if (entrada > 0) dados.push({ valor: entrada, cor: '#2563eb' });
      if (saida > 0) dados.push({ valor: saida, cor: '#dc2626' });
      if (saldo !== 0) dados.push({ valor: Math.abs(saldo), cor: saldo > 0 ? '#facc15' : '#7f1d1d' });

      if (dados.length === 0) {
        svg.innerHTML = '<text x="210" y="160" text-anchor="middle" font-size="18" fill="#64748b">Sem dados para o gráfico</text>';
        return;
      }

      const total = dados.reduce(function(soma, item) { return soma + item.valor; }, 0);
      let anguloAtual = 0;
      const cx = 140;
      const cy = 160;
      const r = 90;

      dados.forEach(function(item) {
        const angulo = (item.valor / total) * 360;

        const path = document.createElementNS('http://www.w3.org/2000/svg', 'path');
        path.setAttribute('d', describeArc(cx, cy, r, anguloAtual, anguloAtual + angulo));
        path.setAttribute('fill', item.cor);
        path.setAttribute('stroke', '#fff');
        path.setAttribute('stroke-width', '2');
        svg.appendChild(path);

        const meio = anguloAtual + angulo / 2;
        const labelPoint = polarToCartesian(cx, cy, r + 26, meio);
        const percent = ((item.valor / total) * 100).toFixed(1).replace('.', ',');

        const text = document.createElementNS('http://www.w3.org/2000/svg', 'text');
        text.setAttribute('x', labelPoint.x);
        text.setAttribute('y', labelPoint.y);
        text.setAttribute('font-size', '13');
        text.setAttribute('text-anchor', labelPoint.x < cx ? 'end' : 'start');
        text.setAttribute('fill', '#111827');
        text.textContent = percent + '%';
        svg.appendChild(text);

        anguloAtual += angulo;
      });
    }

    function desenharColunas(dadosPorDia) {
      const svg = document.getElementById('colunaChart');
      svg.innerHTML = '';

      const entries = Object.entries(dadosPorDia);

      if (entries.length === 0) {
        svg.innerHTML = '<text x="380" y="170" text-anchor="middle" font-size="18" fill="#64748b">Sem dados para o gráfico</text>';
        return;
      }

      const width = 760;
      const height = 340;
      const margin = { top: 30, right: 20, bottom: 55, left: 60 };
      const chartWidth = width - margin.left - margin.right;
      const chartHeight = height - margin.top - margin.bottom;

      const maxValue = Math.max.apply(null, entries.map(function(x) {
        return Math.max(x[1].entrada || 0, x[1].saida || 0, (x[1].entrada || 0) + (x[1].saida || 0));
      }).concat([1]));
      const barWidth = Math.max(18, chartWidth / entries.length * 0.65);
      const gap = chartWidth / entries.length;

      const eixo = document.createElementNS('http://www.w3.org/2000/svg', 'line');
      eixo.setAttribute('x1', margin.left);
      eixo.setAttribute('y1', margin.top + chartHeight);
      eixo.setAttribute('x2', width - margin.right);
      eixo.setAttribute('y2', margin.top + chartHeight);
      eixo.setAttribute('stroke', '#94a3b8');
      eixo.setAttribute('stroke-width', '1');
      svg.appendChild(eixo);

      entries.forEach(function(entry, index) {
        const dia = entry[0];
        const entrada = entry[1].entrada || 0;
        const saida = entry[1].saida || 0;
        const totalDia = entrada + saida;
        const x = margin.left + gap * index + (gap - barWidth) / 2;

        let alturaSaida = 0;
        let alturaEntrada = 0;

        if (totalDia > 0) {
          alturaSaida = (saida / maxValue) * chartHeight;
          alturaEntrada = (entrada / maxValue) * chartHeight;
        }

        const ySaida = margin.top + chartHeight - alturaSaida;
        const yEntrada = ySaida - alturaEntrada;

        if (saida > 0) {
          const rectSaida = document.createElementNS('http://www.w3.org/2000/svg', 'rect');
          rectSaida.setAttribute('x', x);
          rectSaida.setAttribute('y', ySaida);
          rectSaida.setAttribute('width', barWidth);
          rectSaida.setAttribute('height', alturaSaida);
          rectSaida.setAttribute('rx', '4');
          rectSaida.setAttribute('fill', '#dc2626');
          svg.appendChild(rectSaida);
        }

        if (entrada > 0) {
          const rectEntrada = document.createElementNS('http://www.w3.org/2000/svg', 'rect');
          rectEntrada.setAttribute('x', x);
          rectEntrada.setAttribute('y', yEntrada);
          rectEntrada.setAttribute('width', barWidth);
          rectEntrada.setAttribute('height', alturaEntrada);
          rectEntrada.setAttribute('rx', '4');
          rectEntrada.setAttribute('fill', '#2563eb');
          svg.appendChild(rectEntrada);
        }

        const valorTexto = document.createElementNS('http://www.w3.org/2000/svg', 'text');
        valorTexto.setAttribute('x', x + barWidth / 2);
        valorTexto.setAttribute('y', Math.max(yEntrada, margin.top) - 8);
        valorTexto.setAttribute('text-anchor', 'middle');
        valorTexto.setAttribute('font-size', '12');
        valorTexto.setAttribute('fill', '#111827');
        valorTexto.textContent = 'E: ' + formatarMoeda(entrada) + ' | S: ' + formatarMoeda(saida);
        svg.appendChild(valorTexto);

        const label = document.createElementNS('http://www.w3.org/2000/svg', 'text');
        label.setAttribute('x', x + barWidth / 2);
        label.setAttribute('y', height - 18);
        label.setAttribute('text-anchor', 'middle');
        label.setAttribute('font-size', '11');
        label.setAttribute('fill', '#475569');
        label.textContent = dia;
        svg.appendChild(label);
      });
    }

    function gerarRelatorio() {
      const meses = [
        'Janeiro','Fevereiro','Março','Abril','Maio','Junho',
        'Julho','Agosto','Setembro','Outubro','Novembro','Dezembro'
      ];

      const filtrados = lancamentosDoMesSelecionado();
      const fixasFiltradas = despesasFixasDoMesSelecionado();

      let entrada = 0;
      let saida = 0;
      let gastoSim = 0;
      let gastoNao = 0;
      const gastosPorDia = {};

      filtrados.forEach(function(item) {
        const dia = formatarDataBR(item.data).slice(0, 5);
        if (!gastosPorDia[dia]) {
          gastosPorDia[dia] = { entrada: 0, saida: 0 };
        }

        if (item.valor >= 0) {
          entrada += item.valor;
          gastosPorDia[dia].entrada += item.valor;
        } else {
          const gasto = Math.abs(item.valor);
          saida += gasto;
          gastosPorDia[dia].saida += gasto;

          if (item.necessario === 'SIM') {
            gastoSim += gasto;
          } else {
            gastoNao += gasto;
          }
        }
      });

      let totalFixasPagas = 0;
      let totalFixasNaoPagas = 0;

      fixasFiltradas.forEach(function(item) {
        if (item.status === 'PAGO') {
          totalFixasPagas += Number(item.valor || 0);
        } else {
          totalFixasNaoPagas += Number(item.valor || 0);
        }
      });

      const saldo = entrada - saida;
      const saldoLivre = saldo - totalFixasNaoPagas;

      let percentualNaoEssencial = 0;
      if (saida > 0) {
        percentualNaoEssencial = (gastoNao / saida) * 100;
      }

      let statusAnalise = '✅ Controle saudável';
      if (percentualNaoEssencial >= 40) {
        statusAnalise = '⚠️ Alto gasto não essencial';
      } else if (percentualNaoEssencial >= 25) {
        statusAnalise = '🔎 Gasto moderado';
      }

      document.getElementById('resumoMes').innerHTML =
        '<div class="box" style="border-left-color:#2563eb"><strong>Entradas</strong><div class="valor">' + formatarMoeda(entrada) + '</div></div>' +
        '<div class="box" style="border-left-color:#dc2626"><strong>Saídas</strong><div class="valor">' + formatarMoeda(saida) + '</div></div>' +
        '<div class="box" style="border-left-color:' + (saldo >= 0 ? '#facc15' : '#7f1d1d') + '"><strong>Saldo do mês</strong><div class="valor">' + formatarMoeda(saldo) + '</div></div>' +
        '<div class="box" style="border-left-color:#0f766e"><strong>Gastos essenciais</strong><div class="valor">' + formatarMoeda(gastoSim) + '</div></div>' +
        '<div class="box" style="border-left-color:#7c3aed"><strong>Gastos não essenciais</strong><div class="valor">' + formatarMoeda(gastoNao) + '</div></div>' +
        '<div class="box" style="border-left-color:' + (saldoLivre >= 0 ? '#0891b2' : '#b91c1c') + '"><strong>Saldo realmente livre</strong><div class="valor">' + formatarMoeda(saldoLivre) + '</div></div>' +
        '<div class="box" style="border-left-color:#f97316"><strong>Análise do mês</strong><div class="valor" style="font-size:18px">' + statusAnalise + '</div><div style="margin-top:8px;font-size:14px">Não essenciais: ' + percentualNaoEssencial.toFixed(1).replace('.', ',') + '% das saídas</div></div>';

      document.getElementById('fixasPagasBox').textContent = formatarMoeda(totalFixasPagas);
      document.getElementById('fixasNaoPagasBox').textContent = formatarMoeda(totalFixasNaoPagas);

      desenharPizza(entrada, saida, saldo);
      desenharColunas(gastosPorDia);
      preencherTabelaAnalise('analiseEmQue', montarAnaliseAgrupada(filtrados, 'emQue'));
      preencherTabelaAnalise('analiseLocalQuem', montarAnaliseAgrupada(filtrados, 'localQuem'));

      let texto = 'RELATÓRIO FINANCEIRO - ' + meses[obterMesSelecionado() - 1].toUpperCase() + '/' + obterAnoSelecionado() + '\n\n';
      texto += 'Entradas: ' + formatarMoeda(entrada) + '\n';
      texto += 'Saídas: ' + formatarMoeda(saida) + '\n';
      texto += 'Saldo do mês: ' + formatarMoeda(saldo) + '\n';
      texto += 'Saldo realmente livre: ' + formatarMoeda(saldoLivre) + '\n\n';
      texto += 'Gastos essenciais: ' + formatarMoeda(gastoSim) + '\n';
      texto += 'Gastos não essenciais: ' + formatarMoeda(gastoNao) + '\n\n';
      texto += 'CONTAS FIXAS\n';
      texto += 'Valor pago: ' + formatarMoeda(totalFixasPagas) + '\n';
      texto += 'Valor não pago: ' + formatarMoeda(totalFixasNaoPagas) + '\n\n';
      texto += 'ANÁLISE\n';
      texto += statusAnalise + '\n';
      texto += 'Não essenciais: ' + percentualNaoEssencial.toFixed(1).replace('.', ',') + '% das saídas';

      document.getElementById('relatorioTexto').textContent = texto;
    }

    function preencherFiltros() {
      const mesFiltro = document.getElementById('mesFiltro');
      const anoFiltro = document.getElementById('anoFiltro');
      const hoje = new Date();
      const anoAtual = hoje.getFullYear();
      const mesAtual = hoje.getMonth() + 1;

      const meses = [
        'Janeiro','Fevereiro','Março','Abril','Maio','Junho',
        'Julho','Agosto','Setembro','Outubro','Novembro','Dezembro'
      ];

      mesFiltro.innerHTML = meses.map(function(m, i) {
        return '<option value="' + (i + 1) + '">' + m + '</option>';
      }).join('');

      anoFiltro.innerHTML = '';
      for (let ano = anoAtual - 5; ano <= anoAtual + 2; ano += 1) {
        anoFiltro.innerHTML += '<option value="' + ano + '">' + ano + '</option>';
      }

      mesFiltro.value = String(mesAtual);
      anoFiltro.value = String(anoAtual);

      mesFiltro.onchange = trocarMes;
      anoFiltro.onchange = trocarMes;
    }

    function trocarMes() {
      ajustarDataFormularioParaMesSelecionado();
      renderTabela();
      renderTabelaFaturaCartao();
      renderTabelaFixas();
      gerarRelatorio();
    }

    preencherFiltros();
    atualizarDatalists();
    ajustarDataFormularioParaMesSelecionado();
    sincronizarDataComMesEAnoSelecionados();
    atualizarModoVisual();
    renderTabela();
    renderTabelaFaturaCartao();
    renderTabelaFixas();
    gerarRelatorio();
  </script>
</body>
</html>
