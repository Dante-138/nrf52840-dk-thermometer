<p align="center">
  <img src="https://img.shields.io/badge/MCU-nRF52840-blue?style=flat-square" alt="nRF52840"/>
  <img src="https://img.shields.io/badge/RTOS-Zephyr-green?style=flat-square" alt="Zephyr"/>
  <img src="https://img.shields.io/badge/SDK-nRF_Connect-red?style=flat-square" alt="nRF Connect SDK"/>
  <img src="https://img.shields.io/badge/Sensor-Temperature-yellow?style=flat-square" alt="Temperature"/>
  <img src="https://img.shields.io/badge/License-MIT-lightgrey?style=flat-square" alt="MIT"/>
</p>

# nRF52840 DK — Leitura de Temperatura Interna do Chip

Projeto desenvolvido com o **nRF Connect SDK (Zephyr RTOS)** para leitura da temperatura interna do chip nRF52840 utilizando o sensor de temperatura integrado (`TEMP` peripheral).

## Hardware utilizado

- **nRF52840 DK** (Nordic Semiconductor)

## Descrição

O firmware lê a temperatura interna do die do nRF52840 a cada 1 segundo e exibe o valor no console serial (RTT/UART). Utiliza a API de sensores do Zephyr (`sensor_sample_fetch` / `sensor_channel_get`) com o canal `SENSOR_CHAN_DIE_TEMP`.

Funcionalidades:
- Leitura periódica da temperatura a **1 Hz**
- Configuração de **alertas de threshold** (inferior e superior)
- Callback de alerta quando a temperatura ultrapassa os limites definidos

## Estrutura do projeto

```
thermometer/
├── src/
│   └── main.c                          # Código principal
├── boards/
│   ├── nrf52840dk_nrf52840.overlay     # Habilita o nó &temp e define o alias
│   └── nrf52840dk_nrf52840.conf        # CONFIG_TEMP_NRF5=y
├── CMakeLists.txt
├── prj.conf
└── README.md
```

## Como compilar e gravar

Usando o **nRF Connect for VS Code** ou via terminal com West:

```bash
west build -b nrf52840dk/nrf52840
west flash

ou

west build -b nrf52840dk/nrf52840 --pristine
west flash --runner jlink
```

## Saída esperada no console

```
Thermometer Example (arm)
Temperature device is 0x..., name is temp@4000c000
Temperature is 28.0°C
Temperature is 28.2°C
Temperature is 28.5°C
...
```

## Dependências

- [nRF Connect SDK v3.0.2](https://developer.nordicsemi.com/nRF_Connect_SDK/doc/latest/nrf/index.html)
- Zephyr RTOS (incluso no NCS)

## Configurações relevantes

| Arquivo | Configuração | Descrição |
|---|---|---|
| `prj.conf` | `CONFIG_SENSOR=y` | Habilita a subsystem de sensores |
| `prj.conf` | `CONFIG_CBPRINTF_FP_SUPPORT=y` | Suporte a ponto flutuante no printf |
| `boards/nrf52840dk_nrf52840.conf` | `CONFIG_TEMP_NRF5=y` | Driver do sensor de temperatura nRF5 |

## Autor

[Dante-138](https://github.com/Dante-138)
