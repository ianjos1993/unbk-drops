# UnBK Drops

> **Gerenciador Desktop de Campanhas e Recompensas da Twitch para Windows**  
> Desenvolvido por **Igor Anjos ([@ianjos1993](https://github.com/ianjos1993))** · Comunidade **[UnBK](https://www.unbk.com.br)**

[![Versão](https://img.shields.io/badge/versão-v0.9.0-7c3aed?style=for-the-badge)](https://github.com/ianjos1993/unbk-drops/releases/latest)
[![Plataforma](https://img.shields.io/badge/plataforma-Windows%20x64-0078d4?style=for-the-badge)](https://github.com/ianjos1993/unbk-drops/releases/latest)
[![Licença](https://img.shields.io/badge/licença-MIT-10b981?style=for-the-badge)](https://github.com/ianjos1993/unbk-drops)

---

## Download

Baixe a versão portátil mais recente diretamente na aba **[Releases](https://github.com/ianjos1993/unbk-drops/releases/latest)**:

- **[UnBK Drops Portable.exe](https://github.com/ianjos1993/unbk-drops/releases/latest/download/UnBK.Drops.Portable.exe)** *(Portátil · Windows x64 · Atualização automática integrada · Não requer instalação de Python)*

---

## Sobre o Projeto e Orquestração com IA

O **UnBK Drops** é uma aplicação desktop nativa para Windows projetada para automatizar o acompanhamento de campanhas, a seleção inteligente de canais ao vivo e o resgate de recompensas (Drops) na Twitch com transparência e confiabilidade.

Este projeto faz parte do meu portfólio de **Engenharia de Software com Orquestração de IA (AI-Assisted & Agentic Engineering)** — onde atuei na concepção de produto, definição de arquitetura, especificação de protocolos de segurança, diagnóstico de casos de borda reais contra a plataforma e validação orientada a testes automatizados.

### Destaques Técnicos e Arquiteturais

1. **Arquitetura Híbrida Desktop (Electron + Python 3.14 Asyncio)**
   - Interface gráfica moderna renderizada em Electron comunicando-se via protocolo IPC assíncrono sobre `stdio` (`UNBK_CMD`, `UNBK_RESULT`, `UNBK_TELEMETRY`, `UNBK_EVENT`) com o motor nativo em Python 3.14 empacotado.
   - Inicialização e encerramento determinísticos com controle de instância única, minimização para a bandeja do sistema (System Tray) e notificações nativas do Windows registradas com `AppUserModelID` próprio.

2. **Autenticação Resiliente em Duas Modalidades**
   - **Login Manual com Renovação Silenciosa em Segundo Plano:** Utiliza o navegador Chromium instalado no sistema com perfil isolado e renova sessões em segundo plano via Chrome DevTools Protocol (`Target.createTarget` oculto), validando conta, inventário e acesso ao catálogo antes de aplicar cada sessão.
   - **Conector via Extensão de Navegador (Manifest V3):** Alternativa opcional que opera diretamente sobre a sessão já conectada no navegador padrão via pareamento efêmero em loopback (`127.0.0.1`) com segredo por execução.

3. **Detecção Local de Jogos Instalados**
   - Varredura local das bibliotecas **Steam** (`libraryfolders.vdf` e manifestos `appmanifest_*.acf`), **Epic Games** e chaves de desinstalação do **Registro do Windows (Steam/GOG)**.
   - Destaca automaticamente na tela principal as campanhas de jogos que o usuário já possui instalados no computador e emite notificações opcionais quando surgem novas campanhas desses jogos.

4. **Telemetria Verificada e Relógio Preditivo (`ProgressClock`)**
   - Separação estrita entre progresso estimado localmente e minutos efetivamente confirmados pela Twitch.
   - O `ProgressClock` suaviza a contagem regressiva no painel principal absorvendo lotes de confirmação sem saltos visuais ou resets bruscos.

5. **Engenharia Orientada a Testes (90+ Testes Automatizados)**
   - **Testes de Controladores e Protocolos (Node.js `node:test`):** Ciclo de vida do motor, pareamento seguro da extensão, cálculo de contagem regressiva, caminhos portáteis e deduplicação de notificações.
   - **Testes Unitários e Assíncronos do Motor (`unittest` / `IsolatedAsyncioTestCase`):** Máquina de estados de autenticação, renovação em segundo plano, filtros de elegibilidade, seleção de campanhas e resiliência de WebSockets.
   - **Testes End-to-End de Interface Real (Electron Renderer):** Validação do DOM real, responsividade, persistência de formulários (`settings-roundtrip`) e atualização de progresso ao vivo via IPC.

---

## Funcionalidades Principais

- **Painel em Tempo Real:** Acompanhe a etapa atual da coleta (`1/4` a `4/4`), o streamer assistido, a barra de progresso do drop atual e da campanha completa.
- **Busca Instantânea e Filtros:** Pesquise campanhas por nome do jogo (sem diferenciar maiúsculas ou acentos) e filtre por campanhas elegíveis ou jogos instalados no seu PC.
- **Priorização Flexível:**
  - **Iniciar essa Campanha:** Foca temporariamente em uma campanha específica e retoma a fila normal ao concluir.
  - **Priorizar Jogo:** Coloca o jogo no topo da sua lista salva com um clique.
  - **Modos de Ordenação:** *Campanhas que terminam antes*, *Apenas jogos da lista* ou *Menor disponibilidade primeiro*.
- **Controle de Vínculo de Conta:** Identifica campanhas ativas cujo vínculo de conta ainda não foi confirmado pela Twitch e permite optar por coletá-las mesmo antes da confirmação do vínculo.
- **Inventário Completo:** Visualize drops resgatados, em andamento, encerrados ou sem vínculo confirmado com imagens oficiais das recompensas.
- **Atualizador Automático via GitHub Releases:** Verifica periodicamente se há uma nova versão publicada no repositório oficial, exibe um botão de atualização rápida no topo da janela, baixa o novo executável validando o cabeçalho PE (`MZ`) e o tamanho do pacote, substitui o `.exe` portátil e reinicia o aplicativo automaticamente.

---

## Como Usar (Versão Portátil)

1. Baixe o executável **[UnBK Drops Portable.exe](https://github.com/ianjos1993/unbk-drops/releases/latest/download/UnBK.Drops.Portable.exe)** e coloque-o em uma pasta de sua preferência (com permissão de escrita).
2. Abra o aplicativo. Ele criará automaticamente a pasta `UnBK-Data` ao lado do executável para salvar sua sessão e suas preferências.
3. Vá em **Configurações → Conta Twitch**, escolha o método de login (**Login manual** ou **Conta do navegador via extensão**) e clique em **Conectar conta**.
4. No login manual, entre na sua conta Twitch na janela do navegador que abrir e **feche essa janela** para concluir a validação.
5. Escolha seus jogos prioritários ou deixe a seleção automática ativa. O aplicativo buscará canais ao vivo compatíveis e resgatará os drops automaticamente.

> **Dica para atualização (vindo de versões anteriores à 0.9.0):**  
> Para manter seu login sem precisar conectar novamente, abra a sua pasta `UnBK-Data` (ou `Andyz0x-Data`) antes de iniciar a versão `0.9.0` e renomeie a subpasta interna antiga para **`unbk-data`**.

---

## Estrutura de Arquitetura (Repositório Fonte)

```text
UnBKTwitchDrops/
├── electron/          # Processo principal Electron, ponte IPC, bandeja, notificações e scanner local de jogos
├── web/               # Interface desktop principal (HTML5, CSS3 e JavaScript modular)
├── native/            # Adaptadores Python (autenticação, renovação CDP, campanhas, telemetria e extensão MV3)
├── runtime/           # Motor assíncrono integrado e módulos de sessão/validação
└── scripts/           # Automação de preparação do ambiente e smoke tests
```

---

## Autor e Comunidade

- **Autor:** Igor Anjos — [@ianjos1993](https://github.com/ianjos1993)
- **Site Oficial:** [unbk.com.br](https://www.unbk.com.br)
- **Comunidade no Discord:** [discord.gg/unbk](https://discord.gg/unbk)
- **Apoie o Projeto:** [buymeacoffee.com/ianjos1993](https://www.buymeacoffee.com/ianjos1993)

---

## Licença

Distribuído sob a licença **MIT**. Não afiliado à Twitch.
