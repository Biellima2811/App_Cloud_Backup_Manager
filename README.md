# Cloud Backup Manager

## Visão Geral

O **Cloud Backup Manager** é uma ferramenta de desktop desenvolvida em Python com uma interface gráfica em Tkinter. A aplicação foi criada para automatizar e simplificar backups de bancos de dados (Microsoft SQL Server e Firebird) e de pastas de arquivos de clientes, com um poderoso modo de execução em lote.

Desenvolvido por **Gabriel Levi**, este projeto visa fornecer uma solução robusta e de fácil uso para operações internas de backup, otimizando o tempo de atendimento técnico e minimizando falhas humanas.

## Funcionalidades Principais

-   **Interface com Abas**: Separa claramente o backup individual de bancos, o backup de arquivos e a nova automação em lote.
-   **Backup Inteligente de MSSQL**: Conecta-se ao banco para descobrir a localização física dos arquivos (`.mdf`) e usa esse diretório como destino do backup, evitando erros de permissão de escrita.
-   **Backup Robusto de Firebird**: Permite ao usuário especificar a localização do `gbak.exe`, garantindo compatibilidade de versão.
-   **Backup de Arquivos em Rede**: Procura em uma lista configurável de servidores (PODs) pela pasta de um cliente específico e compacta a subpasta `ARQUIVOS`.
-   **Gerenciamento Dinâmico de PODs**: Permite ao usuário adicionar, editar e remover servidores POD através da interface, com as alterações salvas em um arquivo `config.ini` externo.
-   **Feedback Visual**: Logs em tempo real e barras de progresso informam o status da operação.

### Nova Funcionalidade: Automação em Lote

-   **Processamento Múltiplo**: Cadastre múltiplos clientes e execute todos os processos de backup e limpeza de uma só vez.
-   **Validação Pré-Execução**: O sistema verifica automaticamente se os bancos de dados e caminhos informados existem antes de iniciar, prevenindo falhas.
-   **Fluxo Completo por Cliente**: Para cada item na lista, a ferramenta executa o backup do banco, a compactação da pasta `ARQUIVOS` e a exclusão do cliente no CloudUpCmd.
-   **Monitoramento Detalhado**: Uma tabela (Treeview) exibe o status de cada cliente em tempo real ("Pendente", "Validado ✅", "Erro ❌", "Concluído ✅").
-   **Relatório Final**: Ao término, um relatório detalhado em `.txt` é gerado automaticamente na pasta `logs`, resumindo os resultados.

## Estrutura do Projeto

```
/
├── main.py             # Ponto de entrada da aplicação.
├── gui.py              # Lógica da interface gráfica (Tkinter).
├── db_backup.py        # Funções de backup para MSSQL e Firebird.
├── utils.py            # Funções utilitárias (zip, busca de pastas, etc.).
├── logger_conf.py      # Configuração do sistema de logging.
├── config.ini          # Arquivo de configuração para os caminhos dos PODs.
├── icon.ico            # Ícone da aplicação.
└── logs/               # Pasta para logs e relatórios.
```

## Configuração do Ambiente de Desenvolvimento

1.  **Clone o repositório**.

2.  **Crie e ative um ambiente virtual** (altamente recomendado):
    ```bash
    # Cria o ambiente
    python -m venv venv

    # Ativa no Windows
    .\venv\Scripts\activate
    ```

3.  **Instale as dependências** (se houver um `requirements.txt`):
    ```bash
    python -m pip install -r requirements.txt
    ```

## Como Executar

Com o ambiente virtual ativo, execute o script principal:
```bash
python main.py
```

## Como Gerar o Executável (`.exe`)

O projeto está configurado para ser compilado com o **PyInstaller**.

1.  Garanta que o `pyinstaller` esteja instalado no seu ambiente virtual.

2.  Execute o seguinte comando a partir da pasta raiz do projeto:
    ```bash
    python -m pyinstaller --onefile --windowed --name="BackupManager" --icon="icon.ico" --add-data="icon.ico;." --add-data="config.ini;." main.py
    ```
    - **`--onefile`**: Cria um único arquivo `.exe`.
    - **`--windowed`**: Evita que um console de fundo seja aberto.
    - **`--icon`**: Define o ícone do arquivo `.exe`.
    - **`--add-data`**: Inclui arquivos não-Python (como o ícone e o config) dentro do executável.

3.  O executável final (`BackupManager.exe`) estará na pasta `dist/`.

## Disponibilizando para Download no GitHub

Para disponibilizar o `BackupManager.exe` e o manual de usuário, use a funcionalidade de **Releases** do GitHub.

1.  No seu repositório, vá em **"Releases"** e clique em **"Create a new release"**.
2.  Dê um nome para a versão (ex: `v2.0.0`).
3.  Anexe o `BackupManager.exe` e o `Manual_de_Uso.pdf` na seção de binários.
4.  Clique em **"Publish release"**.