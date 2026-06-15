# Ferramenta-de-Risco---Aegis-

# AegisRisk Lite - Gestão de Riscos Cibernéticos

Este repositório contém a versão **Lite do AegisRisk**, uma aplicação web funcional, leve e portátil projetada para o apoio à análise de riscos de TI baseada nas diretrizes de segurança da **ISO/IEC 27005** e nos conceitos de cálculo de score do **CVSS v3.1**.

Este projeto foi concebido sob a premissa de um desenvolvimento prático e objetivo para fins acadêmicos na disciplina de **Segurança Cibernética** do curso de **Análise e Desenvolvimento de Sistemas (ADS)**.

## 🚀 Funcionalidades Ativas

- **Cadastro de Ativos:** Mapeamento de ativos críticos com classificação básica de propriedades CIA (Confidencialidade, Integridade e Disponibilidade).
- **Calculadora Dinâmica de Risco Inerente:** Cálculo instantâneo baseado em fatores clássicos de matrizes operacionais (Risco = Probabilidade x Impacto).
- **Matriz de Calor (Heatmap) 5x5 em Tempo Real:** Distribuição dinâmica das ameaças cadastradas sobre os quadrantes de severidade técnica da matriz.
- **Tratamento de Risco Residual:** Opção de alternar ações de mitigação do risco diretamente no painel, aplicando regras automáticas de amortecimento e reclassificação.
- **Persistência Local (LocalStorage):** Não necessita de infraestrutura de banco de dados externa; os dados persistem no próprio escopo do navegador do usuário.

## 🛠️ Tecnologias Empregadas

- **HTML5** (Estruturação nativa sem dependências de compilação)
- **Tailwind CSS** (Framework de design utilitário responsivo)
- **JavaScript Puro (Vanilla JS)** (Engenharia lógica e manipulação reativa do DOM)
- **Lucide Icons** (Biblioteca de ícones de interface)

## 📁 Como Executar o Projeto

1. Baixe o arquivo `index.html` presente neste repositório.
2. Dê dois cliques sobre o arquivo para abri-lo diretamente no seu navegador padrão.
3. Não há necessidade de executar comandos como `npm install` ou configurar um servidor externo. A aplicação roda nativamente.

---
*Desenvolvido como projeto prático acadêmico para avaliação na matéria de Segurança da Informação.*

código:

<!DOCTYPE html>
<html lang="pt-BR" class="h-full bg-slate-950">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>AegisRisk Lite - Gestão de Riscos e Ativos</title>
    <!-- Tailwind CSS CDN para estilização moderna e rápida -->
    <script src="https://cdn.tailwindcss.com"></script>
    <script>
        tailwind.config = {
            theme: {
                extend: {
                    colors: {
                        cyber: {
                            bg: '#020617',
                            card: '#0f172a',
                            border: '#334155',
                            accent: '#06b6d4',
                            success: '#10b981',
                            warning: '#f59e0b',
                            danger: '#ef4444'
                        }
                    }
                }
            }
        }
    </script>
    <!-- Ícones do Lucide para enriquecer o visual -->
    <script src="https://unpkg.com/lucide@latest"></script>
