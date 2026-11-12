# Gestão de Orçamentos — aplicativo completo

Aplicativo desktop offline, com React, Electron e navegador Chromium incluído. Carrega os arquivos internos no endereço `orcamentos://app/`. Não usa servidor HTTP, porta TCP, Edge, Chrome, Node ou site externo no computador do usuário.

## Instalar

Baixe o instalador `.exe` em **Releases**, execute e abra pelo atalho **Gestão de Orçamentos**. O pacote NSIS inclui o programa inteiro; não é um instalador web. Requer Windows 10/11 x64. A instalação e a primeira abertura funcionam sem internet depois que você tiver baixado o arquivo. O executável não possui assinatura digital de editor; o Windows pode apresentar aviso de reputação.

Os instaladores antigos de 2 MB ou 9 MB foram substituídos. Use a versão 3.0.0 ou superior, que inclui o próprio navegador.

## Dados locais

- Diretório fixo: `%APPDATA%\GestaoOrcamentos`.
- Perfil da aplicação: `%APPDATA%\GestaoOrcamentos\perfil`.
- Banco financeiro: IndexedDB `gestao-orcamentos`, dentro desse perfil. Não é SQLite.
- Projetos, itens, histórico, comprovantes e preferências não são enviados ao GitHub.
- Diagnóstico de inicialização: `%APPDATA%\GestaoOrcamentos\inicializacao.log`; registra somente eventos de abertura e falhas técnicas, sem dados financeiros.
- Exportação e restauração por backup `.gorc`. Dados de versões anteriores em Edge/Chrome devem ser exportados e restaurados; não há migração automática entre perfis.
- O desinstalador preserva os dados. Faça backups regularmente; não há criptografia do banco em repouso.

## Atualizações e sincronização

O botão **Atualizações** consulta releases públicas no GitHub e avisa quando houver versão superior. O usuário abre a página para baixar e instalar; não existe instalação automática nesta versão. Nenhuma consulta é necessária para abrir ou operar offline.

A sincronização WebRTC é entre instâncias abertas e autorizadas, com convite/resposta e confirmação da chave nos dois computadores. Após fechar/reabrir, é necessário parear novamente. Não há descoberta automática. Algumas redes podem exigir configuração própria de STUN/TURN.

## Desenvolvimento

Node 22. Execute `npm ci`, `npm test`, `npm run build` e `npm run desktop`. Compile no Windows com `npm run dist:win`. O workflow no GitHub testa e gera o instalador completo em um runner Windows, com permissão somente de leitura. Os arquivos do aplicativo são empacotados no `app.asar` junto com Electron/Chromium; não há busca de arquivos do aplicativo na internet durante a abertura.

A interface usa isolamento de contexto, sandbox, integração Node desativada, protocolo com validação de origem/caminho, bloqueio de navegação externa e IPC limitado. A consulta de versões só acessa o repositório público correspondente.
