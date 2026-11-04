# Smart NeoCare: Triagem Neonatal Nao-Invasiva por Visao Computacional

Plataforma computacional autonoma para apoio a triagem neonatal nao-invasiva de anemia e ictericia a partir de fotografias do leito ungueal de recem-nascidos.

## 1. Arquitetura Hibrida Optico-Profunda

O sistema opera integralmente no dispositivo do usuario via WebAssembly (ONNX Runtime Web), em uma cascata de dois estagios embarcados diretamente no arquivo `index.html`:

1. **Estagio 1 (Localizacao Anatomica Ungueal via YOLO11s)**:
   * Segmentacao automatica dos leitos ungueais em fotografias completas da mao do recem-nascido ou analise direta de recortes ungueais individuais.
   * Precisao de `98.2%`, sensibilidade de `96.9%` e `mAP50` de `98.3%`.

2. **Estagio 2 (Diagnostico Multimodal via SmartNeoCareDefinitiveNet)**:
   * Fusao entre o extrator convolucional `MobileNetV2` (`224x224`) e um codificador de 20 variaveis espectrofotometricas e perinatais (espaco colorimétrico `CIELAB L*a*b*`, indice de eritema, indice de absorcao optica de bilirrubina em `455 nm`, percentil 25 anti-reflexo especular e diferencial centro-borda).
   * Base de treinamento consolidada com `2.852` recortes de leitos ungueais cobrindo `143` recem-nascidos acompanhados clinicamente.
   * Desempenho no conjunto de validacao estratificada:
     * **Anemia Neonatal**: Acuracia de `92.55%`, Especificidade de `96.45%` e `AUC-ROC` de `0.9685`.
     * **Ictericia Neonatal Ativa**: Acuracia de `93.79%`, Sensibilidade de `97.37%` e `AUC-ROC` de `0.9847`.
     * **Estimativa Continua de Hemoglobina (`Hb`)**: Erro Absoluto Medio (`MAE`) de `1.60 g/dL`.
     * **Estimativa Continua de Hematocrito (`Ht`)**: Erro Absoluto Medio (`MAE`) de `4.29%`.

## 2. Modos de Operacao da Interface

* **Portal de Distribuicao e Download (`index.html`)**: Apresenta a visao geral do sistema, demonstracao tridimensional interativa, capturas de tela das etapas clinicas e termo obrigatorio de adesao juridica e conformidade com a LGPD antes da transferencia do atalho PWA.
* **Modo Aplicativo Nativo Instalado (`?pwa=1` / Standalone PWA)**: Ao acessar pelo atalho instalado na Area de Trabalho ou Tela de Inicio do smartphone, o sistema inicia diretamente em tela cheia nativa para captura de fotografias e emissao do laudo quantitativo.

## 3. Conformidade Legal, Privacidade (LGPD) e Isencao de Responsabilidade

* **Processamento 100% Local**: Toda a execucao neural ocorre na memoria local do navegador do usuario (*client-side*). Nenhuma fotografia, dado biometrico ou informacao de pacientes e transmitida ou armazenada em servidores externos, em estrita conformidade com a Lei Geral de Protecao de Dados Pessoais (Lei Federal n. 13.709/2018).
* **Natureza Experimental e Academica**: Este software possui finalidade exclusivamente cientifica e experimental, nao configurando dispositivo medico homologado pela ANVISA e nao substituindo exames laboratoriais de sangue ou a avaliacao clinica presencial de profissional medico habilitado.
