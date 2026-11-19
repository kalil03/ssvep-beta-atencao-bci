## SSVEP e ritmo beta como indicadores de atenção em BCI

Kalil Nege — RA 2409020 · OPT098 - Tópicos Especiais em Sistemas de Computação (Prof. Dr. Rodrigo Hübner) · UTFPR Campo Mourão

Neste conjunto de notebooks aplico o roteiro de extração de características ao tema da minha revisão de literatura. São estudados dois grupos de características: as do **SSVEP** (SNR e CCA) e as dos **ritmos** (beta e razão teta/beta). O mesmo roteiro é aplicado a duas bases de dados:

| Pasta | Base de dados |
|---|---|
| `avi/` | AVI SSVEP Dataset: 1 canal (Oz), 512 Hz, alvos de 6 a 10 Hz |
| `beta/` | BETA (Liu et al., 2020): 70 participantes, 64 canais, 250 Hz, 40 alvos de 8 a 15,8 Hz |

Cada pasta tem os mesmos cinco notebooks:

1. `01_carregamento_dados`: leitura dos `.mat` e organização dos dados;
2. `02_pre_processamento`: filtros, visualização e recorte das janelas;
3. `03_extracao_caracteristicas`: características de SSVEP e de ritmos;
4. `04_selecao_classificacao`: voto, SVM e seleção com RFE, com validação sem vazamento;
5. `05_analise_atencao`: relação entre SSVEP, beta e TBR.

### Principais resultados (BETA, 70 participantes, janela de 2 s, 40 alvos)

| Método | Validação | Acurácia |
|---|---|---|
| Voto CCA | sem treino | 69,2% |
| SVM | por bloco | 48,8% |
| RFE (40) + SVM | por bloco | 53,2% |
| RFE (40) + SVM | entre participantes | 70,6% |
| SVM só com ritmos | por bloco | 3,0% (acaso = 2,5%) |

### SSVEP, beta e TBR em AVI e BETA

Durante o estímulo, as duas bases apresentaram correlações fracas na mesma direção: SSVEP mais forte veio acompanhado de maior potência beta relativa e menor razão teta/beta (TBR).

| Análise | SSVEP × beta relativa | SSVEP × TBR |
|---|---:|---:|
| [AVI](avi/05_analise_atencao.ipynb), janelas agrupadas após normalização por ensaio | +0,111 | −0,110 |
| [BETA](beta/05_analise_atencao.ipynb), durante o estímulo, mediana por participante | +0,022 | −0,035 |
| BETA, ritmos pré-estímulo, mediana por participante | +0,004 | +0,009 |

A AVI mede o SSVEP por CCA, e a BETA usa SNR de banda estreita. Essas diferenças de métrica e agregação limitam a comparação direta dos coeficientes. Na BETA, durante o estímulo, beta e TBR também variaram com a frequência do alvo, o que é compatível com influência residual do SSVEP. As duas bases carecem de uma medida independente de atenção, portanto a interpretação é exploratória.

Na BETA, entre participantes, maior beta e menor TBR nos canais frontais e centrais se associaram a um quociente BCI maior (ρ em magnitude de 0,21 a 0,31). Após a correção de Bonferroni, apenas a associação com beta durante o estímulo permaneceu estatisticamente significativa.

### Como executar

As bases de dados não estão no repositório.

- **AVI**: copie `SSVEP/datasets/avi` do [`bneurd/tutoriais`](https://github.com/bneurd/tutoriais) para `avi/dados/avi`.
- **BETA**: baixe `S1.mat` … `S70.mat` em http://bci.med.tsinghua.edu.cn/download.html para `beta/dados/brutos/`.

```bash
pip install -r requirements.txt
jupyter notebook
```

Execute os notebooks de cada pasta em ordem (01 → 05). No BETA, a execução completa leva cerca de 20 minutos.
