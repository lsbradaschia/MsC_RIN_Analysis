# NMSIM Protocol - Versão 0.1
# Protocolo de Metodologia reproduzido com G6PC1 - NMSIM

## 1. Informações Gerais
* Análise de Trajetórias com NMSIM

* Protocolo para geração de dados de dinâmica molecular (trajetórias, RMSD, RMSF) utilizando o servidor web NMSIM, processamento de output gerado e transformação em grafo (Bash, Python, RING4.0).
    * **NOTA:** Os scripts `Python` utilizados para concatenação de arquivos em `.csv` são atualmente aplicados no modo [**`CSV Compile`**]() da ferramenta [**`HufflePlots`**](). ADICIONAR OS LINKS DEPOIS

* **Onde encontrar e Como Rodar:**
    * **NMSIM Web Server:** [https://cpclab.uni-duesseldorf.de/nmsim/main.php](https://cpclab.uni-duesseldorf.de/nmsim/main.php)
        * Todo o processo foi realizado a partir do servidor web, utilizando tipo de simulação **`Small scale motions`** default. 
    * **Input NMSIM:**
        * O NMSIM web aceita apenas arquivos no formato **`.PDB`**. Em casos de proteínas em formato **`.CIF`**, foi feita a conversão de arquivo utilizando [**`PyMOL`**](https://www.pymol.org/) ou [**`ChimeraX`**](https://www.cgl.ucsf.edu/chimerax/).


## 2. Arquivos Gerados pelo NMSIM

* Após a conclusão da simulação, o NMSIM fornece 3 arquivos de download. Os arquivos utilizados neste fluxo de trabalho são:
    * **Trajetória:** Arquivo de trajetória, nomenclatura padrão `[NOME]_trajectory.pdb.gz`. Contem todos os *models* (trajetórias). Utilizando o `Small scale motions`, 500 trajetórias são geradas.
    * **RMSD:** Arquivo de cálculo RMSD referente a trajetória, nomenclatura padrão `rmsdtostart.txt`. Cálculo de valores RMSD para cada *model* em relação a *estrutura inicial (sendo este o PDB de entrada da análise)*.
    * **RMSF:** Arquivo de cálculo RMSF referente a trajetória, nomenclatura padrão `rmsfresidue.txt`. Cálculo de Flutuação RMSF por resíduo (posição).

## 3. Pré-Processamento de Trajetória NMSIM - Troubleshooting

* Um problema bastante comum encontrado durante as análises utilizando o `NMSIM` foi arquivos de trajetória `.pdb` gerados pelo NMSIM (`*_trajectory.pdb.gz`) **sem a numeração devida dos MODELS**. Esse erro *impede a transformação correta de todas as trajetórias em grafos pelo RING*. 

* O Protocolo a seguir foi utilizado em **todas as estruturas NMSIM que apresentaram esse problema**. Em terminal (Linux ou VSCode - terminal gitbash ou Linux):
    * **1. Unzip e Checagem de 'MODEL' do arquivo PDB de trajetória em terminal:**
    ```bash
        # Descompactação usando gunzip
        gunzip -k [NOME]_trajectory.pdb.gz 
        ## Exemplo: 
        gunzip -k 9J7K_trajectory.pdb.gz

        # Contagem de strings 'MODEL' no arquivo (pro NMSIM como utilizamos, deve ser 500)
        grep -c "MODEL" [NOME]_trajectory.pdb

        # Checagem de Numeração "MODEL"
        ## Esse comando, por default, lhe devolve a linha onde o string "MODEL" se encontra.
        grep "MODEL" [NOME]_trajectory.pdb
    ```
    * **NOTA:** O parâmetro `-k` (gunzip) cria um novo arquivo descompactado, mas mantém o antigo intacto.

    * Exemplo de output de comando `grep "MODEL" *_trajectory.pdb` **quando "MODEL" NÃO está devidamente numerado**:
    ```bash
        grep "MODEL" 9J7V_trajectory.pdb
        MODEL
        MODEL
        MODEL
        MODEL
        MODEL
        MODEL
        MODEL
        MODEL
        MODEL
        MODEL
        MODEL
    ``` 
    * Exemplo de output de comando `grep "MODEL" *_trajectory.pdb` **quando "MODEL" ESTÁ devidamente numerado**:
    ```bash
        grep "MODEL" 9J7V_trajectory.pdb
        MODEL        1
        MODEL        2
        MODEL        3
        MODEL        4
        MODEL        5
        MODEL        6
        MODEL        7
        MODEL        8
        MODEL        9
        MODEL       10
    ```
    * **2. Shell Script para Numeração Automática de string "MODEL" em arquivos PDB**
    ```bash
        # Comando awk para numeração
        awk 'BEGIN{c=1} /^MODEL/ {printf "MODEL     %4d\n", c++} !/^MODEL/ {print}' [NOME]_trajectory.pdb > [NOVO_NOME].pdb
        ## Exemplo de uso
        awk 'BEGIN{c=1} /^MODEL/ {printf "MODEL     %4d\n", c++} !/^MODEL/ {print}' 9J7V_trajectory.pdb > 9J7V_fixed.pdb
    ```
    * **NOTA1:** ao nomear o novo arquivo gerado pelo comando `awk` acima, **NÃO RENOMEIE O ARQUIVO DE SAÍDA `[NOVO_NOME].pdb` COM O MESMO NOME DO ARQUIVO DE ENTRADA `[NOME]_trajectory.pdb`**. Ao fazer isso, **Perde-se os dois arquivos simultaneamente.** O arquivo original **é removido**, e o novo arquivo gerado **encontra-se vazio**. 

## 4. Pré-Processamento de Dados NMSIM - Compilando RMSD/RMSF em .CSV

* **Script `Python` para concatenar `RMSD/RMSF` em `.csv` para Plotagem em [**`HufflePlots`**](https://huffleplots.streamlit.app/):**

    * **1. Formato de Entrada de arquivos - Lista:**

    ```python
        # Lista de arquivos '.txt' de RMSD/RMSF separadamente
        ## formato [{path-to-file}, {file}.txt]
        ### RMSD
        files_rmsd = []
        ### Exemplo
        files_rmsd = [
            ('/content/exemplo_rmsdtostart.txt', 'NOMEX_WT'), #WT de 'wild-type' form (ex: '9J7V_WT')
            ('/content/exemplo_mutação_rmsdtostart.txt', 'NOMEX_XXXX') #Indico colocar a nomenclatura simplicada da mutação (ex: '9J7V_R83C')
        ]

        ### RMSF
        files_rmsf = []
        ### Exemplo
        files_rmsf = [
            ('/content/exemplo_rmsfresidue.txt', 'NOMEX_WT'),
            ('/content/exemplo_mutação_rmsfresidue.txt', 'NOMEX_XXXX')
        ]
    ```

    * **2. Concatenação de `.csv` de RMSD/RMSF:**
    ```python
        # Bibliotecas
        import pandas as pd

        # RMSD
        # montagem de dataframe concatenando informações de RMSD de diferentes proteínas gerado pelo NMSIM
        rmsd = pd.read_csv(files_rmsd[0][0], sep=r'\s+', header=None, names=['Model', files_rmsd[0][1]])
        # Merge
        for file_name, source in files_rmsd[1:]:
            temp_rmsd = pd.read_csv(file_name, sep=r'\s+', header=None, names=['Model', source])
            rmsd = rmsd.merge(temp_rmsd, on='Model', how='outer')
        # Salva df pra .csv
        rmsd.to_csv('RMSD-[SUFIXO].csv', index=False) #Substitua por sufixo de interesse


        # RMSF
        # montagem de dataframe concatenando informações de RMSF de diferentes proteínas gerado pelo NMSIM
        rmsf = pd.read_csv(files_rmsf[0][0], sep=r'\s+', header=None, names=['Model', files_rmsf[0][1]])
        # Merge
        for file_name, source in files_rmsf[1:]:
            temp_rmsf = pd.read_csv(file_name, sep=r'\s+', header=None, names=['Model', source])
            rmsf = rmsf.merge(temp_rmsf, on='Model', how='outer')
        # Salva o df em .csv
        rmsf.to_csv('RMSF-[SUFIXO].csv', index=False) #Substitua por sufixo de interesse
    ```

## 5. Transformação de Trajetórias em Grafos com RING 4.0

* Feita a correção dos string *`MODEL`* (Etapa 3), o arquivo *corrigido* de trajetória NMSIM é usado como input para conversão de garfos utilizando a versão de terminal da ferramenta [**`RING4.0`**](https://biocomputingup.it/services/download/): 
    * **Comando em terminal Linux - RING:**
    ```bash
        # Execução ferramenta RING
        ## Cria um diretório DENTRO DO ATUAL para o output, indicado pelo $PREFIXO de entrada.
        ./ mkdir [$PREFIXO_SAIDA]_ring

        ## Execução da ferramenta (usando o .pdb numerado da Etapa 3)
        ring -i [TRAJETORIA_NUMERADA].pdb --all_edges --all_models --out_dir [$PREFIXO_SAIDA]_ring
    ```
## 6. Utilitários e Troubleshooting 

* **Tornando um script gerado em Windows executável em Linux:**
    * Ao transferir scripts `.sh` de um sistema Windows para Linux, caracteres ocultos (\r) podem causar erros de execução:
    ```bash
        # Já no terminal, dentro do diretório com o seu arquivo executável dentro:
        sed -i -r 's/\r$//' seuscript.sh

        # Em tese, isso tira os erros de conversão de passagem de arquivo de Windows pra Linux.
    ```
* **Tornando executável e execução geral de scripts executáveis `.sh`**
    ```bash
        # No diretório onde se encontra o executável de interesse:
        ## 1) Torne executável com chmod +x
        chmod +x seuscript.sh

        ## 2) Execute seu arquivo .sh:
        ### DENTRO DO DIRETÓRIO CONTENDO ARQUIVOS DE ENTRADA
        ./executável.sh argumento1 argumento2

        #NOTA: Cada executável tem seu próprio grupo de argumentos (E ORDEM!!), além de REQUISITOS para que possa ser utilizado. Indico sempre abrirem e lerem o arquivo .sh para identificar o que precisa. 
    ```
    









    

