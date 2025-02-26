# Biblioteca ILI9341_SPI para STM32

## Sobre o Projeto
Biblioteca otimizada para controle de displays TFT com controlador ILI9341 via SPI em microcontroladores STM32. Desenvolvida como uma evolução de uma biblioteca de código aberto, com melhorias significativas em:

- Suporte a operações gráficas avançadas (formas geométricas, texto escalável)
- Otimização de desempenho com buffer SPI
- Sistema de paleta de cores personalizável
- Documentação detalhada e exemplos práticos

O código foi testado em placas STM32 com interface SPI, mas pode ser adaptado para outros microcontroladores compatíveis.

## Tecnologias Utilizadas
- **Microcontrolador:** STM32 (testado na série F4)
- **Display:** ILI9341 (240x320 pixels)
- **Ambiente de Desenvolvimento:** STM32CubeIDE
- **Bibliotecas:** HAL da STM32Cube

## Configuração de Hardware
Conexões básicas entre o STM32 e o display ILI9341:

| Função      | Pino STM32  | Pino Display |
|-------------|-------------|--------------|
| SPI_SCK     | PA5         | CLK          |
| SPI_MISO    | PA6         | MISO         |
| SPI_MOSI    | PA7         | MOSI         |
| CS          | PA4         | CS           |
| DC          | PA8         | DC           |
| RESET       | PA9         | RESET        |

## Como Usar

### 1. Instalação das Dependências
1. Adicione os arquivos ao projeto:
   - `ili9341_spi.h`
   - `ili9341_spi.c`
2. Inclua no seu código principal:
```c
#include "ili9341_spi.h"
```

## Configuração Inicial

```c
void main(void) {
  // Inicialização do display
  LCD_Init();
  
  // Configuração da paleta de cores
  set_palette(0, 255, 0, 0); // Vermelho
  set_palette(1, 0, 255, 0); // Verde
  
  // Limpa a tela com preto
  LCD_Clear(0);
}
```

## Exemplos de Uso

### Desenho de formas básicas:

```c
// Retângulo preenchido (x1, y1, x2, y2, cor)
boxfill(10, 10, 100, 50, 1);

// Círculo com borda (x, y, raio, cor)
circle(150, 80, 30, 0);
```

### Texto escalável:

```c
// Texto com escala 2x (x, y, cor_texto, fundo, string, escala)
printstr_scaled(20, 100, 0, -1, (unsigned char*)"Hello World!", 2);
```

## Código Fonte Principal

Trecho essencial da implementação:

```c
// Inicialização do SPI
void LCD_Init(void) {
  // Configuração dos pinos
  ILI9341_CS_hi();
  ILI9341_DC_hi();
  
  // Sequência de inicialização do display
  LCD_WriteComm(0xCF);
  LCD_WriteData(0x00);
  LCD_WriteData(0xC1);
  LCD_WriteData(0x30);
  
  // Configuração adicional...
}

// Função de desenho de pixel
void drawPixel(unsigned short x, unsigned short y, unsigned short color) {
  if(x < X_RES && y < Y_RES) {
    LCD_SetCursor(x, y);
    LCD_WriteData2(color);
  }
}
```

