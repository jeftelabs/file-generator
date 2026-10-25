# Referência da API

O export default de `@jeftelabs/file-generator` é um objeto com as funções abaixo. Todas retornam uma `Promise` do mesmo formato.

## Métodos

| Método | MIME | Extensão padrão |
| --- | --- | --- |
| `generateCsv(fileSize, fileName?)` | `text/csv` | `.csv` |
| `generateDocx(fileSize, fileName?)` | `application/vnd.openxmlformats-officedocument.wordprocessingml.document` | `.docx` |
| `generateGif(fileSize, fileName?)` | `image/gif` | `.gif` |
| `generateJpg(fileSize, fileName?)` | `image/jpeg` | `.jpg` |
| `generatePdf(fileSize, fileName?)` | `application/pdf` | `.pdf` |
| `generatePng(fileSize, fileName?)` | `image/png` | `.png` |
| `generateXlsx(fileSize, fileName?)` | `application/vnd.openxmlformats-officedocument.spreadsheetml.sheet` | `.xlsx` |

`fileSize` é `number | string`: número significa bytes; string é analisada pela biblioteca `bytes`. `fileName` é uma string opcional e é devolvida como nome, sem validação/normalização de extensão. Quando omitida, gera-se `randomUUID()` com a extensão da tabela.

## Tipo do retorno

```ts
{
  contents: Buffer
  size: number
  name: string
  mime: string
}
```

`size` é sempre `contents.length`, portanto representa o buffer final após aplicar o mínimo da fixture. Os geradores de CSV e binários têm os mesmos metadados, embora usem estratégias diferentes para obter os bytes iniciais.

## Erros e limites

Uma expressão string não reconhecida por `bytes` lança `Error` com a mensagem `Invalid fileSize value "...". Expected value to be a number representing bytes or a string expression`. Valores numéricos fora do intervalo permitido por `Buffer.alloc` podem resultar em erro nativo de range/alocação. O pacote não intercepta nem converte esses erros.

Tamanho menor que os bytes da fixture é elevado ao tamanho mínimo. A fixture é repetida para preencher buffers maiores; tipo detectável não implica conteúdo internamente completo. Ver [arquitetura](../architecture/overview.md#limites-de-validade).
