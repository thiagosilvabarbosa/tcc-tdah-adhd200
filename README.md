# A Ressonância Magnética Ajuda a Identificar o TDAH?

Código e resultados do Trabalho de Conclusão de Curso de **Thiago da Silva Barbosa** (Análise e Desenvolvimento de Sistemas, IFPE), orientado por **Luciano Cabral**.

O trabalho avalia, com validação *leave-one-site-out* e sem vazamento de dados, se a conectividade funcional de repouso (rs-fMRI) do dataset ADHD-200 acrescenta informação aos dados fenotípicos (sexo, idade e QI) na classificação de TDAH.

## Resultado principal

| Modalidade | AUC média entre sites | IC95% |
| --- | --- | --- |
| Fenótipos | 0,702 | 0,649–0,756 |
| fMRI, força por ROI | 0,503 | 0,442–0,565 |
| fMRI, 954 ROIs | 0,547 | 0,486–0,607 |
| Fenótipos + fMRI (954 ROIs) | 0,580 | 0,521–0,638 |
| fMRI, AAL (exploratória) | 0,580 | 0,521–0,639 |
| Fenótipos + fMRI, AAL (exploratória) | 0,613 | 0,556–0,670 |

Apenas os fenótipos foram estatisticamente distinguíveis do acaso na família confirmatória (p corrigido = 0,004). Nenhuma representação de fMRI superou os fenótipos.

## Conteúdo

```
TCC_ADHD_Pipeline_Final.ipynb   pipeline completo (Google Colab)
requirements.txt                dependências
resultados/                     saídas da execução usada no TCC
  resumo_final.json             resumo de todas as métricas
  *.csv                         tabelas de inferência, métricas por site, poder etc.
  figuras/                      figuras em 300 dpi
```

## Como reproduzir

1. Obtenha os dados do ADHD-200 nas fontes oficiais (consórcio ADHD-200 / INDI e Preprocessed Connectomes Project), respeitando os termos de uso. Os dados não são redistribuídos aqui.
2. Organize no Google Drive:
   - `TCC-ADHD/treino/` com os arquivos `<SITE>_phenotypic.csv`;
   - `TCC-ADHD/Dados_Oficiais/rois_1000_<site>/rois/` com as séries temporais por ROI e os arquivos AAL (`sfnwmrda*_session_1_rest_1_aal_TCs.1D`).
3. Abra o notebook no Google Colab, ajuste `BASE` na seção 2 se necessário e execute todas as células.
4. As saídas são gravadas em `TCC-ADHD/resultados_finais/`. Os testes de permutação são retomáveis: se a sessão cair, basta rodar de novo.

Sementes fixas (`SEED = 42`) garantem resultados reprodutíveis, exceto por pequenas variações numéricas entre versões de bibliotecas.

## Protocolo, em resumo

- Controle de qualidade `QC_Rest_1 = 1`; sites com uma só classe excluídos (amostra final: 543 participantes, 5 sites).
- Loop externo *leave-one-site-out*; loop interno (aninhado) só entre os sites de treino para escolher C e K.
- Imputação, padronização e seleção de atributos (ANOVA F) ajustadas apenas no treino de cada iteração.
- Regressão logística L2 com pesos de classe balanceados.
- Bootstrap pareado (2.000), permutação dos rótulos dentro de cada site e correção de Bonferroni.
- Explicabilidade com coeficientes e SHAP exato para modelo linear.

## Citação

BARBOSA, T. S. **A Ressonância Magnética Ajuda a Identificar o TDAH? Uma Avaliação Rigorosa de Aprendizado de Máquina no Dataset ADHD-200**. Trabalho de Conclusão de Curso (Tecnologia em Análise e Desenvolvimento de Sistemas) – Instituto Federal de Pernambuco, 2026.
