Rio Verde Assistant - Tutorial Completo Funcionando ✅
Um assistente especializado em Rio Verde (GO) usando NVIDIA Agent Toolkit com LangChain.
🎯 Objetivo
Criar um agente inteligente que responde perguntas sobre Rio Verde usando NVIDIA Agent Toolkit + LangChain com modelo Llama 3.1.
📋 Pré-requisitos

WSL com Ubuntu instalado
Conta NVIDIA com API Key (obtenha em: https://build.nvidia.com/)

🛠️ Instalação Passo a Passo
1. Clone o Repositório
bashcd ~
git clone https://github.com/NVIDIA/AIQToolkit.git
cd AIQToolkit
2. Verifique/Instale Python 3.11
bash# Verifique versão atual
python3 --version

# Se for menor que 3.11, instale:
sudo apt update
sudo apt install software-properties-common
sudo add-apt-repository ppa:deadsnakes/ppa
sudo apt update
sudo apt install python3.11 python3.11-venv python3.11-dev

# Confirme instalação
python3.11 --version
3. Crie e Ative Ambiente Virtual
bash# Crie ambiente virtual
python3.11 -m venv venv

# Ative (deve aparecer "(venv)" no prompt)
source venv/bin/activate
4. Instale AIQ Toolkit Base
bash# Atualize pip
pip install --upgrade pip

# Instale toolkit base
pip install -e ".[agents]"
5. IMPORTANTE: Instale Plugin LangChain
bash# Esta é a parte crucial que faltava!
pip install aiqtoolkit-langchain

# Instale dependências necessárias
pip install langgraph langchain langchain-community
6. Configure API Key NVIDIA
bash# Substitua pela sua chave real
export NVIDIA_API_KEY="sua-api-key-aqui"

# Verifique se foi configurada
echo $NVIDIA_API_KEY
7. Navegue para Diretório de Exemplos
bashcd examples/simple
8. Crie Estrutura de Configuração
bash# Crie diretório
mkdir -p configs

# Crie arquivo de configuração otimizado
nano configs/rio_verde_otimizado.yml
9. Configuração YAML Otimizada (Colar no nano)
yamlfunctions:
  current_datetime:
    type: current_datetime

llms:
  nim_llm:
    type: nim
    model_name: meta/llama-3.1-70b-instruct
    temperature: 0.1
    max_tokens: 1024

workflow:
  type: react_agent
  tool_names: [current_datetime]
  llm_name: nim_llm
  verbose: true
  retry_parsing_errors: false
  max_retries: 1
  system_prompt: |
    Você é um assistente especialista em Rio Verde, Goiás, Brasil.
    Rio Verde é conhecida como a "Capital do Agronegócio" do Brasil!
    
    INFORMAÇÕES PRINCIPAIS:
    - População: ~230.000 habitantes
    - Localização: 229km de Goiânia, Sudoeste de Goiás
    - Maior produtor de grãos do estado
    - Sede da TECNOSHOW (maior feira do agronegócio)
    - PIB forte baseado em agricultura e pecuária
    - Universidades: UnirV, IFG
    - Clima tropical semi-úmido
    
    INSTRUÇÕES:
    1. Para perguntas sobre Rio Verde: responda diretamente com seu conhecimento
    2. Para perguntas sobre data/hora: use a ferramenta current_datetime
    3. Seja conciso e direto
    4. SEMPRE termine com "Final Answer: [sua resposta]"
    
    Ferramentas disponíveis: {tool_names}
    {tools}
Comandos para salvar no nano:

Ctrl+O para salvar
Enter para confirmar
Ctrl+X para sair

10. Teste o Assistente
bashaiq run --config_file configs/rio_verde_otimizado.yml --input "Fale sobre Rio Verde"
🎉 Exemplo de Resultado Esperado
Configuration Summary:
--------------------
Workflow Type: react_agent
Number of Functions: 1
Number of LLMs: 1

[AGENT]
Agent input: Fale sobre Rio Verde
Agent's thoughts:
Rio Verde é uma cidade localizada no sudoeste do estado de Goiás, Brasil. 
É conhecida como a "Capital do Agronegócio" do Brasil devido à sua forte 
produção agrícola e pecuária...

Final Answer: Rio Verde é uma cidade importante para o agronegócio brasileiro, 
conhecida por sua produção agrícola e pecuária, e sede de importantes 
instituições de ensino superior.

Workflow Result:
['Rio Verde é uma cidade importante para o agronegócio brasileiro...']
🔍 Comandos de Teste Adicionais
bash# Teste produtos agrícolas
aiq run --config_file configs/rio_verde_otimizado.yml --input "Quais produtos Rio Verde produz?"

# Teste economia
aiq run --config_file configs/rio_verde_otimizado.yml --input "Como está a economia de Rio Verde?"

# Teste data/hora (usa a ferramenta current_datetime)
aiq run --config_file configs/rio_verde_otimizado.yml --input "Que horas são agora?"

# Teste educação
aiq run --config_file configs/rio_verde_otimizado.yml --input "Quais universidades tem em Rio Verde?"
🚨 Pontos Cruciais que Fazem a Diferença
✅ O que é ESSENCIAL:

Python 3.11+ - Toolkit não funciona com versões antigas
Plugin LangChain - pip install aiqtoolkit-langchain
Dependências LangChain - langgraph, langchain, langchain-community
API Key NVIDIA - Configurada como variável de ambiente
Ambiente Virtual Ativado - Sempre ver (venv) no prompt
Configuração Otimizada - retry_parsing_errors: false e max_retries: 1
Final Answer - Instrução clara no prompt para formato correto

❌ Erros Comuns:

"react_agent not found" → Faltou instalar aiqtoolkit-langchain
"langgraph module not found" → Faltou instalar langgraph
Python version error → Precisa Python 3.11+
API Key error → Não configurou NVIDIA_API_KEY
Loop infinito → Use a configuração otimizada com retry_parsing_errors: false

📁 Estrutura Final
~/AIQToolkit/
├── examples/
│   └── simple/
│       ├── configs/
│       │   └── rio_verde_otimizado.yml  ← Arquivo principal
│       └── ...
├── venv/
└── ...
🔧 Troubleshooting
Se der erro de comando 'aiq' não encontrado:
bash# Verifique se está no ambiente virtual
which python
# Deve mostrar: /home/user/AIQToolkit/venv/bin/python

# Reinstale se necessário
pip install -e ".[agents]"
Se der erro de módulo langgraph:
bashpip install langgraph langchain langchain-community
Se ficar em loop infinito:
bash# Use a configuração otimizada com:
# retry_parsing_errors: false
# max_retries: 1
✨ Diferenças da Configuração Otimizada
rio_verde_otimizado.yml vs configurações básicas:

retry_parsing_errors: false - Evita loops infinitos
max_retries: 1 - Máximo 1 tentativa
Prompt mais estruturado - Instruções claras
"Final Answer:" obrigatório - Formato correto
Informações organizadas - Seções bem definidas

🚀 Próximos Passos Opcionais

Adicionar busca web: Para informações em tempo real
Interface web: Usar FastAPI para dashboard
Mais ferramentas: RAG, memória, análise de documentos
Deploy: Para usar em produção

🏆 Hackathon Ready!
Este projeto está pronto para submissão no NVIDIA Agent Toolkit Hackathon 2025!
Principais diferenciais:

✅ Funciona perfeitamente - Zero loops, respostas limpas
✅ Foco regional específico - Especialista em Rio Verde
✅ Configuração otimizada - Sem problemas de parsing
✅ Tutorial reproduzível - Passo a passo testado
✅ Troubleshooting completo - Soluções para problemas comuns
✅ Respostas precisas - Conhecimento correto sobre agronegócio

📊 Exemplos de Respostas do Sistema
Pergunta: "Quais produtos Rio Verde produz?"
Resposta: "Soja, milho, trigo, carne bovina e leite."
Pergunta: "Fale sobre Rio Verde"
Resposta: "Rio Verde é uma cidade importante para o agronegócio brasileiro, conhecida por sua produção agrícola e pecuária, e sede de importantes instituições de ensino superior."

Criado por: [Seu Nome]
Data: Maio 2025
Hackathon: NVIDIA Agent Toolkit 2025
Status: ✅ 100% FUNCIONANDO
Versão: Otimizada (sem loops)
