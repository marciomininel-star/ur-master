# 14 — Rede e Modbus TCP

**Fontes:** ICRG p.3–11; ICWB p.3–5; ICP p.8–22. As referências são páginas físicas dos PDFs.

## Ideia central

Defina quem inicia requisições, quem oferece registradores e como cada valor é codificado. Ter conexão IP não prova que o endereço, tipo ou unidade do dado está correto.

## Papéis e rede

Modbus TCP usa cliente/servidor; ICP 10 mostra o UR atuando nos dois papéis ao mesmo tempo. O cliente no PolyScope consulta um servidor externo; um PC cliente também pode acessar o servidor do UR na porta 502 descrita pelo curso.

- Cada dispositivo do exercício recebe IP único.
- No exemplo com máscara 255.255.255.0, PC e robô usam os mesmos três primeiros octetos e último diferente.
- Essa regra dos três octetos pertence ao exemplo /24, não a toda máscara/rede possível.
- `127.0.0.1` é acesso ao próprio host: no robô aponta ao robô, no PC aponta ao PC.
- Antes de propor alteração de IP, registre a interface, máscara, endereço de destino e configuração existente.

Os documentos citam aplicações de bancada; esta skill não modifica rede, firewall nem controlador.

## Contrato de registradores

| Campo a documentar | Por que importa |
|---|---|
| Dispositivo servidor e papel cliente | Evita consultar registrador na máquina errada |
| Endereço e tipo de sinal | Digital e registro numérico não são intercambiáveis |
| Leitura/escrita e dono do dado | Evita dois processos sobrescrevendo o mesmo estado |
| Codificação e escala | Registro de 16 bits não é automaticamente float em metros |
| Sinal de novo dado e confirmação | Evita repetir peça ou ler coordenadas antigas |
| Condição de perda/timeout | Evita esperar indefinidamente ou usar dado inválido |

No PolyScope do ICRG p.5, Add MODBUS Unit recebe IP do servidor; configure Type, Address e Name do sinal. A apresentação ICP 14 chama por engano o destino de “client”; a relação operacional é cliente UR → servidor consultado.

## Mapa histórico do servidor UR

ICP p.16–18 relaciona:

| Endereço | Conteúdo no curso |
|---|---|
| 128–255 | 128 registradores gerais de 16 bits |
| 400–402 | TCP x/y/z, em décimos de mm, na Base |
| 403–405 | Componentes de rotação em mrad |
| 260 / 262 | Estados isPowerOnRobot / isEmergencyStopped |

É um recorte de mapa da versão, não uma tabela universal. Não confunda endereço Modbus 128 com índice 128 de outro banco de registradores, nem bits de I/O físico com registradores de campo/RTDE.

ICP p.17 lista leitura de coils/discrete inputs/holding/input registers e escritas de bit/registro único/múltiplo, com exceções de função, endereço e valor. Na investigação, separe ausência de conexão de resposta de exceção; modificar timeout não corrige endereço inexistente.

URScript citado no ICP p.20: `read_port_bit`, `write_port_bit`, `read_port_register`, `write_port_register`. Os nomes espaçados no texto ICRG p.8 não devem ser copiados como identificadores. Usar API de banco local não significa criar automaticamente um cliente de dispositivo externo.

## Exemplo trabalhado — visão e confirmação

ICWB p.3–5 especifica câmera simulada:

1. Sensor indica peça; robô escreve 1 no registro 0 da câmera para disparar.
2. Resultado chega aos registros 128–130 do servidor UR, representando X/Y/Z.
3. A câmera escreve 300 no registro 255 para avisar que há dados novos.
4. Robô espera o aviso, lê/valida coordenadas e calcula alvo/pré-posição.
5. Após a operação prevista, confirma escrevendo 1 no registro 2 da câmera.
6. O fluxo também prevê reset do registro 255; o dono/momento exato do reset deve ser combinado para não apagar uma atualização nova.

ICP p.20 mostra divisão por 10000 para passar décimos de mm a metros; ICWB pede tratar negativos. Essa escala não decorre de qualquer registro geral: é contrato do exercício. Um exemplo do slide escreve 1 em 255, enquanto o handshake do workbook usa 300; não misture os dois avisos.

### Reconstrução da conversão, sem comunicação

```python
def coordenada_m(raw):
    if not isinstance(raw, int) or not 0 <= raw <= 65535:
        raise ValueError("registro fora de 16 bits")
    signed = raw if raw < 32768 else raw - 65536
    return signed / 10000.0
```

É Python 3 de conferência offline, sob hipótese explícita de inteiro assinado de 16 bits em complemento de dois e escala do exercício. `64536 → -1000 → -0.1 m`. O slide ICP p.18 inclui 32768 na faixa positiva; isso não é adotado para esse modelo assinado, cujo limite positivo é 32767. Confirme codificação do produtor antes de aplicar a fórmula.

## Antipadrões e conferência

- Mesmos IPs, máscara assumida ou loopback usado como IP do outro equipamento.
- Usar registro com tipo/endereço diferente do manual do servidor.
- Interpretar raw como metros ou trocar escala sem documentar.
- Aceitar aviso de dados sem validar faixa, referencial e atualidade.
- Resetar flag cedo demais ou confirmar tarefa que não terminou.
- Usar o opcional de dois robôs compartilhando área como solução de segurança pronta: o workbook só propõe a atividade.

**Conexões:** ch03 para pose/frame; ch16 para alternativa socket da câmera; ch17 para PLC e outros bancos; ch11 para paletização.


## Complemento dos manuais — 2026-10-28

SM511 p.108: read_port_register já retorna inteiro assinado. O helper de conversão do curso só se aplica a palavra bruta uint16, sem duplicar interpretação de sinal. Bancos GP, offsets de pacote e endereços Modbus são espaços distintos; ver ch28/ch29.


## Complemento CB3 e artigos de referência — 2026-10-28

MBS p.2 contém errata de signed16: usar raw>=32768 e subtrair65536 para palavra unsigned, não >32768/subtrair65535. O mapa e escalas CB2/CB3 estão em ch31; XYZ400–402 em décimos de mm difere de offset420–422 em mm. read_port_register já é signed em SM341 p.50.


## Complemento do lote ampliado — 2026-10-28

N10/N26 trazemregister_count,FC23,watchdog e valor válido antigo dentro da tolerância; update_frequency de até500Hz é capacidade daAPI da edição, não garantia de taxa da rede. Ch44 distingue cliente avançado do mapa legadoMBS/ch31.
