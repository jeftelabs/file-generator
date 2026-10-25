# Guia de uso

## Instalação e importação

O pacote é ESM para Node.js 18.16.0 ou superior:

```sh
npm install @jeftelabs/file-generator
```

```ts
import fileGenerator from '@jeftelabs/file-generator'
```

O export default expõe sete métodos assíncronos. Cada um recebe o tamanho pretendido e, opcionalmente, o nome do arquivo:

```ts
const file = await fileGenerator.generatePng('1mb', 'avatar.png')
```

## Resultado

Cada chamada resolve para `{ contents, size, name, mime }`:

- `contents`: `Buffer` usado como corpo do arquivo.
- `size`: tamanho real do buffer em bytes (`contents.length`).
- `name`: nome fornecido ou UUID com extensão do formato.
- `mime`: tipo MIME correspondente ao método escolhido.

## Tamanhos aceitos

O argumento `fileSize` aceita número (bytes) ou string de tamanho interpretada pela dependência `bytes`, como `'1mb'` e `'300kb'`. Uma expressão textual que não possa ser convertida gera erro `Invalid fileSize value ...`. Verifique os limites esperados do seu caso de teste antes de passar valores numéricos grandes: o buffer inteiro é alocado em memória.

O tamanho final não fica abaixo do tamanho da fixture inicial. Para CSV, a amostra de conteúdo também impõe o tamanho mínimo. Logo, `size` pode exceder o valor solicitado se ele for menor que o template.

## Uso em testes de upload

Use os dados devolvidos para preencher o campo de arquivo da biblioteca ou cliente HTTP do projeto. Para interfaces multipart de Node que aceitam `Blob`:

```ts
const file = await fileGenerator.generatePng('1mb', 'avatar.png')
const form = new FormData()
form.append('avatar', new Blob([file.contents], { type: file.mime }), file.name)
```

Adapte o exemplo ao contrato da biblioteca multipart utilizada; o pacote não depende nem escolhe um cliente HTTP.

## Limites do conteúdo

As fixtures servem como bytes iniciais e podem passar pela identificação de tipo. Ao aumentar o tamanho, `toBuffer` usa a fixture como padrão repetido para ocupar o espaço. Essa operação não atualiza estruturas internas como diretório ZIP, dimensões de imagem ou tabelas de conteúdo. Os resultados são adequados para exercitar upload, tamanho e sniffing; não são arquivos completos para abrir ou processar sem erros.
