# Cloud Backup Manager

## Visão Geral

O **Cloud Backup Manager** é uma ferramenta de desktop desenvolvida em Python com uma interface gráfica em Tkinter. A aplicação foi criada para automatizar e simplificar backups de bancos de dados (Microsoft SQL Server e Firebird) e de pastas de arquivos de clientes em servidores de rede (PODs).

Desenvolvido por **Gabriel Levi**, este projeto visa fornecer uma solução robusta e de fácil uso para operações internas de backup.

## Funcionalidades Principais

-   **Interface com Abas**: Separa claramente o backup de bancos de dados do backup de arquivos.
-   **Backup Inteligente de MSSQL**: Conecta-se ao banco para descobrir a localização física dos arquivos (`.mdf`) e usa esse diretório como destino do backup, evitando erros de permissão de escrita.
-   **Backup Robusto de Firebird**: Permite ao usuário especificar a localização do `gbak.exe`, garantindo compatibilidade de versão.
-   **Backup de Arquivos em Rede**: Procura em uma lista configurável de servidores (PODs) pela pasta de um cliente específico e compacta a subpasta `ARQUIVOS`.
-   **Gerenciamento Dinâmico de PODs**: Permite ao usuário adicionar, editar e remover servidores POD através da interface, com as alterações salvas em um arquivo `config.ini` externo.
-   **Verificação Pré-Backup**: Antes de iniciar, a ferramenta calcula o tamanho estimado da origem e o espaço livre no destino, exibindo um alerta em caso de espaço insuficiente.
-   **Confirmação de Operação**: Exibe uma caixa de diálogo com um resumo detalhado do backup para confirmação do usuário.
-   **Pós-Processamento**: Inclui opções para compactar o backup, excluir o arquivo original (`.bak`/`.fbk`) e renomear o banco de dados de origem (lógica para MSSQL e física para Firebird).
-   **Feedback Visual**: Logs em tempo real e uma barra de progresso informam o status da operação.
-   **Acesso Fácil**: Botão "Abrir Local do Arquivo" para navegar diretamente até o backup recém-criado.

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
├── requirements.txt    # Lista de dependências do projeto.
└── ...
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

3.  **Instale as dependências** listadas no `requirements.txt`:
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

1.  Garanta que todas as dependências, incluindo `pyinstaller`, estejam instaladas no seu ambiente virtual.

2.  Execute o seguinte comando a partir da pasta raiz do projeto:
    ```bash
    python -m pyinstaller --onefile --windowed --name="BackupManager" --icon="icon.ico" --add-data="icon.ico;." --add-data="config.ini;." main.py
    ```
    * **`--onefile`**: Cria um único arquivo `.exe`.
    * **`--windowed`**: Evita que um console de fundo seja aberto.
    * **`--icon`**: Define o ícone do arquivo `.exe`.
    * **`--add-data`**: Inclui arquivos não-Python (como o ícone e o config) dentro do executável.

3.  O executável final (`BackupManager.exe`) estará na pasta `dist/`. Para distribuição, você precisará apenas do arquivo `.exe`. O `config.ini` será criado na primeira execução se não existir, ou pode ser distribuído junto.

## Disponibilizando para Download no GitHub

Para disponibilizar o `BackupManager.exe` e o manual de usuário para download, a melhor forma é usar a funcionalidade de **Releases** do GitHub.

1.  No seu repositório do GitHub, clique na aba **"Releases"** (geralmente na barra lateral direita).
2.  Clique em **"Create a new release"** ou "Draft a new release".
3.  Dê um nome para a versão (ex: `v1.0.0`).
4.  Escreva uma breve descrição das funcionalidades ou correções desta versão.
5.  Na seção **"Attach binaries by dropping them here or selecting them"**, arraste e solte o seu `BackupManager.exe` e o arquivo `Manual_de_Uso.pdf`.
6.  Clique em **"Publish release"**.

Agora, os usuários poderão baixar os arquivos diretamente da página de releases do seu projeto. Você pode colocar um link para a página de releases no topo do seu `README.md` para fácil acesso:

`[Baixe a última versão aqui] https://github.com/Biellima2811/App_Cloud_Backup_Manager.git`