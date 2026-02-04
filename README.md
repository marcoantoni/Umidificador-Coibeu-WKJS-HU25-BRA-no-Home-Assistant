# Integração do Umidificador Coibeu WKJS-HU25-BRA no Home Assistant com a integração LocalTuya

Este repositório tem como objetivo documentar o processo de integração do umidificador de ar [Coibeu WKJS-HU25-BRA](https://www.mercadolivre.com.br/umidificador-coibeu-de-ar-ultrassonico-inteligente-tuya25l/up/MLBU3500191319?pdp_filters=item_id:MLB5826613250) ao **Home Assistant**, utilizando a integração **Local Tuya**.

Ao adquirir o dispositivo, por ser compatível com o aplicativo Tuya/Smart Life, a expectativa era que ele funcionasse normalmente com a integração oficial. No entanto, na prática, o dispositivo até é adicionado ao Home Assistant, mas os controles não são são exibidos, pois o dispositivo não é suportado.

Resolvi compartilhar a configuração, pois não encontrei nada a respeito sobre isso e tive que descobrir o funcionanmento com base em testes. Com isso, torna-se possível controlar corretamente funções como:
- Liga / desliga
- Intensidade do umidificador
- Iluminação (LED) e troca de cor

> ⚠️ **Observação:**  
> Este tutorial não é oficial e não possui vínculo com a Tuya ou com o fabricante do dispositivo. As configurações aqui apresentadas foram obtidas por testes práticos e podem variar conforme firmware ou região.

---

## 1. Adicionando o dispositivo no Home Assistant

O primeiro passo é adicionar o umidificador **Coibeu WKJS-HU25-BRA** ao Home Assistant utilizando a integração **Local Tuya**.

Nesta etapa, eu precisei inserir o endereço IP do dispositivo, pois não foi descoberto automaticamante. Além disso, precisei  informar a **Local Key** do dispositivo. Em condições normais, essa chave é descoberta automaticamente pelo Home Assistant. Porém, **neste caso específico, a Local Key não foi identificada automaticamente na primeira tentativa**. Essa informação pode ser obtida acessando a **Tuya Developer Platform**, onde é possível visualizar os detalhes do dispositivo previamente vinculado à conta Tuya/Smart Life.

---
## 2. Configurando cada DPS de maneira individual

A seguir estão descritos os DPS utilizados neste dispositivo e como cada um deve ser configurado manualmente no Home Assistant.

### Função Liga / desliga geral

O **DPS 1** corresponde à função de **liga / desliga** do dispositivo como um todo. Ao ser acionado, ele controla tanto a iluminação quanto a função de umidificação.

Parâmetros de configuração:
- **Tipo:** `switch`
- **DPS (ID):** `1`
- **Nome fantasia:** umidificador_power - Ajuste todos os nomes conforme sua preferência.

---

### Liga / desliga da umidificação (spray)

O **DPS 2** controla exclusivamente a função de umidificar. No aplicativo Tuya, essa função é identificada como *spray*.

Parâmetros de configuração:
- **Tipo:** `switch`
- **DPS:** `2`
- **Nome fantasia:** umidificador_nevoa 

---

### Liga / desliga da iluminação

O **DPS 5** é responsável por controlar apenas a iluminação (LED) do dispositivo.

Parâmetros de configuração:
- **Tipo:** `switch`
- **DPS:** `5`
- **Nome fantasia:** umidificador_led_power

---

### Seleção da cor da iluminação

O **DPS 6** controla a **troca de cores da iluminação**. Esse DPS utiliza valores numéricos para representar cada cor (por exemplo, o valor `8` corresponde à cor azul), o que exige um mapeamento manual para facilitar o uso no Home Assistant.

Parâmetros de configuração:
- **Tipo:** `select`
- **DPS:** `6`
- **Nome fantasia:** cor_led_umidificador
- **Entradas válidas (separadas por `;`):**  
  `1;2;3;4;5;6;7;8;9`
- **Opções fantasia ao usuário (separadas por `;`):**  
  `Aleatório;Branco;Vermelho;Laranja;Amarelo;Verde;Ciano;Azul;Roxo`

---

### DPS 23 – Controle da intensidade da névoa

O **DPS 23** é responsável pelo **controle da intensidade da névoa** gerada pelo umidificador.

Parâmetros de configuração:
- **Tipo:** `select`
- **DPS:** `23`
- **Nome fantasia:** umidificador_controle_intensidade
- **Entradas válidas (separadas por `;`):**  
  `level_1;level_2;level_3`
- **Opções fantasia ao usuário (separadas por `;`):**  
  `Fraco;Médio;Forte`