</head>
<body class="h-full text-slate-100 font-sans antialiased selection:bg-cyan-500 selection:text-slate-900">

    <div class="min-full flex flex-col justify-between">
        <!-- HEADER / TOPO DA FERRAMENTA -->
        <header class="border-b border-slate-800 bg-slate-900/50 backdrop-blur px-6 py-4 sticky top-0 z-50">
            <div class="max-w-7xl mx-auto flex flex-col sm:flex-row justify-between items-center gap-4">
                <div class="flex items-center gap-3">
                    <div class="p-2 bg-cyan-950 border border-cyan-500/30 rounded-lg text-cyan-400">
                        <i data-lucide="shield-alert" class="w-6 h-6"></i>
                    </div>
                    <div>
                        <h1 class="text-xl font-bold tracking-tight text-white flex items-center gap-2">
                            AegisRisk <span class="text-xs bg-cyan-500/10 text-cyan-400 px-2 py-0.5 rounded border border-cyan-500/20 font-mono">LITE v1.0</span>
                        </h1>
                        <p class="text-xs text-slate-400">Análise Prática de Riscos Cibernéticos • ISO/IEC 27005 & CVSS</p>
                    </div>
                </div>
                <div class="text-right sm:text-right text-center">
                    <span class="text-xs bg-slate-800 text-slate-300 px-3 py-1.5 rounded-full border border-slate-700 font-mono inline-flex items-center gap-1.5">
                        <span class="w-2 h-2 rounded-full bg-emerald-500 animate-pulse"></span>
                        Status: LocalStorage Ativo
                    </span>
                </div>
            </div>
        </header>

        <!-- CONTEÚDO PRINCIPAL (DASHBOARD) -->
        <main class="flex-grow max-w-7xl w-full mx-auto p-4 sm:p-6 space-y-6">
            
            <!-- SEÇÃO DE CARDS INDICADORES -->
            <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-4 gap-4">
                <div class="bg-slate-900 p-4 rounded-xl border border-slate-800 flex justify-between items-center">
                    <div>
                        <p class="text-xs text-slate-400 font-medium uppercase tracking-wider">Ativos Mapeados</p>
                        <h3 id="stat-assets" class="text-2xl font-bold mt-1 text-white">0</h3>
                    </div>
                    <div class="p-3 bg-slate-800 rounded-lg text-slate-400"><i data-lucide="server" class="w-5 h-5"></i></div>
                </div>
                <div class="bg-slate-900 p-4 rounded-xl border border-slate-800 flex justify-between items-center">
                    <div>
                        <p class="text-xs text-slate-400 font-medium uppercase tracking-wider">Riscos Identificados</p>
                        <h3 id="stat-risks" class="text-2xl font-bold mt-1 text-white">0</h3>
                    </div>
                    <div class="p-3 bg-slate-800 rounded-lg text-amber-500"><i data-lucide="alert-triangle" class="w-5 h-5"></i></div>
                </div>
                <div class="bg-slate-900 p-4 rounded-xl border border-slate-800 flex justify-between items-center">
                    <div>
                        <p class="text-xs text-slate-400 font-medium uppercase tracking-wider">Riscos Mitigados</p>
                        <h3 id="stat-mitigated" class="text-2xl font-bold mt-1 text-white">0</h3>
                    </div>
                    <div class="p-3 bg-slate-800 rounded-lg text-emerald-500"><i data-lucide="check-circle" class="w-5 h-5"></i></div>
                </div>
                <div class="bg-slate-900 p-4 rounded-xl border border-slate-800 flex justify-between items-center">
                    <div>
                        <p class="text-xs text-slate-400 font-medium uppercase tracking-wider">Média de Criticidade</p>
                        <h3 id="stat-avg" class="text-2xl font-bold mt-1 text-white">0.0</h3>
                    </div>
                    <div class="p-3 bg-slate-800 rounded-lg text-cyan-400"><i data-lucide="activity" class="w-5 h-5"></i></div>
                </div>
            </div>

            <!-- GRID CENTRAL: FORMULÁRIOS VS MATRIZ DE CALOR -->
            <div class="grid grid-cols-1 lg:grid-cols-3 gap-6">
                
                <!-- COLUNA DA ESQUERDA: FORMULÁRIOS DE CADASTRO -->
                <div class="space-y-6 lg:col-span-1">
                    
                    <!-- FORMULÁRIO 1: CADASTRO DE ATIVO -->
                    <div class="bg-slate-900 p-5 rounded-xl border border-slate-800 shadow-xl">
                        <h2 class="text-sm font-semibold text-white uppercase tracking-wider flex items-center gap-2 mb-4 border-b border-slate-800 pb-2">
                            <i data-lucide="plus-circle" class="w-4 h-4 text-cyan-400"></i> Cadastrar Ativo de TI
                        </h2>
                        <form id="form-asset" class="space-y-3" onsubmit="handleAssetSubmit(event)">
                            <div>
                                <label class="block text-xs text-slate-400 mb-1">Nome do Ativo</label>
                                <input type="text" id="asset-name" placeholder="Ex: Servidor de Produção, Banco de Dados" required
                                    class="w-full bg-slate-950 border border-slate-800 rounded-lg px-3 py-1.5 text-sm text-white focus:outline-none focus:border-cyan-500 transition-colors">
                            </div>
                            <div>
                                <label class="block text-xs text-slate-400 mb-1">Tipo de Ativo</label>
                                <select id="asset-type" required
                                    class="w-full bg-slate-950 border border-slate-800 rounded-lg px-3 py-1.5 text-sm text-white focus:outline-none focus:border-cyan-500 transition-colors">
                                    <option value="Hardware">Hardware / Servidor</option>
                                    <option value="Software">Software / Aplicação</option>
                                    <option value="Banco de Dados">Banco de Dados / Storage</option>
                                    <option value="Rede">Dispositivo de Rede</option>
                                </select>
                            </div>
                            <div class="grid grid-cols-3 gap-2 pt-1">
                                <div>
                                    <label class="block text-[10px] text-center text-slate-400 mb-1" title="Confidencialidade">C</label>
                                    <select id="asset-c" class="w-full bg-slate-950 border border-slate-800 rounded-md py-1 text-xs text-center text-white font-mono">
                                        <option value="1">1</option><option value="2">2</option><option value="3" selected>3</option>
                                    </select>
                                </div>
                                <div>
                                    <label class="block text-[10px] text-center text-slate-400 mb-1" title="Integridade">I</label>
                                    <select id="asset-i" class="w-full bg-slate-950 border border-slate-800 rounded-md py-1 text-xs text-center text-white font-mono">
                                        <option value="1">1</option><option value="2">2</option><option value="3" selected>3</option>
                                    </select>
                                </div>
                                <div>
                                    <label class="block text-[10px] text-center text-slate-400 mb-1" title="Disponibilidade">D</label>
                                    <select id="asset-digital" class="w-full bg-slate-950 border border-slate-800 rounded-md py-1 text-xs text-center text-white font-mono">
                                        <option value="1">1</option><option value="2">2</option><option value="3" selected>3</option>
                                    </select>
                                </div>
                            </div>
                            <button type="submit" class="w-full bg-cyan-600 hover:bg-cyan-500 text-slate-950 font-semibold text-xs py-2 px-4 rounded-lg transition-colors mt-2 flex items-center justify-center gap-1">
                                <i data-lucide="save" class="w-3.5 h-3.5"></i> Salvar Ativo
                            </button>
                        </form>
                    </div>

                    <!-- FORMULÁRIO 2: REGISTRO DE RISCO -->
                    <div class="bg-slate-900 p-5 rounded-xl border border-slate-800 shadow-xl">
                        <h2 class="text-sm font-semibold text-white uppercase tracking-wider flex items-center gap-2 mb-4 border-b border-slate-800 pb-2">
                            <i data-lucide="shield-alert" class="w-4 h-4 text-amber-500"></i> Registrar Ameaça / Risco
                        </h2>
                        <form id="form-risk" class="space-y-3" onsubmit="handleRiskSubmit(event)">
                            <div>
                                <label class="block text-xs text-slate-400 mb-1">Vincular ao Ativo</label>
                                <select id="risk-asset-id" required
                                    class="w-full bg-slate-950 border border-slate-800 rounded-lg px-3 py-1.5 text-sm text-white focus:outline-none focus:border-cyan-500 transition-colors">
                                    <option value="">Cadastre um ativo primeiro...</option>
                                </select>
                            </div>
                            <div>
                                <label class="block text-xs text-slate-400 mb-1">Descrição do Risco / Vulnerabilidade</label>
                                <input type="text" id="risk-desc" placeholder="Ex: Ataque de Ransomware, Vazamento" required
                                    class="w-full bg-slate-950 border border-slate-800 rounded-lg px-3 py-1.5 text-sm text-white focus:outline-none focus:border-cyan-500 transition-colors">
                            </div>
                            <div class="grid grid-cols-2 gap-2">
                                <div>
                                    <label class="block text-xs text-slate-400 mb-1">Probabilidade (1-5)</label>
                                    <select id="risk-prob" class="w-full bg-slate-950 border border-slate-800 rounded-lg px-3 py-1.5 text-sm text-white font-mono">
                                        <option value="1">1 - Muito Baixa</option>
                                        <option value="2">2 - Baixa</option>
                                        <option value="3" selected>3 - Média</option>
                                        <option value="4">4 - Alta</option>
                                        <option value="5">5 - Quase Certa</option>
                                    </select>
                                </div>
                                <div>
                                    <label class="block text-xs text-slate-400 mb-1">Impacto (1-5)</label>
                                    <select id="risk-impact" class="w-full bg-slate-950 border border-slate-800 rounded-lg px-3 py-1.5 text-sm text-white font-mono">
                                        <option value="1">1 - Desprezível</option>
                                        <option value="2">2 - Menor</option>
                                        <option value="3" selected>3 - Moderado</option>
                                        <option value="4">4 - Maior</option>
                                        <option value="5">5 - Catastrófico</option>
                                    </select>
                                end text-white font-mono">
                                </div>
                            </div>
                            <button type="submit" class="w-full bg-amber-600 hover:bg-amber-500 text-white font-semibold text-xs py-2 px-4 rounded-lg transition-colors mt-2 flex items-center justify-center gap-1">
                                <i data-lucide="calculator" class="w-3.5 h-3.5"></i> Calcular e Registrar
                            </button>
                        </form>
                    </div>

                </div>

                <!-- COLUNA DA DIREITA: MATRIZ DE CALOR DINÂMICA 5x5 -->
                <div class="lg:col-span-2 space-y-6">
                    <div class="bg-slate-900 p-5 rounded-xl border border-slate-800 shadow-xl h-full flex flex-col justify-between">
                        <div>
                            <div class="flex justify-between items-center mb-2 border-b border-slate-800 pb-2">
                                <h2 class="text-sm font-semibold text-white uppercase tracking-wider flex items-center gap-2">
                                    <i data-lucide="grid" class="w-4 h-4 text-cyan-400"></i> Matriz de Riscos Operacionais (5x5)
                                </h2>
                                <span class="text-[10px] text-slate-400 font-mono">Fórmula: Risco = P × I</span>
                            </div>
                            <p class="text-xs text-slate-400 mb-4">A matriz abaixo distribui e conta os riscos em tempo real de acordo com a severidade.</p>
                        </div>

                        <!-- RENDERIZAÇÃO DA MATRIZ -->
                        <div class="relative overflow-x-auto p-2">
                            <div class="min-w-[400px]">
                                <div class="flex flex-col gap-1.5">
                                    
                                    <div class="grid grid-cols-6 text-center text-[10px] font-mono text-slate-400 font-bold mb-1">
                                        <div>Prob. \ Imp.</div>
                                        <div>1 (Muito B.)</div>
                                        <div>2 (Baixo)</div>
                                        <div>3 (Médio)</div>
                                        <div>4 (Alto)</div>
                                        <div>5 (Crítico)</div>
                                    </div>

                                    <!-- LINHA PROB 5 -->
                                    <div class="grid grid-cols-6 gap-1.5 items-center">
                                        <div class="text-[10px] font-mono text-slate-400 font-bold text-right pr-2">5 (Quase Certa)</div>
                                        <div id="cell-5-1" class="p-3 text-center font-bold text-sm rounded bg-yellow-500/20 text-yellow-400 border border-yellow-500/30">0</div>
                                        <div id="cell-5-2" class="p-3 text-center font-bold text-sm rounded bg-orange-500/20 text-orange-400 border border-orange-500/30">0</div>
                                        <div id="cell-5-3" class="p-3 text-center font-bold text-sm rounded bg-red-600/30 text-red-400 border border-red-500/40">0</div>
                                        <div id="cell-5-4" class="p-3 text-center font-bold text-sm rounded bg-red-700/40 text-red-300 border border-red-500/60">0</div>
                                        <div id="cell-5-5" class="p-3 text-center font-bold text-sm rounded bg-red-900/60 text-white border border-red-500">0</div>
                                    </div>

                                    <!-- LINHA PROB 4 -->
                                    <div class="grid grid-cols-6 gap-1.5 items-center">
                                        <div class="text-[10px] font-mono text-slate-400 font-bold text-right pr-2">4 (Alta)</div>
                                        <div id="cell-4-1" class="p-3 text-center font-bold text-sm rounded bg-green-500/10 text-green-400 border border-green-500/20">0</div>
                                        <div id="cell-4-2" class="p-3 text-center font-bold text-sm rounded bg-yellow-500/20 text-yellow-400 border border-yellow-500/30">0</div>
                                        <div id="cell-4-3" class="p-3 text-center font-bold text-sm rounded bg-orange-500/20 text-orange-400 border border-orange-500/30">0</div>
                                        <div id="cell-4-4" class="p-3 text-center font-bold text-sm rounded bg-red-600/30 text-red-400 border border-red-500/40">0</div>
                                        <div id="cell-4-5" class="p-3 text-center font-bold text-sm rounded bg-red-700/40 text-red-300 border border-red-500/60">0</div>
                                    </div>

                                    <!-- LINHA PROB 3 -->
                                    <div class="grid grid-cols-6 gap-1.5 items-center">
                                        <div class="text-[10px] font-mono text-slate-400 font-bold text-right pr-2">3 (Média)</div>
                                        <div id="cell-3-1" class="p-3 text-center font-bold text-sm rounded bg-green-500/10 text-green-400 border border-green-500/20">0</div>
                                        <div id="cell-3-2" class="p-3 text-center font-bold text-sm rounded bg-yellow-500/10 text-yellow-500 border border-yellow-500/20">0</div>
                                        <div id="cell-3-3" class="p-3 text-center font-bold text-sm rounded bg-yellow-500/20 text-yellow-400 border border-yellow-500/30">0</div>
                                        <div id="cell-3-4" class="p-3 text-center font-bold text-sm rounded bg-orange-500/20 text-orange-400 border border-orange-500/30">0</div>
                                        <div id="cell-3-5" class="p-3 text-center font-bold text-sm rounded bg-red-600/30 text-red-400 border border-red-500/40">0</div>
                                    </div>

                                    <!-- LINHA PROB 2 -->
                                    <div class="grid grid-cols-6 gap-1.5 items-center">
                                        <div class="text-[10px] font-mono text-slate-400 font-bold text-right pr-2">2 (Baixa)</div>
                                        <div id="cell-2-1" class="p-3 text-center font-bold text-sm rounded bg-green-500/10 text-green-400 border border-green-500/20">0</div>
                                        <div id="cell-2-2" class="p-3 text-center font-bold text-sm rounded bg-green-500/10 text-green-400 border border-green-500/20">0</div>
                                        <div id="cell-2-3" class="p-3 text-center font-bold text-sm rounded bg-yellow-500/10 text-yellow-500 border border-yellow-500/20">0</div>
                                        <div id="cell-2-4" class="p-3 text-center font-bold text-sm rounded bg-yellow-500/20 text-yellow-400 border border-yellow-500/30">0</div>
                                        <div id="cell-2-5" class="p-3 text-center font-bold text-sm rounded bg-orange-500/20 text-orange-400 border border-orange-500/30">0</div>
                                    </div>

                                    <!-- LINHA PROB 1 -->
                                    <div class="grid grid-cols-6 gap-1.5 items-center">
                                        <div class="text-[10px] font-mono text-slate-400 font-bold text-right pr-2">1 (Muito Baixa)</div>
                                        <div id="cell-1-1" class="p-3 text-center font-bold text-sm rounded bg-green-500/10 text-green-400 border border-green-500/20">0</div>
                                        <div id="cell-1-2" class="p-3 text-center font-bold text-sm rounded bg-green-500/10 text-green-400 border border-green-500/20">0</div>
                                        <div id="cell-1-3" class="p-3 text-center font-bold text-sm rounded bg-green-500/10 text-green-400 border border-green-500/20">0</div>
                                        <div id="cell-1-4" class="p-3 text-center font-bold text-sm rounded bg-yellow-500/10 text-yellow-500 border border-yellow-500/20">0</div>
                                        <div id="cell-1-5" class="p-3 text-center font-bold text-sm rounded bg-yellow-500/20 text-yellow-400 border border-yellow-500/30">0</div>
                                    </div>

                                </div>
                            </div>
                        </div>

                        <!-- LEGENDA -->
                        <div class="flex justify-center items-center gap-4 text-[10px] font-mono text-slate-400 pt-3 border-t border-slate-800 mt-2">
                            <span class="flex items-center gap-1"><span class="w-2.5 h-2.5 bg-green-500/20 border border-green-500/40 rounded"></span> Baixo (1-4)</span>
                            <span class="flex items-center gap-1"><span class="w-2.5 h-2.5 bg-yellow-500/20 border border-yellow-500/40 rounded"></span> Médio (5-9)</span>
                            <span class="flex items-center gap-1"><span class="w-2.5 h-2.5 bg-orange-500/20 border border-orange-500/40 rounded"></span> Alto (10-14)</span>
                            <span class="flex items-center gap-1"><span class="w-2.5 h-2.5 bg-red-600/30 border border-red-500/40 rounded"></span> Crítico (15-25)</span>
                        </div>
                    </div>
                </div>

            </div>

            <!-- SEÇÃO INFERIOR: TABELA DE GERENCIAMENTO -->
            <div class="bg-slate-900 rounded-xl border border-slate-800 shadow-xl overflow-hidden">
                <div class="px-5 py-4 border-b border-slate-800 bg-slate-900/80 flex flex-col sm:flex-row justify-between items-start sm:items-center gap-2">
                    <div>
                        <h2 class="text-sm font-semibold text-white uppercase tracking-wider flex items-center gap-2">
                            <i data-lucide="list-ordered" class="w-4 h-4 text-cyan-400"></i> Registro Geral de Riscos Cibernéticos
                        </h2>
                        <p class="text-xs text-slate-400 mt-0.5">Histórico e ações de mitigação (Controle de Riscos Residuais)</p>
                    </div>
                    <button onclick="clearAllData()" class="text-xs font-mono text-rose-400 hover:text-rose-300 transition-colors flex items-center gap-1 border border-rose-500/20 hover:border-rose-500/40 bg-rose-500/5 px-2.5 py-1 rounded-md">
                        <i data-lucide="trash-2" class="w-3.5 h-3.5"></i> Resetar Banco
                    </button>
                </div>
                
                <div class="overflow-x-auto">
                    <table class="w-full text-left text-sm border-collapse">
                        <thead>
                            <tr class="bg-slate-950 text-slate-400 border-b border-slate-800 text-xs font-mono">
                                <th class="p-4 font-semibold">Ativo Alvo</th>
                                <th class="p-4 font-semibold">Risco / Ameaça</th>
                                <th class="p-4 font-semibold text-center">P × I</th>
                                <th class="p-4 font-semibold text-center">Score</th>
                                <th class="p-4 font-semibold text-center">Severidade</th>
                                <th class="p-4 font-semibold text-center">Tratamento</th>
                                <th class="p-4 font-semibold text-right">Ações</th>
                            </tr>
                        </thead>
                        <tbody id="table-risks-body" class="divide-y divide-slate-800/50">
                            <!-- Injetado dinamicamente via JS -->
                        </tbody>
                    </table>
                    <div id="empty-state" class="p-8 text-center text-slate-500 text-xs flex flex-col items-center justify-center gap-2">
                        <i data-lucide="database" class="w-8 h-8 text-slate-600"></i>
                        Nenhum risco cadastrado até o momento.
                    </div>
                </div>
            </div>

        </main>

        <!-- FOOTER -->
        <footer class="border-t border-slate-800 bg-slate-900/40 px-6 py-3 text-center sm:text-left">
            <div class="max-w-7xl mx-auto flex flex-col sm:flex-row justify-between items-center text-xs text-slate-500 gap-2">
                <p>&copy; 2026 AegisRisk Project • MVP Funcional Prático</p>
                <p class="font-mono bg-slate-900 px-2 py-1 border border-slate-800 rounded text-slate-400">
                    Tecnologia: HTML5 • Tailwind • JavaScript Puro
                </p>
            </div>
        </footer>
    </div>

    <!-- LOGIC SYSTEM (JavaScript Puro) -->
    <script>
        let state = { assets: [], risks: [] };

        function init() {
            const savedData = localStorage.getItem('aegis_risk_db');
            if (savedData) {
                try { state = JSON.parse(savedData); } catch (e) { console.error(e); }
            } else {
                state.assets = [
                    { id: 'a1', name: 'Servidor ERP Cloud', type: 'Software', c: 3, i: 3, d: 3 },
                    { id: 'a2', name: 'Banco de Dados Clientes', type: 'Banco de Dados', c: 3, i: 2, d: 2 }
                ];
                state.risks = [
                    { id: 'r1', assetId: 'a1', desc: 'Ataque de Força Bruta em SSH', prob: 4, impact: 3, mitigated: false },
                    { id: 'r2', assetId: 'a2', desc: 'Vulnerabilidade de SQL Injection', prob: 3, impact: 5, mitigated: true }
                ];
                saveToLocalStorage();
            }
            renderAll();
        }

        function saveToLocalStorage() {
            localStorage.setItem('aegis_risk_db', JSON.stringify(state));
        }

        function handleAssetSubmit(event) {
            event.preventDefault();
            const name = document.getElementById('asset-name').value;
            const type = document.getElementById('asset-type').value;
            const c = parseInt(document.getElementById('asset-c').value);
            const i = parseInt(document.getElementById('asset-i').value);
            const d = parseInt(document.getElementById('asset-digital').value);

            state.assets.push({ id: 'asset_' + Date.now(), name, type, c, i, d });
            saveToLocalStorage();
            document.getElementById('form-asset').reset();
            renderAll();
        }

        function handleRiskSubmit(event) {
            event.preventDefault();
            const assetId = document.getElementById('risk-asset-id').value;
            const desc = document.getElementById('risk-desc').value;
            const prob = parseInt(document.getElementById('risk-prob').value);
            const impact = parseInt(document.getElementById('risk-impact').value);

            if (!assetId) return alert("Cadastre um ativo primeiro.");

            state.risks.push({ id: 'risk_' + Date.now(), assetId, desc, prob, impact, mitigated: false });
            saveToLocalStorage();
            document.getElementById('form-risk').reset();
            renderAll();
        }

        function toggleMitigation(id) {
            const risk = state.risks.find(r => r.id === id);
            if (risk) { risk.mitigated = !risk.mitigated; saveToLocalStorage(); renderAll(); }
        }

        function deleteRisk(id) {
            state.risks = state.risks.filter(r => r.id !== id);
            saveToLocalStorage();
            renderAll();
        }

        function clearAllData() {
            if(confirm("Resetar banco local?")) {
                localStorage.removeItem('aegis_risk_db');
                state.assets = []; state.risks = [];
                renderAll();
            }
        }

        function renderAll() {
            const selectAsset = document.getElementById('risk-asset-id');
            selectAsset.innerHTML = state.assets.length === 0 
                ? '<option value="">Cadastre um ativo primeiro...</option>'
                : state.assets.map(a => `<option value="${a.id}">${a.name}</option>`).join('');

            for (let p = 1; p <= 5; p++) {
                for (let i = 1; i <= 5; i++) { document.getElementById(`cell-${p}-${i}`).innerText = '0'; }
            }

            const tableBody = document.getElementById('table-risks-body');
            tableBody.innerHTML = '';
            let totalScore = 0, countMitigated = 0;

            if (state.risks.length === 0) {
                document.getElementById('empty-state').classList.remove('hidden');
            } else {
                document.getElementById('empty-state').classList.add('hidden');
                state.risks.forEach(risk => {
                    const targetAsset = state.assets.find(a => a.id === risk.assetId) || { name: 'Desconhecido' };
                    let currentImpact = risk.mitigated ? Math.max(1, Math.round(risk.impact / 2)) : risk.impact;
                    if (risk.mitigated) countMitigated++;

                    const score = risk.prob * currentImpact;
                    totalScore += score;

                    const matrixCell = document.getElementById(`cell-${risk.prob}-${currentImpact}`);
                    if (matrixCell) matrixCell.innerText = parseInt(matrixCell.innerText) + 1;

                    let badge = score <= 4 ? 'text-emerald-400 border-emerald-500/30' : score <= 9 ? 'text-yellow-400 border-yellow-500/30' : score <= 14 ? 'text-orange-400 border-orange-500/30' : 'text-rose-400 border-rose-500/30';
                    let label = score <= 4 ? 'Baixo' : score <= 9 ? 'Médio' : score <= 14 ? 'Alto' : 'Crítico';

                    tableBody.innerHTML += `
                        <tr class="hover:bg-slate-800/30 text-xs text-slate-300">
                            <td class="p-4 font-medium text-white">${targetAsset.name}</td>
                            <td class="p-4">${risk.desc}</td>
                            <td class="p-4 text-center text-slate-500">${risk.prob} × ${currentImpact}</td>
                            <td class="p-4 text-center font-bold text-white">${score}</td>
                            <td class="p-4 text-center"><span class="px-2 py-0.5 rounded border text-[10px] uppercase font-semibold ${badge}">${label}</span></td>
                            <td class="p-4 text-center">
                                <button onclick="toggleMitigation('${risk.id}')" class="px-2 py-1 rounded text-[10px] ${risk.mitigated ? 'bg-emerald-500/20 text-emerald-400 border border-emerald-500/40' : 'bg-slate-800 text-slate-400 border border-slate-700'}">
                                    ${risk.mitigated ? '🛡️ Mitigado' : '⚠️ Tratável'}
                                </button>
                            </td>
                            <td class="p-4 text-right"><button onclick="deleteRisk('${risk.id}')" class="text-slate-500 hover:text-rose-400 font-bold">X</button></td>
                        </tr>`;
                });
            }

            document.getElementById('stat-assets').innerText = state.assets.length;
            document.getElementById('stat-risks').innerText = state.risks.length;
            document.getElementById('stat-mitigated').innerText = countMitigated;
            document.getElementById('stat-avg').innerText = state.risks.length > 0 ? (totalScore / state.risks.length).toFixed(1) : '0.0';
            lucide.createIcons();
        }

        window.onload = init;
    </script>
</body>
</html>
