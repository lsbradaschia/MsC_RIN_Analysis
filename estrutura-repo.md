```bash

/Mestrado_RIN_Analysis/   
|
├── 01_data_raw/            # Dados brutos, imutáveis. PDBs, inputs.
|   ├── example_files/      # Dados de exemplo das ferramentas
|   |   ├── HufflePlots_example/
|   |   └── SlytheRINs_example/
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
├── 03_workflows-scripts/     # Workflow e scripts do Fluxo de Trabalho. 
|   |                       
|   ├── g6pc1/                # Folder comporta workflow (formato atual explicatório - .md -; e scripts utilizados)
|   |   ├── *_workflow.md     # Protocolo organizado e explicado de denominada ferramenta e/ou análise.
|   |   └── ...
|   |                         
|   ├── m6a_complex/          
|   └── notebooks/            # Possibilidade ? Não sei o que por ainda
|
├── 04_results_processed/   # Dados gerados. Resultados e Outputs do fluxo de trabalho
|   |                       # Estrutura deve espelhar o fluxo.
|   ├── g6pc1/
|   |   ├── 01_trajectories_output/ #files de output de cada ferramenta de md utilizada
|   |   |   ├── BioEmu/
|   |   |   └── NMSIM/  
|   |   ├── 02_md_rin_graphs/
|   |   |   ├── BioEmu/
|   |   |   └── NMSIM/  
|   |   └── 03_plots_and_analysis/
|   |       ├── HufflePlots/
|   |       └── SlytheRINs/
|   └── m6a_complex/
|       ├── 01_md_trajectories/
|       └── 02_rin_graphs/
|
├── 05_docs_and_papers/     # A "COLA" - Onde o Markdown brilha.
|    ├── references/         # Diretório de ref bibliográficas. Possui um sub-diretório pra cada projeto/ferramenta
|    |   ├── g6pc1/
|    |   ├── HufflePlots/
|    |   └── ...
|    |
|    ├── *_protocol.md      # Direcionamento detalhado - projeto específico
|    ├── *_study.md         # Diário de pesquisa, anotações brutas - projeto específico
|    └── ...        
|
├── README.md               # O MAPA MESTRE.
├── estrutura-repo.md       # Este arquivo
├── requirements.txt        # Configuração (caso necessária) pra uso do DVC e outros pacotes para o REPOSITÓRIO
├── Workflow.md/jpg         # Representação visual (feito nf-core) do fluxo de trabalho geral 
|
|
|
└── ...
```
