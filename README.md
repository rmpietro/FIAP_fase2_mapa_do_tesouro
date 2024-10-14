# Farm Tech - Documentação do Modelo de Dados

## Visão Geral

Projeto de modelagem de dados para um sistema de armazenamento (banco de dados) e análise dos dados coletados pelos sensores para ajustar a quantidade de produtos e água aplicados na plantação.
Este modelo de dados permite um gerenciamento detalhado de cultivos agrícolas, desde o planejamento até o monitoramento, incluindo aplicação de insumos e leituras de sensores para otimização da produção.

## MER - Entidades

### TIPO_CULTURA

Armazena informações sobre os diferentes tipos de culturas geridas pelo modelo.

| Atributo     | Tipo         | Descrição                              |
| ------------ | ------------ | -------------------------------------- |
| id_cultura   | NUMERIC(6)   | Identificador único do tipo de cultura |
| nome         | VARCHAR(100) | Nome da cultura                        |
| data_plantio | Date         | Data de plantio da cultura             |

### AREA_CULTIVO

Contém dados sobre as áreas específicas (com extensão e localização) de cada cultura e sob observação de um ou mais sensores (a mesma cultura pode ter várias áreas atribuídas e vários sensores instalados).

| Atributo        | Tipo          | Descrição                                  |
| --------------- | ------------- | ------------------------------------------ |
| id_area         | NUMERIC(6)    | Identificador único da área de cultivo     |
| id_cultura      | NUMERIC(6)    | Referência ao tipo de cultura cultivada    |
| area_extensao   | NUMERIC(20,2) | Extensão da área de cultivo                |
| end_localizacao | VARCHAR(255)  | Endereço ou localização da área de cultivo |

### TIPO_INSUMO

Lista os diferentes tipos de insumos utilizados no cultivo e que são alvo de recomendações e previsões para ajustes nas diferentes culturas.

| Atributo  | Tipo         | Descrição                             |
| --------- | ------------ | ------------------------------------- |
| id_insumo | NUMERIC(6)   | Identificador único do tipo de insumo |
| nome      | VARCHAR(100) | Nome do insumo                        |
| descricao | VARCHAR(255) | Descrição detalhada do insumo         |

### PREVISAO_AJUSTE

Registra previsões e ajustes para aplicação de insumos em áreas específicas. Essas previsões podem ser ajustes e aplicações de insumos imediatos ou para uma data futura.

| Atributo         | Tipo          | Descrição                              |
| ---------------- | ------------- | -------------------------------------- |
| id_previsao      | NUMERIC(6)    | Identificador único da previsão/ajuste |
| id_area          | NUMERIC(6)    | Referência à área de cultivo           |
| id_insumo        | NUMERIC(6)    | Referência ao tipo de insumo           |
| data_ajuste      | Date          | Data do ajuste ou previsão             |
| acao_recomendada | VARCHAR(255)  | Descrição da ação recomendada          |
| quantidade       | NUMERIC(10,2) | Quantidade de insumo prevista/ajustada |
| unidade_qtd      | CHAR(10)      | Unidade de medida da quantidade        |

### REG_APLICACAO_INSUMO

Registra as aplicações efetivas de insumos nas áreas de cultivo.

| Atributo        | Tipo          | Descrição                                  |
| --------------- | ------------- | ------------------------------------------ |
| id_aplic_insumo | NUMERIC(6)    | Identificador único da aplicação de insumo |
| id_area         | NUMERIC(6)    | Referência à área de cultivo               |
| id_insumo       | NUMERIC(6)    | Referência ao tipo de insumo aplicado      |
| data_aplicacao  | Date          | Data da aplicação do insumo                |
| produto         | VARCHAR(100)  | Nome do produto aplicado                   |
| quantidade      | NUMERIC(10,2) | Quantidade de insumo aplicada              |
| unidade_qtd     | CHAR(10)      | Unidade de medida da quantidade aplicada   |

### SENSOR

Contém informações sobre os sensores utilizados para monitoramento das áreas de cultivo.

| Atributo  | Tipo         | Descrição                     |
| --------- | ------------ | ----------------------------- |
| id_sensor | NUMERIC(6)   | Identificador único do sensor |
| id_area   | NUMERIC(6)   | Referência à área monitorada  |
| descricao | VARCHAR(255) | Descrição do sensor           |
| tipo      | VARCHAR(50)  | Tipo de sensor                |
| modelo    | VARCHAR(100) | Modelo do sensor              |

### REG_LEITURA_SENSOR

Armazena as leituras realizadas pelos sensores nas áreas de cultivo.

| Atributo        | Tipo          | Descrição                      |
| --------------- | ------------- | ------------------------------ |
| id_leitura      | NUMERIC(6)    | Identificador único da leitura |
| id_area         | NUMERIC(6)    | Referência à área monitorada   |
| id_sensor       | NUMERIC(6)    | Referência ao sensor utilizado |
| data_leitura    | Date          | Data da leitura                |
| valor_leitura   | NUMERIC(10,5) | Valor registrado na leitura    |
| unidade_leitura | CHAR(10)      | Unidade de medida da leitura   |

## Relacionamentos

1. TIPO_CULTURA (1) -> (N) AREA_CULTIVO

   - Uma cultura pode ser cultivada em várias áreas.

2. AREA_CULTIVO (1) -> (N) REG_APLICACAO_INSUMO

   - Uma área pode receber múltiplas aplicações de insumos.

3. TIPO_INSUMO (1) -> (N) REG_APLICACAO_INSUMO

   - Um tipo de insumo pode ser aplicado várias vezes em diferentes áreas.

4. AREA_CULTIVO (1) -> (N) REG_LEITURA_SENSOR

   - Uma área pode ter múltiplas leituras de sensores.

5. SENSOR (1) -> (N) REG_LEITURA_SENSOR

   - Um sensor pode realizar várias leituras.

6. AREA_CULTIVO (1) -> (N) SENSOR

   - Uma área pode ter vários sensores instalados.

7. AREA_CULTIVO (1) -> (N) PREVISAO_AJUSTE

   - Uma área pode ter múltiplas previsões ou ajustes de insumos.

8. TIPO_INSUMO (1) -> (N) PREVISAO_AJUSTE
   - Um tipo de insumo pode estar associado a várias previsões ou ajustes.
