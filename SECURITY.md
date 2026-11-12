# Dados e segurança

Aplicação local com perfil próprio em %APPDATA%\GestaoOrcamentos. A interface carrega somente arquivos empacotados pelo protocolo orcamentos://app/, sem servidor HTTP. Não há integração Node na interface; contexto isolado e sandbox ativados. IPC aceita somente a origem interna e operações fixas de informação e consulta de versões. Navegação externa é bloqueada; a página fixa de releases só abre por ação do usuário.

Projetos, pagamentos e comprovantes ficam no IndexedDB local. Não há criptografia em repouso. Faça backup .gorc e proteja o usuário do Windows. A sincronização é explicitamente autorizada nos dois dispositivos. Atualizações instaladas podem acessar o banco; proteja a conta do GitHub e revise releases. O instalador não tem assinatura digital.
