Here's what's inside, section by section:                                                                                            
                                                                                                                                         
  Setup & Math                                                                                                                         
  - Cell 1: imports                                                                                                                      
  - Cell 2: full math derivation in 60 seconds (transition matrix → dangling fix → Google matrix → PageRank as dominant eigenvector)   
                                                                                                                                         
  Graph Builder (Cells 3–4)                                                                                                              
  - 8-node handcrafted graph with a real dangling node (node 3), a cycle (0→1→2→0), and isolated sink — every edge case covered
                                                                                                                                         
  Transition Matrix Pipeline (Cells 5–10)                                                                                              
  - build_transition_matrix: DiGraph → column-stochastic M, leaves dangling columns as zeros                                             
  - fix_dangling_nodes: replaces zero columns with 1/n — explained why (rank leaks without it)                                           
  - build_google_matrix: G = d·M + (1-d)·(1/n)·ones — shows the eigenvalue spectrum to confirm the spectral gap is ≥ 0.15
                                                                                                                                         
  Dual Solver (Cells 11–16)                                                                                                              
  - power_iteration: r = G·r until L1 delta < tol, records full history                                                                  
  - eigen_pagerank: eig(G) for right eigenvectors (not eig(G.T) — the notebook explains that pitfall: G is column-stochastic so its      
  dominant left eigenvector is just all-ones, not PageRank)                                                                              
  - Side-by-side comparison table, max diff ~6e-11                                                                                       
                                                            
  Visualizer (Cells 17–20)                                                                                                               
  - Convergence plot: per-node scores over iterations + log-scale L1 decay vs 0.85^t reference                                           
  - Graph plot: spring layout, node size and YlOrRd color both proportional to rank, with colorbar                                       
                                                                                                                                         
  SNAP at Scale (Cells 21–24)                                                                                                            
  - Downloads p2p-Gnutella08.txt.gz (~6K nodes, ~20K edges), falls back to a Barabási–Albert graph if offline                            
  - Same three functions, no changes — shows the pipeline is graph-agnostic                                                              
  - Three-panel: convergence rate, log-log rank distribution (power law tail), out-degree vs PageRank scatter (out-degree barely         
  correlates — the key insight)                                                                                                          
                                                                                                                                         
  Theory (Cell 25)                                                                                                                       
  - Proof that power iteration → dominant eigenvector: expand r₀ in eigenbasis, show all λᵢ < 1 terms decay geometrically at rate d^k    
                                                                                                                                         
  Clean Pipeline (Cells 26–27)
  - pagerank_from_scratch(G, method='power'|'eigen') — drop-in function with final verification against both methods   