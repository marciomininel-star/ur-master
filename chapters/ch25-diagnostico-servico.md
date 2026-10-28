# 25 — Diagnóstico, serviço e escalonamento

**Fontes:** SVC4 p.6, 9–12, 21, 61–69, 72–178, 233; IG p.11; U5E p.68–69. SVC4 rev.4.0.0 cobre UR3e/5e/10e/16e nas configurações da edição.

## Método: evento → evidência → causa → intervenção → validação

Colete código completo/subcódigo, mensagem, horário, programa/linha, estado, ferramenta/carga ativa, repetição e mudanças recentes. Preserve logs e Support File antes de apagar evidências. Diferencie parada protetiva, safeguard, emergência, falha elétrica, rede e erro de programa.

Para C153, IG p.11 orienta verificar contato inesperado, TCP, carga/CoG, montagem e aceleração. O código isolado não prova colisão nem junta defeituosa. Compare condições vazio/carregado e momento da falha sem provocar uma situação perigosa para reproduzi-la.

SVC4 p.72–173 contém códigos e ações: localizar família e subcódigo no original. Texto pode perder alinhamento; não gerar correção universal pelo número isolado. Navegação em [manual-navigation.md](../references/manual-navigation.md).

## Parada protetiva não é estado normal de produção

SVC4 p.177–178 rejeita reconhecimento/reinício automáticos: investigar causa e fazer reset deliberado após correção. Paradas diárias indicam problema de projeto/aplicação. Melhorar caminho, carga e transições é diferente de aumentar limites de segurança para ocultar sintoma.

Capacidade de unlock/play do Dashboard não autoriza pular investigação. Evitar loops de desbloqueio e instruções para continuar sem diagnóstico. Pausa por watchdog também exige condição de retomada e conferência do estado físico.

## Serviço depende de procedimento e qualificação

SVC4 pede pessoal qualificado e consulta à engenharia de serviço certificada antes de reparo mecânico/elétrico. Isole energias periféricas; parar programa não elimina ferramenta pneumática, carga suspensa ou energia armazenada. U5E p.69 menciona tensão retida na fonte por horas: tempo curto de descarga de outro procedimento não autoriza desmontá-la.

SVC4 p.9–12: proteção ESD inclui embalagem, pulseira íntegra no ponto indicado e manuseio pelas bordas, sem tocar contatos. ESD protege componentes; não substitui isolamento elétrico.

Não usar catálogo histórico como lista atual de compra. Trocar junta, placa ou braço pode exigir compatibilidade de hardware/software e calibração. Preservar calibração é diferente de recalibrar; procedimento e ferramental devem ser confirmados.

## Pacote para suporte

Entregar identidade, versão, mensagem completa, sequência da ocorrência, configurações, mudanças e evidências. SVC4 p.233 trata embalagem, retirada de acessórios e contaminação; materiais perigosos exigem documentação e condições de limpeza adequadas. Confirmar atendimento e peças atuais quando necessário.

Consulta à skill não solicita ticket, mensagem, upload, envio ou operação física. Para diagnóstico, entregar hipóteses ordenadas, evidência faltante e condição da próxima ação.

**Exercício:** parada na aceleração apenas com peça. Resultado: verificar carga/CoG/inércia, contato e montagem, preservar evidência e evitar resets automáticos.


## Complemento CB3 e artigos de referência — 2026-10-28

CAL separa calibração por placa, dualrobot e correção de programa. Requer método, ferramental e avaliação, sem transformar reset de calibração em rotina de diagnóstico. Ver ch33; consulta ao documento não solicita alteração de preset/arquivos persistentes.


## Complemento do lote ampliado — 2026-10-28

N22 éErrorCodesDirectory5.26 com índice de ocorrências por código/página. Causa deve ser investigada antes de reset;SafeBrakeURSeries exige cuidado adicional. Verch38/52.
