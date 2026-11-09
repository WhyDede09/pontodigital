# Controle do carro da empresa

Site de página única para registrar a retirada e o retorno do carro da empresa, com identificação do condutor e assinatura digital. Funciona em computador e celular, sem instalação e sem servidor.

## Funcionalidades

- Relógio, data e hora em tempo real
- Registro de **retirada** com nome do condutor, setor e assinatura desenhada na tela
- Indicação de quem está com o carro, desde que horas e há quanto tempo
- Registro de **retorno** com um único botão
- Histórico com condutor, setor, retirada, retorno, tempo de uso e assinatura (últimos 30 usos exibidos)
- Tema claro e escuro automático, de acordo com o aparelho

## Como usar

1. Abra o arquivo `index.html` no navegador.
3. Para retirar o carro, preencha **nome** e **setor**, assine no campo indicado e toque em **Registrar retirada**.
4. Enquanto o carro estiver em uso, a tela mostra quem está com ele.
5. Ao devolver, toque em **Registrar retorno**. O carro fica livre para o próximo condutor.

Nome, setor e assinatura são obrigatórios para registrar a retirada.

## Arquivos

| Arquivo | Descrição |
| --- | --- |
| `index.html` | O site completo (HTML, CSS e JavaScript no mesmo arquivo) |
| `README.md` | Este documento |

## Publicação

Por ser um único arquivo estático, basta hospedá-lo em qualquer serviço de páginas estáticas (GitHub Pages, Netlify, servidor da empresa etc.) ou distribuí-lo para abrir direto no navegador.

A única dependência externa é a fonte *Bricolage Grotesque*, carregada do Google Fonts. Sem internet, o site usa a fonte padrão do sistema e continua funcionando normalmente.

## Armazenamento dos dados

Os dados são guardados no `localStorage` do navegador, na chave `carro:v1`, com esta estrutura:

```json
{
  "open": { "nome": "", "setor": "", "out": 1700000000000, "sig": "data:image/png;base64,..." },
  "trips": [
    { "nome": "", "setor": "", "out": 1700000000000, "in": 1700003600000, "sig": "data:image/png;base64,..." }
  ]
}
```

- `open` guarda a retirada em andamento (ou `null` quando o carro está livre).
- `trips` guarda os usos concluídos, mantendo os 100 mais recentes.
- Horários são timestamps em milissegundos.

### Limitações

- Os dados ficam **somente no aparelho e no navegador** em que foram registrados. Aparelhos diferentes não compartilham histórico nem o estado do carro.
- Limpar os dados do navegador apaga o histórico.
- Não há login: o controle depende do nome informado e da assinatura.

Para um controle único da empresa, com histórico compartilhado, é necessário trocar o `localStorage` por um banco de dados ou API.

## Personalização

- **Cores e tema:** variáveis CSS no início do arquivo (`--go`, `--back`, `--busy`, `--bg` e outras).
- **Limite do histórico:** altere `slice(-100)` (armazenado) e `slice(0,30)` (exibido) no JavaScript.
- **Tornar a assinatura opcional:** remova a verificação `if(!signed)` na função do botão `btnOut`.
- **Novos campos** (placa, quilometragem, destino): adicione o campo no formulário, inclua o valor no objeto `db.open` e na tabela do histórico.

## Compatibilidade

Navegadores atuais (Chrome, Edge, Firefox e Safari), no computador e no celular. A assinatura funciona com mouse, toque e caneta.
