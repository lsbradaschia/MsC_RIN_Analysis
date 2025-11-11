/Mestrado_RIN_Analysis/   
|
├── 01_data_raw/            # Dados brutos, imutáveis. PDBs, inputs.
|   ├── g6pc1/
|   |   ├── 9J7V.pdb
|   |   ├── 9J7V_H176A.pdb
|   |   ├── 9J7V_R83C.pdb
|   |   └── 9J7V_188R.pdb
|   └── m6a_complex/
|       ├── m6a_wt.pdb
|       ├── m6a-FC_AB.pdb    # Fora do contato (FC), mutação em ambas as cadeias (AB) na mesma estrutura
|       ├── m6a-C_AB.pdb     # Na região de contato (C), mutação em ambas as cadeias (AB) na mesma estrutura
|       ├── m6a-FC_A.pdb     # Fora do contato (FC) mutação APENAS NA METTL3 (A) 
|       ├── m6a-FC_B.pdb     # Fora do contato (FC) mutação APENAS NA METTL14 (B) 
|       ├── m6a-C_A.pdb      # Na região de contato (C) mutação APENAS NA METTL3 (A) 
|       └── m6a-C-B.pdb      # Na região de contato (C) mutação APENAS NA METTL14 (B)
|
├── 02_src_tools/           # Ferramentas python, disponíveis no Streamlit.
|   ├── HufflePlots/  # App Streamlit 1
|   |   ├── app.py
|   |   └── requirements.txt
|   ├── SlytheRINs/ # App Streamlit 2
|   |   ├── app.py
|   |   └── requirements.txt
|   ├── tool_3_rin_visualizer/ # App Streamlit 3
|   |   ├── app.py
|   |   └── requirements.txt
|   └── common_utils/         # Funções Python compartilhadas (ex: parsers)
|
├── 03_analysis_scripts/    # Scripts que USAM as ferramentas - PARA VERSÕES FUTURAS DE NF-CORE! 
|   |                       
|   ├── g6pc1_workflow.py     # Script que lê de 01_data_raw, chama 02_src_tools,
|   |                       # e escreve em 04_results.
|   ├── m6a_workflow.py       # Script para o M6A.
|   └── notebooks/            # Jupyter/Observable para exploração.
|
├── 04_results_processed/   # Dados gerados. Resultados e Outputs do fluxo de trabalho
|   |                       # Estrutura deve espelhar o fluxo.
|   ├── g6pc1/
|   |   ├── 01_trajectories_output/ #files de output de cada ferramenta de md utilizada
|   |   |   ├── BioEmu/
|   |   |   └── NMSIM/  
|   |   ├── 02_rin_graphs/
|   |   └── 03_analysis_plots/
|   └── m6a_complex/
|       ├── 01_md_trajectories/
|       └── 02_rin_graphs/
|
└── 05_docs_and_papers/     # A "COLA" - Onde o Markdown brilha.
    ├── README.md           # O MAPA MESTRE.
    ├── methodology.md      # Descrição do pipeline (PDB -> Grafo -> Análise)
    ├── g6pc1_study.md      # Diário de bordo da G6PC1 (o que falhou, o que funcionou)
    └── m6a_study.md        # Diário de bordo do M6A
