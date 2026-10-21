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

Beta e TBR não identificam o alvo e não acompanham o SSVEP dentro de cada participante. Entre participantes, mais beta e menos TBR frontal aparecem junto com um quociente BCI maior (ρ ≈ 0,2–0,3), mas esse indício fica fraco depois da correção para múltiplos testes.

### Como executar

As bases de dados não estão no repositório.

- **AVI**: copie `SSVEP/datasets/avi` do [`bneurd/tutoriais`](https://github.com/bneurd/tutoriais) para `avi/dados/avi`.
- **BETA**: baixe `S1.mat` … `S70.mat` em http://bci.med.tsinghua.edu.cn/download.html para `beta/dados/brutos/`.

```bash
pip install -r requirements.txt
jupyter notebook
```

Execute os notebooks de cada pasta em ordem (01 → 05). No BETA, a execução completa leva cerca de 20 minutos.
