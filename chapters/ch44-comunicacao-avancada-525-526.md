# 44 — Modbus avançado, RTDE e Flexible EtherNet/IP

**Fontes:** N10/N26 p.117,120–129,175–182. Complementa mapas legadosch31 ePLCch17; não troca banco ou papel por semelhança numérica.

## Modbus cliente: quantidade, validade e falha

modbus_add_signal inclui register_count e modo sequencial. A quantidade define leitura/escrita múltipla e o tipo define functioncodes. modbus_add_rw_signal usaFC23 após primeira escrita, tendo fase inicial de leituraFC3; servidor precisa suportar o contrato. Definir read/writeaddresses separadamente: exemploN10p.122 descreve faixa de escrita inconsistente comwrite_address15. Calcular do parâmetro e confirmar servidor, sem copiar comentário.

modbus_get_signal_status pode devolver último valor válido dentro do tempo do watchdog sem lançar erro. Logo, valor disponível não prova dado recente. Usar estado de erro e tempo desde inválido conforme assinatura da versão; supervisionar idade e definir resposta da aplicação. Aumentar watchdog para ocultar desconexão não é solução.

modbus_set_signal_update_frequency p.127 aceita até500 Hz e0/-1 para acíclico no contexto documentado. Isso não garante taxa física, latência ou compatibilidade do servidor. Sequencial pode ser necessário para alguns dispositivos. modbus_reset_connection por padrão bloqueia enquanto houver erros; não inserir no caminho de tempo crítico sem política de saída.

Há divergência entre cabeçalho e descrição de parâmetro emmodbus_get_error e erro tipográfico “modus” na prosa. Antes de fornecer chamada com argumento opcional, conferir assinatura efetiva na versão alvo. Não promover texto inconsistente a API garantida.

## RTDE no próprio URScript

get_rtde_value(key), N10 p.117, lê buffer de saídaRTDE um passo atrás. Tipo depende do campo;sync é sugerido antes de leitura. Isso não torna cada campo instantâneo nem substitui sincronização de evento. Não inventar chaves ausentes; a lista externa referida pelo manual precisa ser consultada quando necessária.

## Flexible EtherNet/IP Adapter

N10 p.175 situa custom assemblies desde5.16, com serviçoXML-RPC local na porta40000. Não é socket de processo nemDashboard. Instâncias nativas100/112 não podem ser sobrescritas. Scanner/CNC pode exigirEDS correspondente; conexãoativa não valida layout.

O manual descreve operações emthreads não realtime, com teste internoRPI5 ms resultando40 ms médio/90 ms máximo; recomenda não usar assemblies customizadas comRPI<20 ms e tamanho recomendado até500bytes. São observações/recomendações daquela versão, não promessa de desempenho ou limites do protocolo padrão inteiro.

Layout: ID/direção/tamanho → labels com offset/tipo → handlesreader/writer → handshake. Leitura de vários labels retorna struct; escrita de struct garante os labels juntos naquela atualização, não entrega física aoPLC. Escrita por nome individual tem versão mínima própria5.22/10.10. Overflow de tipo pode parar programa.

N10 p.177–179 diferencia offsets embytes, bit_number ebitfield_width; arrays consomem length vezes tamanho do tipo, strings reservam terminadorNULL. Não confundir offset deassembly com índiceGP, endereçoModbus ouPROFINET. clear zera memória da instância e pode alterar comandos: não usar como limpeza inocente.

**Exercício:** projetar contrato de dois labels e validar tamanhos/overlaps offline. Nenhum handle, porta ou conexão é aberto por consultar este capítulo.
