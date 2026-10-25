# Arquitetura e fluxo

## Objetivo

Fornecer buffers de teste com metadados de upload previsíveis, evitando criar arquivos temporários para a maioria dos cenários. Os bytes iniciais permitem identificar formatos binários; a aplicação que testa controla transporte e persistência.

## Fluxo público

1. `index.ts` importa os geradores de `src/files/<format>/generate.ts` e os expõe pelo export default.
2. A função escolhe MIME e nome. Os geradores binários e CSV usam UUID quando não recebem `fileName`.
3. Para DOCX, GIF, JPG, PDF, PNG e XLSX, o gerador lê `fake.<ext>` relativa ao módulo. CSV constrói uma pequena amostra em memória.
4. `toBuffer(fileSize, initialBytes)` converte string com `bytes`, verifica se a expressão foi analisada, e aloca um Buffer preenchido com os bytes iniciais.
5. Se o tamanho desejado for menor do que a amostra, aloca pelo menos o tamanho da amostra; o retorno usa o tamanho final do Buffer.
6. `build` executa `copy:files` após compilar, copiando as fixtures para o bundle distribuído, pois os geradores fazem leitura em runtime.

## Organização

- `index.ts`: interface pública única.
- `src/files/utils.ts`: conversão de tamanho, preenchimento e caminho físico derivado da URL do módulo.
- `src/files/csv/`: geração de CSV a partir de uma amostra textual curta.
- `src/files/{docx,gif,jpg,pdf,png,xlsx}/`: geradores e fixtures binárias.
- `tests/`: contrato público e utilitário de buffer.

## Limites de validade

`file-type` verifica padrões iniciais (magic bytes), não a integridade completa do documento. A repetição da fixture preenche o comprimento requisitado, mas não altera estruturas internas do formato. Assim, um buffer pode ser detectado como PDF, PNG ou XLSX e ainda falhar ao ser lido/renderizado como arquivo completo. O CSV é uma pequena amostra repetida, não um dataset proporcional ao tamanho solicitado. O pacote gera buffers; só o consumidor decide se/onde gravá-los.

## Considerações de recursos

Cada saída ocupa memória do processo na dimensão do buffer final. Evite tamanho desnecessariamente alto ou geração concorrente de muitos buffers. A leitura das fixtures requer que os assets estejam junto ao módulo publicado; `copy:files` é parte do contrato de build e publicação.
