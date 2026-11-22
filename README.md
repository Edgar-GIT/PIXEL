# PIXEL

**Sistema inteligente de assistência à identificação de cores** — Prova de Aptidão Profissional (PAP) — apresentação de introdução.

O PIXEL é um pequeno módulo eletrónico destacável, para acoplar a praticamente qualquer par de óculos. Ajuda pessoas com daltonismo (deficiência da visão cromática) a identificar cores e a interpretar informação visual. Tem dois modos:

- **Traffic Mode** — analisa automaticamente as imagens da câmara, deteta semáforos e o seu estado, e emite um aviso sonoro quando há uma mudança confirmada. Funciona sem comandos de voz nem botões.
- **Free Vision** — responde a perguntas por voz ("PIXEL, qual é a cor do objeto à minha frente?"), usando visão computacional e IA (Gemini).

> ⚠️ O PIXEL é um **protótipo experimental de assistência**. Não é um sistema certificado de segurança rodoviária e não decide se o utilizador pode atravessar. O utilizador deve continuar a observar o ambiente e a sinalização.

## Arquitetura

```
Óculos PIXEL (XIAO ESP32-S3 Sense, C++)  ──BLE + Wi-Fi──►  App Android (Kotlin + OpenCV)  ──HTTPS──►  Backend (Go)  ──►  Gemini API
   câmara · microfone · altifalante                          modos · deteções · voz                     API · encaminhamento
```

| Tecnologia | Responsabilidade |
|---|---|
| C++ | Firmware do ESP32-S3 |
| Kotlin | Aplicação Android |
| Go | Backend e API |
| OpenCV | Visão computacional (no telemóvel) |
| Gemini API | Interpretação de imagens e linguagem natural |
| KiCad 10 | Esquema, PCB e modelos 3D |

## Estado atual (rev. 0.1 — 22/11/2026)

| Componente | Estado |
|---|---|
| Hardware — protótipo em meia placa perfurada 35 × 50 mm (KiCad) | ✅ Desenhado; ERC/DRC sem erros; peças iniciais + divisor de tensão da bateria |
| Hardware — PCB final (KiCad) | ✅ Desenhado; ERC/DRC sem erros; **ainda não fabricado nem testado** |
| Documentação de hardware e orçamento | ✅ [`hardware/PIXEL_HARDWARE.md`](hardware/PIXEL_HARDWARE.md) · [`hardware/PIXEL_BOM.csv`](hardware/PIXEL_BOM.csv) |
| Apresentação PAP | ✅ `apresentacoes/PIXEL_PAP.pptx` + guião `apresentacoes/PIXEL_GUIAO.md` |
| Firmware, aplicação Android, backend | ⏳ Por desenvolver (fora do âmbito desta fase) |

## Estrutura do repositório

```
PIXEL/
├── README.md
├── logo.png                         ← logótipo original
├── apresentacoes/
│   ├── PIXEL_PAP.pptx               ← apresentação da PAP (editável)
│   ├── PIXEL_GUIAO.md               ← o que dizer em cada diapositivo
│   ├── assets/                      ← logótipo transparente, renders KiCad, imagens dos slides
│   └── exports/                     ← PDF da apresentação
├── hardware/
│   ├── PIXEL_HARDWARE.md            ← arquitetura, BOM, ligações, alimentação, comunicação, pendentes
│   ├── PIXEL_BOM.csv                ← lista de materiais com preços e fontes
│   ├── datasheets/                  ← datasheets consultados
│   ├── renders/                     ← protótipo (PIXEL_PROTO_*), layout e 3D da PCB final, esquemas (PDF/PNG)
│   └── kicad/
│       ├── libs/                    ← bibliotecas do projeto (Seeed OPL + modelos 3D)
│       ├── PIXEL/                   ← PCB final (54 × 24 mm) + fabrication/ (Gerbers)
│       ├── PIXEL_DEV/               ← PCB final em vista de layout (mesmo esquema)
│       ├── PIXEL_PROTO/             ← protótipo: layout final (meia placa 35 × 50 mm)
│       ├── PIXEL_PROTO_DEV/         ← protótipo: vista de ligações 70 × 50 mm
│       └── reports/                 ← relatórios ERC/DRC
└── relatorios/                      ← relatório da PAP (outra fase)
```

## Abrir os projetos KiCad

1. Instalar o **KiCad 10** (os ficheiros foram criados e verificados com o KiCad 10.0.8).
2. Abrir `hardware/kicad/PIXEL_PROTO/PIXEL_PROTO.kicad_pro` (protótipo; vista de ligações em `PIXEL_PROTO_DEV`), `hardware/kicad/PIXEL/PIXEL.kicad_pro` (PCB final) ou `hardware/kicad/PIXEL_DEV/PIXEL_DEV.kicad_pro` (layout da PCB final).
3. As bibliotecas do projeto carregam-se automaticamente, com caminhos relativos para `hardware/kicad/libs/`.

Verificação pela linha de comandos:

```bash
cd hardware/kicad/PIXEL
kicad-cli sch erc PIXEL.kicad_sch
kicad-cli pcb drc --schematic-parity PIXEL.kicad_pcb
```

## Créditos e licenças

- Símbolo, footprint e esquema de referência do XIAO ESP32-S3: [Seeed Studio OPL KiCad Library](https://github.com/Seeed-Studio/OPL_Kicad_Library) (CC BY-SA 4.0).
- Modelo 3D do XIAO ESP32-S3 Sense: Seeed Studio (pacote oficial da wiki).
- Restantes símbolos, footprints e modelos 3D: bibliotecas oficiais do KiCad.
