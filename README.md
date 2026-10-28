# UR_Master

Skill técnica em português para Universal Robots CB3, e-Series e UR Series, organizada em 54 capítulos com referências por modelo, versão e página.

Inclui programação PolyScope/URScript, integração PLC e redes, instalação, calibração, diagnóstico, manutenção e acessórios. Conteúdo sintetizado dos documentos fornecidos com book-to-skill: https://github.com/virgiliojr94/book-to-skill.

## Uso

Com acesso autenticado ao repositório privado, instale com `npx skills add https://github.com/marciomininel-star/ur-master --skill ur-master`. Também é possível instalar a pasta como `ur-master` no diretório de skills do seu agente e invocar `$ur-master`.

Exemplos:

- `$ur-master compare Motion Version 2 e OptiMove`
- `$ur-master explique a calibração por key-waypoints no CB3`
- `$ur-master consulte o manual do modelo antes de explicar o código de erro`

## Estrutura

- `SKILL.md`: instruções e índice.
- `chapters/`: conhecimento técnico por tema.
- `references/`: PDFs disponíveis e índices para consulta dirigida.
- `assets/`: imagens de referência.
- `sources.md` e `source_manifest.json`: proveniência e identificação dos documentos.
- `glossary.md`, `patterns.md` e `cheatsheet.md`: apoio à consulta.

A maioria dos PDFs está em `references/`; confirme modelo e versão antes de consultar. O manifesto registra caminhos originais da máquina de origem, que podem não existir em outra instalação.

## Fontes e uso

A skill contém sínteses técnicas e documentos de terceiros, cujos direitos permanecem com seus respectivos titulares. 
