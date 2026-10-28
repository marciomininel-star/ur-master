# 54 — Atualização5.25/5.26, backup e dados persistentes

**Fontes:** N08 p.85–100; N09 p.57–66; N12 p.247–252; N24/N25edições5.25. Não executar atualização, restauração ouMagicFile por uma consulta à skill.

## Caminho de atualização tem conflito documentado

N08 p.86/N09 p.58 repetem caminho incremental até5.5 para versões antigas e salto posterior. N12 p.248 descreveCB5.5 ou anterior comURUP e atualização direta de qualquerPolyScope 5 para5.26. Essas instruções sobrepõem-se e não são idênticas.

Apresentar o conflito, identificarhardware/versãoorigem/destino e confirmar notas/procedimento específico antes de fornecer plano executável. Não usar a frase “qualquerversão5” como licença para ignorar restrições de hardware, firmware, backups ou versão de fabricação. A presença de5.26 no arquivo não prova que é a versão mais recente disponível.

## Compatibilidade dos backups muda

N12 p.248 afirma que backups anteriores5.25 não são compatíveis com5.26 e orienta, para preservar compatibilidade, passar por5.25, gerar novosbackups, atualizar5.26 e gerar novamente. Preservar também backup original e inventário de dados; “criar novo” não significa apagar o anterior.

N08 p.91/N09 p.62 distinguem backup de sistema de programas/instalações, citando cerca4 GB para o sistema. SupportFile tem finalidade diagnóstica e tamanho/fluxo próprios. Não interpretar a palavra “backup” usada genericamente numa seção de suporte como garantia de restauração integral.

## PolyScope X tem outro fluxo

N08 p.92/N09 p.63 dizem quePolyScope X não tem a mesma função de backup de sistema no contexto da edição, mas exporta programas eSupportFiles. Isso não equivale a imagem restaurável doPolyScope 5 nem a uma afirmação eterna sobre versõesX futuras. Os guias contemplamSW10.12 onde indicado; não presumir suporte de toda funcionalidadeX numScript DirectoryPoly5.

Downgrade por pacote antigo difere de restaurar backup compatível. A fonte desaconselha downgrade, especialmente abaixo de fabricação e entre dados/firmware incompatíveis. Preservar calibração, serial e arquivos de instalação; não restaurar outro braço.

## MagicFiles: comportamento depende de segurança da versão

N12 p.249 descreveMagicFiles como scriptsUSB com amplos privilégios de sistema e diz que a execução é desabilitada por padrão. Isso complementaMAG2013/ch23, onde inserção da mídia desencadeava ação no contexto configurado. Não habilitarRunMagicFiles para fazer um simples exemplo.

Separar backup/log/configuração/screenshot de upload. Conferir origem e conteúdo do script, direção, dados afetados e estado da máquina antes de execução solicitada. Deixar arquivos na raiz deUSB pode ter efeito posterior em outro ativo compatível.

## Validar migração

Preservar baseline e changelog da aplicação; verificarAPI,Motion Version,blend/ciclo, carga/transição, reservas de registradores, URCaps, I/O e segurança. Mudança funcional requer teste do processo/retomada, não só conclusão do instalador. OLTS descrito emN08p.85/N09 p.57 refere-se à estratégia documentada; consultar suporte oficial se a pergunta for sobre releaseLTS vigente.

**Entregável:** matrizorigem/destino/hardware, conflitos resolvidos por referência específica, backups verificáveis, testes e plano de recuperação. Nenhuma mídia foi formatada, imagem gravada ou configuração alterada na incorporação do corpus.
