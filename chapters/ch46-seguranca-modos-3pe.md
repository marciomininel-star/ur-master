# 46 — Modos,3PE e geometria de segurança por versão

**Fontes:** N19guia3PETP2020 p.7–10,14–20; N12Software Handbook5.26, seção2 e p.245–246; User ManualsN28–N38. Não fundir seleçãoLocal/Remote com Manual/Automatic.

## Quatro dimensões que se confundem

- Modo operacionalManual/Automatic define disponibilidade de operação e habilitação.
- PerfilLocal/Remote define origem permitida dos comandos.
- Modo de segurançaNormal/Reduced/Recovery descreve limites/estado próprios.
- Estado do3PE descreve botão/habilitação, sem garantir caminho livre.

Pergunta sobre um deles pode depender dos demais; declarar os estados antes de propor sequência. Saída de estado comum via rede não vira canal seguro.

##3PE no guia fornecido

N19 p.8 limita a certificação descrita às caixase-Series e diz software “newer thanV5.8.0”; não traduzir para≥5.8.0 sem ressalva. Manuais mais novos ampliam contexto de modelos, mas não por extrapolação do guia2020. Confirmar combinação pendant/controlador/versão.

P.19 define três posições emManual: solto interrompe movimento sem necessariamente tirar potência/travar freios; pressão leve permite execução; pressão total provoca3PEStop. EmAutomatic, o movimento não exige ação no botão conforme o guia. Portanto,3PE não funciona como emergência universal em qualquer modo.

Acionamento deFreedrive por3PE tem sequência própria; não confundir manter leve com o gesto paraFreedrive. Quando3PETP não está conectado, a configuração pode exigir habilitador externo confirmado; não declarar ausência de pendant como eliminação da função.

## Configuração envolve validação

N19 p.18 descreve seleção3PEEnabled e confirmação de configuração apósrestart. Isso é conteúdo técnico; não solicita mudança de segurança do ativo. Preparar diagrama e ensaios da combinação correta. N16OEM p.32–34 apresenta alternativas de habilitador e necessidade de emergência externa quando pendant não conectado.

Planos/esferas,ToolDirection eSafeHome devem representar geometria da ferramenta e risco. Curva de payload,MotionLimits eforce_mode não substituem limites de segurança. Não assumir queSoftTransition/SmoothTransition autoriza maior energia ou muda categoria certificada.

## Uso de documentação normativa

Os manuais incluem certificações, padrões aplicados e avisos da edição. Cite como declaração da fonte, sem afirmar conformidade vigente da célula. Projeto/validação devem considerar máquina completa, periféricos, energias e falhas. Se usuário pedir norma ou exigência atual, verificar fonte oficial aplicável em vez de reciclar o ano do manual.

**Exercício:** explicar por que soltar3PE pode parar movimento sem tornar seguro desconectar o cabo do robô; identificar diferença entre habilitação, freio e isolamento elétrico.
