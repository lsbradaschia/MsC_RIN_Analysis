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

## 3. 

