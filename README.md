# Gestão de Orçamentos — aplicação local

O instalador Windows inclui a PWA completa dentro do executável. Cada computador hospeda sua própria cópia em http://127.0.0.1:47831/, sem internet desde a primeira abertura. Não existe dependência de site externo para abrir ou operar os orçamentos.

## Instalação e operação

Baixe Gestao-de-Orcamentos-Windows.zip, extraia e execute o instalador. Requer Windows 10/11 x64 e Edge ou Chrome já instalado. Não exige Node, Go, Python, privilégios de administrador ou servidor externo. Instala em %LOCALAPPDATA%\GestaoOrcamentos e cria atalhos. O servidor escuta apenas no loopback; outros computadores não podem acessar essa porta. O processo GestaoOrcamentos.exe permanece em segundo plano enquanto o Windows está aberto, permitindo reabrir a janela. Para encerrá-lo, use o Gerenciador de Tarefas. A desinstalação preserva os dados do navegador.

## Desenvolvimento e compilação

Requisitos de desenvolvimento: Node 22 e Go 1.27.4. O usuário final não precisa deles.

```sh
npm ci
npm test
npm run build
mkdir -p installer/web
cp -R dist/. installer/web/
go -C installer test ./...
GOOS=windows GOARCH=amd64 go -C installer build -trimpath -ldflags='-H windowsgui -s -w' -o ../Gestao-de-Orcamentos-Instalar-Windows-x64.exe .
```

O código-fonte completo está no pacote Gestao-de-Orcamentos-Fonte.zip. O workflow valida o pacote, testa, compila a interface, incorpora os arquivos no executável e disponibiliza artefatos. Não publica hospedagem externa.

## Atualizações

Nesta versão, baixe e execute o novo instalador do GitHub após encerrar o processo local. Faça backup antes. A origem local permanece fixa para preservar o banco do navegador. O botão Atualizações confere o service worker do servidor local; não instala executáveis novos nem consulta o GitHub automaticamente. O instalador não possui assinatura digital. A compilação Windows foi validada, mas precisa de teste final em Windows.

## Armazenamento

- Banco: IndexedDB `gestao-orcamentos`, versão de esquema 1.
- `ops`: histórico imutável com projetos, itens, pagamentos, exclusões e recibos.
- `meta`: identificador do dispositivo e preferências locais.
- Valores monetários: inteiros em centavos.
- Comprovantes: PNG/JPG/WEBP/GIF/PDF, até 5 MB por arquivo, preservados no backup `.gorc`.
- Cache Storage: apenas arquivos do aplicativo.
- Os dados não são enviados ao GitHub. As conexões de sincronização só são abertas por ação explícita.
- O navegador pode negar armazenamento persistente. Limpar os dados do site, apagar o perfil ou mudar a origem pode perder os dados. Faça backups. Não há criptografia em repouso.

## Sincronização distribuída

Cada instância guarda seu próprio histórico. Não há banco central. As alterações são identificadas por UUID e têm relações causais explícitas; a entrega repetida de um evento não duplica pagamentos. Alterações concorrentes no mesmo registro viram um conflito, que exige escolha manual. Pagamentos com versões concorrentes ficam fora dos totais até serem resolvidos. Dois pagamentos distintos podem, por concorrência, ultrapassar o orçamento: ambos são preservados e a interface alerta para revisão.

### Pareamento sem servidor de sinalização

1. Abra **Sincronização** nos computadores A e B.
2. A cria convite e envia o código para B por um canal de confiança.
3. B cola e aceita o convite, gerando a resposta.
4. A cola a resposta e seleciona **Aplicar resposta**.
5. Compare a chave curta nas duas telas e confirme em ambas.

Os códigos contêm informação de rede e um segredo temporário. Não os publique. Somente depois das duas confirmações os históricos financeiros são enviados. O canal WebRTC usa DTLS; as mensagens da aplicação também usam AES-256-GCM. As transferências são divididas em blocos e limitadas a 45 MB de conteúdo por envio.

A sincronização acontece enquanto as instâncias estão abertas, autorizadas e conectadas. Os eventos permanecem no banco quando offline. Ao fechar/reabrir uma instância, é preciso refazer o pareamento; não há descoberta automática nem reconexão sem sinalização. Uma malha de conexões autorizadas retransmite as alterações entre os pares sem duplicá-las.

Na mesma rede, a conexão direta normalmente é possível; firewalls, isolamento Wi-Fi e políticas do navegador podem bloqueá-la. Pela internet, configure seu próprio servidor STUN/TURN nas opções avançadas. Nenhum servidor público é usado por padrão. Um TURN pode retransmitir os pacotes criptografados. Credenciais TURN permanecem nos metadados locais. A disponibilidade de uma conexão direta não é garantida.

## Backup e importação

A restauração completa cria eventos de exclusão/atualização e pode se propagar aos pares conectados. O histórico anterior continua preservado. A opção mesclar mantém os históricos recebidos e mostra conflitos. Backups com conflitos não podem ser restaurados como substituição até serem revisados.

Importação de XLSX/CSV possui mapeamento de colunas e revisão antes de gravar. Pagamento inicial exige valor, data e método; valores excedentes são omitidos e avisados para registro posterior com confirmação. Exportação XLSX contém os itens filtrados e uma aba de pagamentos. O PDF exporta o relatório filtrado.

## Escopo e validação

Esta entrega é uma versão inicial operacional. Testes automatizados cobrem centavos, pagamentos parciais, persistência, duplicação de eventos, conflitos, exclusões, comprovantes e regras de importação. WebRTC entre redes diferentes depende da infraestrutura escolhida e deve ser validado nas redes de destino. Não foi implementada descoberta automática em segundo plano, hospedagem de TURN, notificação local de vencimentos nem os módulos avançados de propostas e comparação de fornecedores da fase 3.
