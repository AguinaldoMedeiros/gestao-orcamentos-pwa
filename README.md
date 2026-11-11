# Gestão de Orçamentos — PWA e instalador Windows

Aplicativo: https://gestao-orcamentos-medeiros.medeiros-agui.chatgpt.site/

## Para instalar e compartilhar

Baixe `Gestao-de-Orcamentos-Windows.zip`, extraia e execute o arquivo `.exe`.
Ele instala atalhos no usuário atual e abre a PWA em janela própria usando
Microsoft Edge ou Google Chrome. Requer Windows 10/11 x64 e internet no
primeiro carregamento para preparar o modo offline. Os dados ficam no banco
local do navegador; não são enviados ao GitHub.

O executável não possui assinatura digital. Foi compilado para Windows,
mas não executado em Windows neste ambiente. O código do instalador está
incluído no pacote de código-fonte.

## Código-fonte

O código completo está em `Gestao-de-Orcamentos-Fonte.zip`. Extraia-o para
obter os diretórios `src`, `public`, `scripts`, `tests` e `installer`.
Não há credenciais, bancos financeiros, backups ou comprovantes no pacote.

```sh
npm ci
npm test
npm run build
```

A automação `.github/workflows/build.yml` valida o SHA-256 do pacote fonte,
extrai no ambiente de compilação, executa os testes e gera os artefatos da PWA
e do instalador. Ela usa somente permissão de leitura no repositório:
não altera o código e não publica releases automaticamente.

A hospedagem atual está no ChatGPT Sites. Publicar uma nova versão na
hospedagem permite aplicá-la pelo botão Atualizações da PWA. Os pushes no
GitHub geram os pacotes de compilação, mas não atualizam essa hospedagem
automaticamente.

## Sincronização

O pareamento WebRTC exige convite/resposta e confirmação nos dois dispositivos.
As mensagens são cifradas e os eventos têm IDs únicos. A conexão precisa estar
aberta nos dois lados; após fechar/reabrir, refaça o pareamento. Pela internet,
pode ser necessário configurar STUN/TURN de sua escolha. Não há descoberta
automática em segundo plano. Conflitos financeiros são mostrados para revisão.

## Arquivos

- `Gestao-de-Orcamentos-Windows.zip`: instalador e instruções.
- `Gestao-de-Orcamentos-Instalar-Windows-x64.exe`: instalador direto.
- `Gestao-de-Orcamentos-Fonte.zip`: código completo.
- `SHA256SUMS.txt`: verificação do executável.
- `.github/workflows/build.yml`: compilação e testes no GitHub Actions.
